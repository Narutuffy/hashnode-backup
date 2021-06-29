## How to Use MobX for Easy State Management in Your Angular App

Angular is a comprehensive frontend framework for building web applications. It comes with a lot of great features like component-based architectures, communication between parent and child components, routing, guards for controlling access to routes, services for shared code, and TypeScript for easy development. However, one big feature that it does not come with it is global state management for the app.

MobX is a simple state management library. It works by allowing any code that references the MobX store to observe values and trigger a change when that value changes. The store also exposes functions that you can use to set the values.

To use MobX, you just import the store into the component where you want to use it, so there’s no boilerplate to add until NgRX/store. This means that the state management is much simpler than with NgRX/store. There is no need for dispatch functions and reducers with MobX.

Angular and MobX work great together because of the MobX-Angular library.

In this article, we will build an address book app that stores contacts in a backend. We use MobX-Angular to share data between components. For styling, we use Ngx-Boostrap library to style the app.

We start by installing the Angular CLI by running `npm i -g @angular/cli`. Next we run `ng new address-book-app` to create the files for our address book app. Be sure to choose to include the routing and SCSS for styling.

Next we install a few libraries that we need to use. We MobX and MobX-Angular for state management, Ng2-Validation for form validation, and Ngx-Bootstrap for styling.

After all the libraries are installed, we run a few commands to create more code files.

    ng g component contactForm
    ng g component homePage
    ng g class contactStore
    ng g service contacts
    ng g class exports
    

This will create files for our home page, contact form, and a service for sending HTTP requests to our server,

Now in `contact-form.component.ts`, we put:

    import { Component, OnInit, Input, Output, SimpleChanges } from '@angular/core';
    import { COUNTRIES } from '../exports';
    import { contactStore } from '../contact-store';
    import { EventEmitter } from '@angular/core';
    import { ContactsService } from '../contacts.service';
    import { NgForm } from '@angular/forms';
    
    @Component({
      selector: 'app-contact-form',
      templateUrl: './contact-form.component.html',
      styleUrls: ['./contact-form.component.scss']
    })
    export class ContactFormComponent implements OnInit {
      contactData: any = <any>{};
      countries = COUNTRIES;
      store = contactStore;
      @Input('edit') edit: boolean;
      @Input('contact') contact: any = <any>{};
      @Output('contactEdited') contactEdited = new EventEmitter();
    
      constructor(
        private contactsService: ContactsService
      ) { }
    
      ngOnInit() {
      }
    
      ngOnChanges(changes: SimpleChanges) {
        if (this.contact) {
          this.contactData = Object.assign({}, this.contact);
        }
      }
    
      getPostalCodeRegex() {
        if (this.contactData.country == "United States") {
          return /^[0-9]{5}(?:-[0-9]{4})?$/;
        } else if (this.contactData.country == "Canada") {
          return /^[A-Za-z]d[A-Za-z][ -]?d[A-Za-z]d$/;
        }
        return /./;
      }
    
      getPhoneRegex() {
        if (["United States", "Canada"].includes(this.contactData.country)) {
          return /^[2-9]d{2}[2-9]d{2}d{4}$/;
        }
        return /./;
      }
    
      saveContact(contactForm: NgForm) {
        if (contactForm.invalid) {
          return;
        }
    
        if (this.edit) {
          this.contactsService.editContact(this.contactData)
            .subscribe(res => {
              this.getContacts();
            })
        }
        else {
          this.contactsService.addContact(this.contactData)
            .subscribe(res => {
              this.getContacts();
            })
        }
      }
    
      getContacts() {
        this.contactsService.getContacts()
          .subscribe(res => {
            this.store.setContacts(res);
            this.contactEdited.emit();
          })
      }
    
    }
    

This is the logic for the contact form. We created functions for getting the regular expression for the phone and postal code depending on the country, `getPostalCodeRegex` and `getPhoneRegex` respectively. Also we have a function for saving our contact and getting it after it’s saved, the `saveContact` and `getContacts` functions respectively. We need the `ngOnChanges` life cycle handler to get an existing contact if it’s passed in.

