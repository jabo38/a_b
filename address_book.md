## Development of the module for NEM wallet «Nano Wallet»

### Foreword

NEM - socially oriented cryptocurrency second generation, with the philosophy of financial freedom, decentralization and equal opportunities.

NEM platform provides users with the following types of wallets for safe storage of tokens - XEM:
-  **NCC Standalone (Nem Comunity Client)** — standalone client. It is recommended to use if you want to raise "cupernodu" or a complete assembly. To install and run at least the minimum necessary skills to use the terminal;
-  **Nano Wallet** — light client. No need to install additional software, blockchain download. All inquiries service remote nodes. Designed using frameworks JavaScript - AngularJS;
-  **Android NEM Wallet** - a mobile wallet for Android system.

«NCC Standalone» in its functionality provides much needed for a wallets module - «Address Book», which is not in the «Nano Wallet».

This article will show you how to create the module - «Address Book», contain the following functionality:
-  New contact;
-  Edit contact;
-  Remove contact;
-  Sort list contacts;
-  Export list contacts;
-  Import list contacts.

### Exordium

The first thing is faced novice developer, has never engaged in writing modules for «Nano Wallet», it is a question - "Where do I start?".

First, check installed on your system Node.js platform, if not, download and install.

Node.js you need to build the project.

Next, download the source code of wallet «Nano Wallet» from [the official repository](https://github.com/NemProject/NanoWallet)

To build the project, open a terminal, go to the root project directory and run the following commands:

```> npm install -g gulp-cli```

```> npm install```

```> gulp```

Now everything is ready, and you can proceed directly to the development of the module itself.

### Module development

Wallet modules are stored in the following directory:
```
NanoWallet/
  src/
    app/
      modules/
```

We call our module without superfluous tricks - AddressBook. All modules wallet files have the following structure:
```
modules/
  addressBook/
    addressBook.config.js
    addressBook.controller.js
    index.js
    addressBook.html
```

**addressBook.config.js** - prescribes module configuration. Here you must specify:
-  **url** - url-address where the module will be available;
-  **controller** - the controller name, the service module;
-  **controllerAs** - alias the controller;
-  **templateUrl** - path to the file, which contains the presentation (html-code) modules;
-  **title** - the name of the module.

```javascript
function AddressBookConfig($stateProvider) {
    'ngInject';

    $stateProvider
        .state('app.addressBook', {
            url: '/address-book',
            controller: 'AddressBookCtrl', 
            controllerAs: '$ctrl',
            templateUrl: 'modules/addressBook/addressBook.html',
            title: 'Address book'
        });

};

export default AddressBookConfig;
```

**addressBook.controller.js** - contains the logic module. Controller module «Address Book» will contain the following properties and methods:
-  **contacts** - the contacts list stored in the browser's local storage;
-  **addContact (), add ()** - add to your address book new contact;
-  **editContact (), edit ()** - editing a contact;
-  **removeContact (), remove ()** - delete a contact;
-  **sortBy ()** - sort contact list;
-  **exportAddressBook ()** - export your contact list;
-  **importAddressBook()** — import your contact list.

```javascript
import helpers from '../../utils/helpers';
import CryptoHelpers from '../../utils/CryptoHelpers';

class AddressBookCtrl {
    constructor($localStorage, DataBridge, Wallet, Alert, $location, $filter, $q, $rootScope) {
        'ngInject';

        this.contacts = this._storage.contacts;
    }

    sortBy(propertyName) { // code };

    addContact() { // code }

    add() { // code }

    editContact(elem) { // code }

    edit() { // code }

    removeContact(elem) { // code }

    remove() { // code }

    saveAddressBook() { // code }

    exportAddressBook() { // code }

    uploadAddressBook() { // code }

    importAddressBook($fileContent) { // code }

    transferTransaction(address) { // code }
}

export default AddressBookCtrl;
```

**index.js** - in this file we create an instance of our module with predefined configuration and controller.

```javascript
import angular from 'angular';

// Create the module where our functionality can attach to
let addressBookModule = angular.module('app.addressBook', []);

// Include our UI-Router config settings
import AddressBookConfig from './addressBook.config';
addressBookModule.config(AddressBookConfig);

// Controllers
import AddressBookCtrl from './addressBook.controller';
addressBookModule.controller('AddressBookCtrl', AddressBookCtrl);

export default addressBookModule;
```

**addressBook.html** - в этом файле сделать HTML-разметку.

```http
<div class="col-sm-3 sidebar" style="margin-top: 10px;">
    <div class="panel panel-default">
        <div class="panel-heading" style="background-color: rgb(68, 68, 68); color: white;border-radius: 0px;">
          <i class="fa fa-chevron-right"></i>
          <span>{{ 'ADDRESS_BOOK_NAVIGATION' | translate }}</span>
        </div>
        <div class="panel-body">
            <ul class="nav nav-pills nav-stacked">
                <li>
                    <button type="button" class="btn btn-operations" ng-click="$ctrl.addContact()">{{ 'ADDRESS_BOOK_NEW' | translate }}</button>
                </li>
                <li>
                    <button type="button" ng-disabled="!$ctrl.contacts.items.length" class="btn btn-operations" ng-click="$ctrl.exportAddressBook()">{{ 'ADDRESS_BOOK_EXPORT_BTN' | translate }}</button>
                    <a id="exportAddressBook" class="hidden" target="_blank"></a>
                </li>
                <li>
                    <button type="button" class="btn btn-operations" ng-click="$ctrl.uploadAddressBook()">{{ 'ADDRESS_BOOK_IMPORT_BTN' | translate }}</button>
                    <input id="uploadAddressBook" accept=".adb" style="visibility:hidden;position:absolute;" import-address-book-file="$ctrl.importAddressBook($fileContent)" type="file">
                </li>
            </ul>
        </div>
    </div>
</div>
```

Next, add a markup to display the contact list and contact management buttons:

```http
<div class="col-sm-9 noPaddingLeft" style="margin-top: 10px;">
    <div class="panel panel-default">
        <div class="panel-heading" style="background-color: rgb(68, 68, 68); color: white;border-radius: 0px;">
          <i class="fa fa-group"></i>  {{ 'ADDRESS_BOOK_LIST' | translate }}
          <div style="float: right; position: relative; display: block;" ng-show="$ctrl.contacts.items.length > $ctrl.pageSize"><button class="buttonStyle" ng-disabled="$ctrl.currentPage == 0" ng-click="$ctrl.currentPage = $ctrl.currentPage-1" style="background-color: transparent; border: medium none;"><span class="fa fa-chevron-left" aria-hidden="true"></span></button><b>{{$ctrl.currentPage+1}}/{{$ctrl.numberOfPages()}}</b><button class="buttonStyle" ng-disabled="$ctrl.currentPage+1 >= $ctrl.numberOfPages()" ng-click="$ctrl.currentPage = $ctrl.currentPage+1" style="background-color: transparent; border: medium none;"> <span class="fa fa-chevron-right" aria-hidden="true"></span></button></div>
        </div>
        <table  class="table table-bordered table-hover table-striped table-condensed" style="border:1px solid #444;table-layout:fixed;word-break:break-all;">
            <thead style="color:white;">
                <tr>
                    <th style="width: 20%;">
                        <a href="javascript:;" ng-click="$ctrl.sortBy('label')">
                            <i class="fa" ng-show="$ctrl.propertyName === 'label'" ng-class="{'fa-caret-down': $ctrl.revers, 'fa-caret-up': !$ctrl.revers}"></i>  {{ 'ADDRESS_BOOK_CONTACT_LABEL' | translate }}
                        </a>
                    </th>
                    <th>
                        <a href="javascript:;" ng-click="$ctrl.sortBy('address')">
                            <i class="fa" ng-show="$ctrl.propertyName === 'address'" ng-class="{'fa-caret-down': $ctrl.revers, 'fa-caret-up': !$ctrl.revers}"></i>  {{ 'ADDRESS_BOOK_ACCOUNT_ADDRESS' | translate }}
                        </a>
                    </th>
                    <th style="width: 200px;">{{ 'ADDRESS_BOOK_ACTIONS' | translate }}</th>
                </tr>
            </thead>
            <tbody style="background-color:white;text-align:center">
                <tr ng-repeat="contact in $ctrl.contacts.items | orderBy:$ctrl.propertyName:$ctrl.revers | startFrom:$ctrl.currentPage*$ctrl.pageSize | limitTo:$ctrl.pageSize">
                    <td style="vertical-align: middle;border-bottom: 1px solid #444">{{contact.label}}</td>
                    <td style="vertical-align: middle;border-bottom: 1px solid #444">{{contact.address}}</td>
                    <td class="actions" style="vertical-align: middle;border-bottom: 1px solid #444;white-space:nowrap;">
                        <button type="button" class="btn btn-xs" ng-click="$ctrl.transferTransaction(contact.address)">Send XEM</button>
                        <button type="button" class="btn btn-xs" ng-click="$ctrl.editContact(contact)">Edit</button>
                        <button type="button" class="btn btn-xs" ng-click="$ctrl.removeContact(contact)">Remove</button>
                    </td>
                </tr>
            </tbody>
        </table>
        <div class="panel-body" ng-show="!$ctrl.contacts.items.length" style="border: 1px solid #444;">
            <p style="margin:0;">{{ 'GENERAL_NO_RESULTS' | translate }}</p>
        </div>
        <div class="panel-footer text-center" style="background-color: #e3e0cf; color: #444;padding:0;">
          <small><b><i>{{ 'ADDRESS_BOOK_MAX_NUMBER' | translate }} {{ $ctrl.pageSize }}</i></b></small>
        </div>
    </div>
</div>
```

and add a modal windows, allow you to add, edit and delete contacts:

```http
<!-- Add new and edit contact modal -->
<div id="contactModal" class="modal fade" role="dialog">
    <div class="modal-dialog">
        <!-- Modal content-->
        <div class="modal-content">
            <div class="modal-header">
                <button type="button" class="close" data-dismiss="modal">×</button>
                <h4 class="modal-title" ng-show="!$ctrl.is_edit">{{ 'ADDRESS_BOOK_NEW' | translate }}</h4>
                <h4 class="modal-title" ng-show="$ctrl.is_edit">{{ 'ADDRESS_BOOK_EDIT' | translate }}</h4>
            </div>
            <div class="modal-body">
                <fieldset class="form-group">
                    <input class="form-control form-control-lg"
                           type="password"
                           placeholder="{{ 'FORM_PASSWORD_FIELD_PLACEHOLDER' | translate }}"
                           ng-model="$ctrl.common.password"/>
                </fieldset>
                <fieldset class="form-group">
                    <input class="form-control form-control-lg"
                           type="text"
                           placeholder="{{ 'ADDRESS_BOOK_CONTACT_LABEL' | translate }}"
                           ng-model="$ctrl.formData.label"/>
                </fieldset>

                <fieldset class="form-group">
                    <input class="form-control form-control-lg"
                           type="text"
                           placeholder="{{ 'FORM_ADDRESS_FIELD_PLACEHOLDER' | translate }}"
                           ng-model="$ctrl.formData.address"/>
                </fieldset>
                <button class="btn btn-success" style="border-radius:0px;"
                        type="submit" ng-show="!$ctrl.is_edit" ng-disabled="$ctrl.okPressed || !$ctrl.common.password.length || !$ctrl.formData.label || !$ctrl.formData.address" ng-click="$ctrl.add()">
                    <i class="fa fa-plus"></i> {{ 'ADDRESS_BOOK_NEW_BTN' | translate }}
                </button>
                <button class="btn btn-success" style="border-radius:0px;"
                        type="submit" ng-show="$ctrl.is_edit" ng-disabled="$ctrl.okPressed || !$ctrl.common.password.length || !$ctrl.formData.label || !$ctrl.formData.address" ng-click="$ctrl.edit()">
                    <i class="fa fa-edit"></i> {{ 'ADDRESS_BOOK_EDIT_BTN' | translate }}
                </button>
            </div>
            <div class="modal-footer">
                <button type="button" class="btn btn-default"  data-dismiss="modal">{{ 'GENERAL_CLOSE' | translate }}</button>
            </div>
        </div>
    </div>
</div>

<!-- Remove contact modal -->
<div id="removeContactModal" class="modal fade" role="dialog">
    <div class="modal-dialog">
        <!-- Modal content-->
        <div class="modal-content">
            <div class="modal-header">
                <button type="button" class="close" data-dismiss="modal">×</button>
                <h4 class="modal-title">{{ 'ADDRESS_BOOK_REMOVE' | translate }}</h4>
            </div>
            <div class="modal-body">
                <fieldset class="form-group">
                    <input class="form-control form-control-lg"
                           type="password"
                           placeholder="{{ 'FORM_PASSWORD_FIELD_PLACEHOLDER' | translate }}"
                           ng-model="$ctrl.common.password"/>
                </fieldset>
                <button class="btn btn-danger" style="border-radius:0px;" type="submit" ng-disabled="$ctrl.okPressed || !$ctrl.common.password.length" ng-click="$ctrl.remove()">
                    <i class="fa fa-remove"></i> {{ 'ADDRESS_BOOK_REMOVE_BTN' | translate }}
                </button>
            </div>
            <div class="modal-footer">
                <button type="button" class="btn btn-default"  data-dismiss="modal">{{ 'GENERAL_CLOSE' | translate }}</button>
            </div>
        </div>
    </div>
</div>
```

It remains to write the styles for the module. The main css wallet code is contained in the file:
```
NanoWallet/
  src/
    css/
      nano.css
```

Styles for our modules:

```css
.address-book-page .panel-default .panel-heading {
	border-bottom: 3px solid #dfa82f;
}

.address-book-page .sidebar .nav li .btn-operations {
	width: 100%;
	border-top: 1px solid #444;
	border-bottom: 1px solid #444;
	border-radius: 0;
	margin-top: 7px;
	color: #444;
	background-color: #e3e0cf;
}

.address-book-page .sidebar .nav li:first-child .btn-operations { margin-top: 0; }
.address-book-page .sidebar .nav li .btn-operations:hover { background-color: #dad6bf; }
.address-book-page .sidebar .nav li .btn-operations:focus { outline: none; }

.address-book-page .sidebar .panel-body { padding: 10px 0; }

.address-book-page table th a { color: #FFF; }
.address-book-page table th a:hover,
.address-book-page table th a:focus,
.address-book-page table th a:active { color: #FFF; text-decoration: none; }
.address-book-page table .actions .btn { border: 1px solid rgba(153, 153, 153, 0.52); background: #E3E0CF; }

.address-book-page table .actions .btn:hover,
.address-book-page table .actions .btn:focus,
#addressBookModal table .btn:hover,
#addressBookModal table .btn:focus {
	background-color: #dad6bf !important;
	text-decoration: none;
}

#addressBookModal .modal-body { padding: 0; }
#addressBookModal .modal-footer { background: #444; }
#addressBookModal .modal-footer .btn {
	border-radius: 3px;
	background-color: #FFF;
	color: #333;
	border: none;
}

#addressBookModal .modal-footer .custom-pagination { color: #FFF; margin-top: 5px; }
#addressBookModal .modal-footer .custom-pagination button { opacity: 1; transition: .3s ease opacity; }
#addressBookModal .modal-footer .custom-pagination button[disabled="disabled"] { opacity: 0.3; }

#addressBookModal table th,
#addressBookModal table td { padding-left: 15px; padding-right: 15px; }
#addressBookModal table th:first-child,
#addressBookModal table td:first-child { border-right: 1px solid #DDDDDD; }
#addressBookModal table tr:first-child td { padding-top: 10px; }
#addressBookModal table tr:last-child td { padding-bottom: 15px; }
#addressBookModal table th { border-bottom: none; }
```

Now you need to import the module created in the application. Open the following file:
```
NanoWallet/
  src/
    app/
      app.js
```

Prescribes the following code to import our module:
```javascript
// Import our app modules
import './modules/addressBook';

// Create and bootstrap application
const requires = [
    'app.addressBook'
];
```

In this module development is completed.

### Сonclusion

So we got ready Address Book module.

As we have seen, the development of modules for the purse «Nano Wallet» is not such a difficult task. I hope this article helps someone.

Have fun designing your own modules! :)
