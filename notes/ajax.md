# AJAX
AJAX stands for Asynchronous JavaScript And XML.
It is a combination of various technologies that enables modern web applications.
It enables web browsers to send requests and receive responses from a web server without removing the currently displayed document, and without blocking other tasks in the meantime.
Its main feature for end users is that it removes the need for page reloads when sending and receiving data to and from the server.

## Asynchronous
- HTTP requests are usually sent over a network, and that means they are relatively slow.
- Users like to still be able to interact with a web application when a HTTP request has been made in the background.
- JavaScript provides asynchronous calls to enable this.
- You tell JavaScript to make a HTTP request and to call a given function once it receives a response.
- It handles all of this in the background, and your subsequent statements can run while it's happening.
- It's a lot like parallel programming for JavaScript, but behind the scenes JavaScript doesn't quite work like that, but who cares!

## JavaScript
- JavaScript provides an object called `XMLHttpRequest` to handle AJAX requests.
- You create an instance of this object, and set some properties and call some methods to make and deal with the HTTP request you want to make.

```javascript
// From: https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest/onreadystatechange
var xhr = new XMLHttpRequest();

xhr.open("GET", "https://developer.mozilla.org/", true);
xhr.onreadystatechange = function () {
  if(xhr.readyState === XMLHttpRequest.DONE && xhr.status === 200) {
    console.log(xhr.responseText);
  }
};
xhr.send();
``` 

- The bells and whistles are generally not used. Libraries like [jQuery](http://jquery.com/) provide easy-to-use wrapper functions.

```javascript
$.get("https://developer.mozilla.org/", function(data) {
  console.log(data);
});
```

## XML
- XML is a way of representing structured data in text.
- It's like HTML where you define your own tags.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!-- From: https://www.w3schools.com/xml/ -->
<note>
  <to>Tove</to>
  <from>Jani</from>
  <heading>Reminder</heading>
  <body>Don't forget me this weekend!</body>
</note> 
```

- You don't have to use XML with AJAX, and most web applications don't.
- We usually use JSON instead as it is so easily parsed in JavaScript.

```json
{
  "note":
  {
    "to": "Tove",
    "from": "Jani",
    "heading": "Reminder",
    "body": "Don't forget me this weekend!"
}
```

## Before AJAX
- To understand why AJAX exists, it's important to know what the web was like before it.
- Web pages had links (or anchors) that people could click on. These either brought users to another, different web page or to another section of the same page.
- They also had forms for users to submit data to the server, and these would bring users to a different page upon submission.
- Basically, everything resulted in the contents of the browser window being completely removed and replaced with different contents, with a certain amount of waiting on the user's part.