Whenever we get contacts, we call `this.store.setContacts` where `this.store` is the MobX store that we will create, and it sets the latest contacts array to the store.

In `contact-form.component.html`, we add:

    <form #contactForm="ngForm" (ngSubmit)="saveContact(contactForm)">
      <div class="form-group">
        <label>First Name</label>
        <input
          type="text"
          class="form-control"
          placeholder="First Name"
          #firstName="ngModel"
          name="firstName"
          [(ngModel)]="contactData.firstName"
          required
        />
        <div *ngIf="firstName.invalid && (firstName.dirty || firstName.touched)">
          <div *ngIf="firstName.errors.required">
            First Name is required.
          </div>
        </div>
      </div>
      <div class="form-group">
        <label>Last Name</label>
        <input
          type="text"
          class="form-control"
          placeholder="Last Name"
          #lastName="ngModel"
          name="lastName"
          [(ngModel)]="contactData.lastName"
          required
        />
        <div *ngIf="lastName?.invalid && (lastName.dirty || lastName.touched)">
          <div *ngIf="lastName.errors.required">
            Last Name is required.
          </div>
        </div>
      </div>
    
      <div class="form-group">
        <label>Address Line 1</label>
        <input
          type="text"
          class="form-control"
          placeholder="Address Line 1"
          #addressLineOne="ngModel"
          name="addressLineOne"
          [(ngModel)]="contactData.addressLineOne"
          required
        />
        <div
          *ngIf="
            addressLineOne?.invalid &&
            (addressLineOne.dirty || addressLineOne.touched)
          "
        >
          <div *ngIf="addressLineOne.errors.required">
            Address line 1 is required.
          </div>
        </div>
      </div>
    
      <div class="form-group">
        <label>Address Line 2</label>
        <input
          type="text"
          class="form-control"
          placeholder="Address Line 2"
          #addressLineTwo="ngModel"
          name="addressLineTwo"
          [(ngModel)]="contactData.addressLineTwo"
        />
      </div>
    
      <div class="form-group">
        <label>City</label>
        <input
          type="text"
          class="form-control"
          placeholder="City"
          #city="ngModel"
          name="city"
          [(ngModel)]="contactData.city"
          required
        />
        <div *ngIf="city?.invalid && (city.dirty || city.touched)">
          <div *ngIf="city.errors.required">
            City is required.
          </div>
          <div *ngIf="city.invalid">
            City is invalid.
          </div>
        </div>
      </div>
    
      <div class="form-group">
        <label>Country</label>
        <select
          class="form-control"
          #country="ngModel"
          name="country"
          [(ngModel)]="contactData.country"
          required
        >
          <option *ngFor="let c of countries" [value]="c.name">
            {{ c.name }}
          </option>
        </select>
        <div *ngIf="country?.invalid && (country.dirty || country.touched)">
          <div *ngIf="country.errors.required">
            Country is required.
          </div>
        </div>
      </div>
    
      <div class="form-group">
        <label>Postal Code</label>
        <input
          type="text"
          class="form-control"
          placeholder="Postal Code"
          #postalCode="ngModel"
          name="postalCode"
          [(ngModel)]="contactData.postalCode"
          required
          [pattern]="getPostalCodeRegex()"
        />
        <div
          *ngIf="postalCode?.invalid && (postalCode.dirty || postalCode.touched)"
        >
          <div *ngIf="postalCode.errors.required">
            Postal code is required.
          </div>
          <div *ngIf="postalCode.invalid">
            Postal code is invalid.
          </div>
        </div>
      </div>
    
      <div class="form-group">
        <label>Phone</label>
        <input
          type="text"
          class="form-control"
          placeholder="Phone"
          #phone="ngModel"
          name="phone"
          [(ngModel)]="contactData.phone"
          required
          [pattern]="getPhoneRegex()"
        />
        <div *ngIf="phone?.invalid && (phone.dirty || phone.touched)">
          <div *ngIf="phone.errors.required">
            Phone is required.
          </div>
        </div>
      </div>
    
      <div class="form-group">
        <label>Age</label>
        <input
          type="text"
          class="form-control"
          placeholder="Age"
          #age="ngModel"
          name="age"
          [(ngModel)]="contactData.age"
          required
          [range]="[0, 200]"
        />
        <div *ngIf="age?.invalid && (age.dirty || age.touched)">
          <div *ngIf="age.errors.required">
            Age is required.
          </div>
          <div *ngIf="age.invalid">
            Age must be between 0 and 200.
          </div>
        </div>
      </div>
    
      <div class="form-group">
        <label>Email</label>
        <input
          type="text"
          class="form-control"
          placeholder="Email"
          #email="ngModel"
          name="email"
          [(ngModel)]="contactData.email"
          required
          email
        />
        <div *ngIf="email?.invalid && (email.dirty || email.touched)">
          <div *ngIf="email.errors.required">
            Email is required.
          </div>
          <div *ngIf="email.invalid">
            Email is invalid.
          </div>
        </div>
      </div>
      <button type="submit" class="btn btn-primary">Submit</button>
    </form>
    

