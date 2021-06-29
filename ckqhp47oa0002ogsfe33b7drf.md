## How to Upload Files with React and Node.js

File upload is a common operation for web applications. In Node.js, with the Express web framework and the Multer library, adding file upload feature to your app is very easy.

To add file upload feature to your app, first you need an input field with type `file`. By default this type of input renders itself as a button which is difficult to style. The user clicks the dialog and if you attach an `onChange` handler to it, then you can access the file object by using JavaScript. The `onChange` handler takes a parameter which is the `event` object. The object has the file under the `target.files` property. Once you have that, you can add it to the `FormData` object with the `append` function to attach it to the form submission.

In this article, we will make a photo manager application that lets users enter a name, description, and upload a photo with the text.

We will use React for the frontend and Express with Multer for backend.

### Backend: Node.js for File Upload

We start by building the backend. We create the project folder and a `backend` folder inside the project folder. Then go into the `backend` folder and run `npx express-generator` to generate the code files for our back end app.

Next, we run `npm i` to install the packages for our backend provided by the generator. We also need Babel to be able to use the `import` syntax, the CORS package for cross-domain communication, Multer for file upload with Express, Sequelize as our ORM, and SQLite3 as the database. We will use SQLite for simplicity.

To install all the packages, run `npm i @babel/cli @babel/core @babel/node @babel/preset-env cors multer sequelize sqlite3` Then create a file called `.babelrc` in the root of the `backend` folder and add:

    {
        "presets": [
            "@babel/preset-env]"
        ]
    }
    

Then in the `scripts` section of `package.json`, we add:

    "start": "nodemon --exec npm run babel-node --  ./bin/www",
    "babel-node": "babel-node"
    

This lets us run our app with Babel instead of regular Node runtime. We should also install `nodemon` to watch for file changes and restart the app. Install it globally by running `npm i -g nodemon`.

Next we run `npx sequelize-cli init` in the `backend` folder to create the Sequelize ORM code to let us make and run migrations.

After that finishes, we should have a `config.js` created. In there, replace the existing code with:

    {
      "development": {
        "dialect": "sqlite",
        "storage": "development.db"
      },
      "test": {
        "dialect": "sqlite",
        "storage": "test.db"
      },
      "production": {
        "dialect": "sqlite",
        "storage": "production.db"
      }
    }
    

This specifies SQLite as our database.

Next we create a migration with the model:

    npx sequelize-cli model:create --name Photo --attributes name:string,description:string,photoPath:string
    

Notice that we no spaces between commas after the `attributes` flag.

Run `npx sequelize-cli db:migrate` to create the database.

After that, we create the routes for manipulating photos. In the `routes` folder, create `photos.js` and add:

    var express = require("express");
    var multer = require("multer");
    var router = express.Router();
    const models = require("../models");
    const storage = multer.diskStorage({
      destination: (req, file, cb) => {
        cb(null, "./uploads");
      },
      filename: (req, file, cb) => {
        cb(null, `${file.fieldname}_${+new Date()}.jpg`);
      }
    });
    
    const upload = multer({
      storage
    });
    
    /* GET users listing. */
    router.get("/", async (req, res, next) => {
      const photos = await models.Photo.findAll();
      res.json(photos);
    });
    
    router.post("/add", upload.single("photo"), async (req, res, next) => {
      try {
        const path = req.file.path;
        const { name, description } = req.body;
        const entry = await models.Photo.create({
          name,
          description,
          photoPath: path
        });
        res.json(entry);
      } catch (ex) {
        res.status(400).send({ error: ex });
      }
    });
    
    router.put("/edit", upload.single("photo"), async (req, res, next) => {
      try {
        const path = req.file && req.file.path;
        const { id, name, description } = req.body;
        let params = {};
        if (path) {
          params = {
            name,
            description,
            photoPath: path
          };
        } else {
          params = {
            name,
            description
          };
        }
        const photo = await models.Photo.update(params, {
          where: {
            id
          }
        });
        res.json(photo);
      } catch (ex) {
        res.status(400).send({ error: ex });
      }
    });
    
    router.delete("/delete/:id", async (req, res, next) => {
      const { id } = req.params;
      await models.Photo.destroy({
        where: {
          id
        }
      });
      res.json({ deleted: id });
    });
    
    module.exports = router;
    

