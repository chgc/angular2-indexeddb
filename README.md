angular2-to-indexeddb
==============

angular2-indexeddb is an experimental service that wraps indexeddb database in an Angular.js 2 service.
It exposes very simple API to enable the usage of indexeddb without most of it plumbing.

Copyright (C) 2016, Gil Fink <gil@sparxys.com>

Installation
------------

You can choose your preferred method of installation:
* Download from github: [angular2-indexeddb.min.js](https://github.com/gilf/angular2-to-indexeddb/blob/master/angular2-indexeddb.min.js)

Usage
-----
Include **angular2-indexeddb.js** in your application.

```html
<script src="components/angular-to-indexeddb/angular2-indexeddb.js"></script>
```

Import the the `AngularIndexedDB` class as a dependency:

```js
import {AngularIndexedDB} from './angular2-indexeddb';
```

### AngularIndexedDB service
First instantiate the service as follows:
```js
let db = new AngularIndexedDB('myDb', 1);
```
The first argument is the name of your database and the second is the database version.
If you forget the version you the service will default to version 1.

Use the APIs that the AngularIndexedDB service exposes to use indexeddb.
In the API the following functions:
* createStore(version, createCallback): initializes objectStore/s.
The first parameter is the version to upgrade the database and the second one is a callback the will handle the creation of objectStores for that version.
**createStore** returns a promise that is resolved when the store updated or rejected if an error occurred.

Usage example:

```js
db.createStore(1, (evt) => {
    let objectStore = evt.currentTarget.result.createObjectStore(
        'people', { keyPath: "id", autoIncrement: true });

    objectStore.createIndex("name", "name", { unique: false });
    objectStore.createIndex("email", "email", { unique: true });
});
```
* getByKey(storeName, key): returns the object that is stored in the objectStore by its key.
The first parameter is the store name to query and the second one is the object's key.
**getByKey** returns a promise that is resolved when we have the object or rejected if an error occurred.

Usage example:

```js
db.getByKey('people', 1).then((person) => {
    console.log(person);
}, (error) => {
    console.log(error);
});
```

* getAll(storeName): returns an array of all the items in the given objectStore.
The first parameter is the store name to query.
**getAll** returns a promise that is resolved when we have the array of items or rejected if an error occurred.

Usage example:

```js
db.getAll('people').then((people) => {
    console.log(people);
}, (error) => {
    console.log(people);
});
```

* add(storeName, key, value): Adds to the given objectStore the key and value pair.
The first parameter is the store name to modify, the second parameter is the key and the third parameter is the value.
**add** returns a promise that is resolved when the value was added or rejected if an error occurred.

Usage example:

```js
db.add('people', undefined, { name: 'name', email: 'email' }).then(() => {
    // Do something after the value was added
}, (error) => {
    console.log(error);
});
```
In the previous example I'm using undefined as the key because the key is configured in the objectStore as auto-generated.

* update(storeName, value): Updates the given value in the objectStore.
The first parameter is the store name to modify and the second parameter is the value to update.
**update** returns a promise that is resolved when the value was updated or rejected if an error occurred.

Usage example:

```js
db.update('people', { id: 3, name: 'name', email: 'email' }).then(() => {
    // Do something after update
}, (error) => {
    console.log(error);
});
```

* remove(storeName, key): Removes the object that correspond with the key from the objectStore.
The first parameter is the store name to modify and the second parameter is the key to remove.
**remove** returns a promise that is resolved when the value was removed or rejected if an error occurred.

Usage example:

```js
db.reomove('people', 3).then(() => {
    // Do something after remove
}, (error) => {
    console.log(error);
});
```

License
----

Released under the terms of the [MIT License](LICENSE).