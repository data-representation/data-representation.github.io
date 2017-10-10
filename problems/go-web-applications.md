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

## 2. Make the text a H1

Change your web application to make "Hello, world!" a level 1 heading in HTML.
Test your application, making sure that the HTML is rendered by your browser.
If it isn't, fix it.


## 3. Current time

Create a new route in your application at "/time".
It should respond with the current date and time.


## 4. Set a cookie

When sending the time as a response, set a cookie called `last`and set its value to the current date and time also.
Each time you send the response, check if the cookie is set.
If the cookie is set, add a line to the response body saying "It is <time delta> hours since you last checked.", replacing "<time delta>" with the number of hours between the cookie time and the current time.


## 5. Add an index.html

Add an index.html file to your web application, and have it served as the root (/) resource instead of "Hello, world!"
You can use the [Bootstrap 4 starter template](https://getbootstrap.com/docs/4.0/getting-started/introduction/#starter-template) if you want.


## 6. Make time serve a template

Make the time into a template, displaying the information in HTML format.
