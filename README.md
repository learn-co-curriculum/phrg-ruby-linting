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

Like `rspec`, `rubocop` is a command line tool that runs some checks against your code. It can be invoked by simply typing `rubocop`, or passing it a path to select a smaller group of files: `rubocop lib/cats/`. Rubocop will lint all the ruby files contained in the directory you run it for. Let's use `rubocop` on this `example.rb` file.

```ruby
def foo

:foo
end

def bar(n)
  1
end

def quux
  bar(50)
 end

```

What does `rubocop` think?

```
[Fri May 25 05:35:10]$ rubocop
Inspecting 1 file
W

Offenses:

example.rb:2:1: C: Layout/EmptyLinesAroundMethodBody: Extra empty line detected at method body beginning.
example.rb:3:1: C: Layout/IndentationWidth: Use 2 (not 0) spaces for indentation.
:foo

example.rb:6:9: W: Lint/UnusedMethodArgument: Unused method argument - n. If it's necessary, use _ or _n as an argument name to indicate that it won't be used. You can also write as bar(*) if you want the method to accept any arguments but don't care about them.
def bar(n)
        ^
example.rb:6:9: C: Naming/UncommunicativeMethodParamName: Method parameter must be at least 3 characters long.
def bar(n)
        ^
example.rb:12:2: W: Layout/DefEndAlignment: end at 12, 1 is not aligned with def at 10, 0.
 end
 ^^^

1 file inspected, 5 offenses detected

[Last command returned error 1]
```

Let's break down this output one line at a time, starting at the very bottom:

---

```
[Last command returned error 1]
```

Since there were style offenses in `example.rb`, the `rubocop` shell command exit with an error 1. Should we have had no offenses, then would would not have seen this message.

---

```
1 file inspected, 5 offenses detected
```

`rubocop` scanned one Ruby file, and found 5 places that broke it's default style guidelines.

---

```
example.rb:12:2: W: Layout/DefEndAlignment: end at 12, 1 is not aligned with def at 10, 0.
 end
 ^^^
```

In the `example.rb` file, on line `12`, on character `2`, there is an offense.

The offense is in called `Layout/DefEndAlignment` where `Layout` is the rubocop department, and `DefEndAlignment` is the name of the rule. Rubocop refers to its rules as "cops".

The message for this offense reads `end at 12, 1 is not aligned with def at 10, 0.`. So what does this mean? This message points out that the position of the line of code with an `end` is not aligned with its corresponding `def`. It even goes as far as reporting the exact position of the `def`.

Below the message, `rubocop` using upticks to highlight the position of the offense in the line. In this case, its the entire `end`.

To fix this, we must delete the extra whitespace in front of end, like this:

```ruby
def quux
  bar(50)
end
```

Now let's run `rubocop` again:

```
[Fri May 25 05:36:12]$ rubocop
Inspecting 1 file
W

Offenses:

example.rb:2:1: C: Layout/EmptyLinesAroundMethodBody: Extra empty line detected at method body beginning.
example.rb:3:1: C: Layout/IndentationWidth: Use 2 (not 0) spaces for indentation.
:foo

example.rb:6:9: W: Lint/UnusedMethodArgument: Unused method argument - n. If it's necessary, use _ or _n as an argument name to indicate that it won't be used. You can also write as bar(*) if you want the method to accept any arguments but don't care about them.
def bar(n)
        ^
example.rb:6:9: C: Naming/UncommunicativeMethodParamName: Method parameter must be at least 3 characters long.
def bar(n)
        ^

1 file inspected, 4 offenses detected

[Last command returned error 1]
```

Great! We are down to 5.

---

