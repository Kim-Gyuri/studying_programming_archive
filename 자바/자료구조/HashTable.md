# 📌 Hash Table
+ HashTable란?
+ Hash Function
+ 충돌(Collision)
+ 충돌 문제를 해결하기 위해 나온 방법은?
+ Open Address 방식 (개방주소법)
+ Separate Chaining 방식 (분리 연결법)
+ Open Address vs Separate Chaining
+ 보조 해시 함수(supplement hash function)
+ 해시 버킷 동적 확장(Resize)
+ 참고자료

HashTable을 정리하기 전에, 우선 `배열과 해시태이블의 차이점`을 정리했다.

## HashTable란?
![해시 테이블 예제](https://user-images.githubusercontent.com/57389368/221553978-e5e99108-1e3f-45de-9cbb-db9f3f7517e8.png) <br>
해시 테이블(Hash Table) 은 키(key)와 값(value)이 하나의 쌍을 이루는 데이터 구조이다. <br> 즉, `키`(key)와 `배열의 인덱스(index)`를 이용하여 키를 저장하는 자료구조이다.  <br>
먼저 이해해야 할 개념은 `hashing`과 `해시 함수(hash function)`이다.


## Hash Function
<img width="615" alt="해시 테이블 개념" src="https://user-images.githubusercontent.com/57389368/221520004-412a51cb-eb09-4dff-b419-a1bce887b481.png"> <br>

해시 테이블은 키를 해시 함수(hash function) 로 계산하여 그 값을 배열의 인덱스로 사용한다. <br> 이때, 해시 함수에 의해 반환된 데이터의 고유 숫자 값을 해시값(Hash Value) 또는 `해시코드(hashcode)`라고 한다. <br>
즉, key 값을 hash function을 통해서 배열의 인덱스로 바꿔주고, 그 자리에 데이터를 저장한다.
> ![해시 함수(hash function) ](https://user-images.githubusercontent.com/57389368/221519914-9afc9612-5d42-4d19-ac12-111b61140e8f.png) <br>

#### 인덱스로 접근
인덱스만 알면 해시 테이블이 아무리 커도 데이터에 빠르게 접근할 수 있다. <br> (배열과 유사하다.) 따라서 데이터에 접근하는 경우 시간복잡도는 O(1) <br>
메모리를 미리 많이 할당해 두어야 하기 때문에 공간효율적이라고 보기가 어렵다. <br>
#### 해시 테이블에도 단점
해시 함수가 서로 다른 두 개의 키에 대해 동일한 해시값을 나타내는 `충돌(collision)현상`이 일어난다는 것이다. 

## 충돌(Collision)
<img width="529" alt="테이블 충돌현상" src="https://user-images.githubusercontent.com/57389368/221520092-69766403-31a5-479a-95d3-d23e963969a2.png"> <br>
서로 다른 문자열이 Hash function을 통해 Hash한 결과가 같은 경우(=중복되는 경우)이다. <br> 충돌을 줄여주는 좋은 해시 함수를 사용하는 것이 좋다. <br>
왜냐하면 충돌이 많아질 수록 탐색의 시간 복잡도가 O(1)에서 O(n)에 가까워지기 때문이다.

## 그렇다면 좋은 hash function는 어떠한 조건들을 갖추고 있어야 하는가?
일반적으로 좋은 hash function는 키의 일부분을 참조하여 해쉬 값을 만들지 않고 키 전체를 참조하여 해쉬 값을 만들어 낸다. <br>
하지만 좋은 해쉬 함수는 키가 어떤 특성을 가지고 있느냐에 따라 달라지게 된다. <br><br>

hash function를 무조건 1:1 로 만드는 것보다 Collision 을 최소화하는 방향으로 설계하고 발생하는 Collision 에 대비해 어떻게 대응할 것인가가 더 중요하다.  <br>
> 1:1 대응이 되도록 만드는 것이 거의 불가능하기도 하지만 <br> 그런 hash function를 만들어봤자 그건 array 와 다를바 없고 메모리를 너무 차지하게 된다. <br>

## 충돌 문제를 해결하기 위해 나온 방법은?
해시 충돌이 발생하더라도 키-값 쌍 데이터를 잘 저장하고 조회할 수 있게 하는 방식에는 대표적으로 두 가지가 있는데,  <br> 하나는 Open Addressing이고, 다른 하나는 Separate Chaining이다. <br>
![충돌 해결방법 가지](https://user-images.githubusercontent.com/57389368/221520155-e5a8a0a2-9090-47c1-8bd7-fa694f94f40e.png) <br>

## Open Address 방식 (개방주소법)
이 방법은 인덱스에 대한 충돌 처리에 대해서 Linked List와 같은 추가적인 메모리 공간을 사용하지 않고, <br> hash table array의 빈공간을 사용하는 방법이다. <br>
해시 충돌이 발생하면, (즉 삽입하려는 해시 버킷이 이미 사용 중인 경우) 다른 해시 버킷에 해당 자료를 삽입하는 방식이다. <br>
> 버킷이란 바구니와 같은 개념으로 데이터를 저장하기 위한 공간이라고 생각하면 된다.

<br>

#### 다른 해시 버킷이란 어떤 해시 버킷을 말하는 것인가?
Collision 이 발생하면 데이터를 저장할 장소를 찾아 헤맨다. <br> Worst Case 의 경우 비어있는 버킷을 찾지 못하고 탐색을 시작한 위치까지 되돌아 올 수 있다. 

#### 3가지 방법
 `1` Linear Probing 방식: 순차적으로 탐색하며 비어있는 버킷을 찾을 때까지 계속 진행된다. <br>
 `2` Quadratic probing 방식: 2차 함수를 이용해 탐색할 위치를 찾는다. <br>
 `3` Double hashing probing 방식: 하나의 해쉬 함수에서 충돌이 발생하면 2차 해쉬 함수를 이용해 새로운 주소를 할당한다.
 > 방법3은 많은 연산량이 요구된다.

## Separate Chaining 방식 (분리 연결법)
이 방법은 JDK내부에서도 사용하고 있는 충돌 처리 방식인데, LinkedList뿐만이 아니라 Tree(Red-Black Tree)를 사용하기도 한다. <br>
두 개를 사용하는 기준은 data가 6개 이하이면 LinkedList를 사용하고 8개 이상이면 Tree를 사용한다. <br>

### 연결 리스트를 사용하는 방식(LinkedList) 
각각의 버킷(bucket)들을 연결리스트(LinkedList)로 만들어 Collision이 발생하면 해당 bucket의 list에 추가하는 방식이다. <br> 
#### 장점
연결 리스트의 특징을 그대로 이어받아 삭제 또는 삽입이 간단하다.  <br>
버킷을 계속해서 사용하는 Open Address 방식에 비해 테이블의 확장을 늦출 수 있다.
#### 단점
작은 데이터들을 저장할 때 연결 리스트 자체의 오버헤드가 부담이 된다.

### Tree를 사용하는 방식 (Red-Black Tree)
기본적인 알고리즘은 Separate Chaining 방식과 동일하며 연결 리스트 대신 트리를 사용하는 방식이다. <br>
"연결 리스트를 사용할 것인가?"와 "트리를 사용할 것인가?"에 대한 기준은 하나의 해시 버킷에 할당된 key-value 쌍의 개수이다. <br> 
데이터의 개수가 적다면 링크드 리스트를 사용하는 것이 맞다. 

#### 장점
데이터의 개수가 많을 때 사용하기 좋다, (트리는 기본적으로 메모리 사용량이 많기 때문이다.) <br> 
> 데이터 개수가 적을 때 Worst Case 를 살펴보면 트리와 링크드 리스트의 성능 상 차이가 거의 없다.  <br>
  따라서 메모리 측면을 봤을 때 데이터 개수가 적을 때는 링크드 리스트를 사용한다.

#### 데이터가 적다는 것은 얼마나 적다는 것을 의미하는가? 
앞에서 말했듯이 기준은 하나의 해시 버킷에 할당된 key-value 쌍의 개수이다. <br>
이 키-값 쌍의 개수가 6개, 8개를 기준으로 결정한다.  <br>
data가 6개 이하이면 linked list를 사용하고 <br>
data가  8개 이상이면 tree를 사용한다. <br>

> "왜 7개가 아니냐?" 라는 의문이 들 수 있다. <br> 
만약 7개일 때, 데이터를 삭제하게 되면 linked list로 바꿔야 하고 <br> 
만약 추가되면 tree로 바꿔야한다.  <br>
이때 바꾸는데 오버헤드가 있기 때문에 기준이 6, 8이다. <br>


<br><br>

> 이때 사용하는 트리는 Red-Black Tree인데,  <br>
Java Collections Framework의 TreeMap과 구현이 거의 같다.  <br>
트리 순회 시 사용하는 대소 판단 기준은 해시 함수 값이다.  <br>
해시 값을 대소 판단 기준으로 사용하면 Total Ordering에 문제가 생기는데, <br> 
Java 8 HashMap에서는 이를 tieBreakOrder() 메서드로 해결한다. <br>


## Open Address vs Separate Chaining
+ 일단 두 방식 모두 Worst Case에서 O(N)이다. 
+ 하지만 Open Address방식은 연속된 공간에 데이터를 저장하기 때문에 Separate Chaining에 비해 캐시 효율이 높다. <br> (따라서 데이터의 개수가 충분히 적다면 Open Address방식이 Separate Chaining보다 더 성능이 좋다.)
+ Separate Chaining 방식에 비해 Open Address방식은 버킷을 계속해서 사용한다. <br> (따라서 Separate Chaining 방식은 테이블의 확장을 보다 늦출 수 있다.)

> 배열의 크기가 커질수록(M 값이 커질수록) 캐시 효율이라는 Open Addressing의 장점은 사라진다. <br>
> 배열의 크기가 커지면, L1, L2 캐시 적중률(hit ratio)이 낮아지기 때문이다.

<br>

+ 일반적으로 Open Addressing은 Separate Chaining보다 느리다. <br> (Open Addressing의 경우 해시 버킷을 채운 밀도가 높아질수록 Worst Case 발생 빈도가 더 높아지기 때문이다.)
+ 반면 Separate Chaining 방식의 경우 해시 충돌이 잘 발생하지 않도록 '조정'할 수 있다면 Worst Case 또는 Worst Case에 가까운 일이 발생하는 것을 줄일 수 있다.
> "해시 버킷 조정" 에 대해서는 "보조 해시 함수" 설명을 참고하자.

## 보조 해시 함수(supplement hash function)
보조 해시 함수의 목적은 key의 해시 값을 변형하여 해시 충돌 가능성을 줄이는 것이다.  <br> 
Separate Chaining 방식을 사용할 때 함께 사용되며 보조 해시 함수로 Worst Case 에 가까워지는 경우를 줄일 수 있다.

## 해시 버킷 동적 확장(Resize)
해시 버킷의 개수가 적다면 메모리 사용을 아낄 수 있지만 해시 충돌로 인해 성능상 손실이 발생한다. <br>
Separate changing에 경우, 버킷이 일정 수준으로 차 버리면 각 버킷에 연결되어 있는 List의 길이가 늘어나기 때문에, <br> 검색 성능이 떨어지기 때문에 버킷의 개수를 늘려줘야 한다.  또한 Open addressing의 경우, 고정 크기 배열을 사용하기 때문에 데이터를 더 넣기 위해서는 배열을 확장해야 한다. 이를 리사이징(Resizing)이라고 한다. <br><br>

보통 두 배로 확장하는데, 확장하는 임계점은 현재 데이터 개수가 해시 버킷의 개수의 75%가 될 때이다. <br>
0.75라는 숫자는 load factor 라고 불린다. <br> 리사이징은 더 큰 버킷을 가지는 array를 새로 만든 다음에, 다시 새로운 array에 hash를 다시 계산해서 복사해줘야 한다.

## 참고 자료
+ [Java HashMap은 어떻게 동작하는가? 글 참고](https://d2.naver.com/helloworld/831311)
+ [면접 준비 글 - DataStructure 글 참고](https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/master/DataStructure)
+ [DS #1. 배열과 해시테이블 (Array and HashTable) 글 참고](https://devowen.com/209)
+ [Java 배열과 배열리스트, 연결리스트에 대한 글 참고](https://mong9data.tistory.com/132)
+ [Hash/HashTable/HashMap 글 참고](https://hee96-story.tistory.com/48)
+ [해시 함수와 충돌문제 해결방법 대하여 글 참고](https://yoongrammer.tistory.com/82#%EA%B0%9C%EB%B0%A9_%EC%A3%BC%EC%86%8C%EB%B2%95_(Open_Addressing))
+ [hashcode란? hashcode와 equals의 관계](https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=travelmaps&logNo=220930144030)
