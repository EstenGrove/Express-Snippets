# A Collection of Middleware Snippets



## Whitelisting w/ ```cors```
First install the middleware: ```npm i cors```

```javascript
// create an array of clients to whitelist
const whitelist = ["http://localhost:3001"];

const corsOptions = {
  origin: (origin, callback) => {
    if (whitelist.indexOf(origin) !== -1 || !origin) {
      callback(null, true);
    } else {
      callback(new Error("Not allowed by CORS"));
    }
  }
};
```