These are all the routes for manipulating photos. The `models` file is created by Sequelize CLI, which contains the model objects which we manipulate to save our data to the Photos table. We have a GET route to get the photos with `findAll`, a POST route for saving a `Photo` with `create`, a PUT route that updates the model with `update`, and a DELETE route that deletes a Photo with `destroy`.

To add file upload, we use the `multer` package which we installed earlier. It is very simple to add. We just specify the file name and folder to upload the file to, like in this block of code:

    const storage = multer.diskStorage({
      destination: (req, file, cb) => {
        cb(null, "./uploads");
      },
      filename: (req, file, cb) => {
        cb(null, `${file.fieldname}_${+new Date()}.jpg`);
      }
    });
    
    const upload = multer({
      storage
    });
    

We include the `multer` middleware with the routes we want to access files with like so:

    upload.single("photo")
    

This specifies that we let the frontend upload a file in the `FormData` with the `photo` field.

Then we can access the file path of the saved file in the routes by using `req.file.path` to save the path to our database.

We need to create an `uploads` folder in the `backend` folder to save the files.

Next in `app.js`, we replace the existing code with:

    var createError = require("http-errors");
    var express = require("express");
    var path = require("path");
    var cookieParser = require("cookie-parser");
    var logger = require("morgan");
    var cors = require("cors");
    
    var indexRouter = require("./routes/index");
    var photosRouter = require("./routes/photos");
    
    var app = express();
    
    // view engine setup
    app.set("views", path.join(__dirname, "views"));
    app.set("view engine", "jade");
    app.use(cors());
    
    app.use(logger("dev"));
    app.use(express.json());
    app.use(express.urlencoded({ extended: false }));
    app.use(cookieParser());
    app.use(express.static(path.join(__dirname, "public")));
    app.use(express.static(path.join(__dirname, "uploads")));
    
    app.use("/", indexRouter);
    app.use("/photos", photosRouter);
    
    // catch 404 and forward to error handler
    app.use(function(req, res, next) {
      next(createError(404));
    });
    
    // error handler
    app.use(function(err, req, res, next) {
      // set locals, only providing error in development
      res.locals.message = err.message;
      res.locals.error = req.app.get("env") === "development" ? err : {};
    
    // render the error page
      res.status(err.status || 500);
      res.render("error");
    });
    
    module.exports = app;
    

In this file, we added `app.use(cors());` to enable frontend cross-domain requests to backend. We expose the `photos` routes to the frontend with the following code:

    `var photosRouter = require(“./routes/photos”);
    `app.use("/photos", photosRouter);
    

And we add a static path route to access the files:

    app.use(express.static(path.join(__dirname, "uploads")));
    

Now that back end is done, we can move on to frontend.

### Frontend: File Upload with React

We will use React to build the frontend with MobX for simple state management. To create the skeleton code, we run `npx create-react-app frontend`.

Next we have to install some packages. We will install MobX, Bootstrap for styling, React Router for routing, Formik and Yup for form value handling and form validation respectively, and Axios for making HTTP requests.

To do this run `npm i axios bootstrap formik mobx mobx-react react-bootstrap react-router-dom yup` to install all the packages.

With all the packages installed, we can start writing code. To start, we replace the existing code in `App.js` with:

    import React from "react";
    import { Router, Route, Link } from "react-router-dom";
    import HomePage from "./HomePage";
    import TopBar from "./TopBar";
    import { createBrowserHistory as createHistory } from "history";
    import { photosStore } from "./store";
    const history = createHistory();
    
    function App() {
      return (
        <div className="App">
          <Router history={history}>
            <TopBar />
            <Route
              path="/"
              exact
              component={props => (
                <HomePage {...props} photosStore={photosStore} />
              )}
            />
          </Router>
        </div>
      );
    }
    
    export default App;
    

This adds a top bar which we will create next, and we define the routes so that we see the home page and address generator page when we go to the defined URLs.

Next create `HomePage.js` in the `src` for our home page. In it, we will display the table of entries, which has the name, description, the photo, and buttons to open the add / edit photo forms, and delete the entries.

