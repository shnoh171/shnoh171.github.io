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
    s = "How are you?\tI'm fine\n"  # \t also works

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
### Tuple type
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
### If Statement
```python
if __name__ == '__main__':
    left = right = 10
    if left < right:
        print("left < right")
    elif left == right:
        print("left = right")
    else:
        print("left > right")
```
