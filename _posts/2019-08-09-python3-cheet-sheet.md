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
    str = "I'm shnoh"
    str = 'He said "Hello"'
    str = "He said \"Hello\""
    str = "How are you?\tI'm fine\n"  # \t also works

    str = "test"
    print(len(str))      # 4
    print(str[2])        # s
    print(str[-1])       # t

    str = str + str
    str = str * 2

    str = "20190809"
    year = str[:4]       # i < 4        2019
    month = str[4:6]     # 4 <= i < 6   08
    day = str[6:]        # 6 <= i       09

    # str[8] = '8'       # error, string is immutable

    str = "I have %d %s" % (10, "cars")
    # %c, %f, %o, %x, %% also work
    str = f"Today is {month}/{day}/{year}"

    str = "my hobby"
    print(str.count('b'))    # 2
    print(str.find('b'))     # first place    5
    print(str.find('z'))     # -1
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
