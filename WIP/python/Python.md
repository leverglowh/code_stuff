- [Python](#python)
  - [Data types](#data-types)
    - [Lists](#lists)
    - [Tuples](#tuples)
  - [Base functions](#base-functions)
    - [`input()`](#input)
    - [`print()`](#print)
    - [`type()`](#type)
    - [`range()`](#range)
    - [type casting:](#type-casting)
  - [Conditionals and loops](#conditionals-and-loops)
    - [`if..elif..else..`](#ifelifelse)
    - [loops](#loops)
      - [`for..in..`](#forin)
      - [`while`](#while)
  - [Custom functions](#custom-functions)
    - [Lambda](#lambda)
    - [Decorators](#decorators)
  - [Object-oriented python](#object-oriented-python)
    - [`class`](#class)
  - [Exceptions](#exceptions)
  - [Special variables](#special-variables)
  - [Operations](#operations)
    - [Strings](#strings)
    - [Math](#math)
  - [Python Examples](#python-examples)
    - [First n Fibonacci numbers:](#first-n-fibonacci-numbers)
    - [First n prime numbers:](#first-n-prime-numbers)
  - [Libraries](#libraries)
    - [`turtle`: draw like a turtle!](#turtle-draw-like-a-turtle)
    - [`random`: generate a random number](#random-generate-a-random-number)
  - [Importing stuff](#importing-stuff)

# Python

Remember:

> Python is very much space sensitive. **USE THE FREAKING TAB**

---

## Data types

### Lists

```python
list1 = [1, 3, 5, 7, 100]

print(len(list1)) # 5
list1.append(200) # [1, 3, 5, 7, 100, 200]
list1.insert(1, 400) # [1, 400, 3, 5, 7, 100]
list1 += [1000, 2000]
list1.remove(3)
list1.pop(0)
list1.pop(len(list1) - 1)
list1.clear()
l = sorted(list1)
l = sorted(list1, reverse=True)
l = sorted(list1, key=len)
# Slicing works here too!
```

### Tuples
Non mutable lists.

```python
t = ("harry", "ron", "hermione")

len(t) # 3
t[1] # "ron"
t.append("draco") # error

```

---

## Base functions

### `input()`

```python
a = int(input('a = '))
```

### `print()`

```python
print('%d / %d = %f' % (a, b, a / b))
print('%d %% %d = %d' % (a, b, a % b))
print('{0} * {1} = {2}'.format(a, b, a * b))
print(f'{a} * {b} = {a * b}')
```

### `type()`

```python
a = 13
type(a) 	 	# <class 'int'>
```

### `range()`

```python
range(101) # 0-100
range(1, 100) # 1-99
range(1, 100, 2) # 1-99, +2 every step
```

### type casting:

```python
int(x)
float(x)
str(x)
chr(x) 		# int -> char [16846 -> 䇎][10 -> '\n']
ord(x)		# char -> int reverses chr()
```

---

## Conditionals and loops

### `if..elif..else..`

```python
if username == 'admin' and password == '666999':
    print('oooook!')
elif username == 'user'
    print('oks')
else:
    print('noooop')
```

### loops

#### `for..in..`

```python
sum = 0
for x in range(101): # 0-100
    sum += x
print(sum) # prints the sum of the first 100 integers
```

#### `while`

```python
while 1:
  doSomething()
  break
```

---

## Custom functions

```python
def my_function(a=0): #if not given, a is 0 by default
  a = a + 12345
  return a
```

### Lambda

```python
people = [
  {"name": "Harry", "house": "Gryffindor"},
  {"name": "Cho", "house": "Ravenclaw"},
  {"name": "Draco", "house": "Slytherin"}
]

prople.sort() # error '<' not supported in sort()

def f(person):
  return person["name"]

people.sort(key=f) # cho - draco - harry
# lambda person: person["name"] is the lambda function
people.sort(key=lambda person: person["name"]) # cho - draco - harry

```

### Decorators
A function that takes a function as input, and returns a modified version of that function.

```python
def announce(f):
  def wrapper():
    print("About to run the function...")
    f()
    print("Done with the function.")
  return wrapper
  
@announce
def hello():
  print("Hello, world!")

hello()
# About to run the function...
# Hello, world!
# Done with the function.
```

---

## Object-oriented python

### `class`
Datastructure with properties and methods.
- All methods of classes have `self` as an argument.
```python
class Point():
  def __init__(self, x, y): # called every time I create a Point
    self.x = x;
    self.y = y;
    
p = Point(2, 8)
print(p.x) # 2
print(p.y) # 8
```
```python
class Flight():
  def __init__(self, capacity):
    self.capacity = capacity
    self.passengers = []

  def add_passenger(self, name):
    if not self.open_seats():
      return False
    self.passengers.append(name)
    return True

  def open_seats(self):
    return self.capacity - len(self.passengers)

flight = Flight(3)

people = ["Harry", "Ron", "Hermione", "Ginny"]
for person in people:
  if flight.add_passenger(person):
    print(f"Added {person} to flight successfully")
  else:
    print(f"No available seats for {person}")
```

---

## Exceptions

```python
import sys

x = int(input("x: "))
y = int(input("y: "))

result = x / y;
print(f"{x} / {y} = {result}")
# if y = 0 -> ZeroDivisionError
# if x or y is not int -> ValueError

try:
  x = int(input("x: "))
  y = int(input("y: "))
except ValueError:
  print("Error: Invalid input.")
  sys.exit(1)

try:
  result = x / y
except ZeroDivisionError:
  print("Error: Cannot divide by 0.")
  sys.exit(1)
```

---

## Special variables

`__name__`

```python
if __name__ == '__main__':
  print("I'm actually executing now")
```

---

## Operations

### Strings

```python
s1 = 'hello '
s2 = 'world'

print(s1 * 3) # hello hello hello
print(s1 + s2) # hello world
print('ll' in s1) # True, check if contains
print(s1.strip()) # strips white spaces at the start and end
```

```python
s3 = 'abc123456'
print(s3[2]) # c
print(s3[2:5]) # c12
print(s3[2:]) # c123456
print(s3[2::2]) # c246
print(s3[::2]) # ac246
print(s3[::-1]) # 654321cba
print(s3[-3:-1]) # 45

print(s3.isdigit()) # False
print(s3.isalpha()) # False
print(s3.isalnum()) # True
```

```python
s = 'hello, world!'
print(len(s)) # 13
print(s.capitalize()) # Hello, world!
print(s.title()) #Hello, World!
print(s.upper()) #HELLO, WORLD!
print(s.find('or')) # 8
print(s.find('bubu')) # -1
print(s.startswith('He')) # False, case sensitive!
print(s.endswith('!')) # True
print(s.center(20, '*')) # ***hello, world!****
print(s.rjust(20, '-')) # -------hello, world!
```

### Math

```python
a = 321
b = 123

print(a + b)    # 444
print(a - b)    # 198
print(a * b)    # 39483
print(a / b)    # 2.60975..
print(a // b)   # 2 Floor div
print(a % b)    # 75
print(a ** b)   # 19958... Power!!!

print(a % 10) #1 last number
print(a // 10 % 10) #2 second number
print(a // 100) #3 third number and first
```

---

## Python Examples

### First n Fibonacci numbers:

```python
n = 20
l = [] # empty list
l.append(1)
l.append(1) # insert first 2 numbers

while len(l) < 20:			# find first 20 fibonacci numbers
    x = len(l)				# x is the current free index
    l.append(l[x-1]+l[x-2])	# add the sum(last, secondotolast) to the list
print(l)					# l is and array of first 20 fibonacci numbers
```

### First n prime numbers:

```python
n = 20
l = []
c = 2 # my counter
l.append(c) # First prime number

def is_prime(n):
    for x in range(2, n):
        if n%x == 0:
            return True
    return False

while len(l) < 20:
    if is_prime(c):
        l.append(c)
    c = c + 1

print(l)
```

---

## Libraries

### `turtle`: draw like a turtle!

```python
import turtle
turtle.pensize(4)
turtle.pencolor('green')

turtle.forward(100) # draw 100 px
turtle.right(90)    # turn 90 deg right

turtle.mainloop()   # handle other events
```

### `random`: generate a random number

```python
import random

random_number = random.randint(1, 100) # 1-100
```

---

## Importing stuff

```python
import functions
a = functions.square(2)

from functions import absolute
a = absolute(-4)

```
