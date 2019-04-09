---
layout: post
title:  "2019_03_26_TIL(process & thread)"
date:   2019-03-26 01:06:23 +0700
categories: [computer]
---

### 2019.03.26 TIL

(TIL은 스스로 이해한 것을 바탕으로 정리한 것으로 오류가 있을 수 있습니다)

\# 질문에 답하기

* program 와 process 그리고 thread
* process
    * process state
    * scheduling
        * 선점형 스케줄링(preemptive scheduling)
        * 비선점형 스케줄링
        * 구현 알고리즘
            * 우선순위 알고리즘(특징, 문제점, 해결책)
            * 라운드로빈 알고리즘(특징, 문제점, 해결책)
    * context switching
        * PCB(process control unit)
        * 문제점
* thread
    * 정의
        * process 안의 실행흐름(인스트럭션)의 단위(“프로세스 내에서 실행되는 여러 흐름의 단위”)
        * TCB
    * multi process & multi thread
    * race condition(경쟁 조건)
        * mutual exclusion(상호배제)
 

# program와 process 그리고 thread

* program : HDD에 저장되어 있는 image
* process : 실행 중인(ram에 메모리가 올라왔다) 프로그램/ 운영체제로부터 자원을 할당받는 단위
* thread : 할당받은 자원을 이용하는 실행의 단위/ 프로세스 내에서 실행되는 여러 흐름의 단위

# 프로세스

## 프로세스 스테이트(process state)

### 미리 알고 가기

* program은 1개지만 같은 프로그램을 2개 띄울 수 있음/ 단 완전히 다른 메모리를 가짐
* 더블클릭을 하면 각자의 vas(virtual address space)가 생김
* 각자만의 가상 공간이 생긴다.
* 서로 다른 공간인지 구분은 PID번호로 구분 PID(process ID) 
    * 윈도우에서는 랜덤으로 발급
    * 리눅스에서는 순서대로 발급

### process state

<img width="702" alt="process state" src="https://user-images.githubusercontent.com/46436843/54987979-e1c86080-4ff8-11e9-9dfb-8ddf53dfc25e.png">


* 우리가 더블클릭을 하는 순간 바로 램을 할당 받는 것 같지만 그렇지 않다.
    * RUN Queue로 들어간다.(priority queue 우선순위 큐)
    * 단 우선순위가 빠른 실행이 오면 제일 앞으로 온다.(heap와 트리로 구현할 것이다)
    * Run Queue로 들어간게 만약에 running에 들어있는 것보다 더 빠르면 running이 바뀐다.(디스패치)
    * running은 항상 프로세스 1개만 실행된다.(1개의 cpu기준)
    * running에 올라가 있던 애는 waiting으로 다시 내려온다.(프리엠션)
    * 연산작업이 아닌 I/O(입출력)작업을 할 때는 blocked로 내려간다.
    	* I/O(input /output)
	    	1. 파일 i/o
			2. 네트워크 통신
			3. keyboard(in) monster(out)
			4. i/o가 많이 쓰인다 i/o bound라고 하고 / cpu가 많이 쓰인다. cpu bound라고 한다.
			5. async Io 는 쓰레드를 i?
	* I/O 작업이 끝나면 다시 waiting(실행가능) 상태로 돌아가서 대기한다.


## scheduling

**정의 : 운영체제가 여러 프로세스의 cpu할당 순서를 결정하는 것**

1. 선점형 스케줄링(pre-emptive scheduling)

	* 위의 것을 지원해서 멀티테스킹이 가능하다.
	* 선점형 os(지금 돌고 있는 애를 아무때나 정지시키고 다음에 실행될 프로세스를 올림)
	    * 어떤 상황인지는 전혀 고려하지 않는다.
    * 마치 동시에 실행하고 있는 것처럼 작업을 할당(round - robin 알고리즘)
	    
2. 비선점형 스케줄링(non - pre-emptive scheduling)
	* 프로세스가 종료되거나 I/O작업에 들어가 명시적으로 cpu를 반환해야지 다음으로 넘어감

