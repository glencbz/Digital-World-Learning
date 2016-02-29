So you have a Python problem
===

😪 Foreword
---
This reference is intended to be an introduction to some of tips and tricks in Python with regards to technical jargon and common code patterns. I know that documentation can be extremely difficult to understand if you don't come from a programming background, so hopefully this cheatsheet cuts through some of that difficulty.

If possible, I wish that students avoid using this as a kind of substitute for StackOverflow and official documentation (especially those intending to do a lot of code in the future) because frankly, it is an extremely important skill to be able to look up you problems and solve them yourself because the real world isn't nearly so nice (but TAs can afford to be nice).

All said and done, this reference is meant for you guys. You know yourselves the best so do try to use it in the way that helps you the most.

💡Things you should know
---
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
