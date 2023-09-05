# The Integer class

Ruby differentiates between whole numbers, or `Integer`s, and decimal numbers, or `Float`s.

 ```ruby
pp 7.class
pp 7.0.class
```
{: .repl #integers_vs_floats title="Integer vs Float Class" points="1"}

You can always call the method `.class` on _any_ object, ever, at any time, to ask it what class it is.

We'll learn about `Integer`s first.

##  Syntax

`Integer`s begin with a digit (0-9) and end when Ruby meets anything other than a digit (a space, a comma in an array, a closing parentheses at the end of method arguments, etc) — _except_ for underscores, which you can use as a delimiter if it helps to understand the number:

```ruby
10000000    # Is this 1 million? 100 million?
10_000_000  # Ah, it's easier to tell now.
```

##  Methods

Let's experiment with some common methods for `Integer`s:

### + - * / % ** (math)

We, of course, have the standard math methods, like the calculator language. These methods all have the same syntactic sugar that the `String` versions did, so we can say `12 + 5` rather than `12.+(5)` (thankfully).

Try each of the following in the runnable code block below:

```ruby
12 + 5
12 - 5
12 * 5
12 / 5
```

```ruby
pp 12 + 5
```
{: .repl #try_some_math title="Try some Integer math" points="1"}

Whoa! Did you get what you expected for that last one?

It turns out that the `Integer` version of division will only return another `Integer`, and so `/` only returns the whole part (like in elementary school). If you want the remainder, you have to use the `%` (called the "modulus") operator. Try this:

```ruby
pp 12 % 5
```
{: .repl #modulus title="Modulus operator" points="1"}

Another maybe unexpected thing: raising a number to a power, e.g. 3<sup>2</sup>, is not done using the `^` like in many other computing environments. Instead, use the double-star `**` operator:

```ruby
pp 3 ** 2
```
{: .repl #squared title="To the power of" points="1"}

If you divide the number of days in a regular year by the number of days in a week, what's the remainder? Use the graded code block below with the math operators you learned above to output the answer.

```ruby
# fill in these variables
days_in_year =
days_in_week = 

# do some math on 'em
pp "do some math here :)"
```
{: .repl #integer_math title="Remainder after division" }

```ruby
describe "Remainder after division" do
  it "should print '1'" do
    path = "/tmp/code.rb"

    File.foreach(path) do |line|
      if line.match(/^p.*1/)
        expect(line).to_not match(/1/),
          "Expected graded code block to NOT print the Integer literal '1', but did."
      end
    end
    
    expect { require_relative(path) }.to output(/1/).to_stdout
  end
end
```
{: .repl-test #integer_math_test_1 for="integer_math" title="Remainder after division should print '1'" points="1"}

### odd? and even? 

The `.odd?` and `.even?` methods return `true` or `false` based on whether the number is, well, odd or even. Don't be thrown off by the question mark at the end of the method name — it's nothing special, just another letter. Rubyists like to end method names with a question mark when methods return `true` or `false`.

```ruby
pp 7.odd?
pp 7.even?
```
{: .repl #odd_or_even title="odd? or even?" points="1"}

### rand 

There's another special method like `pp` that we are allowed to call "in space", i.e. not on the right side of a dot, called `rand`. It returns a random number, and is very useful for all kinds of stuff, everything from games to statistical analysis:

<aside markdown="1">
`rand` is another method defined on `Kernel`, so the longhand would be `Kernel.rand(6)`.
</aside>

```ruby
rand(6) # => returns a random integer between 0 and 5
```

Somewhat oddly, `rand(n)` will return a random integer between `0` and `n - 1` rather than between `1` and `n`. That may seem surprising, but it's actually pretty handy because a lot of times what we want to do is generate a random _offset_ and it's convenient for that to include 0 as a possibility.

Give it a try, run this runnable code block a few times and see the changing result:

```ruby
# random number between 0 and 8
pp rand(9)
```
{: .repl #rand title="rand" points="1"}

- Experiment! Trying to run `rand("9")` results in...
- `no implicit conversion of String into Integer`
    - Yes! Another error message! We'll see how to solve it below.
- A random number between 0 and 8
    - Really? Did you try it?
- `no implicit conversion of Integer into String`
    - Really? Did you try it?
{: .choose_best #rand_string title="rand with String" points="1" answer="1" }

### String#to_i 

Let's go back to `String` for a moment and talk about the method `to_i`, or `String#to_i`. (We write `Object#method` often, and we'll see in a later lesson how it is different from `Object.method`.)

Sometimes you have a `String` that contains a number, usually input from a user, and want to do math on it. `to_i` will attempt to convert a `String` object into an `Integer` object.

```ruby
first_num = "8"
second_num = "100"

# this will end in an error message
pp first_num * second_num

# but you can fix that by uncommenting this line
# pp first_num.to_i * second_num.to_i
```
{: .repl #to_i title="to_i" points="1"}

- What would you add to the previous error code `rand("9")` to get it working?
- `rand("9").to_i`
    - Not quite. Think about the order of operations
- `rand.to_i("9")`
    - No, we want the argument "9" passed to `rand` not `to_i`
- `rand(9)`
    - Technically yes, but what would we _add_ to the code that we just learned about?
- `rand("9".to_i)`
    - Yes!
{: .choose_best #rand_fixed title="rand with String, fixed" points="1" answer="4" }

The graded code block below causes an error message now. Debug it, and make it print 16:

```ruby
a_number = "4"

pp a_number ** 2
```
{: .repl #to_i_graded title="to_i, debug" readonly_lines="1"}

```ruby
describe "to_i, debug" do
  it "should print '16'" do
    path = "/tmp/code.rb"

    File.foreach(path) do |line|
      if line.match(/^p.*16/)
        expect(line).to_not match(/16/),
          "Expected graded code block to NOT print the Integer literal '16', but did."
      end
    end
    
    expect { require_relative(path) }.to output(/16/).to_stdout
  end
end
```
{: .repl-test #to_i_graded_test_1 for="to_i_graded" title="to_i, debug should print '16'" points="1"}

### to_s 

Okay, back to `Integer` methods. We often will want to combine our `Integer`s with `String`s when crafting output for our users. Give it a try:

```ruby
lucky_number = rand(100)
pp "Your lucky number is " + lucky_number
```
{: .repl #to_s title="to_s" points="1"}

Uh oh! [RTEM](https://learn.firstdraft.com/lessons/67#seriously-please-read-the-error-message)!

It turns out that `String`'s `+` method can only add two strings together, not a string and an object of some other class. So, a lot of times we'll need to convert an `Integer` into a `String` prior to output. Fortunately `Integer` has a handy method, `to_s` (or "to string"), that does just that:

```ruby
lucky_number = rand(100)
pp "Your lucky number is " + lucky_number.to_s
```
{: .repl #to_s_fixed title="to_s fixed" points="1"}

Complete this graded code block test using the `to_s` methods. It should print "The 29th is my 32nd birthday":

```ruby
the_day = 29
the_age = 32

pp "The " + the_day + "th is my " + the_age + "nd birthday"
```
{: .repl #debug_to_s title="Debug with to_s"}

```ruby
describe "Debug with to_s" do
  it "should print 'The 29th is my 32nd birthday'" do
    path = "/tmp/code.rb"

    File.foreach(path) do |line|
      if line.match(/^p.*"The 29th is my 32nd birthday"/)
        expect(line).to_not match(/The 29th is my 32nd birthday/),
          "Expected graded code block to NOT print the Integer literal 'The 29th is my 32nd birthday', but did."
      end
    end
    
    expect { require_relative(path) }.to output(/The 29th is my 32nd birthday/).to_stdout
  end
end
```
{: .repl-test #debug_to_s_test_1 for="debug_to_s" title="Debug with to_s should print 'The 29th is my 32nd birthday'" points="1"}

### to_f 

Similar to `to_i`, there's a `to_f` (or "to float") method to convert an `Integer` to a `Float`, which is often handy for doing math, as we'll see next.

##  Conclusion

That's it for `Integer`. Next up, its close cousin: `Float`.

- Approximately how long (in minutes) did this lesson take you to complete?
{: .free_text_number #time_taken title="Time taken" points="1" answer="any" }

<span style="font-size: large">**When you are done here, close the window and return to Canvas for the next lesson in the series.**</span>

----
