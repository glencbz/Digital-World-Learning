So you have a Python problem
===

😪 Foreword
---
This reference is intended to be an introduction to some of tips and tricks in Python with regards to technical jargon and common code patterns. I know that documentation can be extremely difficult to understand if you don't come from a programming background, so hopefully this cheatsheet cuts through some of that difficulty.

If possible, I wish that students avoid using this as a kind of substitute for StackOverflow and official documentation (especially those intending to do a lot of code in the future) because frankly, it is an extremely important skill to be able to look up you problems and solve them yourself because the real world isn't nearly so nice (but TAs can afford to be nice).

All said and done, this reference is meant for you guys. You know yourselves the best so do try to use it in the way that helps you the most.

💡Things you should know
---

**`return` vs `break` vs `continue`**

These keywords are a source of a lot of confusion among students, partially because they may not have the most intuitive names, but in summary:

- `return` returns to where the program was before this function was called
- `break` breaks out of a loop
- `continue` continues to the *next* iteration of the loop without running the rest of the loop code

`return` is used exclusively by functions. When you `return`, the function stops immediately, regardless of whether you have a loop or whatnot. It's named as such because normally a program is executed line by line, but when you perform a function call, you jump to another line. When that function ends, you need to `return` to wherever you were before you called the function. `return` is extremely important because it's used to determine the value of the function. Think of a function in terms of math, where f(x) has a value based on x. That value is what is `return`ed by the function.

```
def add1(x):
  return x + 1

print add1(1) #prints 2
```

`break` and `continue` on the other hand are used exclusively by loops. You can think of `break` as like a `return` for loops (or vice-versa) because `break` will stop the loop completely. On the other hand, `continue` does not stop the loop. `continue` will cause the loop to go to the next iteration without executing any more lines. Try running the example below to see if you understand.

```
i = 0
while True:
  i += 1
  if i >= 10:
    break
  elif i == 5:
    continue
  print i
```

A question that some students ask is whether or not they should terminate a loop with `break` or `return`. Basically, as long as you eventually return the same value, it doesn't really matter. Using `return` might save you a line and `break` might make debugging a little easier, but generally you won't notice the difference.

**Backslashes(\\) and escape sequences**

Modern programming languages usually use \\ as a special character. \\ is used to represent certain useful characters that you can't type on a keyboard (remember how \\n is a newline)? This combination of \\ and another character is known as an escape sequence and is treated differently. If you want to type a literal backslash, just type two of them back to back `"\\"`. This is known as escaping the backslash. **Special note to Windows users:** Windows usually uses \\ in its file paths. So if you want to type backslashes in your path name, always remember to escape them.

In fact, to represent a single of these bad boys, I've been typing two backslashes like so \\\\ (which of course is 4 of them). See https://xkcd.com/1638/

**Iterable**

An object capable of returning its members one at a time. Common examples are strings, lists, tuples and dictionaries. For loops use iterables by cycling through the elements one by one.

E.g.
`for digit in ["1", "2", "3"]:` and `for digit in "123":` will achieve the same effect. The for loop will iterate over the right hand side, assigning the values to the variable `digit`.

**Callable**

An object that can be called like a function. Recall that you call a function, say `my_function` by using parentheses, like such: `my_function()`. You may see the word callable in some error messages, so make sure you aren't mistaking some variable for a function.

💡Good practices
---

**`while` vs `for` loops**

Where possible, try to avoid `while` loops. `while` loops aren't necessary for a huge majority of the problems you will come across and mistakes can be made very easily.

To understand why `while` loops are not really necessary, consider the structure of both kinds of loops.
- `for` loops work on an iterable, thus they do something a fixed number of times
- 'while' loops operate until a condition is not met, thus they can run an infinite amount of times

If we want to print all of the indices in a list for example, we can do this using either a `while` loop or a `for` loop:
```
for i in range(len(my_list)):
  print i
```
vs
```
i = 0
while i < len(my_list):
  print i
  i += 1
```
Yes, both of these loops accomplish the same effect, but look at the number of lines you have to use for the `while` loop. What if you forget to increment `i`? Then you've got an infinite loop and you need to find the right place to do stuff. Make `for` loops a habit.

**When to iterate over indices vs values**

Generally, when you're working with one list, values will suffice. It's easier to do and when you reread your code, it will be pretty obvious what you're trying to do.

```
for i in range(len(my_list)):
  print my_list[i]
```
vs
```
for ele in my_list:
  print ele
```

If what you want to do is to iterate over two lists simultaneously though (say, to calculate the pairwise sum), you can't do that using just values because you need 1 index to refer to two lists at the same time

```
for i in range(len(my_list)):
  print my_list[i] + your_list[i] #you can't do this without the indices
```

💡Useful snippets
---
###Strings

**`STRING.join(ITERABLE)`**

Takes the values in the iterable and joins them into a single string. These values will be separated by the left hand side STRING. Useful if you have a list but you need to return a string. Take note that the values in the iterable have to be strings, if they aren't, you'll have to make the effort to convert them yourself.
```
>>> " ".join(["1","2","3"])
'1 2 3'
>>> "".join(["1","2","3"])
'123'
```

