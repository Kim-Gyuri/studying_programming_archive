# 📌 Stack과 Queue
![자료구조 전체](https://user-images.githubusercontent.com/57389368/221125912-cc3008af-5aea-407f-9225-d36024ec233f.png)

+ Stack
  + Stack 선언
  + Stack 사용법
  + Stack의 활용 예시
  + Stack의 실제 사용 예시
+ Queue
  + Queue 선언
  + Queue 사용법  
  + Queue의 활용 예시
  + Queue의 실제 사용 예시

+ 구현
  + Stack을 사용하여 미로찾기 구현하기
  + Queue를 사용하여 Heap 자료구조 구현하기
  + Stack 두 개로 Queue 자료구조 구현하기
  + Stack으로 괄호 유효성 체크 코드 구현하기

+ 참고자료


## Stack
+ 선형 자료구조의 일종 <br>
+ Last In First Out (LIFO) - 나중에 들어간 원소가 먼저 나온다. 
+ 여기서 비어있는 스택에서 원소를 추출하려고 할 때 stack underflow.
+ 스택이 넘치는 경우 stack overflow.
+ 삽입하는 연산을 push()
+ 삭제하는 연산을 pop()
> 차곡차곡 쌓이는 구조로 먼저 Stack 에 들어가게 된 원소는 맨 바닥에 깔리게 된다. <br> 그렇기 때문에 늦게 들어간 원소는 그 위에 쌓이게 되고, <br> 호출 시 가장 위에 있는 원소가 호출되는 구조이다.

### Stack 선언
사용할 타입을 넣어 선언한다. <br>
```java
import java.util.Stack; //import
Stack<Integer> stack = new Stack<>(); //int형 스택 선언
Stack<String> stack = new Stack<>(); //char형 스택 선언
```

### Stack 사용법
+ 값 추가 : push(value)
+ 값 삭제 : pop() 또는 clear()
+ 값 출력 : peek() 
> pop() : 가장 위쪽에 있는 원소를 제거 <br>
> clear() : 스택의 값을 모두 제거 <br>
> peek() : 스택의 가장 위에 있는 값을 출력 <br>
 
### Stack의 활용 예시
+ 그래프의 깊이 우선 탐색(DFS) 구현
+ 웹 브라우저 방문기록 (뒤로 가기) 
+ 실행 취소 (undo)
+ 수식의 괄호 검사 (연산자 우선순위 표현을 위한 괄호 검사)

### Stack의 실제 사용 예시
자바의 Stack 메모리 영역
> 지역변수와 매개변수 데이터 값이 저장되는 공간이며, 메소드 호출시 메모리에 할당되고 종료되면 메모리가 해제된다. <br>
> 시스템 해킹에서 버퍼오버플로우 취약점을 이용한 공격을 할 때 스택 메모리의 영역에서 한다. <br>

## Queue
+ 선형 자료구조
+ First In First Out (FIFO).  먼저 들어간 원소가 먼저 나온다. <br> 큐는 한쪽 끝에서 삽입이, 다른 쪽 끝에서 삭제가 양쪽으로 이루어진다.
+ EnQueue : Queue 맨 뒤에 데이터 추가
+ DeQueue : Queue 맨 앞쪽의 데이터 삭제

### Queue 선언
자바에서 Queue는 LinkedList를 활용하여 생성해야 합니다.
> 참고로 Java Collection에서 Queue는 인터페이스이다. 이를 구현하고 있는 PriorityQueue<> 등을 사용할 수 있다.
```java
import java.util.LinkedList; //import
import java.util.Queue; //import
Queue<Integer> queue = new LinkedList<>(); //int형 queue 선언, linkedlist 이용
Queue<String> queue = new LinkedList<>(); //String형 queue 선언, linkedlist 이용
```

### Queue 사용법

+ 값 추가 : add(value) 또는 offer(value)
> add(value)는, 만약 삽입에 성공하면 true를 반환하고, Queue에 여유 공간이 없어 삽입에 실패하면 IllegalStateException 발생. <br>

+ 값 삭제 : poll()이나 remove()
> poll()는 Queue가 비어있으면 null을 반환합니다.  <br> 
> remove()는 가장 앞쪽에 있는 원소의 값을 제거. <br>
> clear()는 Queue의 모든 요소를 제거  <br>

### Queue의 활용 예시
> 큐는 주로 데이터가 입력된 시간 순서대로 처리해야 할 필요가 있는 상황에 이용된다.

+ 프로세스 관리
+ 너비 우선 탐색(BFS, Breadth-First Search) 구현
+ 캐시(Cache) 구현
+ 컴퓨터 버퍼에서 주로 사용된다. (많은 데이터 입력인 경우, 버퍼(큐)를 만들어 대기 시켰다가 처리한다.)


### Queue의 실제 사용 예시
Queue는 OS의 스케쥴러
> 자원의 할당과 회수를 하는 스케쥴러 역할을 큐가 할 수 있다. <br> 메모리에 적재된 다수의 프로세스 중 어떤 프로세스에게 자원을 할당할 것인가, <br>
> 그 순서를 결정하는 것이 자원의 효율적인 사용에 있고, <br> 가장 단순한 형태의 스케쥴링 정책이 선입선처리(FCFS, First Come First Served) 즉, 큐라고 볼 수 있다.

## 구현
#### [Stack을 사용하여 미로찾기 구현하기](https://github.com/Kim-Gyuri/studying_programming_archive/blob/main/%EC%9E%90%EB%B0%94/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0/%EA%B5%AC%ED%98%84%20%EC%BD%94%EB%93%9C/Stack%20%EC%9D%84%20%EC%82%AC%EC%9A%A9%ED%95%98%EC%97%AC%20%EB%AF%B8%EB%A1%9C%EC%B0%BE%EA%B8%B0%20%EA%B5%AC%ED%98%84%ED%95%98%EA%B8%B0.md)
#### [Queue를 사용하여 Heap 자료구조 구현하기](https://github.com/Kim-Gyuri/studying_programming_archive/blob/main/%EC%9E%90%EB%B0%94/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0/%EA%B5%AC%ED%98%84%20%EC%BD%94%EB%93%9C/Queue%EB%A5%BC%20%EC%82%AC%EC%9A%A9%ED%95%98%EC%97%AC%20Heap%20%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0%20%EA%B5%AC%ED%98%84%ED%95%98%EA%B8%B0.md) 
#### [Stack 두 개로 Queue 자료구조 구현하기](https://github.com/Kim-Gyuri/studying_programming_archive/blob/main/%EC%9E%90%EB%B0%94/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0/%EA%B5%AC%ED%98%84%20%EC%BD%94%EB%93%9C/%EB%91%90%20%EA%B0%9C%EC%9D%98%20stack%EC%9C%BC%EB%A1%9C%20Queue%EB%A7%8C%EB%93%A4%EA%B8%B0.md)
#### [Stack으로 괄호 유효성 체크 코드 구현하기](https://github.com/Kim-Gyuri/studying_programming_archive/blob/main/%EC%9E%90%EB%B0%94/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0/%EA%B5%AC%ED%98%84%20%EC%BD%94%EB%93%9C/Stack%EC%9C%BC%EB%A1%9C%20%EA%B4%84%ED%98%B8%20%EC%9C%A0%ED%9A%A8%EC%84%B1%20%EC%B2%B4%ED%81%AC%20%EC%BD%94%EB%93%9C%20%EA%B5%AC%ED%98%84%ED%95%98%EA%B8%B0.md)

## 참고자료
[스택과 큐 참고글1](https://dev-coco.tistory.com/16) <br>
[스택과 큐 참고글2](https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/master/DataStructure#array-vs-linked-list) <br>
[큐 사용법](https://coding-factory.tistory.com/602) <br>
