# Linting code

![Lint](https://raw.githubusercontent.com/powerhome/phrg-ruby-linting/master/cotton-linters.png?raw=true "Lint")

## Code Style

Code style can be boiled down to anything that is a stylistic choice in the code that has little to no affect on its behavior. An infamous example of a stylistic code choice is using tabs versus spaces. Another Ruby specific example is using `map` vs. `collect`. These differences have no effect on our code's behavior but will cause slight inconsistency with how the code reads.

Isn’t it enough that our code works? Well, there are a couple of reasons to care about our code styles. The first is consistency. A large codebase with multiple team members strives to make that codebase look as if it was written by just one author. When a team agrees on a given style, it will help keep the code looking similar file to file. When everyone follows the same code conventions, this also helps keep source control changes to a minimum. For example, Terry may like double quotes but Briana may like single quotes. Changing quotes style in a Pull Request will cause unneeded confusion when another developer reviews the changes on Github. The is not really about preference, but about writing clean and consistent code.

## Linters

In techonology, a `linter` refers to a tool that analyzes source code to flag programming errors, bugs, stylistic errors, and suspicious constructs. The analysis performed by lint tools can also serve to teach better coding conventions that optimize performance (generate faster executing code) or improve security.

Every programming language has its own linting libraries. Nitro is no exception. The library we use for linting Javascript is called [`ESLint`](https://eslint.org/). And the library we use for linting Ruby is called [`Rubocop`](https://rubocop.readthedocs.io/en/latest/).

## Install Rubocop

![Rubocop Logo](https://raw.githubusercontent.com/powerhome/phrg-ruby-linting/master/rubo-logo-horizontal.png?raw=true "Rubocop Logo")

```
gem install rubocop
```

## Configure in Sublime

## Using Rubocop

* Example of highlighting in text editor
* Running command
* Solving (indention) violation
* Researching offenses

## Linting in future Flatiron Labs

* They are inconsistent.
