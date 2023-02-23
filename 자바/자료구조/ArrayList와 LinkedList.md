# 📌  DataStructure
![자바 자료구조 트리](https://user-images.githubusercontent.com/57389368/220826122-1bf675ed-4afb-4933-84af-3a52288491b4.PNG) <br>

<br>

+ Array vs ArrayList vs LinkedList
    + [Array](#Array)
        + 배열이란
        + 장점
        + 배열의 한계
    + [List](#List)
        + 배열과 리스트의 차이
        + 리스트의 기능
        + List로 2가지를 지원한다.
    + [ArrayList](#ArrayList)
        + ArrayList 장단점
        + Array와 차이점
        + Array와 공통점
    + [LinkedList](#LinkedList)
        + Array와 LinkedList, ArrayList 차이점
    + [요약정리](#요약정리)
    + [자바로 ArrayList 구현](#자바로-ArrayList-구현)
    + [자바로 LinkedList 구현](#자바로-LinkedList-구현)
    + [Array를 기반으로한 ArrayList](#Array를-기반으로한-ArrayList)
    + [Array를 기반으로한 LinkedList 구현](#Array를-기반으로한-LinkedList-구현)
    + [ArrayList를 기반으로한 LinkedList 구현](#ArrayList를-기반으로한-LinkedList-구현)
+ [참고자료](#참고자료) 

# Array vs ArrayList vs LinkedList
## Array
![배열](https://user-images.githubusercontent.com/57389368/220829483-a001740d-3f48-49b7-ae48-e3ad37abb390.gif) <br>
```java 
String[] student = new String[5]; 
``` 

배열이란 연관된 데이터를 하나의 변수에 그룹핑해서 관리하기 위한 방법이다.  <br>
value, index를 가진 element 단위를 가진다. <br>
초기화 시 크기를 고정한다. <br>
인덱스로 해당 원소에 접근한다. 시간 복잡도 O(1) <br>
삭제할 때, 먼저 해당 원소에 접근해야 한다. 
> 삭제하면 "배열 연속성"이 깨지게 된다. (삭제하면 빈공간으로 남는다.) <br> 시간 복잡도는 O(n)

### 장점
데이터가 많은 경우, 여러 데이터를 하나의 이름으로 그룹핑해서 관리하기 위한 구조이다.

### 배열의 한계
크기가 한정적이다 (정해진 크기) <br>
원소 추가/삭제 기능이 없다. 

> 사실 "크기가 한정적이다"라는 점은 장점이 되기도 한다. <br>
> 사실 좋은 부품의 조건이다. "작고 가볍다" "단순하다" 라는 특징이기 때문이다.

## List
### 배열과 리스트의 차이
`삭제` <br>
List는 삭제 시, 해당 element는 삭제되고 다음 값이 이어진다. (공간 변화가 있다.) <br>
Array는 삭제 시, 해당 element 공간이 빈공간으로 남는다. (공간 변화가 없다.) <br>

`조회` <br>
index, element로 해당 원소에 접근 가능하다. <br>
삭제하면, 해당 element가 삭제되고 다음값이 이어진다. (공간 변화가 있다.)

> `Array의 단점` <br>
> 인덱스를 이용해서 데이터를 가져오려면 데이터에 대한 인덱스의 값이 고정되어야 한다. <br> 자연스럽게 어떤 엘리먼트가 삭제되면 삭제된 상태를 빈 공간으로 된다. <br>
> 메모리의 낭비가 된다. <br> 또한 배열에 데이터가 있는지 없는지를 체크하는 로직이 필요하다는 의미이기도 하다.

### 리스트의 기능
처음/끝/중간에 element를 추가/삭제할 수 있다. <br>
모든 데이터에 접근 가능하다. <br>
리스트에 해당 데이터가 있는지 조회 가능하다. <br>

> `언어별 비교` <br>
> c 언어 : List를 지원하지 않는다. 배열은 있다. (List를 직접 구현해야 한다.) 
> javascript : 배열이 리스트이기도 하다. <br>
> python : 리스트가 배열이다. <br>
> <br>
> -> 요약하자면, "최근 언어는 리스트를 기본적으로 지원한다."

### List로 2가지를 지원한다.
+ LinkedList
+ ArrayList

> `조회 속도 차이가 있다.` <br>
> LinkedList : head부터 해당 데이터까지 찾아가야 한다. <br>
> ArrayList : 인덱스로 바로 찾는다. <br>

## ArrayList
![ArrayList](https://user-images.githubusercontent.com/57389368/220837079-aa67b56a-aa4a-4fb1-ad3f-0b594b422af3.png) <br>

```java  
ArrayList<Integer> numbers = new ArrayList<>();
```

List 인터페이스를 상속받은 클래스 <br>
크기가 가변적인 선형 리스트 <br> 
index로 객체 관리한다. <br>
생성 시, 타입을 명시해야 한다. (Generics 개념)
> `Generics` <br> 사용하려는 ArrayList가 내부적으로 element들이 숫자형(Integer) 이라고 미리 정한다.
   

### ArrayList 장단점
배열을 이용해서 리스트를 구현한 것 <br>
내부적으로 배열을 이용하기 때문에 인덱스를 이용해서 접근하는 것이 빠르다.  <br>
하지만 데이터의 추가와 삭제가 느리다. 

> add(index, value) : 해당 index에 element를 추가하기 <br>
> add(value) : 맨 뒤에 element를 추가하기 <br>

### Array와 차이점
초기화 시 크기 지정없이 동적 삽입/삭제 가능하다. (크기 가변적이다) <br>
삭제했다면, 그 만큼 이동시켜야 한다. (데이터가 많을수록 비효율적) <br>

### Array와 공통점
index로 조회 가능하다.

## LinkedList 
![LinkedList](https://user-images.githubusercontent.com/57389368/220838074-de47095c-551c-49b4-b55b-11a2f4cdd08c.png) <br>

데이터, 포인터를 가지고 한 줄로 연결된 자료구조 (이전 노드<->다음노드를 연결한다.) <br>
자기 자신 다음 원소를 기억한다. <br>
원소 추가/삭제할 때 해당 원소값만 변경해주면 된다. 시간 복잡도는 O(1) <br>
조회 시, head 원소부터 해당 원소까지 순차탐색해야 한다. 시간복잡도 O(n) <br>


### Array와 LinkedList, ArrayList 차이점
`조회` <br>
Array는 index로 해당 원소를 찾는다. <br>
LinkedList는 head 원소부터 해당 원소까지 순회하며 찾아야 한다.  <br>
(Array를 보완한 사항) : 양방향 연결 리스트이기 때문에 "정방향" "역순" 순회 가능하다. <br>

`삭제/추가` <br>
LinkedList는 앞 뒤 포인터만 변경된다. (ArrayList는 인덱스 변화가 있어, 원소이동변화가 있다.) <br>

`구조` <br>
ArrayList는 내부에 배열을 사용한다. <br>
LinkedList는 객체를 만들고 객체와 객체를 연결(래퍼런스)하여 구현한다. <br>



## 요약정리
### Array
인덱스로 해당 원소에 접근할 수 있다. 시간 복잡도 O(1)
삭제 또는 삽입의 과정에서는 해당 원소에 접근하여 작업을 완료한 뒤(O(1)), 또 한 가지의 작업을 추가적으로 해줘야 하기 때문에, 시간 복잡도는 O(n)가 된다.
> 원소 추가/삭제 시, 추가 작업 :모든 원소들의 인덱스를 1 씩 shift 해줘야 한다.

### Linked List
`장점` <br>
Array의 추가/삭제 작업 복잡도를 해결하기 위한 자료구조다. <br>
해당 값만 수정하면 되므로, 삭제/추가 과정의 시간 복잡도는 O(1)이 된다. <br>

`단점` <br>
해당 원소를 조회하는 과정에 있어서 첫번째 원소(head)부터 다 확인해야 한다. 시간 복잡도 O(n)

`자료구조 특징` <br>
 LinkedList는 Tree 구조의 근간이 되는 자료구조이며, Tree에서 사용되었을 때 그 유용성이 드러난다.

## 자바로 ArrayList 구현
[코드 링크](https://github.com/Kim-Gyuri/studying_programming_archive/blob/main/%EC%9E%90%EB%B0%94/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0/%EA%B5%AC%ED%98%84%20%EC%BD%94%EB%93%9C/%EC%9E%90%EB%B0%94%EB%A1%9C%20ArrayList%EB%A5%BC%20%EA%B5%AC%ED%98%84.md)
## 자바로 LinkedList 구현
[코드 링크](https://github.com/Kim-Gyuri/studying_programming_archive/blob/main/%EC%9E%90%EB%B0%94/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0/%EA%B5%AC%ED%98%84%20%EC%BD%94%EB%93%9C/%EC%9E%90%EB%B0%94%EB%A1%9C%20LinkedList%EB%A5%BC%20%EA%B5%AC%ED%98%84.md)

## Array를 기반으로한 LinkedList 구현
[코드 링크](https://github.com/Kim-Gyuri/studying_programming_archive/blob/main/%EC%9E%90%EB%B0%94/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0/%EA%B5%AC%ED%98%84%20%EC%BD%94%EB%93%9C/Array%20%EB%A5%BC%20%EA%B8%B0%EB%B0%98%EC%9C%BC%EB%A1%9C%ED%95%9C%20Linked%20List%20%EA%B5%AC%ED%98%84.md) 

## ArrayList를 기반으로한 LinkedList 구현
[코드 링크](https://github.com/Kim-Gyuri/studying_programming_archive/blob/main/%EC%9E%90%EB%B0%94/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0/%EA%B5%AC%ED%98%84%20%EC%BD%94%EB%93%9C/ArrayList%EB%A5%BC%20%EA%B8%B0%EB%B0%98%EC%9C%BC%EB%A1%9C%ED%95%9C%20Linked%20List%20%EA%B5%AC%ED%98%84.md)

## 참고자료
[Computer Science/Data structure 글을 참고했다.](https://dev-coco.tistory.com/19)<br>
[생활코딩 자료구조 글을 참고했다.](https://opentutorials.org/module/1335) <br>
[Interview_Question 글을 참고했다.](https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/master/DataStructure) <br>
[Java - 문자열(String)을 비교하는 방법](https://codechacha.com/ko/java-string-compare/) <br>

