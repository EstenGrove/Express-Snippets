# Requests and Responses
A set of methods, helpers, tools and code snippets for working with requests and responses in Express.


## Requests








----------------

## Responses


### Response Actions


##### File Download (download on the client)
- To send a file to the client: ```res.download()```
  - Once a user hits a route that sends a file, the user will be prompted to download a file in the browser.
  - The ```res.download()``` also includes a single use callback

```javascript
app.get('/', (req, res) => res.download('./puppy.pdf'))

// You can send a custom file name with the file
res.download('./puppy.pdf', 'CutePuppies.pdf');

// single-use callback
res.download('./puppy.pdf', 'CutePuppies.pdf', (err) => {
  if(err) {
    // handle error
    return;
  } else {
    // do something
  }
});
```


##### Validating Input
All form values must be validated prior to being sent to a database or other storage mechanism.
- Use the ```express-validator``` middleware: ```npm i express-validator```
  - Use the ```check()``` function in the callback of the POST handler
  - ```express-validator``` uses **validator.js** as the internal validating method.
  - **validator.js** allows regex for validation methods
  - Allowed methods within **validator.js** include: 
```
isAscii()
isBase64()
isBoolean()
isCurrency()
isDecimal()
isEmail()
isEmpty()
isFQDN(), // is a fully qualified domain name?
isFloat()
isHash()
isHexColor()
isIP()
isIn(), // check if the value is in an array of allowed values
isInt()
isJSON()
isLatLong()
isLength()
isLowercase()
isMobilePhone()
isNumeric()
isPostalCode()
isURL()
isUppercase()
isWhitelisted() // checks the input against a whitelist of allowed characters

// Checking dates
isAfter()  // if the entered date is after the one you pass to the method
isBefore() // if entered date is before what's passed to the method
```

**Example**
```javascript
const express = require('express');
const app = express();

const { check } = require('express-validator/check');

// pass an array of "check()" calls
app.post('/form', [
  check('name').isLength({ min: 3 }),
  check('email').isEmail(),
  check('age').isNumeric()
], (req, res) => {
  const name  = req.body.name
  const email = req.body.email
  const age   = req.body.age
})

// you can pipe several check() functions together
check('name')
  .isAlpha()
  .isLength({ min: 10 })

```
- If there's an error a message will be sent back to the client in the response.
- However, you can override the default error messaging: 
```javascript
check('name')
  .isAlpha()
  .withMessage('Only alphabetical characters allowed')
  .isLength({ min: 8 })
  .withMessage('Must be 8 characters long')
```
- **Or you can use a custom written validator**:
```javascript
app.post('/form', [
  check('name').isLength({ min: 3 }),
  check('email').custom(email => {
    if (alreadyHaveEmail(email)) {
      throw new Error('Email already registered')
    }
  }),
  check('age').isNumeric()
], (req, res) => {
  const name  = req.body.name
  const email = req.body.email
  const age   = req.body.age
})

check('email').custom(email => {
  if (alreadyHaveEmail(email)) {
    throw new Error('Email already registered')
  }
})


// or rewritten as
check('email').custom(email => {
  if (alreadyHaveEmail(email)) {
    return Promise.reject('Email already registered')
  }
})
```
