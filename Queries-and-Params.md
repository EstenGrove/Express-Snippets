# Queries and Params
Working w/ queries and params of the request object in Express/Node.


## Quick Access
- [Request object](#request-request-object)
  - [Content-Types on the request object](#request-content-types)
- [Query strings](#query-strings)
- [Response object](#response-headers--content-types)
  - [Content-Types on the response object](#response-headers)
  - [Response Methods](#response-methods)
    - [Handling cookies](#handling-cookies-in-a-response)


--------

# ```REQUEST``` Object
The ```request``` object that's accessible within an express HTTP method function (ie ```app.get()```, ```app.put()``` etc).

### Request (```REQUEST```) Object 
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

## ```REQUEST```: Content Types
There a set of content types that a request can send data as.
- ```Content-Type: application/json``` requires the ```express.json()``` middleware
- ```Content-Type: application/www-form-urlencoded``` requires the ```express.urlencoded()``` middleware
  - This type is often used with forms

---------

## Response Headers & Content Types
When a server responds to a client, the ```Content-Type``` is determined by what data type is passed to the response. The ```send()``` automatically sets the ```Content-Length``` header.
- String: ```text/html``` 
- Object|Array: ```application/json```

#### Response Methods
- To send an empty response: ```res.end()```
- Send the response status: ```res.status()```
  - Example: ```res.status(404).end()```
  - Example: ```res.status(404).send('File not found')
- ```sendStatus()``` is the shortcut for ```res.status()```
- Send "OK" status: ```res.status(200).send('OK')
- Handling cookies: ```res.cookies()```

#### Handling Cookies in a Response
When working with cookies in a HTTP response you should use: ```res.cookies()```

**Useful Cookie Options**
- ```domain```
- ```expires```
- ```httpOnly``` - means the cookie is ONLY accessible by the web server
- ```maxAge``` - represents the expiry time relative to the current time, in milliseconds
- ```path``` - the cookies' path
- ```secure``` - whether it's secure or not
- ```signed``` - whether it's signed or not
- ```sameSite``` - only valid for the same site 

```javascript
// SYNTAX: res.cookies(key, value, options)

// Example 1: username cookie w/ a value of 'Esten'
// includes the domain, path and whether the request was secure
res.cookies('username', 'Esten', { domain: '.echo-alchemist.com', path: '/user', secure: true })

// Example 2: "options" includes an expiration date for the cookie and it's only valid on the web server
res.cookies('username', 'Esten', { expires: new Date(Date.now() + 900000), httpOnly: true })

```


#### Request Headers
The ```request``` headers can be accessed using the ```req.headers``` property. 
**To access ONLY a single header value you can use: ```req.header(<header_value>)```**

- Example of accessing a single header value: ```req.header('User-Agent')```

###### Change the Header Value
```javascript
req.set('Content-Type', 'text/html')

// SHORTCUT FOR THE "Content-Type" header:
res.type('html');  // -> res.set('Content-Type', 'text/html');
res.type('json');  // -> res.set('Content-Type', 'application/json');
res.type('png');   // -> res.set('Content-Type', 'image/png');
```

--------------

## Redirects
Redirects can be done server-side using the ```response.redirect()``` method. You can specify, the type (ie 301, 302 etc.) as well as use an absolute URL, absolute path or use the ```..``` to go back

```javascript
// 302 redirect is the default if not code is provided
res.redirect('/go-here');

// 302 redirect w/ a URL
res.redirect('https://domain.com/go-here');

// 301 redirect
res.redirect(301, '/go-here');

// 301 redirect w/ a URL
res.redirect(301, 'https://domain.com/go-here');
```
- You can navigate backwards in the history by using the HTTP referrer headers value: ```res.redirect('back');``` which will default to ```/```




