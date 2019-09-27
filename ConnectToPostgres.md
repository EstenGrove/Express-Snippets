# Working with Postgres

## Quick Access
- [Setup Postgres Database](#setup-postgres-database)
- [Setup Express packages](#setup-express-packages)
- [Add DB Queries in Express](#add-db-queries)
  - ["GET"](#get-request)
  - ["POST"](#post-request)
  - ["PUT"](#put-request)
  - ["DELETE"](#delete-request)

#### Helpful Packages for Working with Postgres
- "pg": ```npm i pg```
- "knex": ```npm i knex```

------------

# Steps: Setup Postgres & Express Connection
**Steps**

__1.)__ Install packages: ```npm i pg knex```

__2.)__ Setup Postgres database

__3.)__ Create db query functions in express

__4.)__ Use "knex" to set up db connection values (ie. client, host, username, password, database etc.)

__5.)__ Whitelist client IP (ie where the requests are coming from) this is typically with ```cors``` package.

__6.)__ Setup Express API routes

__7.)__ Test in Postman

## Setup Postgres Database
```sql
---Create DB:
CREATE DATABASE <db_name>;

---Create table:
CREATE TABLE <table_name> (
  id SERIAL PRIMARY KEY,
  username VARCHAR(100),
  password VARCHAR(100),
  email VARCHAR(100)
  added TIMESTAMP NOT NULL
);

---Insert any applicable data:
INSERT INTO <table_name> (<col_1>, <col_2>...) VALUES (
  1,
  'Jakey_da_Snakey',
  'j@k3Sn@k3',
  'jsnake1234@aol.com',
  current_timestamp
);

---Double check the data was inserted:
SELECT * FROM <table_name>;
```

## Setup Express Packages
```javascript
// index.js (main express file)
const express = require('express');

// require the env variables
require('dotenv').config();

const helmet = require('helmet');
const bodyParser = require("body-parser");
const cors = require("cors");
const morgan = require("morgan");
```
- Create a folder for the DB controllers (ie. DB queries): ```mkdir controllers```

#### Add DB Queries

###### "GET" request
```javascript
const getData = (req, res, db) => {
  db.select("*")
    .from(<table_name>)
    .then(items => {
      if(items.length){
        res.json(items);
      } else {
        res.json({ dataExists: 'false' });
      }
    })
    .catch(err => res.status(400).json({ dbError: "db error occured" }));
}
```
###### **"POST" request**
```javascript
const postData = (req, res, db) => {
  const { username, password, email } = req.body;  // values to save to db
  const added = new Date();
  
  db(<table_name>)
    .insert({ username, password, email, added })  // values to insert
    .returning("*")
    .then(item => {
      res.json(item);
    })
    .catch(err => res.status(400).json({ dbError: "db error occured" }))
}
```
###### **"PUT" request**
```javascript
const putTableData = (req, res, db) => {
  const { id, first, last, email, phone, location, hobby } = req.body;  // values to update
  db(<table_name>)
    .where({ id })
    .update({ first, last, email, phone, location, hobby })  // values to update
    .returning("*")
    .then(item => {
      res.json(item);
    })
    .catch(err => res.status(400).json({ dbError: "db error" }));
};
```
###### **"DELETE" request**
```javascript
const deleteTableData = (req, res, db) => {
  const { id } = req.body;  // value used to find item to delete
  db(<table_name>)
    .where({ id })
    .del()
    .then(() => {
      res.json({ delete: "true" });
    })
    .catch(err => res.status(400).json({ dbError: "db error occured" }));
};
```
- Then, finally export all the query functions: 
```javascript
module.exports = {
  getData,
  postData,
  putData,
  deleteData
}
```









