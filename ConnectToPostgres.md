# Working with Postgres

## Quick Access
- [Setup Express packages](#setup-express-packages)
- [DB Queries in Express](#queries)
  - ["GET"](#get-request-select--query)
    - ["GET" by ID](#get-by-id-request-select--query-w-condition)
  - ["POST"](#post-request-insert-query)
  - ["PUT"](#put-request-update-query)
  - ["DELETE"](#delete-request-delete-query)

#### Helpful Packages for Working with Postgres
- "pg": ```npm i pg```
- "knex": ```npm i knex```

------------

## Setup Express Packages

```bash
npm i pg cors body-parser
```
#### Init Packages in Express
```javascript
const express = require('express');

// require the env variables, only useful if using env vars
require('dotenv').config();


const bodyParser = require("body-parser");
const cors = require("cors");
const morgan = require("morgan"); // not required, used for logging
```

------------

# Main Method of Connecting to Postgres
The simpler way is simply using ```pg``` and ```cors``` with a simple connection Pool to keep the connection active.

__1.)__ Install packages: ```npm i pg cors```

__2.)__ Setup Postgres database

__3.)__ [Create the db connection](#create-pool-connection)

__3a.)__ [Create A "controllers" Directory for Queries](#create-directory-for-queries)

__4.)__ [Create DB queries](#queries)

__5.)__ Then export the query functions: 
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

----------

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

--------

# Queries

## "GET" Request: SELECT * Query

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

## "GET" by ID Request: SELECT * Query w/ CONDITION
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

## "POST" Request: INSERT Query
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
## "PUT" Request: UPDATE Query
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
## "DELETE" Request: DELETE Query
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