3. 스케줄링을 하는데 구현 알고리즘이 존재
    * priority algorithm
        * the higher priority, the earlier running
        * 우선 순위가 계속 낮은 아이는 할당받지 못함.  
				* 기아상태 발생 —> 일정시간 cpu를 할당받지 못하면 우선순위를 높임—> 에이징

	* round-robin algorithm (고의적으로 높게 잡지 않으면 다 비슷할 것이다)
        * if same priority (모든 프로세스에게 일정 시간 할당, 최대쟁점은 내가 아무리 중요한 일을 하고 있어도 일단 끌어 내림)
        * time slice(quantum 퀀덤) : 프로세스에서 할당하는 시간
        * sys.getswitchinterval() : 이것을 통해 time slice가 얼마인지 확인 가능
4. scheduler(job scheduler는 언제 동작하나?)
    * when
        * every time slice : 타임 슬라이스 마다 실행
        * process가 실행되었을 때, process가 끝날 때
            * round-robin을 하고 있는데 먼저 끝나버릴 때 
        * a running process 가 (I/O혹은 system(ad)) 블로킹이 걸릴 때
            * blocked가 걸리면 따로 빠져서 io작업을 한다.
            * io작업이 다 끝나면 os가 시그널을 쏘아주고 waiting으로 간다.
            * round/robin으로 내 차례가 올 때까지
            * 또는 system call이 되어도 blocked로 빠진다.
                * os kernel안에 시스템 프로그래머가 가져다가 쓸 수 있도록 열어놓은 함수가 있음 
                * ex)sleep() —> 거이 양보해
            * waiting은 언제든지 실행될 수 있고 blocked는 i/o가 끝날 때 까지 실행못함
		
## 콘텍스트 스위칭(엄청 많이 쓰는 이야기이다.)

* context는 현재 cpu의 레지스터에 있는 값들을 context라고 한다.
* 지금 실행되는 애를 내리고 지금 실행해야 될 애의 context를 스케듈러가 다시 cpu로 내릴 때를 컨텍스트 스위칭이라고 한다.

<img width="903" alt="컨텍스트 스위칭" src="https://user-images.githubusercontent.com/46436843/55041078-bd0ed000-506e-11e9-8baa-e8da354c6572.png">

* 현재 실행되고 있던 A가 cpu-bound 관련된(for문)걸 하고 있을 때 cpu의 자원을 뺏김
* 계산이 끝나고 마지막 올리기만 하면 되는 경우 쫒겨날 때 memory 상에 pcb(process control block)를 만들어서 cpu의 담겨져 있는 것을 저장한다.
* PCB(Process control unit)을 통해 컨텍스트 스위칭이 이루어진다.
	* PCB에는 프로그램 카운터, 스택 프레임의 정보(스택포인터, 프레임 포인터), 연산 중인 데이터가 저장된 범용레지스터 등이 포함된다.

### 콘텍스트 스위칭의 단점

* 자주 일어날수록 사용자는 멀티 테스킹이 동시에 일어난다고 보인다.
* 하지만 타임슬라이스를 너무 짧게 잡으면  20cycle 왕복 40cycle ~ 200cycle씩 계속 일어나면 컴퓨테 os에게 큰 부담을 주는 것이다. 짧게 잡으면 완전 멀티테스팅처럼 보일 것이고, 길게 잡으면 os에게 부담을 덜 줄 것이다. 이것을 얼마만큼 설정할지가 굉장히 중요하다.
  
# thread
* 정의
    * process 안의 실행흐름(인스트럭션)의 단위(“프로세스 내에서 실행되는 여러 흐름의 단위”)
    * TCB

* thread의 필요성
    * 인스트럭션의 나열 —> 함수 실행
    * concurrency programming (동시성 프로그래밍)
        * 언제 필요하냐? 
            * 어떤 서버에서 200g짜리 데이터를 받고, 그 데이터를 데이터 전 처리(orocessing)을 하고 분석하고 싶을 때 먼저 데이터를 다 받아야 할 것이다.
            * 이렇게 하는 것이 아니라 multi thread를 활용해 한곳에는 데이터를 받고 한 곳에서는 데이터 분석한다.