This is the contact form itself. The `email` directive is provided by `ng2-validation` for validating email. Also the `[range]=”[0, 200]”` input is also provided by the same library for validation number range.

Next we work on the home page. In `home-page.component.ts`, we add:

    import { Component, OnInit, TemplateRef } from '@angular/core';
    import { BsModalService, BsModalRef } from 'ngx-bootstrap/modal';
    import { ContactsService } from '../contacts.service';
    import { contactStore } from '../contact-store';
    
    @Component({
      selector: 'app-home-page',
      templateUrl: './home-page.component.html',
      styleUrls: ['./home-page.component.scss']
    })
    export class HomePageComponent implements OnInit {
    
      modalRef: BsModalRef;
      store = contactStore;
      edit: boolean;
      contacts: any[] = [];
      selectedContact: any = <any>{};
      constructor(
        private modalService: BsModalService,
        private contactsService: ContactsService
      ) { }
    
      openModal(addTemplate: TemplateRef<any>) {
        this.modalRef = this.modalService.show(addTemplate);
      }
    
      openEditModal(editTemplate: TemplateRef<any>, contact) {
        this.selectedContact = contact;
        this.modalRef = this.modalService.show(editTemplate);
      }
    
      closeModal() {
        this.modalRef.hide();
        this.selectedContact = {};
      }
    
      ngOnInit() {
        this.contactsService.getContacts()
          .subscribe(res => {
            this.store.setContacts(res);
          })
      }
    
      deleteContact(id) {
        this.contactsService.deleteContact(id)
          .subscribe(res => {
            this.getContacts();
          })
      }
    
      getContacts() {
        this.contactsService.getContacts()
          .subscribe(res => {
            this.store.setContacts(res);
          })
      }
    }
    

We have the `getContacts` function to get contacts, `deleteContact` function to delete a contact, and `openModal` / `openEditModal` functions to open the add contact and edit contact modals respectively. The `closeModal` function is used to close the modal with the `contactEdited` event emitted from `ContactFormComponent` .

Whenever we get contacts, we call `this.store.setContacts` to set the latest contacts array to the store.

Next, in `home-page.component.html` we add:

    <div class="home-page" *mobxAutorun>
      <h1 class="text-center">Address Book</h1>
      <button
        type="button"
        class="btn btn-primary"
        (click)="openModal(addTemplate)"
      >
        Add Contact
      </button>
    
      <div class="table-container">
        <table class="table">
          <thead>
            <tr>
              <th>First Name</th>
              <th>Last Name</th>
              <th>Address</th>
              <th>Phone</th>
              <th>Age</th>
              <th>Email</th>
              <th></th>
              <th></th>
            </tr>
          </thead>
          <tbody>
            <tr *ngFor="let c of store.contacts">
              <td>{{ c.firstName }}</td>
              <td>{{ c.lastName }}</td>
              <td>
                {{ c.addressLineOne }}, {{ c.city }},
                {{ c.country }}
              </td>
              <td>{{ c.phone }}</td>
              <td>{{ c.age }}</td>
              <td>{{ c.email }}</td>
              <td>
                <button
                  type="button"
                  class="btn btn-primary"
                  (click)="openEditModal(editTemplate, c)"
                >
                  Edit
                </button>
              </td>
              <td>
                <button
                  type="button"
                  class="btn btn-primary"
                  (click)="deleteContact(c.id)"
                >
                  Delete
                </button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
    
    <ng-template #addTemplate>
      <div class="modal-header">
        <h4 class="modal-title pull-left">Add Contact</h4>
      </div>
      <div class="modal-body">
        <app-contact-form
          (contactEdited)="closeModal()"
          [edit]="false"
        ></app-contact-form>
      </div>
    </ng-template>
    
    <ng-template #editTemplate>
      <div class="modal-header">
        <h4 class="modal-title pull-left">Edit Contact</h4>
      </div>
      <div class="modal-body">
        <app-contact-form
          (contactEdited)="closeModal()"
          [edit]="true"
          [contact]='selectedContact'
        ></app-contact-form>
      </div>
    </ng-template>
    