In the file, we add:

    import React, { useState, useEffect } from "react";
    import * as yup from "yup";
    import "./HomePage.css";
    import Modal from "react-bootstrap/Modal";
    import PhotoForm from "./PhotoForm";
    import Button from "react-bootstrap/Button";
    import Table from "react-bootstrap/Table";
    import { observer } from "mobx-react";
    import { getPhotos, deletePhoto, APIURL } from "./requests";
    
    function HomePage({ photosStore }) {
      const [show, setShow] = useState(false);
      const [showEdit, setShowEdit] = useState(false);
      const [initialized, setInitialized] = useState(false);
      const [selectedPhoto, setSelectedPhoto] = useState({});
      const handleClose = () => setShow(false);
      const handleShow = () => setShow(true);
    
      const handleEditClose = () => setShowEdit(false);
      const handleEditShow = photo => {
        setSelectedPhoto(photo);
        setShowEdit(true);
      };
    
      const getAllPhotos = async () => {
        const response = await getPhotos();
        photosStore.setPhotos(response.data);
      };
    
      const deletePhotoById = async id => {
        await deletePhoto(id);
        await getAllPhotos();
      };
    
      const onSave = () => {
        setShow(false);
        setShowEdit(false);
      };
    
      useEffect(() => {
        if (!initialized) {
          getAllPhotos();
          setInitialized(true);
        }
      });
    
      return (
        <div className="home-page">
          <h1>Photos</h1>
          <Button variant="primary" onClick={handleShow}>
            Add Photo
          </Button>
    
          <Table striped bordered hover style={{ marginTop: 10 }}>
            <thead>
              <tr>
                <th>Name</th>
                <th>Description</th>
                <th>Photo</th>
                <th></th>
                <th></th>
              </tr>
            </thead>
            <tbody>
              {photosStore.photos.map((p, i) => {
                const splitPath = p.photoPath.split("");
                const path = splitPath[splitPath.length - 1];
                return (
                  <tr key={i}>
                    <td>{p.name}</td>
                    <td>{p.description}</td>
                    <td>
                      <img src={`${APIURL}/${path}`} style={{ width: 200 }} />
                    </td>
                    <td>
                      <Button onClick={handleEditShow.bind(this, p)}>Edit</Button>
                    </td>
                    <td>
                      <Button onClick={deletePhotoById.bind(this, p.id)}>
                        Delete
                      </Button>
                    </td>
                  </tr>
                );
              })}
            </tbody>
          </Table>
    
          <Modal show={show} onHide={handleClose}>
            <Modal.Header closeButton>
              <Modal.Title>Add Photo</Modal.Title>
            </Modal.Header>
    
            <Modal.Body>
              <PhotoForm
                edit={false}
                photosStore={photosStore}
                onSave={onSave.bind(this)}
              />
            </Modal.Body>
          </Modal>
    
         <Modal show={showEdit} onHide={handleEditClose}>
            <Modal.Header closeButton>
              <Modal.Title>Edit Photo</Modal.Title>
            </Modal.Header>
    
            <Modal.Body>
              <PhotoForm
                edit={true}
                photosStore={photosStore}
                selectedPhoto={selectedPhoto}
                onSave={onSave.bind(this)}
              />
            </Modal.Body>
          </Modal>
        </div>
      );
    }
    
    export default observer(HomePage);
    

The `Table` is provided by React Bootstrap. We just display every entry in its own row. The entries are provided by our MobX store which we will create. The modals contain the `PhotoForm` which has all the inputs for manipulating photos. We will use it for both add and edit so we need to pass in the `edit` prop to distinguish between add and edit. We also pass in a `onSave` function so that we can close the modals. If there is an entry selected for edit, we also pass in `selectedPhoto` prop, which contains the entry that the user is editing.