## 어떻게 구현하나?(multi process & multi thread)

* multi-process(여러개의 프로그램을 동시에 실행하여 multi로 처리한 것 처럼 보이게 한다.)
    * 치명적 단점 : shared data가 필수적으로 발생하나, 서로 다 따로 작동되므로 각 데이터를 다 따로 가지고 있어야 한다.
    * IPC (inter process co…)를 통해 데이터를 공유하여 해결할 수 있으나 힘들다.

<multi-process>
<img width="871" alt="멀티process" src="https://user-images.githubusercontent.com/46436843/55041284-b9c81400-506f-11e9-80b6-cbe3df15a9ff.png">
  
* multi - thread
    * thread는 프로세스 안에 포함된다.  쓰레드는 stack만 나누어 가질 뿐이지 code ,data, heap은 완벽히 공유한다.
    * 이렇게 되면 각각 실행하는 것을 쓰레드가 각각 맡아서 하고 나머진 다 공유한다.
    * 왜 스텍은 공유할 수 없는가?
        * 뜨레스는 인스트럭션의 나열이다. 
        * thread1 은 함수 1,2
        * thread2 은 함수 3,4 
        * 뜨레드가 스텍만 따로 가지고 있으면 함수를 나누어서 실행가능
* i/o bound한 프로그램이라면 (네트워크 관련) asynchronous I/O 비동기 i/o

<multi - tread>
<img width="852" alt="멀티thread" src="https://user-images.githubusercontent.com/46436843/55041286-bdf43180-506f-11e9-81b6-113519483ac9.png">
        
## thread 구현

1.  GIL (Global Interpreter Lock)
    1. 파이썬의 큰 단점
        1. multi 코어라도 바이트 코어의 실행을 한번에 하나의 쓰레드로 막는다.
        2. 코어가 2개라도 바이트 코어의 실행을 한 바이트로만 실행하게 한다.
        3. GIL을 아직 해결하지 못했다.
        4. 멀티쓰레드 스레딩을 해도 속도가 단축이 안된다.
        5. gil이 있기 때문에 multithreading을 해도 시간상의 성능향상은 어렵다.
2. 잘못된 생각
    1. 4sec가 걸리는 것을 1초 단위로 나누어서 실행하면 더 빠를 것 같지만 결국 4초와 맞먹는 시간이 걸린다.
    2. 스피드를 위해 concurrency를 하는 것은 아니다.
        1. for multitasking
3. single core, multi thread 일 때
    1. concurrency를 해결해주어도 스피드를 해결해 주진 않는다.
4. 듀얼 코어 , 4개 thread 일때
    1. concurrency를 해결해주고 스피드도 해결
5. 4개 코어, 4개  thread일 때
    1. concurrency이자 parellerism programming 처럼 된다.

## multi-tread의 문제 (race condition 발생)
예). 

1. 글로벌은 메모리 데이터에 저장된다.
2. sum += 1
    1. move eax, [g_num]
    2. add eax , 1
    3. move [g_num], eax
3. round-robin에 의해 덜 되었어도 스케줄러가 바꾸어 버림
4. 따라서 실행이 덜 되었는데(2번째 인스트럭션까지 실행되었는데) 넘겨지는 상황이 발생함
5. **레이스 컨디션(서로 공유자원을 쓰기 위해 경쟁한다)**

### race condition 해결책

1. mutual exclusion : 상호배제
2. 화장실 생각. 서로 들어갈려고 한다. 즉 누가 들어갔을 때 락을 걸어버린다.
3. 한번 들어갔던 애가 비워줄 때 까지 락을 걸었다가 풀었다가 한다.
4. 하지만 상호배제를 하면 쓰레드를 쓰나 마나가 되고 오히려 더 안 좋아질수도 있다.


## 참고사항
1. 코어 안에 담겨져 있는 thread와는 우리가 지금 배우는 thread와 다르다.
2. 하지만 alu를 쓰지 않고 작업하는 것은 나누어 볼 수 있지 않을까?가 thread이다.
3. 이것을 이해하기 위해서는 파이프 라이닝을 또 공부해야 한다.
