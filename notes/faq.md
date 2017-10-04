# Frequently Asked Questions
This document will be updated with common questions from students.
Please review the document regularly throughout the semester.


## Should I have one repository for each program?
In industry, it's generally best practice to have one application or library per repository.
However, in college, I would use one repository containing all my small programs when learning a language.
It seems more convenient, both for you and for anyone who wants to view your work in the future.

In general, you must ask yourself what is the purpose of the task at hand and make the decision on "many small projects versus one big project" based on that.
This is a key architectural decision that every software developer must make when embarking on a new assignment.

The only caveat I would add is to follow any instructions given in a problem sheet or project specification - they usually specify how to organise your code.


## Where can I find the past exam papers?
They are available on the course homepage.

## How do I read Go's package documentation?
Go's standard library is [documented on Go's documentation website (golang.org/pkg)](https://golang.org/pkg/).
For instance, the documentation for the `math/big` package is [here](https://golang.org/pkg/math/big/).

At the top of the page is the import statement: `import "math/big"`.
Remember to prefix every call to the library `big`.
Further down it suggests how to create a new big integer:

```go
var z1 Int
z1.SetUint64(123)
```

The `Int` must be prefixed with `big` so the first line becomes `var z1 big.Int`.
If that's a bit confusing, you can always use Google.
When I google "go big integer" I immediately find two relevant Stack Overflow posts:

- [Using big integer values in Go? (ParseInt only converts up to “2147483647”?)
](https://stackoverflow.com/questions/10747411/using-big-integer-values-in-go-parseint-only-converts-up-to-2147483647)
- [Golang, math/big: what is the max value of *big.Int](https://stackoverflow.com/questions/17564335/golang-math-big-what-is-the-max-value-of-big-int)

Reading through these I notice following:
```go
ten := big.NewInt(10)
bi := big.NewInt(0)
```

So, I run a quick test in Go:

```go
package main

import (
  "fmt"
  "math/big"
)


func main() {
  a := big.NewInt(123)
  b := big.NewInt(456)
  
  fmt.Println(a, b)
}
```

The program compiles and I see the intended output on the console.
Great, now I'll try to add the two big integers.

```go
  a := big.NewInt(123)
  b := big.NewInt(456)
  c := big.NewInt(456)

  c = a + b

  fmt.Println(a, "+", b, "=", c)
```

The program fails to compile:

```sh
.\bigint.go:14: invalid operation: a + b (operator + not defined on pointer)
```

It looks like I can't just write `a + b`.
Back in the documentation page I see `c.Add(a, b)` and it says that this will store the value of `a+b` in `c`.
I then try the following:

```go
package main

import (
  "fmt"
  "math/big"
)


func main() {
  a := big.NewInt(123)
  b := big.NewInt(456)
  c := big.NewInt(0)
  
  c.Add(a, b)
  
  fmt.Println(a, "+", b, "=", c)
}
```

Now the program compiles and shows the expected output.
You've used the Go documentation, Google and Stack Overflow to learn the basic functionality of the `math/big` package.


## How will my continuous assessment submissions be marked?
In marking student submissions we are guided by both the [QQI Award Standard in Computing](https://www.qqi.ie/Publications/Publications/Computing%20-%20QQI%20Awards%20Standards.pdf) and GMIT's [Quality Assurance Framework](https://www.gmit.ie/general/quality-assurance-framework), among other guidelines.
Award classifications are listed in [Code of Practice 3](http://www.gmit.ie/sites/default/files/public/general/docs/3.1-student-assessment-marks-and-standards.pdf) and are summarised below.
Module grading should normally align with this.

| Classification | % Boundaries | Description                                                   |
|----------------|--------------|---------------------------------------------------------------|
| Distinction    | 70-100%      | Most respects is significantly and consistently beyond pass.  |
| Merit 1        | 60-69%       | Many respects is significantly beyond pass.                   |
| Merit 2        | 50-59%       | Some respects is significantly beyond pass.                   |
| Pass           | 40-49%       | Attains all the minimum intended programme learning outcomes. |
| Fail           | 0-39%        | Not attained all learning outcomes.                           |

A pass mark means the student attained the learning outcomes.
At level 7, the QQI document lists the following learning outcomes, among others.
These have had a large bearing on this module's learning outcomes, and your programme's learning outcomes.
> The learner will be able to:
> - integrate concepts learned across a variety of subject areas.
> - justify selections from alternatives.
> - develop innovative solutions to pragmatic situations.
> - identify and analyse criteria and specifications appropriate to specific problems and plan strategies for their solution.
> - recognise the suitability of a given solution to a problem.
> - plan and organise work.
> - monitor project work against a plan.
> - apply knowledge learned in new situations.

It's clear that, at level 7, it's not enough just to know how to solve specific, common problems.
You must be able to take a new problem, and consider and compare several possible solutions to it.
Problems that have more than one solution and/or several ways to get to that solution are generally called open-ended problems.
You will face them daily in industry.

In my experience, open-ended problems present a challenge to students in third year.
You may be more used to building up your computing skills in previous years by solving closed-ended problems where the problems are carefully defined and the solutions are certain.
However, this year you will have to attain a standard appropriate to industry and that means learning to solve open-ended problems, including making reasonable assumptions about problems and solutions.
It's not an exact science - it's one of the reasons why a mark of 70% is considered to be excellent.

As submissions will vary from student to student, I will grade them using a rubric like the example below.
A tick in the poor column would typically result in attempt marks only for that category.
A tick in the distinct column would typically result in full marks for that category.
Each category would be equally weighted.

| Category      | Description                            | Poor | Fair | Average | Excellent | Distinct |
|---------------|----------------------------------------|------|------|---------|-----------|----------|
| Research      | Investigation of problem and solutions | | | | | |
| Development   | Architecture and code                  | | | | | |
| Consistency   | Planning and pragmatism                | | | | | |
| Documentation | Descriptions and explanations          | | | | | |

It might be worth, at this stage, taking a repository that you created for another module in the past and grade yourself on this rubric to see where you think you would be.