**`STRING.split(STRING)`**

Splits a string into a list of words using the argument to separate the words. The separator can be omitted to split the string by whitespace. Useful when you have one string with multiple values separated by a certain pattern. Take note that the separator is always omitted in the resulting list.
```
>>> "1 2 3".split()
['1', '2', '3']
>>> "1     2 3".split()
['1', '2', '3']
>>> "1.2.3".split(".")
['1', '2', '3']
>>> "12113".split("1")
['', '2', '', '3']
```

###Lists

**Create a list of length x**

Python provides a handy shorthand to replicate lists and strings using the `*` operator.

Sometimes if you just need a list of length x, you can just use `range(x)`. If you're working with numbers though, I don't recommend it because `range(x)` produces a list of running numbers from 0 to x - 1, and it can be difficult to debug. You can use `[None] * 3` so you can see what's going on.

```
>>> [1,2] * 5
[1, 2, 1, 2, 1, 2, 1, 2, 1, 2]
>>> [None] * 3
[None, None, None]
```

**Creating a list from another list**

For those who like their code to be really really short, the list comprehensions feature is extremely helpful in creating new lists from existing lists or other iterables. You can also optionally filter the values. A similar syntax can also be used for tuples.

```
>>> [x * 2 for x in range(10)]
[0, 2, 4, 6, 8, 10, 12, 14, 16, 18]
>>> [x * 2 for x in range(10) if x % 2 == 0]
[0, 4, 8, 12, 16]
>>> tuple(x * 2 for x in range(10) if x % 2 == 0)
(0, 4, 8, 12, 16)
```

**`LIST.sort()` vs sorted`(LIST)`**

`sort()` sorts the original list and returns None, `sorted()` returns a NEW list that is the sorted form of the original. While they achieve similar effects, these two functions are **NOT INTERCHANGEABLE**. Both functions take in optional arguments that specify
1. `cmp`: how to compare values
2. `key`: how to extract values from the iterable
3. `reverse`: whether or not to reverse the sorting

```
>>> x = [3, 2, 1]
>>> sorted(x)
[1, 2, 3]
>>> print x
[3, 2, 1]
>>> print x.sort()
None
>>> x
[1, 2, 3]
```

Sample demonstration of key:
```
>>> x = [(0,3), (0,2), (0,1)]
>>> sorted(x, key=lambda x:x[1])
[(0, 1), (0, 2), (0, 3)]
```

###Functions

**`lambda`**

Lambda is a handy keyword to define short, one line functions. These functions don't even need to be named. In the example below, we create an unnamed (aka anonymous) function using the `lambda` keyword that takes one argument `x` and returns the value of `x + 1`. We can assign this function to the variable `add1` and call `add1` like any other function.

```
>>> add1 = lambda x: x + 1
>>> add1(1)
2
>>> add1(10)
11
```

###Dictionaries

**Iterating over dictionaries**

When you try to iterate over a dictionary using a `for` loop, you will iterate over only the keys like below:
```
>>> d = {'hi' : 1, 'man' :2}
>>> for key in d:
...     print key
...
hi
man
```

If you need to iterate over keys and values, you can do something like:
```
>>> d = {'hi' : 1, 'man' :2}
>>> for key in d:
...     print key, d[key]
```

or, the somewhat nicer way to do it:

```
>>> d = {'hi' : 1, 'man' :2}
>>> for key, value in d.items(): #I suggest you play with the items() method to see what it outputs
...     print key, value
```

💡Misc tips and tricks
---

**Debugging**

The easiest way to debug is just to insert `print` statements into your code so see what is being executed. Say I was using a conditional with multiple ways of returning `True`, but I don't know which condition is actually being fulfilled:

```
if x > 3:
  return True
elif x < 0:
  return True
elif type(x) == float:
  return True
```

You can use `print` statements to diagnose the problem

```
if x > 3:
  print 'LOL'
  return True
elif x < 0:
  print 'LOLOL'
  return True
elif type(x) == float:
  print 'LOLOLOL'
  return True
```

or, more cleverly, try to print relevant variables

```
if x > 3:
  print 1, x
  return True
elif x < 0:
  print 2, x
  return True
elif type(x) == float:
  print 3, x
  return True
```

Those of you using IDEs like Canopy may find breakpoints useful as well. However if you're not used to it, I suggest just sticking to `print` statements; it's worked for me for years.

**Terminal**

An important part of not creating bugs in your program is knowing what bit of code does what. But what if you have to pull out that obscure function that you only know by name because someone mentioned it once while you were falling asleep? Besides reading the documentation, I suggest testing the function first in your terminal (or command prompt as some of you might call it). Just key in the function you want to run and see how it behaves before trying to use it in your code.

Most IDE users (Canopy, PyCharm, etc.) should have a terminal window somewhere on their screen (usually at the bottom). The text editor hipsters (Sublime Text, IDLE, Notepad?) will probably need to open a separate terminal app and run the `python` command to do the same magic. Mac users should have no issues with running python from the terminal. If you get some unknown command or binary not on $PATH or something, I suggest googling 'add python to path' and follow a guide.