Besides the descriptive error messages, another nice thing about rubocop is that its "cops" are well documented. If you ever are confused about what the offense is describing, you can just drop the name of the cop and `rubocop` into Google, and the first hit will provide some additional documentation and good and bad examples. Here is the [doc page on Layout/DefEndAlignment](https://www.rubydoc.info/gems/rubocop/RuboCop/Cop/Layout/DefEndAlignment).

---

```
example.rb:6:9: C: Naming/UncommunicativeMethodParamName: Method parameter must be at least 3 characters long.
def bar(n)
        ^
```

In the `example.rb` file, on line `6`, on character `9`, there is an offense.

The offense is called `Naming/UncommunicativeMethodParamName` where `Naming` is the department, and `UncommunicativeMethodParamName` is the name of the cop.

The message reads `Method parameter must be at least 3 characters long.`

The position of the offense is highlighted with upticks like so:

```ruby
def bar(n)
        ^
```

Ok. So we can see that the `bar` method is invoked insdie the `quux` method. And the argument it passes `bar` id a number. So let us rename the `n` variable to `number`, which is more communicative.

Like so:

```ruby
def bar(num)
```

And run `rubocop` again.

```
[Fri May 25 05:46:49]$ rubocop
Inspecting 1 file
W

Offenses:

example.rb:2:1: C: Layout/EmptyLinesAroundMethodBody: Extra empty line detected at method body beginning.
example.rb:3:1: C: Layout/IndentationWidth: Use 2 (not 0) spaces for indentation.
:foo

example.rb:6:9: W: Lint/UnusedMethodArgument: Unused method argument - num. If it's necessary, use _ or _num as an argument name to indicate that it won't be used. You can also write as bar(*) if you want the method to accept any arguments but don't care about them.
def bar(num)
        ^^^

1 file inspected, 3 offenses detected
```

Down to 3!

---

```
example.rb:6:9: W: Lint/UnusedMethodArgument: Unused method argument - num. If it's necessary, use _ or _num as an argument name to indicate that it won't be used. You can also write as bar(*) if you want the method to accept any arguments but don't care about them.
def bar(num)
        ^^^
```

In the `example.rb` file, on line `6`, on character `9`, there is an offense.

The offense is called `Lint/UnusedMethodArgument` where `Lint` is the department, and `UnusedMethodArgument` is the name of the cop.

The message reads `Unused method argument - num. If it's necessary, use _ or _num as an argument name to indicate that it won't be used. You can also write as bar(*) if you want the method to accept any arguments but don't care about them.`

The position of the offense is highlighted with upticks like so:

```ruby
def bar(num)
        ^^^
```

Wait, didn't we just fix this line?

Well, no. It's possible for more than one offense to be on one line of code. And this message points out that we never actually use our argument in the bar method. We should definitely be using all the arguments that we define in a method. And oh yeah!, `num` should be getting added to 1. Like so:

```ruby
def bar(num)
  num + 1
end
```

Let's run `rubocop` again.

```
[Fri May 25 05:57:41]$ rubocop
Inspecting 1 file
C

Offenses:

example.rb:2:1: C: Layout/EmptyLinesAroundMethodBody: Extra empty line detected at method body beginning.
example.rb:3:1: C: Layout/IndentationWidth: Use 2 (not 0) spaces for indentation.
:foo


1 file inspected, 2 offenses detected

[Last command returned error 1]
```

---

Break down the last two rubocop offenses piece by piece:

```
example.rb:3:1: C: Layout/IndentationWidth: Use 2 (not 0) spaces for indentation.
:foo
```

```
example.rb:2:1: C: Layout/EmptyLinesAroundMethodBody: Extra empty line detected at method body beginning.
```

Our final product should look like

```ruby
def foo
  :foo
end

def bar(num)
  num + 1
end

def quux
  bar(50)
end

```

And running `rubocop` returns:

```
[Fri May 25 06:25:47]$ rubocop
Inspecting 1 file
.

1 file inspected, no offenses detected
```

## Configuring Sublime

Next let's configure Sublime to highlight rubocop offenses in our test editor. Navigate to `Preferences > Package Control`. Start typing "Install Packages" and select it from the dropdown. Then type "rubocop" and select the plugin. Finish by restarting Sublime.

The original offending code example will now like this:

`insert pic here`

Notice that your text editor highlights "offending" Ruby style offenses with red underlines, pipes, and squares. As we move forward, this will help steer us towards using consistent and clear syntax.

## Rubocop in Nitro

Out of the box, `rubocop` enforces all of its default configurations. However, every cop in `rubocop` has the ability to either be turned off, or modified to enforce a different coding style. That is because, for the most part, the important take away from using a linter is that everyone adheres to one style.

Nitro mostly uses all of `rubocop`'s default configurations. So understanding these defaults, what they mean, and how to fix your style offenses is more than enough to consider as you continue your studies as a Student Developer at Power.

## Linting in future Flatiron Labs

The Flatiron labs themselves do not adhere to consistent style practices. You will notice multiple offenses in code from here on out. There are many reasons for this. Here are a few:

1. The curriculum wants to expose you to various syntaxes and styles.
1. The curriculum has been written over a number of years by different individuals that adhere to different practices and habits.
1. The curriculum was written in various versions of Ruby. Many "better" practices where not recognized until recently.
1. The labs in the curriculum are small projects. Using consistent style is of the greatest benefit to projects that employ multiple developers. Using a linter becomes more important as the scope of a project grows.

## Resources

* [Rubocop Docs](https://rubocop.readthedocs.io/en/latest/)

* [Rubocop Repository](https://github.com/bbatsov/rubocop)
