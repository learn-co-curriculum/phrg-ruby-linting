# Linting code

![Lint](https://raw.githubusercontent.com/powerhome/phrg-ruby-linting/master/cotton-linters.png?raw=true "Lint")

## Linters

In techonology, a `linter` refers to a tool that analyzes source code to flag programming errors, bugs, stylistic errors, and suspicious constructs. The analysis performed by lint tools can also serve to inform developers of better coding conventions that may improve security or optimize performance (generate faster executing code).

Every programming language has its own linting libraries. Nitro is no exception. The library we use for linting Ruby is called [`Rubocop`](https://rubocop.readthedocs.io/en/latest/). And the library we use for linting Javascript is called [`ESLint`](https://eslint.org/).

## Code Style

Code style can be boiled down to anything that is a stylistic choice that has little to no affect on code's behavior. An infamous example of a stylistic code choice is using tabs versus spaces for indentation. Another Ruby specific example is using the `map` vs. the `collect` enumerator. These differences have no effect on our code's behavior, but will cause slight inconsistency with how the code reads.

Isn’t it enough that our code works? Well, there are a couple of reasons to care about our code styles. The first is consistency. A large codebase with multiple team members strives to make that codebase look as if it was written by one author. Once a team agrees on a given style, it helps to keep the code looking similar in every file. This also helps keep source control changes to a minimum.

For example, Terry may like to use double quotes but Briana may prefer to use single quotes. When Terry modifies updates quotes to his liking and pushes them to Github, it makes it harder for another developer to understand and review the more important changes in his pull request. Deciding on one way to use quotes is not really about preference, but about minimizing changes and superfluous code style discussions amongst the team. There are more important issues to discuss.

## Install Rubocop

![Rubocop Logo](https://raw.githubusercontent.com/powerhome/phrg-ruby-linting/master/rubo-logo-horizontal.png?raw=true "Rubocop Logo")

The Nitro codebase uses most of the default configurations that come with Rubocop. To get started, installing these two `rubocop` gems to your laptop with the following commands:

```
gem install rubocop
```

```
gem install rubocop-performance
```

## Using Rubocop

Like `rspec`, `rubocop` is a command line tool that runs some checks against your code. It can be invoked by simply typing `rubocop`, or passing it a path to select a smaller group of files. For example, `rubocop lib/cats/tabbies/`. Rubocop will lint all the ruby files contained in the directory you run it for. Let's use `rubocop` on the code below. This code is contained in a file named `example.rb`.

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

Since there were style offenses in `example.rb`, the `rubocop` shell command exit with an error 1. Should we have had no offenses, then you would not have seen this message.

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

In the `example.rb` file, in line `12`, on character `2`, there is an offense.

The offense is in called `Layout/DefEndAlignment` where `Layout` is the rubocop department, and `DefEndAlignment` is the name of the rule. Rubocop refers to its rules as "cops".

The message for this offense reads `end at 12, 1 is not aligned with def at 10, 0.`. So what does this mean? This message points out that a method's `end` position is not aligned with its corresponding `def`. It even goes as far as reporting the exact position of both the `end` and the `def`.

Below the message, `rubocop` uses upticks to highlight the position of the offense in the line. In this case, its the entire `end`.

```ruby
 end
 ^^^
```

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

Great! We are down to 4.

---

Besides the descriptive error messages, another nice element about using rubocop is that it is well documented. When you are confused about what the offense is describing, just drop the name of the cop and "rubocop" into Google, and the first hit will provide some additional explanation with good and bad examples. Here is the [doc page on Layout/DefEndAlignment](https://www.rubydoc.info/gems/rubocop/RuboCop/Cop/Layout/DefEndAlignment).

---

```
example.rb:6:9: C: Naming/UncommunicativeMethodParamName: Method parameter must be at least 3 characters long.
def bar(n)
        ^
```

In the `example.rb` file, in line `6`, on character `9`, there is an offense.

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

In the `example.rb` file, in line `6`, on character `9`, there is an offense.

The offense is called `Lint/UnusedMethodArgument` where `Lint` is the department, and `UnusedMethodArgument` is the name of the cop.

The message reads `Unused method argument - num. If it's necessary, use _ or _num as an argument name to indicate that it won't be used. You can also write as bar(*) if you want the method to accept any arguments but don't care about them.`

The position of the offense is highlighted with upticks like so:

```ruby
def bar(num)
        ^^^
```

Wait, didn't we just fix this line?

Well, no. It's possible for more than one offense to be on one line of code. And this message points out that we never actually use our argument in the bar method. We should definitely be using all the arguments that we define in a method. And oh yeah!, `num` and `1` should be getting added together. Like so:

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

Break down the last two rubocop offenses piece by piece like we have done above.

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

:tada:

## Configuring VSCode

Next, let's configure VSCode to highjlight rubocop offenses in our text editor.

Click on the "Extensions" left sidebar icon. Then type "rubocop" into the search bar and click "Install" for the `ruby-rubocop` extension.

![VSCode Rubocop](https://raw.githubusercontent.com/powerhome/phrg-ruby-linting/master/VSCode-Rubocop-Extension.png?raw=true "VSCode Rubocop")

The original offending code example will now like this:

![Rubocop Highlighting](https://raw.githubusercontent.com/powerhome/phrg-ruby-linting/master/rubocop-vscode-highlighting.png?raw=true "Rubocop Highlighting")

Notice that your text editor highlights "offending" Ruby style offenses with underlines. To learn about what offense has been detected, hover your mouse over an underlined fragment of code. As we move forward, this will help steer us towards using consistent and clear syntax.

## Rubocop in Nitro

Out of the box, `rubocop` enforces all of its default configurations. However, every cop has the ability to either be turned off, on, or modified to enforce a different style. That is because, for the most part, the important take away from using a linter is that everyone adheres to one style.

Nitro mostly uses `rubocop`'s default configurations. So understanding these defaults, what they mean, and how to fix your style offenses is enough to consider as you continue your studies as a `Student Developer` at Power Home Remodeling Group.

## Linting in Flatiron Labs

The Flatiron labs themselves do not adhere to consistent style practices. You will notice multiple offenses in code from here on out. There are many reasons for this. Here are a few:

1. The curriculum wants to expose you to various syntaxes and styles.
1. The curriculum has been written over a number of years by different individuals that adhere to different practices and habits.
1. The curriculum was written in various versions of Ruby. Many "better" practices where not recognized until recently.
1. The labs in the curriculum are small projects. Using consistent style is of the greatest benefit to projects that employ multiple developers. Using a linter becomes more important as the scope of a project grows.

## Apply Nitro linting conventions

Instead of just using `rubocop` defaults, it makes more sense for us to get used to Nitro's Ruby linting standards. We can accomplish this by copying a generic version of these configurations to our laptops. To do so, run this command:

```bash
curl "https://raw.githubusercontent.com/powerhome/phrg-ruby-linting/master/.rubocop.yml" -o "$HOME/.rubocop.yml"
```

## Resources

* [Rubocop Docs](https://docs.rubocop.org/en/latest/)

* [Rubocop Repository](https://github.com/rubocop-hq/rubocop)
