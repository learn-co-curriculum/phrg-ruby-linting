# Linting code

![Lint](https://raw.githubusercontent.com/powerhome/phrg-ruby-linting/master/cotton-linters.png?raw=true "Lint")

## Linters

In techonology, a `linter` refers to a tool that analyzes source code to flag programming errors, bugs, stylistic errors, and suspicious constructs. The analysis performed by lint tools can also serve to inform developers of better coding conventions that may optimize performance (generate faster executing code) or improve security.

Every programming language has its own linting libraries. Nitro is no exception. The library we use for linting Javascript is called [`ESLint`](https://eslint.org/). And the library we use for linting Ruby is called [`Rubocop`](https://rubocop.readthedocs.io/en/latest/).

## Code Style

Code style can be boiled down to anything that is a stylistic choice in the code that has little to no affect on its behavior. An infamous example of a stylistic code choice is using tabs versus spaces. Another Ruby specific example is using `map` vs. `collect`. These differences have no effect on our code's behavior but will cause slight inconsistency with how the code reads.

Isn’t it enough that our code works? Well, there are a couple of reasons to care about our code styles. The first is consistency. A large codebase with multiple team members strives to make that codebase look as if it was written by just one author. When a team agrees on a given style, it will help keep the code looking similar file to file. When everyone follows the same code conventions, this also helps keep source control changes to a minimum. For example, Terry may like using double quotes but Briana may prefer using single quotes. But changing quotes style in a pull request causes unneeded confusion when another developer reviews the changes on Github. Deciding on one way to use quotes is not really about preference, but about writing clean and consistent code.

## Install Rubocop

![Rubocop Logo](https://raw.githubusercontent.com/powerhome/phrg-ruby-linting/master/rubo-logo-horizontal.png?raw=true "Rubocop Logo")

The Nitro codebase adheres to most of the default configurations that come with using Rubocop. Start by installing this gem to your laptop:

```
gem install rubocop
```

## Using Rubocop

Like `rspec`, `rubocop` is a command line tool that runs some checks against your code. It can be invoked by simply typing `rubocop`, or passing it a path to select a smaller group of files: `rubocop lib/cats/`. Rubocop will lint all the ruby files contained in the directory you run it for. Let's use `rubocop` on this `foo.rb` file.

```ruby
class Foo

  def bar(p)
    1
    end

end

```

What does `rubocop` think?

```
[Tue May 22 08:25:46]$ rubocop
Inspecting 1 file
W

Offenses:

foo.rb:1:1: C: Style/Documentation: Missing top-level class documentation comment.
class Foo
^^^^^
foo.rb:2:1: C: Layout/EmptyLinesAroundClassBody: Extra empty line detected at class body beginning.
foo.rb:3:11: W: Lint/UnusedMethodArgument: Unused method argument - p. If it's necessary, use _ or _p as an argument name to indicate that it won't be used. You can also write as bar(*) if you want the method to accept any arguments but don't care about them.
  def bar(p)
          ^
foo.rb:3:11: C: Naming/UncommunicativeMethodParamName: Method parameter must be at least 3 characters long.
  def bar(p)
          ^
foo.rb:5:5: W: Layout/DefEndAlignment: end at 5, 4 is not aligned with def at 3, 2.
    end
    ^^^
foo.rb:6:1: C: Layout/EmptyLinesAroundClassBody: Extra empty line detected at class body end.

1 file inspected, 6 offenses detected

[Last command returned error 1]
```

Let's break down this output one line at a time, starting at the very bottom:

---

```
[Last command returned error 1]
```

Since there were style offenses in `foo.rb`, the `rubocop` shell command exit with an error 1. Should we have had no offenses, then would would not have seen this message.

---

```
1 file inspected, 6 offenses detected
```

`rubocop` scanned one Ruby file, and found 6 places that broke it's default style guidelines.

---

```
foo.rb:6:1: C: Layout/EmptyLinesAroundClassBody: Extra empty line detected at class body end.
```

In the `foo.rb` file, on line `6`, on index `1`, there is an offense.


* Solving (indention) violation
* Researching offenses

## Configuring Sublime

Next let's configure Sublime to highlight rubocop offenses in our test editor. Navigate to Preferences > Package Control. Start typing "Install Packages" and selecting it from the dropdown. Then type "rubocop" and select the plugin. Finish by restarting Sublime.

* Example of highlighting in text editor


## Linting in future Flatiron Labs

* They are inconsistent.
