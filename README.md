# Express-Snippets
A set of Node and Express snippets, helpers, tools and middleware.


## Create an Express App
```javascript
const express = require('express');
const app = express();

const PORT = process.env.port || 3001

app.get('/', () => {
  res.send('App running...');
});

app.list(PORT, () => console.log(`App is listening on port ${PORT}...`))

```