Next we build the photo form. Create a file called `PhotoForm.js` in the `src` folder and add:

    import React from "react";
    import { useEffect, useState } from "react";
    import { Formik } from "formik";
    import Form from "react-bootstrap/Form";
    import Col from "react-bootstrap/Col";
    import Button from "react-bootstrap/Button";
    import * as yup from "yup";
    import "./PhotoForm.css";
    import { getPhotos, addPhoto, editPhoto } from "./requests";
    import { observer } from "mobx-react";
    
    const schema = yup.object({
      name: yup.string().required("Name is required"),
      description: yup.string().required("Description is required")
    });
    
    function PhotoForm({ photosStore, edit, selectedPhoto, onSave }) {
      const fileUpload = React.createRef();
      const [photo, setPhoto] = useState(null);
      const [fileName, setFileName] = useState("");
      const getAllPhotos = async () => {
        const response = await getPhotos();
        photosStore.setPhotos(response.data);
      };
    
      const handleSubmit = async evt => {
        const isValid = await schema.validate(evt);
        if (!isValid) {
          return;
        }
        try {
          let bodyFormData = new FormData();
          if (!edit) {
            bodyFormData.set("name", evt.name);
            bodyFormData.set("description", evt.description);
            bodyFormData.append("photo", photo);
            await addPhoto(bodyFormData);
          } else {
            bodyFormData.set("id", selectedPhoto.id);
            bodyFormData.set("name", evt.name);
            bodyFormData.set("description", evt.description);
            if (photo) {
              bodyFormData.append("photo", photo);
            }
            await editPhoto(bodyFormData);
          }
        } catch (error) {
          alert("Upload must be an image");
        }
    
      await getAllPhotos();
        onSave();
      };
    
      const setFile = evt => {
        setPhoto(evt.target.files[0]);
        setFileName(evt.target.files[0].name);
      };
    
      const openUploadDialog = () => {
        fileUpload.current.click();
      };
    
      return (
        <div>
          <Formik
            validationSchema={schema}
            onSubmit={handleSubmit}
            initialValues={edit ? selectedPhoto : {}}
          >
            {({
              handleSubmit,
              handleChange,
              handleBlur,
              values,
              touched,
              isInvalid,
              errors
            }) => (
              <Form noValidate onSubmit={handleSubmit}>
                <Form.Row>
                  <Form.Group as={Col} md="12" controlId="name">
                    <Form.Label>Name</Form.Label>
                    <Form.Control
                      type="text"
                      name="name"
                      placeholder="Name"
                      value={values.name || ""}
                      onChange={handleChange}
                      isInvalid={touched.name && errors.name}
                    />
                    <Form.Control.Feedback type="invalid">
                      {errors.name}
                    </Form.Control.Feedback>
                  </Form.Group>
                  <Form.Group as={Col} md="12" controlId="description">
                    <Form.Label>Description</Form.Label>
                    <Form.Control
                      type="text"
                      name="description"
                      placeholder="Description"
                      value={values.description || ""}
                      onChange={handleChange}
                      isInvalid={touched.description && errors.description}
                    />
    
                    <Form.Control.Feedback type="invalid">
                      {errors.description}
                    </Form.Control.Feedback>
                  </Form.Group>
                </Form.Row>
                <Form.Row>
                  <Form.Group as={Col} md="12" controlId="photo">
                    <input
                      type="file"
                      ref={fileUpload}
                      name="photo"
                      style={{ display: "none" }}
                      onChange={setFile}
                    />
                    <div className="file-box">
                      <Button type="button" onClick={openUploadDialog}>
                        Upload Photo
                      </Button>
                      <span style={{ paddingLeft: "10px", marginTop: "5px" }}>
                        {fileName}
                      </span>
                    </div>
                  </Form.Group>
                </Form.Row>
                <Button type="submit" style={{ marginRight: "10px" }}>
                  Save
                </Button>
              </Form>
            )}
          </Formik>
        </div>
      );
    }
    
    export default observer(PhotoForm);
    

In this file, we have the file input for getting the photo file and the form fields for name and description. To get the file, we pass the `setFile` function into the `onChange` prop of the file input, where we get the file object by using `ev.target.files[0]`. The `[0]` means that we only want the first file.

We wrapped the React Boostrap form inside the `Formik` component to automatically handle form values, which will be available in the parameter of the `handleSubmit` function. In that function, we validate the text inputs with the `schema.validation` function. Then we add the text data and file to the `FormData` object. After everything is added, we submit the `FormData` object to our back end via HTTP.

After that, we call `getAllPhotos` to get the latest data and set the new data in our MobX store. Then we call `onSave` function, which is passed in from `HomePage` so that we can close the dialog box after everything is done.

Next create `PhotoForm.css` and add:

    .file-box {
      display: flex;
    }
    

Then create `requests.js` in the `src` folder and add:

    const axios = require("axios");
    export const APIURL = "http://localhost:3000";
    
    export const getPhotos = () => axios.get(`${APIURL}/photos`);
    
    export const addPhoto = data =>
      axios({
        method: "post",
        url: `${APIURL}/photos/add`,
        data,
        config: { headers: { "Content-Type": "multipart/form-data" } }
      });
    
    export const editPhoto = data =>
      axios({
        method: "put",
        url: `${APIURL}/photos/edit`,
        data,
        config: { headers: { "Content-Type": "multipart/form-data" } }
      });
    
    export const deletePhoto = id => axios.delete(`${APIURL}/photos/delete/${id}`);
    

