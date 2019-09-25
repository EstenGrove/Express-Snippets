# Express-Snippets
A set of Node and Express snippets, helpers, tools and middleware.


## Create an Express App
__1.)__ Create a ```package.json``` file by running: ```npm init```

__2.)__ Install Express: ```npm i express```

__3.)__ Create an index.js file for the app: ```touch index.js```

__4.)__ Paste the following code into the index.js:
```javascript
const express = require('express');
const app = express();

const PORT = process.env.port || 3001

app.get('/', (req, res) => {
  res.send('App running...');
});

app.listen(PORT, () => console.log(`App is listening on port ${PORT}...`))
```
__5.)__ Install "nodemon", which is like hot reloading: ```npm i -D nodemon```

__6.)__ Change the "scripts" section to include: 
  ```javascript
  "scripts": {
    "start": "nodemon index.js"
  }
  ```
__7.)__ Then start the server: ```npm start```



-------------


## Tools and Middleware

#### ```body-parser```
"body-parser" is a package for handling HTTP requests and parsing the JSON body.
- Install: ```npm i body-parser```

**Example**
```javascript
// sets up json parsing
// accepts encoded urls
  
const bodyParser = require('body-parser');

app.use(bodyParser.json());
app.use(
  bodyParser.urlencoded({
    extended: true
}))
```

#### ```pg```
"pg" is a package for working with Postgres in Express that includes: connection pooling, async notifications, bulk import/export etc.
- Install: ```npm i pg```

**Example**
```javascript
// setup connection pool
// setup connection details

const Pool = require('pg').Pool;
const pool = new Pool({
  user: 'me',
  host: 'localhost',
  database: 'database name here',
  password: 'database password here',
  port: 5432
})
```









