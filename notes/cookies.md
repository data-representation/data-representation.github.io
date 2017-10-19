# Cookies
The following is largely taken from [MDN's HTTP cookies documentation](https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies) and [Go's net/http documentation](https://golang.org/pkg/net/http/).


## What are cookies?
Cookies are just name-value pairs.
For example, a cookie's name might be `age` and its value `21`.
Note that both the name and value of a cookie are text, and so are represented in programming languages as strings.
So, the value `21` above is not stored as an integer but rather as a string with the two characters '2' and '1'.
Remember, HTTP is mainly just text.


## Why use cookies?
Here's [MDN's HTTP documentation](https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview) on the fact that HTTP is a stateless protocol:

> *HTTP is stateless, but not sessionless:*
> There is no link between two requests being successively carried out on the same connection. This immediately has the prospect of being problematic for users attempting to interact with certain pages coherently, for example, using e-commerce shopping baskets. But while the core of HTTP itself is stateless, HTTP cookies allow the use of stateful sessions. Using header extensibility, HTTP Cookies are added to the workflow, allowing session creation on each HTTP request to share the same context, or the same state.

So, statelessness means that there is no way for a server to chain two requests together.
It must deal with each request as an atomic, fully-contained unit by itself.
Cookies can be used to identify requests coming from the same client.


## Workflow

1. Client sends a first HTTP request to server. No cookies are set yet.
2. Server receives the request, checks for a cookie and sees there isn't one. It sends a response with a `Set-Cookie` header line.
3. The client receives the response and stores the cookie.
4. The client sends another request to the same server with the cookie in the `Cookie` header line.
5. The server accepts the request, sees the cookie is set and changes its response based on the cookie.


```http
HTTP/1.0 200 OK
Content-type: text/html
Set-Cookie: yummy_cookie=choco
Set-Cookie: tasty_cookie=strawberry

[page content]
```

```http
GET /sample_page.html HTTP/1.1
Host: www.example.org
Cookie: yummy_cookie=choco; tasty_cookie=strawberry
```

## Sessions

Cookies can be used to set up sessions.
Sessions are a series of communications between a HTTP client and server, usually over a short period of time.
The server sets a session cookie (no timestamp) with a unique identifier for each user/session.
The cookie is often deleted by the browser upon closing.


## Cookies in Go

In Go, the `http` package provides a struct to represent a cookie.
Structs are just convenient ways of grouping variables together - rather than having two variables in scope called `cookie_name` and `cookie_value`, we encapsulate them user a single variable of type Cookie, along with any other related variables.
We can then access them using dot notation, e.g. `mycookie.name` and `mycookie.value`. 

```go
// https://golang.org/pkg/net/http/#Cookie
type Cookie struct {
        Name  string
        Value string

        Path       string    // optional
        Domain     string    // optional
        Expires    time.Time // optional
        RawExpires string    // for reading cookies only

        // MaxAge=0 means no 'Max-Age' attribute specified.
        // MaxAge<0 means delete cookie now, equivalently 'Max-Age: 0'
        // MaxAge>0 means Max-Age attribute present and given in seconds
        MaxAge   int
        Secure   bool
        HttpOnly bool
        Raw      string
        Unparsed []string // Raw text of unparsed attribute-value pairs
}
```

To create a new `Cookie`, we can use any of the following.
Note that the latter two are pointers, which is the most typical way to deal with instances.

```go
mycookie := http.Cookie{Name: "uid", Value: "ke2fowp3"}
mycookieptr := &http.Cookie{Name: "uid", Value: "ke2fowp3"}
mycookienewptr := new(http.Cookie)
```

The snippet `http.Cookie{Name: "uid", Value: "ke2fowp3"}` is called a struct literal, like the way `"Hello, world!"` is called a string literal.

## Setting a cookie in Go
The `http` package provides a function to set a cookie on a `ResponseWriter` (more on those below).
Here is the function signature from the documentation.
```go
func SetCookie(w ResponseWriter, cookie *Cookie)
```
So, `SetCookie` is a function that takes two arguments and doesn't return anything.
The first argument is a `ResponseWriter`.
The second is a pointer to cookie.
So, we can set a cookie on a `ResponseWriter` as follows.
Note the ampersand before the struct literal, because the function is looking for a pointer.

```go
SetCookie(w, &http.Cookie{Name: "uid", Value: "ke2fowp3"})
```

