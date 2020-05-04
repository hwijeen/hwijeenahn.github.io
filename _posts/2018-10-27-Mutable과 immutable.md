---
layout: post
title: "Mutable과 immutable"
description: "Python mutable, immutable"
comments: true
categories: [Python]
tags:
- Python
---



## Background: 변수와 객체

"파이썬에서는 모든 것이 객체이다." 객체라는 건 메모리 위에 저장된 개별 데이터를 의미한다. 정수 4, 문자열 hi 등 모든 데이터를 객체라고 생각하면 된다. (같이 자주 등장하는 단어인 [클래스는 데이터의 '유형'을 나타낸다](https://python.bakyeono.net/chapter-8-2.html)). 컴퓨터 메모리에 할당되어서 저장되는 건 객체 그 자체이다.

변수는 객체의 위치(정확한 메모리 주소는 아니지만 객체의 고유 번호)를 가리킨다. 객체의 위치를 가리키게 하는 과정을 *'바인딩'*이라고 부른다. (마치 C언어의 포인터마냥..)

```python
# python
# 22222라는 객체를 생성하고, 변수 a에 바인딩한다
a = 22222
```

"파이썬에서는 모든 것이 객체이다"라는 말을 강조하는 이유는, 다른 언어에는 모든 것이 객체이지 않아서이다. 예를 들어, 아래와 같은 C언어에서는 22222가 객체가 아니다. 메모리에 할당되는 것은 int 타입의 변수 a이고, a 안에 22222이라는 값이 *대입*된다. (변수 a의 주소를 가리키는 것은 포인터다.)

```C
// C
int a = 22222
```



## Mutable vs. Immutable

Mutable과 immutable은 **객체**의 종류를 의미한다. Mutable은 값을 수정할 수 있는, immutable은 그렇지 못한 객체를 의미한다. 기술적으로 말하자면 mutable 객체는 `__setattr__`이라는  special method가 정의되어 있고, immutable은 그렇지 않다. 

자주 사용하는 파이썬 기본 자료형중 `string`, `tuple`, `int`, `float`, `bool` 등이 immutable에 속한다. Mutable은 `list`, `dict`, `set`을 포함하며, **사용자가 정의한 class도 기본적으로는 mutable**로 생성된다. 



## "Call by object reference"

객체가 mutable한지 immutable한지가 중요해지는 것은 함수의 인자로 사용될 때다. ['call by value'냐 'call by reference'냐 관점](https://wayhome25.github.io/cs/2017/04/11/cs-13/)으로 본다면, 함수의 인자로 들어온 객체가 mutable냐 immutable냐에 따라 다르다.  아래 예시에서와 같이 **인자가 mutable일 경우 call by reference처럼 동작**하므로 주의해야한다.

```python
def append_end(some_list):
    some_list.append('end')
    return some_list

my_list = ['hi', 'there']
print(append_list(my_list)) # ['hi', 'there', 'end']
print(append_list(my_list)) # ['hi', 'there', 'end', 'end']
```

[더 좋은 정리는 python의 함수는 'call by object reference'를 따른다는 것이다.](https://item4.github.io/2015-07-18/Some-Ambiguousness-in-Python-Tutorial-Call-by-What/)

[global 관련해서 헷갈릴 수 있는 부분](https://stackoverflow.com/questions/31435603/python-modify-global-list-inside-a-function)



## Immutable as a default function parameter

```python
def my_func1(input_list, args=None):
    if arg is None:
        args = []
    args.append(input_list)
    return args
    
def my_func2(input_list, args=[]):
    args.append(input_list)
    return args
```

[mutable 객체를 함수의 default인자로 사용하는 것을 지양해야한다.](https://docs.python-guide.org/writing/gotchas/) mutable default argument는 함수가 정의되는 시점에서 단 한 번만 evaluate된다. 그러니가 `my_func2`에서 `args=[]`는 여러 번 호출될 때엔 적용되지 않는다는 얘기다. args를 사용해서 caching을 할 것이 아니라면 `my_func1`과 같이 immutable을 default 인자로 삼아야한다.



- TODO: 최상위 클래스 object의 정체? 이걸 상속받기 때문에 `__setattr__'이 생기는 게 맞는지







