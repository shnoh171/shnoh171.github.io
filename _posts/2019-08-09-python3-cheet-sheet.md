---
layout: post
title: Python3 Cheet Sheet
categories:
  - Others
---
### Print a + b
```python
if __name__ == '__main__':
    a = int(input())
    b = int(input())
    print(a + b)
```
### Number Type
```python
if __name__ == '__main__':
    a = 2004
    a = 1.2
    a = 3.14E-10
    a = 0o77          # octal
    a = 0x3f          # hexadecimal
    a = 2 ** 10       # power
    a = 123 // 11     # quotient
    a = 123 % 11      # remainder
    print(a)
    print(type(a))
```
### String Type
```python
if __name__ == '__main__':
    s = "I'm shnoh"
    s = 'He said "Hello"'
    s = "He said \"Hello\""
    s = "How are you?\tI'm fine\n"

    s = "test"
    print(len(s))      # 4
    print(s[2])        # s
    print(s[-1])       # t

    s = s + s
    s = s * 2

    s = "20190809"
    year = s[:4]       # i < 4        2019
    month = s[4:6]     # 4 <= i < 6   08
    day = s[6:]        # 6 <= i       09

    # s[8] = '8'       # error, string is immutable

    s = "I have %d %s" % (10, "cars")
    # %c, %f, %o, %x, %% also work
    s = f"Today is {month}/{day}/{year}"

    s = "my hobby"
    print(s.count('b'))    # 2
    print(s.find('b'))     # first place    5
    print(s.find('z'))     # -1

    s = s + str(2)         # my hobby2
```
### List type
```python
if __name__ == '__main__':
    l = list()
    l = [1, 2, 3, 4, 5]
    l = ['a', 'b', 'c']
    l = [1, 2, ['a', 4]]

    print(l[0] + l[1])      # 1 + 2 = 3
    print(l[2][1])          # 4
    print(l[0:2])           # [1, 2]

    l1 = [1, 2, 3]
    l2 = [4, 5, 6]
    l = l1 + l2             # [1, 2, 3, 4, 5, 6]
    l = l1 * 2              # [1, 2, 3, 1, 2, 3]

    print(len(l))           # 6

    l[1] = 1                # [1, 1, 3, 1, 2, 3]
    del(l[1])               # [1, 3, 1, 2, 3]
    del l[1]                # [1, 1, 2, 3]
    del(l[2:])              # [1, 1]
    l.append(0)             # [1, 1, 0]

    l.sort()                # [0, 1, 1]
    l.sort(reverse = True)  # [1, 1, 0]
    l.reverse()             # [0, 1, 1]

    print(l.index(1))       # 1 (first index)
    # print(l.index(2))     # error

    l.insert(2, 4)          # [0, 1, 4, 1]
    l.remove(1)             # [0, 4, 1]

    l.pop()                 # [0, 4]
    print(l.pop(0))         # 0  [4]

    print(l.count(4))       # 1
    l.extend([3, 2])        # [4, 3, 2]
    l += [1, 0]             # [4, 3, 2, 1, 0]
```
### Tuple Type
```python
if __name__ == '__main__':
    t = ()
    t = (1,)                # when only one element
    t = ('a', 2, (1, 2))

    # del t[0]              # error
    # t[0] = 'b'            # error

    t = (1, 2, 3, 4)
    print(t[2])             # 3
    print(t[1:])            # (2, 3, 4)
    t = t + (5, 6)          # (1, 2, 3, 4, 5, 6)

    t = (1, 2)
    t = t * 2               # (1, 2, 1, 2)

    print(len(t))           # 4
```
### Dictionary Type
```python
if __name__ == '__main__':
    d = {1: 'hello'}
    d[2] = 'world'
    d['reverse'] = 3
    del d['reverse']

    print(d[2])

    d[2] = 'shnoh'      # overwrite (not desired)
                        # {1: 'hello', 2: 'shnoh'}
    d[(1, 2)] = 'okay'
    # d[[1, 1]] = 'no'  # TypeError (list cannot be a key)

    d = {1: 'hello', 2: 'world', 3: '!'}

    print(d.keys())     # dict_keys([1, 2, 3])
    for k in d.keys():
        print(k)        # 1
                        # 2
                        # 3

    l = list(d.keys())  # [1, 2, 3]
                        # should convert to list to use
                        # append, insert, pop, remove, sort

    print(d.values())    # dict_values(['hello', 'world', '!'])
    l = list(d.values()) # ['hello', 'world', '!']

    print(d.items())    # dict_items([(1, 'hello'),
                        # (2, 'world'), (3, '!')])
    l = list(d.items()) # [(1, 'hello'), (2, 'world'), (3, '!')]

    d.clear()           # {}

    d = {1: 'hello', 2: 'world', 3: '!'}

    print(d[1])         # hello
    # print(d[4])       # KeyError: 4
    print(d.get(1))     # hello
    print(d.get(4))     # None

    print(1 in d)       # True
    print(4 in d)       # False
```
### Set Type
```python
if __name__ == '__main__':
    s = set([1, 2, 3])          # {1, 2, 3}
    s = set("Hello")            # {'H', 'l', 'o', 'e'}

    l = list(s)                 # ['e', 'o', 'H', 'l']
    t = tuple(s)                # ('H', 'l', 'o', 'e')

    s1 = set([1, 2, 3])
    s2 = set([3, 4, 5])
    s = s1 & s2                 # {3}
    s = s1.intersection(s2)     # {3}
    s = s1 | s2                 # {1, 2, 3, 4, 5}
    s = s1.union(s2)            # {1, 2, 3, 4, 5}
    s = s1 - s2                 # {1, 2}
    s = s1.difference(s2)       # {1, 2}

    s.add('a')                  # {1, 2, 'a'}
    s.update([3, 4])            # {1, 2, 3, 4, 'a'}
    s.remove(1)                 # {2, 3, 4, 'a'}
    # s.remove(5)               # KeyError: 5
```
### Bool Type
```python
if __name__ == '__main__':
    b = True
    b = False

    b = 1 < 2           # True
    b = 1 == 2          # False

    # False: "", [], (), {}, 0, None

    print(bool([]))     # False

    l = [1, 2, 3]
    while l:
        print(l.pop())  # 3  2  1
```
### Bitwise Operator
```python
if __name__ == '__main__':
    a = 0b10101010
    b = 0b11110000

    c = a & b
    c = a | b
    c = a ^ b
    c = ~a

    c = a << 2
    c = a >> 2

    print(bin(c))       # int(c), oct(c), hex(c)
```
### Variable
```python
from copy import copy

if __name__ == '__main__':
    a = [1, 2, 3]
    b = a

    print(id(a))
    print(id(b))                # same
    print(a is b)               # True
    a[1] = 3
    print(b)                    # [1, 3, 3]

    a = [1, 2, 3]
    b = copy(a)                 # after importing copy
                                # b = a[:] also works
    print(id(a))
    print(id(b))                # different
    print(a is b)               # False
    a[1] = 3
    print(b)                    # [1, 2, 3]

    a, b = ('hello', 'world')   # a: hello
                                # b: world
    a, b = ['hello', 'world']   # a: hello
                                # b: world

    a, b = b, a                 # a: world
                                # b: hello

    a = b = "hello"             # a: hello
                                # b: hello
```
### If Statement
```python
if __name__ == '__main__':
    left = right = 10
    if left < right:
        print("left is smaller then right")
    elif left == right:
        print("left is equal to right")
    else:
        print("left is larger then right")

    if left == right and not left == 5:
        print ("left is not 5 and equal to right")

    wallet = ['money', 'card', 'coupon']
    if 'money' in wallet:
        print ("pay in cash")

    wallet.remove('money')
    if 'money' in wallet:
        pass
    else:
        print("beg for money")

    score = 89
    grade = 'A' if score >= 90 else 'F'
    print(grade)
```
### While Statement
```python
if __name__ == '__main__':
    i = 0
    while i < 5:
        print(i)
        i = i + 1

    i = 0
    while i < 10:
        if i == 5:
            break
        print(i)
        i = i + 1

    i = 0
    while i < 10:
        if i % 2 == 0:
            i = i + 1
            continue
        print(i)
        i = i + 1
```
### For Statement
```python
if __name__ == '__main__':
    l = [1, 2, 3]
    for i in l:
        print(i)

    l = [(1, 2), (3, 4)]
    for (first, last) in l:
        print (first + last)

    sum = 0
    for i in range(11):     # range(0, 11) also works
        sum += i
    print(sum)              # 55 (0 + ... + 10)

    l1 = [1, 2, 3, 4]
    l2 = [num * 3 for num in l1]          # [3, 6, 9, 12]
    l2 = [num * 3 for num in l1 if num % 2 == 0] # [3, 9]

    l = [x * y for x in range(2, 10)
               for y in range(1, 10)]
```
