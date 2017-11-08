# Problem set: Regular expressions
This problem set is about [regular expressions in Go](https://gobyexample.com/regular-expressions).
Create a single Git repository as your submission, complete with README and gitignore files.
**NB:** after completing each exercise, commit your code - there should be at least one commit per exercise.
You be will required to submit a URL to the repository and the use of [GitHub](http://www.github.com) is recommended for this purpose.
All code should be fully commented, and the README should explain how to clone your repository and run the code.


## 1. Random response
Write a function called `ElizaResponse` in Go that takes a single string as input and returns a single string as output.
The function should randomly return one of the following three strings.
- "I'm not sure what you're trying to say. Could you explain it to me?"
- "How does that make you feel?"
- "Why do you say that?"

Add a `main` function to your Go code.
Test your `ElizaResponse` function by calling it from `main` with the following test inputs.

- "People say I look like both my mother and father."
- "Father was a teacher."
- "I was my father's favourite."
- "I'm looking forward to the weekend."
- "My grandfather was French!"

Have main print both the input to and output from the function to the terminal.


## 2. Recognise "father"
Adapt `ElizaResponse` to check, using a regular expression, if the input contains the word "father".
If it does, it should return the string "Why don't you tell me more about your father?"
Otherwise, it should return one of the three random responses as before.
You should make sure that your regular expression recognises the first three test inputs, but not the last two.


## 3. Capture "I am "
Adapt the `ElizaResponse` function to, if the input does not contain the word "father", check the input begins with "I am ".
If it does, use a regular expression to capture the rest of the user input.
Return the string "How do you know you are _?", replacing the underscore with the captured part of the input.
Add, along with the five previous test inputs, the following test inputs.

- "I am happy."
- "I am not happy with your responses."
- "I am not sure that you understand the effect that your questions are having on me."
- "I am supposed to just take what you're saying at face value?"


## 4. "i am ", "I AM ", "I'm ", "Im ", "i'm "
Adapt the function to respond in the same way as with "I am _", when the input begins with any reasonable variant of "I am ", such as "I'm" or "Im".


## 5. Reflect pronouns
Adapt the function to reflect the pronouns in the captured groups, where necessary.
For instance, when the following input is given:

> "I am not sure that you understand the effect your questions are having on me."

the function should return:

> "How do you know that you are not sure that *I* understand the effect *my* questions are having on *you*."

## 6. More input patterns
Add three of your own input patterns to the function, with corresponding outputs.