We need these functions to send the requests to the backend. Note that we have `config: { headers: { “Content-Type”: “multipart/form-data” } }` in the post so that we send form data instead of the default JSON to the back end. Form data can include files but JSON cannot.

Next we create the MobX Store. Create a file called `store.js` and add:

    import { observable, action, decorate } from "mobx";
    
    class PhotosStore {
      photos = [];
    
      setPhotos(photos) {
        this.photos = photos;
      }
    }
    
    PhotosStore = decorate(PhotosStore, {
      photos: observable,
      setPhotos: action
    });
    
    const photosStore = new PhotosStore();
    
    export { photosStore };
    

We have the function `setPhotos` to put the photo data in the store, which we used in `HomePage` and `PhotoForm` and we instantiated it before exporting so that we only have to do it in one place.

Next we create the top bar by creating a `TopBar.js` file in the `src` folder and add:

    import React from "react";
    import Navbar from "react-bootstrap/Navbar";
    import Nav from "react-bootstrap/Nav";
    import { withRouter } from "react-router-dom";
    
    function TopBar({ location }) {
      const { pathname } = location;
    
      return (
        <Navbar bg="primary" expand="lg" variant="dark">
          <Navbar.Brand href="#home">Photo App</Navbar.Brand>
          <Navbar.Toggle aria-controls="basic-navbar-nav" />
          <Navbar.Collapse id="basic-navbar-nav">
            <Nav className="mr-auto">
              <Nav.Link href="/" active={pathname == "/"}>
                Home
              </Nav.Link>
            </Nav>
          </Navbar.Collapse>
        </Navbar>
      );
    }
    
    export default withRouter(TopBar);
    

This contains the React Bootstrap `Navbar` to show a top bar with a link to the home page and the name of the app.

Finally in `index.html`, we replace the existing code with:

    <!DOCTYPE html>
    <html lang="en">
      <head>
        <meta charset="utf-8" />
        <link rel="shortcut icon" href="%PUBLIC_URL%/favicon.ico" />
        <meta name="viewport" content="width=device-width, initial-scale=1" />
        <meta name="theme-color" content="#000000" />
        <meta
          name="description"
          content="Web site created using create-react-app"
        />
        <link rel="apple-touch-icon" href="logo192.png" />
        <!--
          manifest.json provides metadata used when your web app is installed on a
          user's mobile device or desktop. See https://developers.google.com/web/fundamentals/web-app-manifest/
        -->
        <link rel="manifest" href="%PUBLIC_URL%/manifest.json" />
        <!--
          Notice the use of %PUBLIC_URL% in the tags above.
          It will be replaced with the URL of the `public` folder during the build.
          Only files inside the `public` folder can be referenced from the HTML.
    
          Unlike "/favicon.ico" or "favicon.ico", "%PUBLIC_URL%/favicon.ico" will
          work correctly both with client-side routing and a non-root public URL.
          Learn how to configure a non-root public URL by running `npm run build`.
        -->
        <title>Photo App</title>
        <link
          rel="stylesheet"
          href="https://maxcdn.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css"
          integrity="sha384-ggOyR0iXCbMQv3Xipma34MD+dH/1fQ784/j6cY/iJTQUOhcWr7x9JvoRxT2MZw1T"
          crossorigin="anonymous"
        />
      </head>
      <body>
        <noscript>You need to enable JavaScript to run this app.</noscript>
        <div id="root"></div>
        <!--
          This HTML file is a template.
          If you open it directly in the browser, you will see an empty page.
    
          You can add webfonts, meta tags, or analytics to this file.
          The build step will place the bundled scripts into the <body> tag.
    
          To begin the development, run `npm start` or `yarn start`.
          To create a production bundle, use `npm run build` or `yarn build`.
        -->
      </body>
    </html>
    

This adds the Bootstrap CSS file in the `link` tag and change the title of the app.

After writing all that code, we can run our app. First start the backend by running `npm start` in the `backend` folder and `npm start` in the `frontend` folder, then choose `‘yes’` if you’re asked to run it from a different port.

The post [How to Upload Files with React and Node.js](https://thewebdev.info/2021/06/28/how-to-upload-files-with-react-and-nodejs/) appeared first on [The Web Dev](https://thewebdev.info).