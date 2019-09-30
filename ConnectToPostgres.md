# Working with Postgres

## Quick Access
- [Setup Postgres Database](#setup-postgres-database)
- [Setup Express packages](#setup-express-packages)
- [Add DB Queries in Express](#add-db-queries)
  - ["GET"](#get-request-query)
  - ["POST"](#post-request)
  - ["PUT"](#put-request)
  - ["DELETE"](#delete-request)

#### Helpful Packages for Working with Postgres
- "pg": ```npm i pg```
- "knex": ```npm i knex```

------------


# Main Method of Connecting to Postgres
The simpler way is simply using ```pg``` and ```cors``` with a simple connection Pool to keep the connection active.

__1.)__ Install packages: ```npm i pg cors```

__2.)__ Setup Postgres database

__3.)__ Create the db connection:

__3a.)__ Create a ```controllers/``` directory and inside create a ```main.js``` file for the queries & connection:


__5.)__ Export the query functions: 
```javascript
module.exports = {
  getAllItems,
  getItemById,
  createItem,
  updateItem,
  deleteItem,
}
```

__6.)__ Inside "index.js", import the queries and setup the routes to call each query

```javascript
// import queries
const main = require("./controllers/main");
const { getAllItems, getItemById, createItem, updateItem, deleteItem } = main;

// for getAllItems()
app.get('/api/items', (req, res, db) => getAllItems(req, res, db)));

// for getItemById()
app.get('/api/items/:id', (req, res, db) => getItemById(req, res, db)));

// for createItem()
app.post('/api/items', (req, res, db) => createItem(req, res, db));

// for updateItem()
app.put('/api/items/:id', (req, res, db) => updateItem(req, res, db));

// for deleteItem()
app.delete('/api/items/:id', (req, res, db) => deleteItem(req, res, db));
```

#### Create Directory for Queries

```bash
mkdir controllers && cd controllers
touch main.js
```

#### Create "Pool" connection

```javascript
// main.js
// create the connection Pool and enter creds
const Pool = require("pg").Pool;
const pool = new Pool({
  user: "",
  host: "localhost",
  database: "my_db",
  password: ""
});

// NOTE: you can also add a "port" property to the "Pool"
```

#### "GET" Request: SELECT * Query

```javascript
// query all rows in the "components" table
const getAllItems = (req, res, db) => {
  pool.query("SELECT * FROM components ORDER BY id ASC", (err, results) => {
    if(err){
      throw err;
    }
    res.status(200).json(results.rows);
  })
}

```

#### "GET" by ID Request: SELECT * Query w/ CONDITION
```javascript
// get item by id
const getItemById = (req, res, db) => {
  const { id } = req.params;
  pool.query("SELECT * FROM components WHERE id = $1", [id], (err, results) => {
    if(err){
      throw err;
    }
    res.status(200).json(results.rows);
  })
}
```

#### "POST" Request: INSERT Query
```javascript
// add a new item to the db
const createItem = (req, res, db) => {
  const { item } = req.body;
  
  pool.query('INSERT INTO components (item_name, item_desc, item_type) VALUES ($1, $2, $3)', [item.name, item.desc, item.type], (err, results) => {
    if(err){
      throw err;
    }
    response.status(201).send(`Item added with ID: ${result.insertId}`)
  })
}
```
#### "PUT" Request: UPDATE Query
```javascript
// update existing item
const updateItem = (req, res, db) => {
  const id = parseInt(req.params.id);
  const { name, desc, type, id } = req.body.item;
  
  pool.query(
    'UPDATE components SET item_name = $1, item_desc = $2, item_type = $3 WHERE id = $4',
    [name, desc, type, id],
    (error, results) => {
      if (error) {
        throw error
      }
      response.status(200).send(`Item modified with ID: ${id}`)
    }
  )
}
```
#### "DELETE" Request: DELETE Query
```javascript
// delete an item
const deleteItem = (request, response) => {
  const id = parseInt(request.params.id)

  pool.query('DELETE FROM components WHERE id = $1', [id], (error, results) => {
    if (error) {
      throw error
    }
    response.status(200).send(`Item deleted with ID: ${id}`)
  })
}
```



------------

## Alternate Setup Method: Postgres & Express Connection
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


--------------