## Responses in Go
In the last section we mentioned `ResponseWriter`.
This is defined in the `http` package as an interface.
Interfaces in Go are just types that contain collections of method signatures.
They allow us to use different objects in the same situation.
Any type that implements the methods defined in the `ResponseWriter` interface below can be used wherever a `ResponseWriter` is accepted.
```go
type ResponseWriter interface {
        // Header returns the header map that will be sent by
        // WriteHeader. The Header map also is the mechanism with which
        // Handlers can set HTTP trailers.
        //
        // Changing the header map after a call to WriteHeader (or
        // Write) has no effect unless the modified headers are
        // trailers.
        //
        // There are two ways to set Trailers. The preferred way is to
        // predeclare in the headers which trailers you will later
        // send by setting the "Trailer" header to the names of the
        // trailer keys which will come later. In this case, those
        // keys of the Header map are treated as if they were
        // trailers. See the example. The second way, for trailer
        // keys not known to the Handler until after the first Write,
        // is to prefix the Header map keys with the TrailerPrefix
        // constant value. See TrailerPrefix.
        //
        // To suppress implicit response headers (such as "Date"), set
        // their value to nil.
        Header() Header

        // Write writes the data to the connection as part of an HTTP reply.
        //
        // If WriteHeader has not yet been called, Write calls
        // WriteHeader(http.StatusOK) before writing the data. If the Header
        // does not contain a Content-Type line, Write adds a Content-Type set
        // to the result of passing the initial 512 bytes of written data to
        // DetectContentType.
        //
        // Depending on the HTTP protocol version and the client, calling
        // Write or WriteHeader may prevent future reads on the
        // Request.Body. For HTTP/1.x requests, handlers should read any
        // needed request body data before writing the response. Once the
        // headers have been flushed (due to either an explicit Flusher.Flush
        // call or writing enough data to trigger a flush), the request body
        // may be unavailable. For HTTP/2 requests, the Go HTTP server permits
        // handlers to continue to read the request body while concurrently
        // writing the response. However, such behavior may not be supported
        // by all HTTP/2 clients. Handlers should read before writing if
        // possible to maximize compatibility.
        Write([]byte) (int, error)

        // WriteHeader sends an HTTP response header with status code.
        // If WriteHeader is not called explicitly, the first call to Write
        // will trigger an implicit WriteHeader(http.StatusOK).
        // Thus explicit calls to WriteHeader are mainly used to
        // send error codes.
        WriteHeader(int)
}
```

## Reading cookies
To read a cookie from a HTTP response in Go, we use the `Cookie` method on the `Request` type.
The `Request` type is discussed below, but first let's have a look at the `Cookie` method's signature. 

```go
func (r *Request) Cookie(name string) (*Cookie, error)
```

This is a method in Go, as opposed to a function.
The difference is that a method receives a special argument called a receiver.
There's nothing all that special about a receiver, other than the syntax of calling the method.
Suppose myreq is an instance of type `Request`.
We can call Cookie as follows.

```go
mycookie, err := myreq.Cookie('uid')
```

The method takes one argument, a string containing the name of the cookie.
It returns two items, a `Cookie` and an `Error`, which is just an interface.

```go
type error interface {
        Error() string
}
```

