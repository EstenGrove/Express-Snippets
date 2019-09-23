# Express-Snippets
A set of Node and Express snippets, helpers, tools and middleware.


## Create an Express App
1. Create a ```package.json``` file by running: ```npm init```
2. Install Express: ```npm i express```
3. Create an index.js file for the app: ```touch index.js```
4. Paste the following code into the index.js:
```javascript
const express = require('express');
const app = express();

const PORT = process.env.port || 3001

app.get('/', () => {
  res.send('App running...');
});

app.list(PORT, () => console.log(`App is listening on port ${PORT}...`))
```
5. Install "nodemon", which is like hot reloading: ```npm i -D nodemon```
6. 