We have a table to display the list of contacts. The `*ngFor=”let c of store.contacts”` directive is in the `tr` tag to loop through the contacts store them in our MobX store.

The `ng-template` components are modals provided by `ngx-bootstrap`. We have `addTemplate` for using the contact form to add a contact, and `editTemplate` to display the same form for editing.

In `home-page.component.scss`, we add:

    .home-page {
        padding: 20px;
    }
    
    .table-container {
        margin-top: 20px;
    }
    

In `app-routing.module.ts`, which we get by including the routing when running the Angular CLI wizard, we put:

    import { NgModule } from '@angular/core';
    import { Routes, RouterModule } from '@angular/router';
    import { HomePageComponent } from './home-page/home-page.component';
    
    const routes: Routes = [
      { path: '', component: HomePageComponent },
    ];
    
    @NgModule({
      imports: [RouterModule.forRoot(routes)],
      exports: [RouterModule]
    })
    export class AppRoutingModule { }
    

This allows users to navigate to the home page.

In `app.component.html`, we replace the existing code with:

    <nav class="navbar navbar-expand-lg navbar-light bg-light">
      <a class="navbar-brand" href="#">Address Book</a>
      <button
        class="navbar-toggler"
        type="button"
        data-toggle="collapse"
        data-target="#navbarSupportedContent"
        aria-controls="navbarSupportedContent"
        aria-expanded="false"
        aria-label="Toggle navigation"
      >
        <span class="navbar-toggler-icon"></span>
      </button>
    
      <div class="collapse navbar-collapse" id="navbarSupportedContent">
        <ul class="navbar-nav mr-auto">
          <li class="nav-item active">
            <a class="nav-link" href="#">Home </a>
          </li>
        </ul>
      </div>
    </nav>
    <router-outlet></router-outlet>
    

Here we add the Bootstrap navigation bar for showing the app title and navigation links.

Next in `app.module.ts`, we replace the existing code with:

    import { BrowserModule } from '@angular/platform-browser';
    import { NgModule } from '@angular/core';
    import { FormsModule } from '@angular/forms';
    import { HttpClientModule } from '@angular/common/http';
    import { AppRoutingModule } from './app-routing.module';
    import { AppComponent } from './app.component';
    import { HomePageComponent } from './home-page/home-page.component';
    import { ContactFormComponent } from './contact-form/contact-form.component';
    import { ModalModule } from 'ngx-bootstrap/modal';
    import { CustomFormsModule } from 'ng2-validation'
    import { MobxAngularModule } from 'mobx-angular';
    import { ContactsService } from './contacts.service';
    
    @NgModule)({
      declarations: [
        AppComponent,
        HomePageComponent,
        ContactFormComponent
      ],
      imports: [
        BrowserModule,
        AppRoutingModule,
        FormsModule,
        HttpClientModule,
        CustomFormsModule,
        ModalModule.forRoot(),
        MobxAngularModule
      ],
      providers: [
        ContactsService
      ],
      bootstrap: [AppComponent]
    })
    export class AppModule { }
    

This includes all the libraries we use for building the app along with the code we generated.

