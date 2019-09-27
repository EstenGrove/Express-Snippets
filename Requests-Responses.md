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
