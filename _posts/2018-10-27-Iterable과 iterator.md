---
layout: post
title: "Iterable과 iterator"
description: "Python iterable, iterator"
comments: true
categories: [개발 / 환경]
tags:
- Python
---



## Iterable

`for item in iterable`구문을 사용할 수 있는 객체들. 이러한 객체들은 스페셜 메서드 `__iter__`이 정의되어 있으며, 이는 이터레이터를 반환한다. 파이썬 내장함수 `iter(iterable)`을 통해서 이터러블의 이터레이터(?!)를 얻을 수 있다. 자주 사용하는 기본 자료형 중 `list`, `string`, `dictionary`, `set`등이 이터러블이다. 

참고로, [내장 함수 sum()은 모든 iterable에 대해서 동작한다.](https://soooprmx.com/archives/8007)



## Iterator

Iterable객체가 `__iter__`함수로 반환하는 것이 이터레이터라고 했다. 이 또한 하나의 객체이며, 스페셜 메서드 `__next__`를 가지고 있다. 파이썬 내장 함수 next()를 통해서도 이 함수를 실행할 수 있다. 이 함수가 하는 일은 순차적으로 탐색하며 데이터를 하나씩 반환하는 것이다. 

`for item in iterable` 구문을 사용하면 내부적으로는 iterable의 이터레이터 생성 및 next()함수 호출이 진행된다. ([이는 C언어에서 for문이 작동하는 방식과 매우 다르다.](https://soooprmx.com/archives/8007)) 

이터러블과 이터레이터는 다른 객체이지만, 경우에 따라서는 동일한 객체가 이터러블이면서 이터레이터로써 작동하게 할 수 있다. [하나의 클래스가 `__iter__`과 `__next__` 메서드를 둘 다 가지는 경우이다.](https://anandology.com/python-practice-book/iterators.html)

참고로,[`__next__`대신 `__getitem__`을 통해서 이터레이터를 만들 수도 있고](https://dojang.io/mod/page/view.php?id=1113), [이터러블은 unpacking이 가능하다.](https://dojang.io/mod/page/view.php?id=1112)



## Generator

제너레이터는 이터레이터를 만드는 함수이다. 함수의 반환을 나타내는 return대신 yield가 쓰인다는 게 특징이다. yield문을 사용할 경우 파이썬이 함수 내용을 사용해서 제너레이터 객체를 만들어낸다. 직접 이터레이터 프로토콜을 정의하지 않았지만, next()함수를 통해 yield하는 값을 받아올 수 있다. 모든 데이터가 동시에 필요한 상황이 아니라면, 제너레이터를 사용해서 매 시점에 필요한 데이터만을 메모리로 로드하는 효율적인 프로그래밍이 가능하다.

```python
def my_generator(n):
    while n<10:
    	yield n+1
        n += 1 # yield 이후에 statement 추가 가능
    
a = my_generator(5) # a is an generator = iterator
next(a)
```





