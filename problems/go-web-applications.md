# Problem set: Web applications
This problem set is for you to learn the fundamentals of [creating a web application in Go](https://golang.org/doc/articles/wiki/).
Create a single Git repository as your submission, complete with README and gitignore files.
**NB:** after completing each exercise, commit your code - there should be at least one commit per exercise.
You be will required to submit a URL to the repository and the use of [GitHub](http://www.github.com) is recommended for this purpose.
All code should be fully commented, and the README should explain how to clone your repository and run the code.


## 1. Guessing game

Create a web application in Go that responds with the text "Guessing game".
This should be the response body irrespective of what request is received.
Explain in your README how to examine the response, including the headers, using curl.

## 2. Make the text a H1

Change your web application to make "Guessing game" a level 1 heading in HTML.
Test your application, making sure that the HTML is rendered by your browser.
If it isn't, fix it.


## 3. Serve a page using Bootstrap

Change the web application to serve a web page rather than hard-coding the text into the web application executable.
Use the [Bootstrap starter template](https://getbootstrap.com/docs/4.0/getting-started/introduction/#starter-template), changing the header to say "Guessing game".
Add a link on the page to the relative URL `/guess` with the text "New game".
Have this page served as the root resource in the web application.


## 4. Add a guess route

Create a new route in your application at `/guess`.
Have it serve a new page called guess.html.
Use the same Bootstrap code for the page as in index.html but add a level 2 heading with the text "Guess a number between 1 and 20".
Add a form, with a [text](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/number) input with the name "guess" and a submit button with the label "Guess".
The action of the form should be `/guess` and the method should be `GET`.


## 4. Turn the guess page into a template

Change the web application to use the `guess.tmpl` file as a template.
Add a single variable to the template called `Message` and create a struct in Go containing a single string.
Create an instance of the struct with the string set to "Guess a number between 1 and 20" and have the template render this in the H2 tag.


## 5. Check for a cookie

In the `/guess` handler check if the cookie called `target` is set.
If it is not, generate a random number between 1 and 20 and set a cookie called `target` to that value in the response.
Otherwise, leave the cookie at its current value.


## 6. Check for a variable

Have the `/guess` handler check if a URL encoded variable called `guess` has been set to an integer.
If it has, have the text "You guessed {guess}." inserted into the template where `{guess}` is replaced with the value of `guess`.


## 7. Compare the cookie and the guess

If the `target` cookie and the `guess` variable are both set, then have the `/guess` handler compare them.
If they are equal, set the `target` cookie to another randomly generated integer, and have the template display a congratulations message and a link to create a new game.
Otherwise, have the template display a message telling the user what their guess was and whether it was too high or too low.


## 8. Use the POST method

Change your HTML form in `guess.html` to use the `POST` method instead.
Make sure your application still works, bug fixing it if necessary.