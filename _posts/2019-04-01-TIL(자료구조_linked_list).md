---
layout: post
title:  "2019_04_01_TIL(자료구조/linked List)"
date:   2019-04-01 01:06:23 +0700
categories: [computer]
---

### 2019.04.01 TIL

(TIL은 스스로 이해한 것을 바탕으로 정리한 것으로 오류가 있을 수 있습니다)

\# 질문에 답하기

1. 자료구조의 큰 틀에 대해 이야기하기
 
# 자료구조

**자료구조는 메모리에 data를 어떻게 저장할 것인가에 대한 고민부터 시작된다.**    
list, tuple, dictionary 모두 자료구조의 한 형태이다


자료구조는 딱 3가지이다.

1. 데이터를 어떻게 삽입할 것인가?(insert)
2. 데이터를 어떻게 검색할 것인가?(search)
3. 데이터를 어떻게 삭제할 것인가?(delete)

# ADT(abstract data type) : 추상 자료형

### 자료 구조에서 삽입, 탐색, 삭제 등을 담당하는 함수들의 사용 설명서

1. 어떤 자료구조가 가지고 있는 오퍼레이션(함수)의 나열(목록)
2. 함수 시그니처(인터페이스)만 나열할 뿐, 내부 구현은 표기하지 않음
3. 함수(operation)의 작동 방식 설명.  

ex)

```python
>>>	help(list.append)
help on method_descriptor:

append(...)
	L.append(object) -> None
>>>
```

우리는 여기에서 append함수가 어떻게 구현되었는지 알 수 없다. 하지만 append함수를 사용하는 것에는 전혀 문제가 없다. 이러한 특징을 인터페이스와 구현이 분리되었다고 하며 추상화라는 표현을 쓴다.

# linked List

**데이터와 참조로 구성된 노드가 한 방향 혹은 양방향으로 쭉 이어져 있는 자료구조**

* single linked list
* double linked list

## 노드

### 자료구조를 구현할 때 데이터를 담는 틀

<img width="645" alt="node" src="https://user-images.githubusercontent.com/46436843/55322765-31ef6900-54b8-11e9-9419-8f2e79a98fd8.png">

* 노드는 저장할 데이터와 다음 노드를 가르키는 참조로 이루어져 있다.(단일 연결리스트)
* double linked list에서는 다른 형태의 노드가 활용된다.

## single linked list

### single linked list에서의 노드 구현

```python
class Node:
	def __init__(self, data=None):
		self.__data = data
		self.__next = None


# 소멸자
# 객체가 사라지기 전에 반드시 한번 호출하는 것을 보장한다.
# 소멸자를 호출하고 지우는 것을 보장해준다.
# 실제 로드가 사라지는 것을 확인하기 위해서 가지고 옴
	def __del__(self):
		print(f'node[{self.__data}] deleted!')
		pass

	@property
	def data(self): 
		return self.__data
	
	@data.setter
	def data(self, data):
		self.__data = data
	
	@property
	def next(self):
		return self.__next
	
	@link.setter
	def next(self, next):
		self.__next = next
		
```

* 자료구조에서는 보안이 중요하므로 정보은닉 2가지를 모두 적용해주었다.
* 멤버 __data에는 데이터를 저장하고, 멤버 __ next는 다음 노드를 가르킨다.

### single linked list의 인스턴스 멤버

1. head -> 리스트의 첫 번째 노드를 가르킨다.
2. d_size -> 리스트의 요소 개수이다.

### single linked list의 ADT

1. S.empty() ---> Boolean 
	* 리스트가 비었으면 True, 아니면 False

1. S.size() ---> integer 
	* 리스트에 있는 요소 개수

1. S.add() ---> None 
	* 노드를 리스트의 맨 앞에 추가

1. S.search(target) ---> node 
	* 리스트에서 target을 찾는다.
	* 찾으면 노드를, 못 찾으면 None 반환

