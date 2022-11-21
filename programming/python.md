# Python

## List comprehension

> `V = [2**i for i in range(13)]`
>
> `M = [x for x in S if x % 2 == 0]`

## Dict comprehension

> d = {key: value for (key, value) in iterable}

## Differences v2 and v3

[http://sebastianraschka.com/Articles/2014\_python\_2\_3\_key\_diff.html](http://sebastianraschka.com/Articles/2014\_python\_2\_3\_key\_diff.html)

## How to add an element in a Tuple?

Tuples are immutable; you can't change which variables they contain after construction. However, you can concatenate or slice them to form new tuples.

```
a = (1, 2, 3)
b = a + (4, 5, 6)
c = b[1:]
```

## Immutable vs Mutable

Since everything in Python is an Object, every variable holds an object instance. When an object is initiated, it is assigned a unique object id. Its type is defined at runtime and once set can never change, however its state can be changed if it is mutable. Simple put, a **mutable** object can be changed after it is created, and an**immutable** object can’t._Objects of built-in types like (int, float, bool, str, tuple, unicode) are immutable. Objects of built-in types like (list, set, dict) are mutable. Custom classes are generally mutable. To simulate immutability in a class, one should override attribute setting and deletion to raise exceptions._

## Order of the keys() method dictionary

The`.keys()`method returns the keys in an arbitrary order.

## Generators

Generators functions allow you to declare a function that behaves like an iterator, i.e. it can be used in a for loop.

The simplification of code is a result of generator function and generator expression support provided by Python.

To illustrate this, we will compare different implementations that implement a function, "firstn", that represents the first n non-negative integers, where n is a really big number, and assume (for the sake of the examples in this section) that each integer take up a lot of space, say 10 megabytes each.

Note: Please note that in real life, integers do not take up that much space, unless they are really, really, really, big integers. For instance you can represent a 309 digit number with 128 bytes (add some overhead, it will still be less than 150 bytes).

First, let us consider the simple example of building a list and returning it.

1 # Build and return a list

2 def firstn(n):

3 num, nums = 0, \[]

4 while num < n:

5 nums.append(num)

6 num += 1

7 return nums

8

9 sum\_of\_first\_n = sum(firstn(1000000))

The code is quite simple and straightforward, but its builds the full list in memory. This is clearly not acceptable in our case, because we cannot afford to keep all n "10 megabyte" integers in memory.

So, we resort to the generator pattern. The following implements generator as an iterable object.

1 # Using the generator pattern (an iterable)

2 class firstn(object):

3 def \_\_init\_\_(self, n):

4 self.n = n

5 self.num, self.nums = 0, \[]

6

7 def \_\_iter\_\_(self):

8 return self

9

10 # Python 3 compatibility

11 def \_\_next\_\_(self):

12 return self.next()

13

14 def next(self):

15 if self.num < self.n:

16 cur, self.num = self.num, self.num+1

17 return cur

18 else:

19 raise StopIteration()

20

21 sum\_of\_first\_n = sum(firstn(1000000))

## Yield

Python provides generator functions as a convenient shortcut to building iterators. Lets us rewrite the above iterator as a generator function:

1 # a generator that yields items instead of returning a list

2 def firstn(n):

3 num = 0

4 while num < n:

5 yield num

6 num += 1

7

8 sum\_of\_first\_n = sum(firstn(1000000))

## Lambda

Python supports the creation of anonymous functions (i.e. functions that are not bound to a name) at runtime, using a construct called "lambda". This is not exactly the same as lambda in functional programming languages, but it is a very powerful concept that's well integrated into Python and is often used in conjunction with typical functional concepts like filter(), map() and reduce().

This piece of code shows the difference between a normal function definition ("f") and a lambda function ("g"):

`>>> def f (x): return x**2`

`>>> print f(8)`

`64`

`>>>`

`>>> g = lambda x: x**2`

`>>>`

`>>> print g(8)`

`64`

## Parsing

```
>>> import re
>>> date_re = re.compile('(?P<a_year>\d{2,4})-(?P<a_month>\d{2})-(?P<a_day>\d{2}) (?P<an_hour>\d{2}):(?P<a_minute>\d{2}):(?P<a_second>\d{2}[.\d]*)')
>>> found = date_re.match('2016-02-29 12:34:56.789')
>>> if found is not None:
...     print found.groupdict()
... 
{'a_year': '2016', 'a_second': '56.789', 'a_day': '29', 'a_minute': '34', 'an_hour': '12', 'a_month': '02'}
>>> found.groupdict()['a_month']
'02'
```

## REST

```
>>> import requests

>>> r = requests.get('https://api.github.com/events')
>>> r.json()
[{u'repository': {u'open_issues': 0, u'url': 'https://github.com/...
```

Example:

```
import json, requests, pprint

url = 'http://maps.googleapis.com/maps/api/directions/json?'

params = dict(
    origin='Chicago,IL',
    destination='Los+Angeles,CA',
    waypoints='Joplin,MO|Oklahoma+City,OK',
    sensor='false'
)


data = requests.get(url=url, params=params)
binary = data.content
output = json.loads(binary)

# test to see if the request was valid
#print output['status']

# output all of the results
#pprint.pprint(output)

# step-by-step directions
for route in output['routes']:
        for leg in route['legs']:
            for step in leg['steps']:
                print step['html_instructions']
```

```
Check response:
# Check for HTTP codes other than 200
if response.status_code != 200:
print('Status:', response.status_code, 'Problem with the request. Exiting.')
exit()
```

## Fizzbuzz

```
for num in xrange(1,101):
    if num % 5 == 0 and num % 3 == 0:
        print "FizzBuzz"
    elif num % 3 == 0:
        print "Fizz"
    elif num % 5 == 0:
        print "Buzz"
    else:
        print num
```

## Given the following output in a file "access\_log", show a count by HTTP codes sorted by most common

```
# 62.210.215.113 - - [14/Jul/2016:16:14:13 -0400] "GET /resume/feed/ HTTP/1.1" 200 2248 "-" "Mozilla/5.0 (X11; Linux i686) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/30.0.1599.66 Safari/537.36"
# 107.183.178.139 - - [14/Jul/2016:16:20:24 -0400] "GET / HTTP/1.0" 200 74545 "
http://myblog.com/
" "Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Maxthon/4.4.3.4000 Chrome/30.0.1599.101 Safari/537.36"
# 107.183.178.139 - - [14/Jul/2016:16:20:24 -0400] "GET /wp-login.php HTTP/1.1" 403 292 "
http://myblog.com/
" "Opera/9.80 (Windows NT 6.2; Win64; x64) Presto/2.12.388 Version/12.17"
# 107.183.178.139 - - [14/Jul/2016:16:20:24 -0400] "GET /wp-login.php HTTP/1.1" 403 292 "
http://myblog.com/wp-login.php
" "Opera/9.80 (Windows NT 6.2; Win64; x64) Presto/2.12.388 Version/12.17"
# 107.183.178.139 - - [14/Jul/2016:16:20:25 -0400] "GET / HTTP/1.1" 200 74545 "
http://myblog.com/
" "PHP/5.{3|2}.{1|2|3|4|5|6|7|8|9|0}{1|2|3|4|5|6|7|8|9|0}"
# 107.183.178.139 - - [14/Jul/2016:16:20:25 -0400] "POST /xmlrpc.php HTTP/1.1" 403 290 "
http://myblog.com/
" "PHP/5.3.57"
# 107.183.178.139 - - [14/Jul/2016:16:20:25 -0400] "GET / HTTP/1.0" 200 74545 "-" "PHP/5.3.57"
# 54.234.214.67 - - [14/Jul/2016:16:23:19 -0400] "GET / HTTP/1.1" 301 - "-" "Mozilla/5.0 (Windows NT 6.1; WOW64; rv:40.0) Gecko/20100101 Firefox/40.0"
# 54.234.214.67 - - [14/Jul/2016:16:23:19 -0400] "GET / HTTP/1.1" 200 74545 "-" "Mozilla/5.0 (Windows NT 6.1; WOW64; rv:40.0) Gecko/20100101 Firefox/40.0"
# 54.234.214.67 - - [14/Jul/2016:16:23:19 -0400] "GET / HTTP/1.1" 301 - "-" "Mozilla/5.0 (Windows NT 6.1; WOW64; rv:40.0) Gecko/20100101 Firefox/40.0"
# 54.234.214.67 - - [14/Jul/2016:16:23:20 -0400] "GET / HTTP/1.1" 200 74545 "-" "Mozilla/5.0 (Windows NT 6.1; WOW64; rv:40.0) Gecko/20100101 Firefox/40.0"
# 104.202.52.41 - - [14/Jul/2016:16:28:22 -0400] "GET / HTTP/1.0" 200 74545 "-" "Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Maxthon/4.4.3.4000 Chrome/30.0.1599.101 Safari/537.36"
```

```
#!/usr/bin/python
f = open('access_log', 'r')
d = {}
while (line = f.readline()):
words = line.split(" ")
if words[9] not in d:
  d[words[9]] += 1
else:
  d[words[9]] = 0
for k, v in sorted(d.items(), key=operator.itemgetter(0)):
  print "HTTP Code" + k + "has" + v + "elements"

$ cat access_log | awk -F ' ' '{ print $9 }' | wc -l
```

## Implement the 'paste' command.

```
$ more file{1,2,3} | cat
::::::::::::::
file1
::::::::::::::
one
two
longer three
::::::::::::::
file2
::::::::::::::
San Francisco
Sunnyvale
San Jose
::::::::::::::
file3
::::::::::::::
Green
Red
Orange
Purple
Yellow
Brown
$ paste file{1,2,3}
one  San Francisco  Green
two  Sunnyvale  Red
longer three San Jose Orange
Purple
Yellow
Brown
```

```
f = open(f1, 'r')
f2 = open(f2, 'r')
f3 = open(f3, 'r')
lines = f.readlines()
lines2 = f2.readlines()
lines3 = f3.readlines()

num = max (len(lines), len(lines2), len(lines3))

for count in range(num):
  count = 0
  s = line
  if lines[count]:
    s += lines[count]
  if lines2[count]:
    s += lines2[count]
  if lines3[count]:
    s += lines3[count]
  count += 1
  l.append(s)

for i in l:
  print i
```

More questions:

[https://www.codementor.io/sheena/essential-python-interview-questions-du107ozr6](https://www.codementor.io/sheena/essential-python-interview-questions-du107ozr6)

## Difference between range and xrange

For the most part,`xrange`and`range`are the exact same in terms of functionality. They both provide a way to generate a list of integers for you to use, however you please. The only difference is that`range`returns a Python`list`object and`xrange`returns an`xrange`object.

What does that mean? Good question! It means that`xrange`doesn't actually generate a static list at run-time like`range`does. It creates the values as you need them with a special technique called_yielding_. This technique is used with a type of object known as_generators_. If you wan t to read more [Python generators and the yield keyword](https://www.pythoncentral.io/python-generators-and-yield-keyword/).

Okay, now what does\_that\_mean? Another good question.\_That\_means that if you have a really gigantic range you'd like to generate a list for, say one billion,`xrange`is the function to use. This is especially true if you have a really memory sensitive system such as a cell phone that you are working with, as`range`will use as much memory as it can to create your array of integers, which can result in a`MemoryError`and crash your program. It's a memory hungry beast.

That being said, if you'd like to iterate over the list multiple times, it's probably better to use`range`. This is because`xrange`has to generate an integer object every time you access an index, whereas`range`is a static list and the integers are already "there" to use.

## Read terabyte file

`import re`

`def words(file, buffersize=2048):`

`buffer = ''`

`for chunk in iter(lambda: file.read(buffersize), ''):`

`words = re.split("\W+", buffer + chunk)`

`buffer = words.pop() # partial word at end of chunk or empty`

`for word in (w.lower() for w in words if w):`

`yield word`

`if buffer:`

`yield buffer.lower()`

\>>> demo = StringIO('''\\

... Lorem ipsum dolor sit amet, consectetur adipiscing elit. Pellentesque in nulla nec mi laoreet tempus non id nisl. Aliquam dictum justo ut volutpat cursus. Proin dictum nunc eu dictum pulvinar. Vestibulum elementum urna sapien, non commodo felis faucibus id. Curabitur

... ''')

\>>> for word in words(demo, 32):

... print word

...

lorem

ipsum

dolor

sit

amet

consectetur

adipiscing

elit

pellentesque

in

nulla

nec

mi

laoreet

tempus

non

id

nisl

aliquam

dictum

justo

ut

volutpat

cursus

proin

dictum

nunc

eu

dictum

pulvinar

vestibulum

elementum

urna

sapien

non

commodo

felis

faucibus

id

curabitur