Then we create the MobX store. In `contact-store.ts`, we add:

    import { observable, action } from 'mobx-angular';
    
    class ContactStore {
        @observable contacts = [];
        @action setContacts(contacts) {
            this.contacts = contacts;
        }
    }
    
    export const contactStore = new ContactStore();
    

The store is very simple. It has the `contacts` array which we decorate with the `observable` decorator so that it will be always updated everywhere when we reference it in our code. The `setContacts` function lets us set the value of the `contacts` array which will be propagated to any code that references it since we have the `observable` decorator in front of it. The `action` decorator means that we designate the function to be able to update `observable` values in the store.

We export the instance of the store, which we use in our components wherever we reference `contactStore` .

Next in `contact.service.ts`, we add:

    import { Injectable } from '@angular/core';
    import { HttpClient } from '@angular/common/http';
    import { environment } from 'src/environments/environment';
    
    @Injectable({
      providedIn: 'root'
    })
    export class ContactsService {
    
    constructor(
        private http: HttpClient
      ) { }
    
      getContacts() {
        return this.http.get(`${environment.apiUrl}/contacts`);
      }
    
      addContact(data) {
        return this.http.post(`${environment.apiUrl}/contacts`, data);
      }
    
      editContact(data) {
        return this.http.put(`${environment.apiUrl}/contacts/${data.id}`, data);
      }
    
      deleteContact(id) {
        return this.http.delete(`${environment.apiUrl}/contacts/${id}`);
      }
    }
    

This lets make HTTP requests to our backend for manipulating contacts.

