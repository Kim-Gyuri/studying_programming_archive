# 📌 Hash Table
+ HashTable란?
+ Hash Function
+ 충돌(Collision)
+ 충돌 문제를 해결하기 위해 나온 방법은?
+ Open Address 방식 (개방주소법)
+ Separate Chaining 방식 (분리 연결법)

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

### 인덱스로 접근
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

<br>

#### 3가지 방법
 `1` Linear Probing 순차적으로 탐색하며 비어있는 버킷을 찾을 때까지 계속 진행된다. <br>
 `2` Quadratic probing 2차 함수를 이용해 탐색할 위치를 찾는다. <br>
 `3` Double hashing probing 하나의 해쉬 함수에서 충돌이 발생하면 2차 해쉬 함수를 이용해 새로운 주소를 할당한다. (위 2가지 방법에 비해 많은 연산량을 요구하게 된다.)

## Separate Chaining 방식 (분리 연결법)
