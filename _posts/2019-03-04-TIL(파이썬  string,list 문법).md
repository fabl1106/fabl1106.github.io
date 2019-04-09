---
layout: post
title:  "2019_03_04_TIL(파이썬 string,list 문법)"
date:   2019-03-04 21:06:23 +0700
categories: [python]
---

###2019.03.04 TIL

(TIL은 스스로 이해한 것을 바탕으로 정리한 것으로 오류가 있을 수 있습니다)

# 파이썬 기본 문법

========================
1.기본 문법(string,list)
-------------
python 기본 문법의 시작이다.

오늘은 string/list으로 나누어 기본 문법을 정리해보려고 한다.


## String

String은 기본적으로 immutable로서 변경할 수 없다.(tuple와 마찬가지). 

<img width="640" alt="string" src="https://user-images.githubusercontent.com/46436843/54571993-6c70f480-4a28-11e9-8c19-c4e8ade6db33.png">


String은 index를 사용할 수 있으며 "abcdef"[0],    

### String은immutable하므로 보이는 형태라도 변경하기 위한 방법


1. slicing   

	* b = a[0:2] + "x" + a[3:]

2. replace ---> 특정 문자 찾아 바꾸기

	* b = a.replace("a", 'x')

3. a.split()

	* ---> 공백을 뛰어서 list로 바꿈

위와 같은 방법으로 변경할 수 있다.



### string에서 빈칸을 없애는 방법


1. a.lstrip() ---> 왼쪽 공백 제거
2. a.rstrip() ---> 오른쪽 공백 제거
3. a.strip() ---> 양쪽 공백 제거
4. a.replace(" ",'') ---> 띄어쓰기 모두 제거

위의 4가지 방법을 쓸 수 있다.


### string의 길이, 요소 파악


* len(a)를 사용한다

* a.count("a") ---> 특정 요소의 갯수 파악


### string 내에서 특정한 값을 대입 혹은 바꿔야 할 때


1. format

	* "{},{}".format(a, b)

2. 포매팅

	* "I love %s" %you


### string내에서의 join

1. "str".join(list)

	* list의 요소마다 str을 넣어서 string으로 변경 .   ---> 단 list의 요소는 다 문자로 구성되어야 함

2. "str".join(str)

	* str의 사이마다 str을 넣어서 변경


## list

정의 : python에서 list는 다양한 타입의 변수 모임이라고 본다   

<img width="628" alt="list" src="https://user-images.githubusercontent.com/46436843/54572098-08026500-4a29-11e9-8468-aa230b93de0b.png">

li = []  
li = [ 1, "b", (1,2,3) ]

Number, string, tuple 등 다양한 타입이 모두 올 수 있다.

### 요소 삭제
1. li[1:3] = []  ---> index 활용 삭제

2. del li[0] ---> 특정 인덱스 요소 삭제

3. li.remove() ---> 특정 요소 직접 삭제

#### 요소 추가

1. li.append() ---> 제일 뒤에 1개의 특정 요소 추가

2. li.insert(index, ele) ---> 특정 인덱스에 원하는 요소 추가

3. li.extend([   ]) ---> list 요소들을 한번에 추가


### 요소 추출 

1. li[0] ---> 인덱싱

2. li[0:2] ---> 슬라이싱

3. li.index("ele") ---> 특정 요소의 인덱스 파악

4. li.count("ele") ---> 특정 요소의 갯수 파악

5. li.pop() ---> 특정 인덱스의 요소 추출 및 제거(없을시 가장 뒷 요소)

### 요소 수정

1. li[0] = "1" ---> 하나의 값 수정

2. li[0:2] = [1, 2, 3] ---> 해당 슬라이싱 만큼 특정 값 추가 



### 요소 정렬

1. li.sort() ---> 원본이 바뀌는 정렬

2. li.reverse() ---> 원본을 거꾸로 

3. li.sort(reverse = True)

4. len(a)

5. sorted(li) ---> 원본이 바뀌지 않는 정렬