In `exports.ts`, we add:

    export const COUNTRIES = [
        { "name": "Afghanistan", "code": "AF" },
        { "name": "Aland Islands", "code": "AX" },
        { "name": "Albania", "code": "AL" },
        { "name": "Algeria", "code": "DZ" },
        { "name": "American Samoa", "code": "AS" },
        { "name": "AndorrA", "code": "AD" },
        { "name": "Angola", "code": "AO" },
        { "name": "Anguilla", "code": "AI" },
        { "name": "Antarctica", "code": "AQ" },
        { "name": "Antigua and Barbuda", "code": "AG" },
        { "name": "Argentina", "code": "AR" },
        { "name": "Armenia", "code": "AM" },
        { "name": "Aruba", "code": "AW" },
        { "name": "Australia", "code": "AU" },
        { "name": "Austria", "code": "AT" },
        { "name": "Azerbaijan", "code": "AZ" },
        { "name": "Bahamas", "code": "BS" },
        { "name": "Bahrain", "code": "BH" },
        { "name": "Bangladesh", "code": "BD" },
        { "name": "Barbados", "code": "BB" },
        { "name": "Belarus", "code": "BY" },
        { "name": "Belgium", "code": "BE" },
        { "name": "Belize", "code": "BZ" },
        { "name": "Benin", "code": "BJ" },
        { "name": "Bermuda", "code": "BM" },
        { "name": "Bhutan", "code": "BT" },
        { "name": "Bolivia", "code": "BO" },
        { "name": "Bosnia and Herzegovina", "code": "BA" },
        { "name": "Botswana", "code": "BW" },
        { "name": "Bouvet Island", "code": "BV" },
        { "name": "Brazil", "code": "BR" },
        { "name": "British Indian Ocean Territory", "code": "IO" },
        { "name": "Brunei Darussalam", "code": "BN" },
        { "name": "Bulgaria", "code": "BG" },
        { "name": "Burkina Faso", "code": "BF" },
        { "name": "Burundi", "code": "BI" },
        { "name": "Cambodia", "code": "KH" },
        { "name": "Cameroon", "code": "CM" },
        { "name": "Canada", "code": "CA" },
        { "name": "Cape Verde", "code": "CV" },
        { "name": "Cayman Islands", "code": "KY" },
        { "name": "Central African Republic", "code": "CF" },
        { "name": "Chad", "code": "TD" },
        { "name": "Chile", "code": "CL" },
        { "name": "China", "code": "CN" },
        { "name": "Christmas Island", "code": "CX" },
        { "name": "Cocos (Keeling) Islands", "code": "CC" },
        { "name": "Colombia", "code": "CO" },
        { "name": "Comoros", "code": "KM" },
        { "name": "Congo", "code": "CG" },
        { "name": "Congo, The Democratic Republic of the", "code": "CD" },
        { "name": "Cook Islands", "code": "CK" },
        { "name": "Costa Rica", "code": "CR" },
        {
            "name": "Cote D"Ivoire", "code": "CI"
        },
        { "name": "Croatia", "code": "HR" },
        { "name": "Cuba", "code": "CU" },
        { "name": "Cyprus", "code": "CY" },
        { "name": "Czech Republic", "code": "CZ" },
        { "name": "Denmark", "code": "DK" },
        { "name": "Djibouti", "code": "DJ" },
        { "name": "Dominica", "code": "DM" },
        { "name": "Dominican Republic", "code": "DO" },
        { "name": "Ecuador", "code": "EC" },
        { "name": "Egypt", "code": "EG" },
        { "name": "El Salvador", "code": "SV" },
        { "name": "Equatorial Guinea", "code": "GQ" },
        { "name": "Eritrea", "code": "ER" },
        { "name": "Estonia", "code": "EE" },
        { "name": "Ethiopia", "code": "ET" },
        { "name": "Falkland Islands (Malvinas)", "code": "FK" },
        { "name": "Faroe Islands", "code": "FO" },
        { "name": "Fiji", "code": "FJ" },
        { "name": "Finland", "code": "FI" },
        { "name": "France", "code": "FR" },
        { "name": "French Guiana", "code": "GF" },
        { "name": "French Polynesia", "code": "PF" },
        { "name": "French Southern Territories", "code": "TF" },
        { "name": "Gabon", "code": "GA" },
        { "name": "Gambia", "code": "GM" },
        { "name": "Georgia", "code": "GE" },
        { "name": "Germany", "code": "DE" },
        { "name": "Ghana", "code": "GH" },
        { "name": "Gibraltar", "code": "GI" },
        { "name": "Greece", "code": "GR" },
        { "name": "Greenland", "code": "GL" },
        { "name": "Grenada", "code": "GD" },
        { "name": "Guadeloupe", "code": "GP" },
        { "name": "Guam", "code": "GU" },
        { "name": "Guatemala", "code": "GT" },
        { "name": "Guernsey", "code": "GG" },
        { "name": "Guinea", "code": "GN" },
        { "name": "Guinea-Bissau", "code": "GW" },
        { "name": "Guyana", "code": "GY" },
        { "name": "Haiti", "code": "HT" },
        { "name": "Heard Island and Mcdonald Islands", "code": "HM" },
        { "name": "Holy See (Vatican City State)", "code": "VA" },
        { "name": "Honduras", "code": "HN" },
        { "name": "Hong Kong", "code": "HK" },
        { "name": "Hungary", "code": "HU" },
        { "name": "Iceland", "code": "IS" },
        { "name": "India", "code": "IN" },
        { "name": "Indonesia", "code": "ID" },
        { "name": "Iran, Islamic Republic Of", "code": "IR" },
        { "name": "Iraq", "code": "IQ" },
        { "name": "Ireland", "code": "IE" },
        { "name": "Isle of Man", "code": "IM" },
        { "name": "Israel", "code": "IL" },
        { "name": "Italy", "code": "IT" },
        { "name": "Jamaica", "code": "JM" },
        { "name": "Japan", "code": "JP" },
        { "name": "Jersey", "code": "JE" },
        { "name": "Jordan", "code": "JO" },
        { "name": "Kazakhstan", "code": "KZ" },
        { "name": "Kenya", "code": "KE" },
        { "name": "Kiribati", "code": "KI" },
        {
            "name": "Korea, Democratic People"S Republic of", "code": "KP"
        },
        { "name": "Korea, Republic of", "code": "KR" },
        { "name": "Kuwait", "code": "KW" },
        { "name": "Kyrgyzstan", "code": "KG" },
        {
            "name": "Lao People"S Democratic Republic", "code": "LA"
        },
        { "name": "Latvia", "code": "LV" },
        { "name": "Lebanon", "code": "LB" },
        { "name": "Lesotho", "code": "LS" },
        { "name": "Liberia", "code": "LR" },
        { "name": "Libyan Arab Jamahiriya", "code": "LY" },
        { "name": "Liechtenstein", "code": "LI" },
        { "name": "Lithuania", "code": "LT" },
        { "name": "Luxembourg", "code": "LU" },
        { "name": "Macao", "code": "MO" },
        { "name": "Macedonia, The Former Yugoslav Republic of", "code": "MK" },
        { "name": "Madagascar", "code": "MG" },
        { "name": "Malawi", "code": "MW" },
        { "name": "Malaysia", "code": "MY" },
        { "name": "Maldives", "code": "MV" },
        { "name": "Mali", "code": "ML" },
        { "name": "Malta", "code": "MT" },
        { "name": "Marshall Islands", "code": "MH" },
        { "name": "Martinique", "code": "MQ" },
        { "name": "Mauritania", "code": "MR" },
        { "name": "Mauritius", "code": "MU" },
        { "name": "Mayotte", "code": "YT" },
        { "name": "Mexico", "code": "MX" },
        { "name": "Micronesia, Federated States of", "code": "FM" },
        { "name": "Moldova, Republic of", "code": "MD" },
        { "name": "Monaco", "code": "MC" },
        { "name": "Mongolia", "code": "MN" },
        { "name": "Montenegro", "code": "ME" },
        { "name": "Montserrat", "code": "MS" },
        { "name": "Morocco", "code": "MA" },
        { "name": "Mozambique", "code": "MZ" },
        { "name": "Myanmar", "code": "MM" },
        { "name": "Namibia", "code": "NA" },
        { "name": "Nauru", "code": "NR" },
        { "name": "Nepal", "code": "NP" },
        { "name": "Netherlands", "code": "NL" },
        { "name": "Netherlands Antilles", "code": "AN" },
        { "name": "New Caledonia", "code": "NC" },
        { "name": "New Zealand", "code": "NZ" },
        { "name": "Nicaragua", "code": "NI" },
        { "name": "Niger", "code": "NE" },
        { "name": "Nigeria", "code": "NG" },
        { "name": "Niue", "code": "NU" },
        { "name": "Norfolk Island", "code": "NF" },
        { "name": "Northern Mariana Islands", "code": "MP" },
        { "name": "Norway", "code": "NO" },
        { "name": "Oman", "code": "OM" },
        { "name": "Pakistan", "code": "PK" },
        { "name": "Palau", "code": "PW" },
        { "name": "Palestinian Territory, Occupied", "code": "PS" },
        { "name": "Panama", "code": "PA" },
        { "name": "Papua New Guinea", "code": "PG" },
        { "name": "Paraguay", "code": "PY" },
        { "name": "Peru", "code": "PE" },
        { "name": "Philippines", "code": "PH" },
        { "name": "Pitcairn", "code": "PN" },
        { "name": "Poland", "code": "PL" },
        { "name": "Portugal", "code": "PT" },
        { "name": "Puerto Rico", "code": "PR" },
        { "name": "Qatar", "code": "QA" },
        { "name": "Reunion", "code": "RE" },
        { "name": "Romania", "code": "RO" },
        { "name": "Russian Federation", "code": "RU" },
        { "name": "RWANDA", "code": "RW" },
        { "name": "Saint Helena", "code": "SH" },
        { "name": "Saint Kitts and Nevis", "code": "KN" },
        { "name": "Saint Lucia", "code": "LC" },
        { "name": "Saint Pierre and Miquelon", "code": "PM" },
        { "name": "Saint Vincent and the Grenadines", "code": "VC" },
        { "name": "Samoa", "code": "WS" },
        { "name": "San Marino", "code": "SM" },
        { "name": "Sao Tome and Principe", "code": "ST" },
        { "name": "Saudi Arabia", "code": "SA" },
        { "name": "Senegal", "code": "SN" },
        { "name": "Serbia", "code": "RS" },
        { "name": "Seychelles", "code": "SC" },
        { "name": "Sierra Leone", "code": "SL" },
        { "name": "Singapore", "code": "SG" },
        { "name": "Slovakia", "code": "SK" },
        { "name": "Slovenia", "code": "SI" },
        { "name": "Solomon Islands", "code": "SB" },
        { "name": "Somalia", "code": "SO" },
        { "name": "South Africa", "code": "ZA" },
        { "name": "South Georgia and the South Sandwich Islands", "code": "GS" },
        { "name": "Spain", "code": "ES" },
        { "name": "Sri Lanka", "code": "LK" },
        { "name": "Sudan", "code": "SD" },
        { "name": "Suriname", "code": "SR" },
        { "name": "Svalbard and Jan Mayen", "code": "SJ" },
        { "name": "Swaziland", "code": "SZ" },
        { "name": "Sweden", "code": "SE" },
        { "name": "Switzerland", "code": "CH" },
        { "name": "Syrian Arab Republic", "code": "SY" },
        { "name": "Taiwan, Province of China", "code": "TW" },
        { "name": "Tajikistan", "code": "TJ" },
        { "name": "Tanzania, United Republic of", "code": "TZ" },
        { "name": "Thailand", "code": "TH" },
        { "name": "Timor-Leste", "code": "TL" },
        { "name": "Togo", "code": "TG" },
        { "name": "Tokelau", "code": "TK" },
        { "name": "Tonga", "code": "TO" },
        { "name": "Trinidad and Tobago", "code": "TT" },
        { "name": "Tunisia", "code": "TN" },
        { "name": "Turkey", "code": "TR" },
        { "name": "Turkmenistan", "code": "TM" },
        { "name": "Turks and Caicos Islands", "code": "TC" },
        { "name": "Tuvalu", "code": "TV" },
        { "name": "Uganda", "code": "UG" },
        { "name": "Ukraine", "code": "UA" },
        { "name": "United Arab Emirates", "code": "AE" },
        { "name": "United Kingdom", "code": "GB" },
        { "name": "United States", "code": "US" },
        { "name": "United States Minor Outlying Islands", "code": "UM" },
        { "name": "Uruguay", "code": "UY" },
        { "name": "Uzbekistan", "code": "UZ" },
        { "name": "Vanuatu", "code": "VU" },
        { "name": "Venezuela", "code": "VE" },
        { "name": "Viet Nam", "code": "VN" },
        { "name": "Virgin Islands, British", "code": "VG" },
        { "name": "Virgin Islands, U.S.", "code": "VI" },
        { "name": "Wallis and Futuna", "code": "WF" },
        { "name": "Western Sahara", "code": "EH" },
        { "name": "Yemen", "code": "YE" },
        { "name": "Zambia", "code": "ZM" },
        { "name": "Zimbabwe", "code": "ZW" }
    ]
    