1. S.delete() ---> None 
	* 맨 앞의 노드 삭제

### single linked list 구현

```python

class SLinkedlist:
	
	def __init__(self):
		self.head = None
		self.d_size = 0
		
	def empty(self):
		if self.d_size == 0:
			return True
		else:
			return False
			
	def size(self):
		return self.d_size
		
	def add(self, data):
		new_data = Node(data)
		new_data.next = self.head
		self.head = new_data
		self.d_size += 1
		
	def search(self, target):
		cur = self.head
		while != None:
			if cur.data == target:
				print(f"찾으신 데이터:{cur.data}가 있습니다")
				return cur
			cur = cur.next
		return None
		
	def delete(self):
		self.head = self.head.next
	
def show_list(sll):
    cur = sll.head
    for _ in range(sll.size()):
        print(cur.data, end=" ")
        cur = cur.next
    
if __name__ == "__main__"
	single = SLinkedlist()
	single.add(1)
	single.add(2)
	single.add(3)
	single.add(4)
	single.add(5)
	
	print(single.size())
	single.search(3)
	show_list(single)
	
	single.delete()
	show_list(single)

5
찾으신 데이터 3가 있습니다.
1 2 3 4 5
node[5] deleted!
2 3 4 5
node[4] deleted!
node[3] deleted!
node[2] deleted!
node[1] deleted!

```

## Dummy double linked list

* dummy
	* 더미란 데이터를 가지지 않는 노드를 의미한다.
	* double linked list에서는 양 옆에 배치한다.

* 구현 이유
	* 더미가 있게 됨으로서 데이터가 있던지 없던지 상관없이 간단하게 데이터를 추가 삭제할 수 있게 되었다.원래는 뒤에 데이터가 있는지 앞에 데이터가 있는지 확인을 해야하지만 더미(head와 tail)이 있게 되므로서 상관하지 않고 구현할 수 있다.



### double linked list 노드

<img width="724" alt="노드1" src="https://user-images.githubusercontent.com/46436843/55327352-decfe300-54c4-11e9-8ef9-965a471d4554.png">

### Dummy double linked list instance Member

* instance member

    * head : 리스트 맨 앞에 있는 더미를 가르킨다.
    * tail : 리스트 맨 뒤에 있는 더미를 가르킨다.
    * d_size : 리스트의 요소 개수

* ADT

<img width="923" alt="double linked list 1" src="https://user-images.githubusercontent.com/46436843/55327732-b7c5e100-54c5-11e9-80bc-122c7e654708.png">

<img width="899" alt="double linked list 2" src="https://user-images.githubusercontent.com/46436843/55327739-bbf1fe80-54c5-11e9-8d47-174f4c357bae.png">

### Dummy double linked list 구현

