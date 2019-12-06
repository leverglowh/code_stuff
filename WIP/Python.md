# Python

---

Remember:

> Python is very much space sensitive. **USE THE FREAKING TAB**

##### Libraries

- `turtle`: draw like a turtle!

```python
import turtle
turtle.pensize(4)
turtle.pencolor('green')

turtle.forward(100) # draw 100 px
turtle.right(90)	# turn 90 deg right

turtle.mainloop() 	# handle other events
```

- `random`: generate a random number

```python
import random

random_number = random.randint(1, 100) # 1-100
```

---

##### Math operations

```python
a = 321
b = 123

print(a + b)	# 444
print(a - b)	# 198
print(a * b)	# 39483
print(a / b)	# 2.60975..
print(a // b)	# 2 Floor div
print(a % b)	# 75
print(a ** b)	# 19958... Power!!!
```

---

### Base functions

##### `input()`

```python
a = int(input('a = '))
```

##### `print()`

```python
print('%d / %d = %f' % (a, b, a / b))
print('%d %% %d = %d' % (a, b, a % b))
```

##### `type()`

```python
a = 13
type(a) 	 	# <class 'int'>
```

##### type casting:

```python
int(x)
float(x)
str(x)
chr(x) 		# int -> char [16846 -> 䇎][10 -> '\n']
ord(x)		# char -> int reverses chr()
```

##### `range()`

```python
range(101) # 0-100
range(1, 100) # 1-99
range(1, 100, 2) # 1, 3, 5, 7, ... odd numbers. 2 specifies the increment
```

---

### Conditionals and loops

##### `if..elif..else..`

```python
if username == 'admin' and password == '666999':
    print('oooook!')
elif username == 'user'
    print('oks')
else:
    print('noooop')
```

##### loops

###### `for..in..`

```python
sum = 0
for x in range(101): # 0-100
    sum += x
print(sum) # prints the sum of the first 100 integers
```



### Python Examples

##### First n Fibonacci numbers:

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

##### First n prime numbers:

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

