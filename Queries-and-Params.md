# Queries and Params
Working w/ queries and params of the request object in Express/Node.

# ```REQUEST``` Object
The ```request``` object that's accessible within an express HTTP method function (ie ```app.get()```, ```app.put()``` etc).

### Properties Available on the ```REQUEST``` Object
All properties are available on the request object: ```req.baseURL```, ```req.cookies``` etc.

- ```baseURL``` - the base url of the http request
- ```body``` - data submitted in the request body
- ```cookies``` - cookies sent with the request (use the ```cookie-parser``` middleware)
- ```hostname``` - the server host
- ```ip``` - the server IP
- ```params``` - the route params
- ```path``` - the URL path
- ```protocol``` - the request protocol
- ```query``` - the query strings sent with the URL
- ```secure``` - a boolean value that returns ```true``` if the request was sent from "https"
- ```signedCookies``` - contains the signed cookie (use the ```cookie-parser``` middleware).


### Query Strings
The ```query``` property on the ```request``` object is accessible directly from the property itself:
```javascript
// Example request: http://localhost:3000/?name=Esten&age=32

app.get('/', (req, res) => {
  const { query } = req;
  console.log(query);  // { name: 'Esten', age: '32' }
  res.send(`Query strings: ${JSON.stringify(query)}`);
})

// Iterating over the query strings
for (const key in req.query) {
  // key, value pair
  console.log(key), req.query[key]);
}

```
Since the request object and the query strings are in an object form you can access them by their properties:
```javascript
// Example request: http://localhost:3000/?name=Esten&age=32

app.get('/', (req, res) => {
  const { query } = req;
  const { name, age } = query;
  
  console.log(query);  // { name: 'Esten', age: '32' }
  console.log(name);   // 'Esten'
  console.log(age);    // '32'
})

```

---------------

## Content Types on the ```request``` Object
There a set of content types that a request can send data as.
- ```Content-Type: application/json``` requires the ```express.json()``` middleware
- ```Content-Type: application/www-form-urlencoded``` requires the ```express.urlencoded()``` middleware
  - This type is often used with forms
