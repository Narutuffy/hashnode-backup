## Serving a dynamic HTML template page in Next.js

A lot of times, you don't want to port the HTML code you have to react and want to serve it directly. If it is a static page, you can just place this file in `public` dir and Next.js will take care of serving it. But if you want to change it according to some params and serve it on a different path, then you have to tweak it a bit. For this, we'll use 
- `Next Rewrites`, used to proxy routes in Next.js
- `Handlebars`, simple templating language which will help us make the HTML page dynamic

Handlebars may be an overkill but I found it to be the simplest way to achieve this. Here's what we'll do:

1. Create an API route that will return the dynamic HTML
2. Proxy the path where you want to serve the HTML to that API route

### Step 1: Creating the API 

Let's suppose the snippet below is the HTML template we want to serve (if it is this small, then you should definitely port it to react :P). Observe the `{{` and `}}`, this is where our params will be replaced. You can learn more about handlebars [here](https://handlebarsjs.com/)

```html
<!DOCTYPE html>
<html>
  <body>
    <h1>{{heading}}</h1>
		<p>Hello, my name is {{name}}</p>
    <script>
      const name = '{{name}}';
      console.log('👋, my name is ', name);
    </script>
  </body>
</html>
```

Create an API on the route `/api/template`

```js
const handlebars = require('handlebars');
const fs = require('fs');
const util = require('util');
const { resolve, join } = require('path');
const readFile = util.promisify(fs.readFile);

async function templateAPI(req, res) {
  const { heading = 'heading', name = 'luffy' } = req.query;
  try {
    // template is placed in the `templates` dir
    const templateDir = resolve(process.cwd(), 'templates');
    const templateFile = join(templateDir, 'template.hbs');
    let source = await readFile(templateFile, 'utf8');
    source = handlebars.compile(source);
    // params are being passed here
    const finalHTML = source({
      heading,
      name,
    });
    res.setHeader('Content-Type', 'text/html; charset=utf-8');
    res.write(finalHTML);
    res.end();
  } catch (error) {
    return res.status(500).end();
  }
}

export default templateAPI;
```

If I visit `http://localhost:3000/api/template?name=vamsi`, I can see it returning the HTML page with the params I passed

![Screenshot 2022-02-06 at 1.57.16 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1644136253857/X5jD_QMz-.png)


### Step 2: Proxy the path

Now that we have the API ready, we can proxy our preferred path to it. Next.js has a simple way of proxying paths using [Rewrites](https://nextjs.org/docs/api-reference/next.config.js/rewrites). If we want to serve it on path `/intro`, this is what we have to do 

```js
//In next.config.js

module.exports = {
  async rewrites() {
    return [
      {
        source: '/intro',
        destination: '/api/template',
      },
    ]
  },
}
```
Now when I visit `http://localhost:3000/intro?name=vamsi&heading=Welcome` this is what it returns
![Screenshot 2022-02-06 at 2.10.27 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1644136841159/8TcELenaB.png)

Of course, we've taken a trivial use case for this article but you can extend it for other scenarios with complex DB calls as well.

---
I recently wanted to use it for one of my projects, so thought of documenting it. If you have some suggestions or feedback, feel free to reply :)




