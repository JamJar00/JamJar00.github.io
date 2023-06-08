---
layout: post
title: Pyash (and why I can't have nice things anymore)
tags: python, bash, showdev
---

A few months back an innocent thought came into my mind:

> Can you make Python run this bash statement...?
>
> `cat "kittens.txt" | grep "fluff" > fluffy_kittens.txt`

Yes. Well nearly. But, mostly yes.

## An Adventure Begins
The Saturday after my innocent thought was spent trying to get as close as possible to making Python run the above line.

Most users probably see `cat "kittens.txt"` and think it isn't in the slightest syntactically correct and would be completely impossible to get to compile. You would be right actually. I never worked this one out.

So I lowered my goal a little:
```python
cat("kittens.txt") | grep("fluff") > fluffy_kittens.txt
```

This is a little more reasonable! Now we have simple, Pythonic function calls surrounded by errors!

Let's start.

The first thing to do is to make `cat` and `grep` functions that we can call here, since we're obviously making function calls. I really wanted the ability to run *any* executable on my PC, just like you can in bash, because I really like that aspect of shell scripting, so I wrote something along the lines of this:
```python
import os, glob

for path in os.environ["PATH"].split(";"):
    for executable_path in glob.glob(path + "\\*.exe"):
        name = os.path.splitext(os.path.basename(executable_path))[0]

        globals()[name] = lambda *args, e=executable_path: os.system("\"" + e + "\" " + " ".join(args))
```
(I work on a Windows machine with Git Bash, hence the search for `.exe` files)

It's janky, but this chunk of Python essentially searches through each path in the `PATH` variable for executables and creates a lambda function that runs the executable when called. It then adds that to the dictionary of globals so that you can now do things like:
```python
mv("kittens.txt", "bed.txt")
```
Or even:
```python
python("world-domination.py")
```
Great!

And it was at this point I realised that the `print` function had been overwritten to invoke `print.exe` instead...

*Sigh* :unamused:

## Frankenstein's Python
This is a great start. At this point I actually moved that code into a library of its own so that you can import the functions individually (it helped with the whole `print` issue!)
```python
from shell import cat, grep, kubectl # not you print
```

Let's look at a harder example:
```python
ps() > "processes.txt"
```
In bash, this would take the output of the `ps` command and put it in the specified text file.

You'll notice that I've enclosed the name of the file in quotes, it's still valid bash so it doesn't count as cheating... :eyes:

This isn't so hard. In normal Python, this just gets evaluated as: *is the result of `ps()` greater than the string "processes.txt"? True or False?* If we return a custom object from `ps()` we can then *overload* the greater than operator to be able to compare itself with strings. (*Overloading* operators is essentially just a fancy term for redefining them with a custom implementation, so Python will use our implemention for performing greater than comparisons rather than its normal one.)

Here's a simple class we can return from `ps()` that does it:
```python
class EvaluationResult:
    def __init__(self, output):
        self._output = output

    def __gt__(self, target):
        with open(target, 'w') as f:
            f.write(self._output)
```
In this case we've overridden the greater than operator to write to a file of the name taken from the string it's comparing against.

I won't show my full code, because at this point I switched to using `subprocess.Popen` instead of `os.system` to run the executables, and that's a whole other ropic I don't want to explain here.

Let's throw in a different example:
```python
print(ps())
```
Here we want to output the result of `ps` to the console but with the above code you'll just get the memory address of our `EvaluationResult` class:
```
<__main__.EvaluationResult object at 0x03783030>
```

An easy fix, we just need to overload `__str__`:
```python
class EvaluationResult:
    def __init__(self, output):
        self._output = output

    def __gt__(self, target):
        with open(target, 'w') as f:
            f.write(self._output)

    def __str__(self, target):
        with open(target, 'w') as f:
            f.write(self._output)
```
(`__str__` is called when Python wants a string representation of an object, you can override this to provide better visibility into your classes while debuging if you like!)

So how about our original example then?
```python
cat("kittens.txt") | grep("fluff") > fluffy_kittens.txt
```
The pipe operator (`|`, normally or) is also available to overload in Python, so all we need to do is overload it to pass the output of our first executable to the input of the second.

Unfortunately, Python, like most languages, executes `cat("kittens.txt")` first, then executes `grep("fluff")`, and then finally executes the or. By the time we get to our overloaded or operator we'll have already executed the second command and wont be able to pass it the output from the first (in actual fact, `grep` will have hung waiting for input, so the or overload we write won't ever be called here anyway, *doh*).

The only solution I came up with for this was to lazily run the executables underneath the functions, that is to say that when you run `cat("kittens.txt")` it doesn't actually run the `cat` executable until the or gets evaluated later, at which point we know where it's output goes.

```python
class LazyInvocation:
    def __init__(self, output):
        self._output = output

    def __gt__(self, target):
        self.run()

        with open(target, 'w') as f:
            f.write(self._output)

    def __or__(self, target):
        self.run()
        target._input = self._output

    def run(self):
        # actually run the command, passing self._input in and assigning self._output
```

It makes everything a little wierd to run now though, because:
```python
ps()
```
Won't actually run `ps` anymore...

## Pyash
By the end of my Saturday, I had a nice set of bash syntax implemented!
```python
from pyash import cat, grep, rm

cat("animals.txt") | grep("cat") | grep("-v", "caterpillar") > "nice_things.txt"
find(".", "-name", "*sunset*") >> "nice_things.txt"

print(nc("localhost", "22") < "nice_things.txt")

rm("nice_things.txt")
```

So I present to you my first PyPI repository, Pyash!

{% github JamJar00/pyash %}
('pyash' being 'python-bash', if you didn't get that... :grimacing:)

I don't recommend you use it; I can't imagine your coworkers would be best pleased with you using it either, and frankly it fails a number of PEP-8 style guidelines. However, it does go to show how much you can abuse Python and what you can do with a little creativity!

It's not nice, nor is it a good idea and I feel like I've made a very beautiful language into a monster. This is why I can't have nice things.


I hope you've enjoyed this story, if you have any of your own thoughts of things that could be added to Pyash or ways in which it can abuse Python further please do comment below and/or give them a go yourself!
