# HTTP
An excellent guide to HTTP is available at http://www.jmarshall.com/easy/http.
The following is mainly taken from that.

## HyperText Transfer Protocol
- **HyperText** - Text with links.
- **Transfer** - Communication of data.
- **Protocol** - Set of rules for communication.

## Versions
- `HTTP/1.0` was the first main version.
- `HTTP/1.1` is the current widely used version.
- `HTTP/2` use is growing.


## What is HTTP?
- You'll know HTTP from typing `http://` in your browser's location bar.
- `RFC 2616` details `HTTP/1.1`.
- It is a standard way of transmitting data, such as HTML, CSS, JavaScript (text), images, and videos across the web.

![GMIT Hopepage](../images/gmit-homepage.png)

## Why study HTTP?
>HTTP is the network protocol of the Web. It is both simple and powerful. Knowing HTTP enables you to write Web browsers, Web servers, automatic page downloaders, link-checkers, and other useful tools. - *James Marshall*

- **HTTP** is the main protocol used by web browsers.
- **Netflix** uses HTTP to stream video (using DASH).
- **Instagram** is basically just a HTTP API.
- **Facebook, GMail and Twitter** use HTTP.


## Why is HTTP so widely used?  
- **HTTP** is often used instead of protocols that are more suited to the application.
- **Browsers** are one of the main reasons for this.
- **Ooperating systems** typically have a browser installed by default.
- **Web servers** and browsers mainly talk over HTTP.
- **Libraries** exist for most programming languages to make HTTP requests.
- **HTTP** is relatively straight-forward.
- **Firewalls** usually allow HTTP by default.  


## How does HTTP work?  
- **Clients** (e.g. Firefox) perform requests.
- **Servers** (e.g. Apache) respond to requests.
- **Requests** are chunks of data (e.g. text files) sent by clients to servers.
- **Responses** are chunks of data sent by server to clients.
- **URLs** specify the server's location and the resource on the server.
- **Headers** are text metadata added to the start of requests and responses.
- **Bodies** are the main content of responses, and sometimes requests.
- **HTML** typically forms the body of a response.  


## Request-Response
![Request Response model](/images/request-response.png)


## Uniform Resource Locator
URLs locate resources.

`http://username:password@www.reddit.com:80/r/funny/?limit=1`

| Part         | Name      |
|--------------|-----------|
| `http`       | Protocol  |
| `username`   | Username  |
| `password`   | Password  |
| `www`        | Subdomain |
| `reddit.com` | Domain    |
| `80`         | Port      |
| `/r/funny/`  | Path      |
| `limit=1`    | Parameter |


## Request (GET) Example
```http
GET /courses/all-courses HTTP/1.1
Host: gmit.ie
User-Agent: curl/7.50.1
Accept: */*
```

## Response Example
```http
HTTP/1.1 200 OK
Date: Mon, 27 Jul 2009 12:28:53 GMT
Server: Apache/2.2.14 (Win32)
Last-Modified: Wed, 22 Jul 2009 19:15:56 GMT
Content-Length: 88
Content-Type: text/html
Connection: Closed

<html>
  <body>
    <h1>Hello, World!</h1>
  </body>
</html>
```


## Request and Response Format
Requests and responses both have this format:
- Initial line.
- Zero or more header lines.
- A blank line.
- Optional message body (e.g. a HTML file)


## Resources
> HTTP is used to transmit resources...a resource is some chunk of information that can be identified by a URL...The most common kind of resource is a file, but a resource may also be a dynamically-generated query result... - *James Marshall*

- In the 90's most resources were normal (static) text files and image files sitting on the server.
- They were requrested using their path: `http://www.gmit.ie/ian/homepage.html`
- These days most resources are dynamically generated from templates and database queries.
- A request will trigger a database query and the result set will be inserted into a HTML template, which forms the resource.


## HTTP Methods
- `GET` -  Retrieve information from the server.
- `HEAD` -  Like get, but retrieve only the response header.
- `POST` -  Send data to the server.
- `PUT` -  Set the resource at the URL to the request data.
- `DELETE` -  Delete the resource at the URL.
- `CONNECT` -  Set up tunnel for other traffic to pass through HTTP.
- `OPTIONS` -  Find the allowable operations at the given URL.
- `TRACE` -  Echo the received request.
- `PATCH` -  Partial resource modification.


## Status codes
- `404` means you requested a resource that doesn't exist on the server.
- `200` means everything is OK, and is the most common one in everyday browsing.
- `All` status codes are three digit numbers.
- `1xx` indicates an informational message only
- `2xx` indicates success of some kind
- `3xx` redirects the client to another URL
- `4xx` indicates an error on the client's part
- `5xx` indicates an error on the server's part


## Standard 404 page
Many websites will send a special web page with when they are returning a 404 status.
Here's Google's:
![Google's 404 page](../images/google-404.png)
So,t a response can contain data even when the code is not 200.


## HTTP is not like a phone call
- Suppose Alice rings Bob's Pizza Place, and orders a pizza.
- Bob can ask Alice "What kind and what size would you like?"
- Alice can respond.
- HTTP doesn't work like that.
- With HTTP Alice would have to say "Hello, I want a 12" pepporoni pizza delivered to 123 Face St. please."
- Misunderstandings result in the server responding with an error, and hanging up.
- Status codes indicate errors, amongst other things.


## Sending data to the server
- Often the client will want to send extra information to the server with the request.
- Resources can be generated differently based on this extra data.
- For instance Google (used to?) let users send their search terms in URL.
- Try opening `google.ie/` in your browser, and then open `google.ie/?q=gmit`.
- Both times the requested resource is `/`, the root resource.
- Another way to send extra data to the server is in the request body (the typical way for `POST` requests.)

![Google's root resource with q=gmit](../images/google-q-gmit.png)


## URL encoding
To send data from the client to the server using the URL, we have to encode the data, since certain characters in a URL have special meanings.

- Any character can be encoded to `%XX` where `XX` is its ASCII value.
- Characters for which we have to do this include =, &, %, and +.
- All spaces must be changed to plusses.
- Names and values must be in the format: `name1=value1\&name2=value2`.



## GET request with parameters
```http
GET /r/ireland?limit=1 HTTP/1.1
Host: reddit.com
User-Agent: curl/7.50.1
Accept: */*
```


## POST request with parameters
```http
POST /path/script.cgi HTTP/1.0
From: frog@jmarshall.com
User-Agent: HTTPTool/1.0
Content-Type: application/x-www-form-urlencoded
Content-Length: 32

home=Cosby&favorite+flavor=flies
```

## JSON in POST
- It's unusual to use URL encoding rules to send data in a POST request these days.
- JavaScript Object Notation (JSON) is used instead.

The following:
```http
home=Cosby&favorite+flavor=flies
```
becomes:
```json
{"home": "Cosby", "favorite_flavor": "flies"}
```