---
layout: post
title:  "2019_03_28_TIL(Iterator & generator)"
date:   2019-03-28 01:06:23 +0700
categories: [computer]
---

### 2019.03.28 TIL

(TIL은 스스로 이해한 것을 바탕으로 정리한 것으로 오류가 있을 수 있습니다)

* iterator와 generator는 너무나도 정리가 잘된 블로그가 있어서 첨부드립니다. 
* [iterator & generator 원본] (https://nvie.com/posts/iterators-vs-generators/)
* [이터레이터와 제너레이터 한국어 번역] (https://mingrammer.com/translation-iterators-vs-generators/)


\# 질문에 답하기

1. iterator
2. generator
 
# 전체적인 그림

<img width="770" alt="iterator generator" src="https://user-images.githubusercontent.com/46436843/55204518-9b4e4e00-5212-11e9-80f8-bff9db345b5c.png">

# Iterator

1. next()함수에 의해서 값을 하나씩 반환한다. (next()함수 호출시점)
2. Stopiteration

## list의 iterator

```python
>>>	li = [1,2,3,4,5]
>>>	it = item(li)        # list의 내장함수 dir(list)를 통해 알 수 있다.
>>>	print(type(it))
list_iterator
>>>	next(it)         #li의 값을 1부터 하나씩 반환한다.
>>>
>>>	for e in li:
>>>		print(e)            #이거를 실행하는 순간 내부적으로 iter(li)가 만들어져서 하나씩 반환한다.
```

* 실제로 list는 미친듯이 느리다.
* 따라서 필요에 의해 list를 iterator로 바꾸어 사용한다.

## 나만의 iterator 만들기

```python
>>>
>>>	class Myiter:
>>>		def __init__(self.li):			
>>>			self.continer = li
>>>			self.index = 0
>>>
>>>		def __iter__(self):
>>>			return self
>>>
>>>		def __next__(self):
>>>			if self.index >= len(self.continer):
>>>				raise StopIteration
>>>			ret = self.container[self.index]
>>>			self.index += 1
>>>			return ret
>>>
>>>	it_obj = Myiter([1,2,3,4,5])
>>>
>>>	next(it_obj)
1
>>>	next(it_obj)
2
>>>	it = iter(it_obj)    #중간에 iter을 만들어도 기존에 것 해당x
>>>	type(it)
__main__.Mylter
>>>	next(it)
3

```

## 파일을 불러와 iterator 추출하기

```python
>>>
>>>	class Reader:
>>>		def __init__(self, filename):
>>>			self.f = open(filename, 'rt')
>>>		
>>>		def __iter__(self):
>>>			return self
>>>
>>>		def __next__(self):
>>>			a = self.f.readline()
>>>			if a == '':
>>>				self.f.close()
>>>				raise StopIteration
>>>			print(a)
>>>			
```

# Generator

### 제너레이터는 특별한 종류의 이터레이터이다.

* 제너레이터 역시 함수이다. 독특한 함수이다.
* 함수 body안에 yield라는 키워드가 있어야 한다.(이게 나오면 자동으로 generator이다)
* next와 yield이 서로 서로에게 실행을 넘겨주며 호출한다.
* yield는 양보하다 라는 뜻을 가지고 있다.

	* 제너레이터 에러 막기
	* 제너레이터로 피보나치 만들기
	* coroutine function
	* send()
	* yield from

## 제너레이터 에러 막기

```python
>>>	def gen():
>>>		print('gen start')
>>>		yield 1
>>>		print("a")
>>>		yield 2
>>>		print("b")
>>>		yield 3
>>>		return 'done'
>>>
>>>	g = gen()    #제너레이터 객체가 생성된다. 실행x
>>>	print(g)
<generator object gen at 0x1060a85c8>
>>>
>>>	next(g)   # 함수를 위에서 내려오며 yield을 만날 떄까지 진행
gen start    # yield가 1을 value로 반환한다.
>>>
>>>	print(next(g))
a           # yield 1을 반환하며 blocking되고 그 이후부터 실행
2			  # yield 2를 만나면서 2를 반환하다.
>>>	
>>>	next(g) #b를 print하며 3을 반환하고 blocking
b
>>>
>>>	#next(g)를 하게 되면 StopIteration error가 발생
>>>	#에러를 막기 위해 try exception을 이용
>>>	
>>>	try:
>>>		next(g)
>>>	except StopIteration as exc:
>>>		print exc.value
>>>		pass
>>>	
c
done
>>>
```

## 제너레이터로 피보나치 구현하기

```python
>>>	def fib():
>>>		prev, curr = 0, 1
>>>		while True:
>>>			yield prev    #prev 넣으면 0부터 curr 을 넣으면 1부터
>>>			prev, curr = curr, prev+curr
>>>
>>>	f = fib()
>>>	for i in range(10):
>>>		print(next(f), end = ' ')
0 1 1 2 3 5 8 13 21 34
>>>
```

## coroutine function

* 원래 함수는 실행할 때 스텍프레임이 생성되었다가 종료되면 스텍프레임이 없어진다. 하지만 제너레이터를 보면 실행 중에 멈추어서 다른 일을 하다가 또 호출을 해도 그 이전의 진행했던 사항들을 기억하고 있다. 이것을 가능하게 해주는 것이 coroutine function이다. 

### 함수 실행 도중에 실행 주도권을 다른 함수에 넘길 수 있고, 내가 원하는 시점에 다시 실행 주도권을 가져올 수 있는 함수!!

함수 실행 도중이라는 것은 스텍프레임을 쌓아놓고 다른 곳에 실행주도권을 주는데 그렇게 하기위해 어디에 **스텍프레임이 살아 있어야 한다.**
명시적으로 실행주도권을 넘긴다.

원래는 os의 스케줄러가 라운드 로빈을 통해 선점형으로 실행되는데 내가 원하는 때 준다는 것은 비선점형이다.

중요한 것은 파이썬의 스텍 프레임이 heap에 할당된다는 것이다. 파이썬의 스텍프레임이 실행주도권을 넘기더라도 살아있을 수 있다. (힙에 저장되어 있으므로)
다른 언어에는 coroutine이 없는가라는 질문이 있을 수 있다.

c나 c++에서는 언어차원에서는 지원하지 않으나, 

c : finite state machine기법 -> FSM
1. 엔트리포인트의 위치를 적절하게 저장하여서 구현(실제로는 스텍프레임이 사라지지만 실행의 순서가 이어지듯이 구현

c++ : stack swapping
1. 이미 스텍프레임에 쌓인 것은 어쩔수 없지만 heap영역에 마음대로 쌓을 수 있다. stack와 heap이 서로 오가며 스텍프레임을 쌓음

위와 같은 방법을 통해 해결한다.

파이썬의 코루틴은 generator 기반으로 만들어졌고 파이썬의 스텍프레임이 heap에 저장되므로 가능하다.

## send()

* send()는 제너레이터의 내장함수이다.
* next는 실행주도권을 넘기는꺼지 가능하지만 send를 통해 실행주도권뿐만 아니라 데이터까지 함께 넘길 수 있다.

코드를 통해 send를 알아보자.

```python
>>>	def gen():
>>>		print('gen start')
>>>		deta1 = yield 1  #yield 1이 실행되면 blocking이 걸리므로 아직 data1 변수는 만들어지지 않는다.(오른쪽에서 왼쪽으로 이동)
>>>		print(f"gen[1] : {data1}")
>>>		data2 = yield 2
>>>		print(f"gen[2] : {data2}")
>>>		return 'done'
>>>	g = gen()
>>>	g
generator object gen at 0x1060a8a40
>>>
>>>	first = g.send(None)     #보통 시직할 때는 None을 넣어준다. 
gen start
>>>
>>>	first       #yield 1로부터 1을 받아온다.
1
>>>
>>>	second = g.send('world')     #yield 1에 다시 'world'를 보내준다.
gen[1] : world
>>>	second # yield 2로부터 2를 받아온다.
2 
>>>
>>>	try:
>>>		g.send('hello')
>>>	except StopIterator as exc:
>>>		print(exc.value)
gen[2] : Hello
done
>>>
```

## yield from

* delegate(중계)만을 한다.
* 아직까지 명확하게 어떻게 사용해야 할지는 감이 오지 않는다.

```python
>>>	def gen():
>>>		print('gen start')
>>>		deta1 = yield 1
>>>		print(f"gen[1] : {data1}")
>>>		data2 = yield 2
>>>		print(f"gen[2] : {data2}")
>>>		return 'done'
>>>	
>>>	def delegate():
>>>		g = gen()
>>>		print('start')
>>>		ret = yield from g   #g는 따로 관여하지 않고 delegate만 한다. gen()
>>>		print('end')
>>>		print(f'return value:{ret}')
>>>		return ret
>>>
>>>	g = delegate()
>>>	print(g)
generator object delegate 
>>>	first = g.send(None)       #yield from g를 받게 되면 gen으로 이동하며 gen실행이 끝날 때까지 다시 delegate로 내려 올 수 없다.
start
gen start
>>>	print(first)
1
>>>	second = g.send('world')
gen[1] : world
>>>	second
2
>>>	g.send('hello')
gen[2] : hello
end
return value : done

StopIteration  Traceback (most recent call last)
<ipython-input-130-40ccd6fc328d> in <module>
----> 1 g.send('hello')

StopIteration: done
>>>
```

* yield from에 대해서는 어떤 상황에 쓰는지 좀 더 공부를 해봐야 할 것 같다.