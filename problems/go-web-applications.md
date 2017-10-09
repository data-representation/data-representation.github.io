# Problem set: Web applications
This problem set is for you to learn the fundamentals of [creating a web application in Go](https://golang.org/doc/articles/wiki/).
Create a single Git repository as your submission, complete with README and gitignore files.
*NB:* after completing each exercise commit your code - there should be at least one commit per exercise.
You be will required to submit a URL for the repository and the use of [GitHub](http://www.github.com) is recommended for this purpose.
All code should be fully commented, and the README should explain how to clone your repository and run the code.


## 1. Hello, world!

Create a web application in Go that responds with the text "Hello, world!".
This should be the response (body) no matter what the request is.
Explain in your README how to examine the response, including the headers, using curl.


## 2. Current time

Create a new route in your application at "/time".
It should respond with the current date and time.


## 3. Set a cookie

Create a new route at "/cookie".
Have it respond with the following: "<h1>I have seen you 1 times before</h1>".
Set a cookie called "Seen" in every response and set it to the value 1.


## 4. Update the cookie

When a request comes into "/cookie", check to see if the "Seen" cookie exists.
If it doesn't, set the cookie to 1 (as above) and respond with "<h1>We've never met</h1>".
Otherwise:

1. Read the cookie's value.
2. Increase its value by 1 and add it to the repsonse.
3. Respond with "<h1>I have seen you n times before</h1>" where n is the value of the cookie.
