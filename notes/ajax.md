---
title: AJAX
description: An introduction to the why and how of Asynchronous JavaScript and XML.
---

# AJAX

AJAX stands for Asynchronous JavaScript and XML, and XML in turn stands for eXtensible Markup Language.
AJAX is a term coined by a designer called Jesse James Garrett to describe a then-new way of programming web applications.
It involved technologies like [HTML](https://developer.mozilla.org/en-US/docs/Web/HTML), the [Document Object Model (DOM)](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Introduction) and [XML](https://developer.mozilla.org/en-US/docs/XML_Introduction).
The latter has now been largely replaced by [JSON](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON).
There is a plethora of information about AJAX [on the web](https://developer.mozilla.org/en-US/docs/AJAX), but we'll consider only the main use case.

In the days before JavaScript, every user interaction with a webpage resulted in the browser making a new HTTP request in the foreground.
Upon clicking on a link or submitting a form, the user would wait for the browser view to be replaced by a new page.
This made web applications [slow and cumbersome](https://developer.mozilla.org/en-US/Apps/Fundamentals/Performance/Performance_fundamentals), and the [user experience (UX)](https://www.udacity.com/course/intro-to-the-design-of-everyday-things--design101) was not great.

The 90's brought a range of attempts at improving UX by extending a browser's capability to incorporate dynamic (i.e. interactive) elements.
Two of the attempts that gained traction were Netscape's [JavaScript](https://developer.mozilla.org/bm/docs/Web/JavaScript) and, within JavaScript, Microsoft's [XMLHttpRequest object](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest/Using_XMLHttpRequest).
Both are [somewhat ugly hacks](https://www.destroyallsoftware.com/talks/wat) but, like HTML, are the way they are because of a series of quick fixes and compromises between competitors like Microsoft and Mozilla.

## The main use case

The main use case for AJAX goes something like this.

1. JavaScript is used to capture events that happen on a web page in a user's browser, such as the user clicking a Submit button on a form. By capture we mean two things: the default action of the form is prevented, and something else is done instead. In jQuery, that looks something like this.
```js
$("myform").submit(function(event){
  event.preventDefault();
  doSomethingElse();
});
```
2. The `doSomethingElse()` is replaced with a HTTP request that happens in the *background* of the web browser. This means that the web browser's view is not cleared and replaced with another resource. The user might not see any effect of the HTTP request being made in the background at all, though that would be bad practice. In jQuery, that looks something like this.
```js
$.post("/submit", function(data){
  console.log("The server responded with: " + data);
});
  ```
 3. It's not very interesting to just print the response to the console, in AJAX we are usually interested in doing something with the response. Let's say we want to add a new list item to the page that's *currently* displayed in the browser. That is, we're not going to load a new resource/page, but rather we'll just manipulate the one that is currently displayed. This is enabled by the [DOM](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model) which is the browser's model of the page. In jQuery, it's very easy.
```js
$("#mylist").append('<li>' + data + '</li>');
```
 4. This assumes that data is a single string that contains something that the user wants to see. Usually, we have the server return more than just a single string, which is where [JSON](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON) comes in. JSON stands for JavaScript Object Notation and is simply a way of representing data in text format that heavily borrows from JavaScript's syntax. We can use JSON by using `postJSON()` instead of `post()` in jQuery.
 ```js
$.postJSON("/submit", function(data){
  $("#mylist").append('<li>' + data.user + ' <i>' + data.joined + '</i></li>');
});
 ```


## XML and JSON
In the past, AJAX involved [XML](https://developer.mozilla.org/en-US/docs/XML_Introduction).
The body of the HTTP response received by the browser after an AJAX HTTP request contained XML.
XML is a way of representing structured data in text.
It's like HTML where you make up your own tags.
You can open a text editor and type XML into it because it's just text.
Here's an example that represents data regarding a reminder in XML.
You might use it to send information about the reminder from the HTTP server to a web browser.

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

The browser, upon receiving XML like the above, would parse the XML and convert it into some sort of data structure in memory.
Then the information could be displayed to the user by manipulating the DOM.

You don't have to use XML with AJAX, and most web applications don't.
JSON is typically used instead as it is so easily parsed in JavaScript.
The same information might be written in JSON as follows.

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

We use JSON mainly because of its similarity to and ease-of-use in JavaScript.
The [JavaScript (ECMAScript) standard](https://tc39.github.io/ecma262/) defines two useful methods for dealing with JSON.
The first is [JSON.parse()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/parse), which converts a string containing JSON text into a JavaScript object (instance) in memory.
```js
var digits = JSON.parse('{"1": "one", "2": "two", "3": "three"}');
digits[1]
// Outputs "one"
```
The second method is [JSON.stringify](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).
It's the counterpart to `JSON.parse()` in turning JavaScript objects into JSON text.
```js
var car = {make: "Toyota", model: "Corolla"}
var carjson = JSON.stringify(car)
carjson
// Outputs '{"make":"Toyota","model":"Corolla"}'
```
With AJAX, when a HTTP response is received from an AJAX request, we can just call `JSON.parse()` on the body of the request and we have an object ready to use.


## Asynchronous
AJAX requests happen [asynchronously](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest/Synchronous_and_Asynchronous_Requests), which sometimes makes programming with them challenging, but it is essential.
HTTP requests are usually sent over a network, and that means they are much slower than most other things your computer is doing.
Users like to still be able to interact with a web application when an AJAX request has been made in the background.
JavaScript provides asynchronous calls to enable this.
You tell JavaScript to make a HTTP request and to call a given function once it receives a response.
A function which the interpreter will call once it is finished something asynchronously is called a *callback*.
The interpreter handles all of this in the background, and your subsequent statements can run while the AJAX call is pending.
It's like threads for JavaScript, [but behind the scenes](https://developer.mozilla.org/en-US/docs/Web/JavaScript/EventLoop), JavaScript [doesn't quite work like that](https://stackoverflow.com/a/39961).


## XMLHttpRequest
Under the hood, browsers provide a JavaScript object called `XMLHttpRequest` to handle AJAX requests.
You create an instance of this object, and set some properties and call some methods to make and deal with the HTTP request you want to make.
Note that it's very unusual to make an AJAX call by using `XMLHttpRequest` directly.
We usually use JavaScript libraries like jQuery and Angularjs to make AJAX requests using convenience functions they provide.
In any case, here [an example](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest/onreadystatechange).
```javascript
var xhr = new XMLHttpRequest();

xhr.open("GET", "https://developer.mozilla.org/", true);
xhr.onreadystatechange = function () {
  if(xhr.readyState === XMLHttpRequest.DONE && xhr.status === 200) {
    console.log(xhr.responseText);
  }
};
xhr.send();
``` 