```python
class Node:
	def __init__(self, data=None):
		self.__data = data
		self.__prev = None
		self.__next = None
		print(self)
	
	def __del__(self):
		print("data of {} is deleted".format(self.data))
	
	@property
	def data(self):
		return self.__data
	
	@data.setter
	def data(self, data):
		self.__data = data
	
	@property
	def prev(self):
		return self.__prev
	
	@prev.setter
	def prev(self, b):
		self.__prev = b
	
	@property
	def next(self):
		return self.__next
	
	@next.setter
	def next(self, n):
		self.__next = n
	
	def __str__(self):
		return f"created:[{self.__data}]"


class DoubleLinkedList:

	def __init__(self):
		self.head = Node()  
		self.tail = Node()
		self.d_size = 0
			
		self.head.next = self.tail
		self.tail.prev = self.head
	
	def empty(self):
		if self.d_size == 0:
		    return True
		else:
		    return False
	
	def size(self):
		return self.d_size
	
	def add_first(self, data):  
		new_node = Node(data)  
		new_node.next = self.head.next
		new_node.prev = self.head
		self.head.next.prev = new_node
		self.head.next = new_node
		self.d_size += 1

       
	def add_last(self, data):  # 항상 제일 앞에는 first가 있다고 가정한다.
		new_node = Node(data)
		new_node.next = self.tail
		new_node.prev = self.tail.prev
		self.tail.prev.next = new_node
		self.tail.prev = new_node
		self.d_size += 1

	def insert_after(self, data, node):
		new_node = Node(data)			
		new_node.next = node.next
		new_node.prev = node
		node.next.prev = new_node
		node.next = new_node
		self.d_size += 1

	def insert_before(self, data, node):
		new_node = Node(data)
		new_node.next = node
		new_node.prev = node.prev
		node.prev.next = new_node
		node.prev = new_node
		self.d_size += 1
	
	def search_forward(self, target):
		cur = self.head.next
		while cur is not self.tail:
			if cur.data == target:
				return cur
			cur = cur.next
		return None  

	def search_backward(self, target):
		cur = self.tail.prev
		while cur is not self.head:
			if cur.data == target:
				return cur
			cur = cur.prev
		return None


	def delete_first(self):
		if self.empty():
			return
		else:	    
			self.head.next = self.head.next.next
			self.head.next.prev = self.head

		self.d_size -= 1

	def delete_last(self):
		if self.d_size == 0:
			return

		self.tail.prev = self.tail.prev.prev
		self.tail.prev.next = self.tail
		self.d_size -= 1

	def delete_node(self, node):
		node.prev.next = node.next
		node.next.prev = node.prev
		
		self.d_size -= 1
    
    # 편의 함수
	def traverse(self, start=True):
		if start:
		    # 리스트의 첫 데이터부터 순회를 시작
		    cur = self.head.next
		    while cur is not self.tail:  # is는 객체 자체의 비교이다.
		        yield cur
		        cur = cur.next
		  
		else:
		    # 리스트의 마지막 데이터부터 순회
		    cur = self.tail.prev
		    while cur is not self.head:
		        yield cur  # 만약에 아래 쪽에 node.data를 안했으면 cur.data해야 한다.
		        cur = cur.prev


def show_list(d_list):
    g = dlist.traverse()
    for node in g:
        print(node.data, end=" ")
    print()


if __name__ == "__main__":
    dlist = DoubleLinkedList()
    dlist.add_first(1)
    dlist.add_first(2)
    dlist.add_first(3)
    dlist.add_first(4)
    dlist.add_first(5)

    show_list(dlist)

    # 데이터를 찾은 이유는 그 뒤에 데이터를 넣기 위해서
    searched_data = dlist.search_backward(3)
    if searched_data:
        # dlist.delete_node(searched_data)
        dlist.insert_before(15, searched_data)
        print(f"searched data:{searched_data.data}")
    else:
        print('기준 노드가 없습니다. 다시 확인해주세요.')

    show_list(dlist)
    # dlist.delete_first()
    # dlist.delete_last()
    del_data = dlist.search_forward(15)
    if del_data:
        # del_data가 15를 가르키고 있으므로 레퍼런스 카운터가 0이 안된다. 따라서 None을 해서 레퍼런스 카운터를 0으로 만든다.
        dlist.delete_node(del_data)
        del_data = None
    else:
        print("기준 노드가 없습니다. 다시 확인해주세요")
    show_list(dlist)
    print("*" * 100)
    # generator 객체


created:[None]
created:[None]
created:[1]
created:[2]
created:[3]
created:[4]
created:[5]
5 4 3 2 1 
created:[15]
searched data:3
5 4 15 3 2 1 
data of 15 is deleted
5 4 3 2 1 
****************************************************************************************************
data of None is deleted
data of 1 is deleted
data of 2 is deleted
data of 3 is deleted
data of 4 is deleted
data of 5 is deleted
data of None is deleted

```

* self를 앞에 붙이는 것은 무언가 지속적으로 남겨놓을 때 붙인다.
* 따라서 지속적으로 남겨놓을 필요가 없을 때는 붙이지 않는다.