So that we have a list of countries for the Countries drop down in our contact form.

In `environment.ts`, we add:

    export const environment = {
      production: false,
      apiUrl: 'http://localhost:3000'
    };
    

This allows us to talk to our back end.

Then in `index.html`, we have:

    <!DOCTYPE html>
    <html lang="en">
      <head>
        <meta charset="utf-8" />
        <title>Angular Address Book App</title>
        <base href="/" />
    
        <meta name="viewport" content="width=device-width, initial-scale=1" />
        <link rel="icon" type="image/x-icon" href="favicon.ico" />
        <link
          href="[https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0/css/bootstrap.min.css](https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0/css/bootstrap.min.css)"
          rel="stylesheet"
        />
      </head>
      <body>
        <app-root></app-root>
      </body>
    </html>
    

This changes the default title to our own and added Bootstrap CSS.

To run our app, we use the JSON server package located at [https://github.com/typicode/json-server](https://github.com/typicode/json-server) to create a simple backend without writing any code. Run `npm i -g json-server` to install it.

We create a file called `db.json` in the root folder and add:

    {
      "contacts": []
    }
    

This automatically generates the routes that we previously specified in `contacts.service.ts`.

Then we run `json-server --watch db.json` to start our back end, and run `ng serve` on to start our app.

The post [How to Use MobX for Easy State Management in Your Angular App](https://thewebdev.info/2021/06/28/how-to-use-mobx-for-easy-state-management-in-your-angular-app/) appeared first on [The Web Dev](https://thewebdev.info).