## Requests
In Go, HTTP requests are represented by the `Request` type in the `http` package.
```go
type Request struct {
        // Method specifies the HTTP method (GET, POST, PUT, etc.).
        // For client requests an empty string means GET.
        Method string

        // URL specifies either the URI being requested (for server
        // requests) or the URL to access (for client requests).
        //
        // For server requests the URL is parsed from the URI
        // supplied on the Request-Line as stored in RequestURI.  For
        // most requests, fields other than Path and RawQuery will be
        // empty. (See RFC 2616, Section 5.1.2)
        //
        // For client requests, the URL's Host specifies the server to
        // connect to, while the Request's Host field optionally
        // specifies the Host header value to send in the HTTP
        // request.
        URL *url.URL

        // The protocol version for incoming server requests.
        //
        // For client requests these fields are ignored. The HTTP
        // client code always uses either HTTP/1.1 or HTTP/2.
        // See the docs on Transport for details.
        Proto      string // "HTTP/1.0"
        ProtoMajor int    // 1
        ProtoMinor int    // 0

        // Header contains the request header fields either received
        // by the server or to be sent by the client.
        //
        // If a server received a request with header lines,
        //
        //	Host: example.com
        //	accept-encoding: gzip, deflate
        //	Accept-Language: en-us
        //	fOO: Bar
        //	foo: two
        //
        // then
        //
        //	Header = map[string][]string{
        //		"Accept-Encoding": {"gzip, deflate"},
        //		"Accept-Language": {"en-us"},
        //		"Foo": {"Bar", "two"},
        //	}
        //
        // For incoming requests, the Host header is promoted to the
        // Request.Host field and removed from the Header map.
        //
        // HTTP defines that header names are case-insensitive. The
        // request parser implements this by using CanonicalHeaderKey,
        // making the first character and any characters following a
        // hyphen uppercase and the rest lowercase.
        //
        // For client requests, certain headers such as Content-Length
        // and Connection are automatically written when needed and
        // values in Header may be ignored. See the documentation
        // for the Request.Write method.
        Header Header

        // Body is the request's body.
        //
        // For client requests a nil body means the request has no
        // body, such as a GET request. The HTTP Client's Transport
        // is responsible for calling the Close method.
        //
        // For server requests the Request Body is always non-nil
        // but will return EOF immediately when no body is present.
        // The Server will close the request body. The ServeHTTP
        // Handler does not need to.
        Body io.ReadCloser

        // GetBody defines an optional func to return a new copy of
        // Body. It is used for client requests when a redirect requires
        // reading the body more than once. Use of GetBody still
        // requires setting Body.
        //
        // For server requests it is unused.
        GetBody func() (io.ReadCloser, error)

        // ContentLength records the length of the associated content.
        // The value -1 indicates that the length is unknown.
        // Values >= 0 indicate that the given number of bytes may
        // be read from Body.
        // For client requests, a value of 0 with a non-nil Body is
        // also treated as unknown.
        ContentLength int64

        // TransferEncoding lists the transfer encodings from outermost to
        // innermost. An empty list denotes the "identity" encoding.
        // TransferEncoding can usually be ignored; chunked encoding is
        // automatically added and removed as necessary when sending and
        // receiving requests.
        TransferEncoding []string

        // Close indicates whether to close the connection after
        // replying to this request (for servers) or after sending this
        // request and reading its response (for clients).
        //
        // For server requests, the HTTP server handles this automatically
        // and this field is not needed by Handlers.
        //
        // For client requests, setting this field prevents re-use of
        // TCP connections between requests to the same hosts, as if
        // Transport.DisableKeepAlives were set.
        Close bool

        // For server requests Host specifies the host on which the
        // URL is sought. Per RFC 2616, this is either the value of
        // the "Host" header or the host name given in the URL itself.
        // It may be of the form "host:port". For international domain
        // names, Host may be in Punycode or Unicode form. Use
        // golang.org/x/net/idna to convert it to either format if
        // needed.
        //
        // For client requests Host optionally overrides the Host
        // header to send. If empty, the Request.Write method uses
        // the value of URL.Host. Host may contain an international
        // domain name.
        Host string

        // Form contains the parsed form data, including both the URL
        // field's query parameters and the POST or PUT form data.
        // This field is only available after ParseForm is called.
        // The HTTP client ignores Form and uses Body instead.
        Form url.Values

        // PostForm contains the parsed form data from POST, PATCH,
        // or PUT body parameters.
        //
        // This field is only available after ParseForm is called.
        // The HTTP client ignores PostForm and uses Body instead.
        PostForm url.Values

        // MultipartForm is the parsed multipart form, including file uploads.
        // This field is only available after ParseMultipartForm is called.
        // The HTTP client ignores MultipartForm and uses Body instead.
        MultipartForm *multipart.Form

        // Trailer specifies additional headers that are sent after the request
        // body.
        //
        // For server requests the Trailer map initially contains only the
        // trailer keys, with nil values. (The client declares which trailers it
        // will later send.)  While the handler is reading from Body, it must
        // not reference Trailer. After reading from Body returns EOF, Trailer
        // can be read again and will contain non-nil values, if they were sent
        // by the client.
        //
        // For client requests Trailer must be initialized to a map containing
        // the trailer keys to later send. The values may be nil or their final
        // values. The ContentLength must be 0 or -1, to send a chunked request.
        // After the HTTP request is sent the map values can be updated while
        // the request body is read. Once the body returns EOF, the caller must
        // not mutate Trailer.
        //
        // Few HTTP clients, servers, or proxies support HTTP trailers.
        Trailer Header

        // RemoteAddr allows HTTP servers and other software to record
        // the network address that sent the request, usually for
        // logging. This field is not filled in by ReadRequest and
        // has no defined format. The HTTP server in this package
        // sets RemoteAddr to an "IP:port" address before invoking a
        // handler.
        // This field is ignored by the HTTP client.
        RemoteAddr string

        // RequestURI is the unmodified Request-URI of the
        // Request-Line (RFC 2616, Section 5.1) as sent by the client
        // to a server. Usually the URL field should be used instead.
        // It is an error to set this field in an HTTP client request.
        RequestURI string

        // TLS allows HTTP servers and other software to record
        // information about the TLS connection on which the request
        // was received. This field is not filled in by ReadRequest.
        // The HTTP server in this package sets the field for
        // TLS-enabled connections before invoking a handler;
        // otherwise it leaves the field nil.
        // This field is ignored by the HTTP client.
        TLS *tls.ConnectionState

        // Cancel is an optional channel whose closure indicates that the client
        // request should be regarded as canceled. Not all implementations of
        // RoundTripper may support Cancel.
        //
        // For server requests, this field is not applicable.
        //
        // Deprecated: Use the Context and WithContext methods
        // instead. If a Request's Cancel field and context are both
        // set, it is undefined whether Cancel is respected.
        Cancel <-chan struct{}

        // Response is the redirect response which caused this request
        // to be created. This field is only populated during client
        // redirects.
        Response *Response
        // contains filtered or unexported fields
}
```


