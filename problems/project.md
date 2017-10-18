# Project 2017

The following are the instructions to complete the project for the module [Data Representation and Querying](https://data-representation.github.io) for 2017.
This project is worth 50% of your marks for the module, unless you are also completing the optional problem sheets [in which case it may be worth 30%](https://data-representation.github.io/#assessment-information).
Please see the [course homepage for the deadline](https://data-representation.github.io/#submit-ca).

## Overview
In this project you will create a chatbot web application in Go.
Users will be able to visit the web application through their browser, type in sentences such as "Hi, my name is John." and the web application will reply with sentences such as "Hello John, how are you?"
You are free to use your artistic license in creating the chatbot, such as by giving it a certain type of personality, but you should be guided by the [ELIZA](https://en.wikipedia.org/wiki/ELIZA) program.
If you wish to use any technique to enhance your chatbot, such as machine learning methods, that is okay as long as the chatbot works.
However, reasonable care must be taken to ensure that the chatbot you create does not act in an offensive manner, and students are reminded of the [GMIT Code of Student Conduct](http://www.gmit.ie/sites/default/files/public/general/docs/7.1-code-student-conduct-2017-2018.pdf).

## Instructions
1. Create a git repository with a README.md and an appropriate gitignore file. The README should explain who you are, why you created the chatbot, how you created the chatbot, how to download and run the chatbot and any references you have used.
2. In the repository, create a web application that serves a HTML page as the root resource. The page should contain an input textbox for the user to talk to the chatbot, and an area to display both the chatbot's output and the user's previous inputs. Think of applications like WhatsApp and iMessage, but in a web browser and you are talking to a bot rather than a person. The input text box should use AJAX to interact with the chatbot.
3. Add a route to your application that accepts requests containing user inputted text and responds with the chatbot's response.

## Submission
To submit your project, you must make your git repository available using a git hosting service like [GitHub](https://github.com/) or [GitLab](https://gitlab.com).
Use the [submission link on the course webpage](https://data-representation.github.io/#submit-ca) to submit the URL for your hosted repository.
You can submit at any time before the deadline, the earlier the better.
The last commit you make to the repository before the deadline will be corrected irrespective of when you submitted your link.
If your repository is private you must add the lecturer as a collaborator.

## Grading 
Your project will be graded using the following rubric.

| Category      | Description                            | Poor | Fair | Average | Excellent | Distinct |
|---------------|----------------------------------------|------|------|---------|-----------|----------|
| Research      | Investigation of problem and solutions | | | | | |
| Development   | Architecture and code                  | | | | | |
| Consistency   | Planning and pragmatism                | | | | | |
| Documentation | Descriptions and explanations          | | | | | |
