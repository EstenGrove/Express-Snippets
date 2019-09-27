# Express-Snippets
A set of Node and Express snippets, helpers, tools and middleware.


## Quick Access
- [Create Express App](#create-an-express-app)
- [Middleware](#tools-and-middleware)
  - A list of common middle ware can be found [here](https://expressjs.com/en/resources/middleware.html)
  - [body-parser](#body-parser)
  - [cookie-parser](#cookie-parser)
  - [pg: Postgres connection](#pg)
  - [morgan: http logger](#morgan)
  - [ping: a ping TCP wrapper](#ping)
------------------------


## Create an Express App
[Here's a great resource for setup up w/ a database](https://blog.logrocket.com/setting-up-a-restful-api-with-node-js-and-postgresql-d96d6fc892d8/)

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
*Nearly* all middleware is implemented by telling Express to "*use*" it: ```app.use(middleWare)```

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

#### ```cookie-parser```
"cookie-parser" is a package that handles parsing cookie headers and will populate the ```req.cookies``` property of the request object.

**Example**
```javascript
// imports the middleware
const cookieParser = require('cookie-parser');

// tells express to use it
app.use(cookieParser())
```
- Working w/ signed and unsigned cookies
```javascript
// to access cookies 

app.get('/', function (req, res) {
  console.log('Cookies: ', req.cookies);  // Cookies that have not been signed

  console.log('Signed Cookies: ', req.signedCookies)   // Cookies that have been signed
})
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

#### ```morgan```
"morgan" is an HTTP request logging package.

**Example**
```javascript
//import package
const morgan = require('morgan');

app.use(morgan('combined'));

// syntax
morgan(format, options)
```


#### ```PING``` 
The ```PING``` middleware is used for opening/closing TCP socket connections to specified hosts in Node.

**Example**

```javascript
const ping = require('ping');

// DO NOT include "https://"
const hosts ['192.168.8.1', 'echo-alchemist.com', 'sgore.dev'];

hosts.forEach((host, index) => {
  ping.sys.probe(host, (isAlive) => {
    const msg = isAlive ? 'Host: ' + host + ' is alive' : 'Host: ' + host + ' is down';
    console.log(msg);
  });
});

// Promise Version
hosts.forEach((host, index) => {
  ping.sys.probe(host)
    .then(res => console.log(res))
    .catch(err => console.log(err))
})
```








