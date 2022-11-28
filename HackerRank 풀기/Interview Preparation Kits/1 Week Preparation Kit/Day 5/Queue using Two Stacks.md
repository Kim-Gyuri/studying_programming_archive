## 문제
두개의 스택을 이용해서 큐를 구현하기

## 풀이
+ 스택 두 개로 데이터를 움직이는데, 최소한의 움직임으로 큐를 구현해야 한다.
+ s1은 데이터를 입력받기용, s2는 pop, peek을 통해 다른 스택으로 모든 데이터를 옮기는 용도로 사용한다. 
+ 이후 q=2,3을 실행해야 하는 경우, 다른쪽 스택의 데이터를 순차적으로 빼거나 출력하면 된다.

> `참고 사이트` [스택을 사용하는 이유 - 그림설명](https://levelup.gitconnected.com/queue-using-stacks-leetcode-972bd1d53a9c) <br>
> `참고 코드` [참고한 코드 - all case pass](https://www.hackerrank.com/challenges/one-week-preparation-kit-queue-using-two-stacks/forum/comments/1117390?h_l=interview&isFullScreen=true&playlist_slugs%5B%5D=preparation-kits&playlist_slugs%5B%5D=one-week-preparation-kit&playlist_slugs%5B%5D=one-week-day-five) <br>

<br> <br>

### 코드 스케치
```
# MyQueue 클래스 생성
class My Queue {               #입력 중 값이 longest이 있음 -> Long타입으로 선언한다.
 Stack<Long> s1 = Stack<>();   #s1,s2 생성
 Stack<Long> s2 = Stack<>();
 
class MyQueue {
    Stack<Long> s1 = new Stack<>();
    Stack<Long> s2 = new Stack<>();
    
    void enqueue(Long v) { # q=1 경우, s1에 입력을 받는다.
       s1.push(value);
    }
    
    Long dequeue() {      # q=2 경우, s2에서 값 꺼내버리기
      if (s2가 비었다면) {
        for (s1 크기만큼 반복) {
            s2.push(s1에서 pop한 값);
         }
      }
      return s2.pop();  #s1 -> s2 옮긴 후 꺼낼 때 FIFO이 적용됨
     }
     
     Long print() {     # q=3 경우, s2에 top값부터 1개를 출력한다.
      if (s2에 값이 있다면) {
        return s2.peek();
     } else {
      moveAll();   #s2가 비었으니, s1에서 모두 옮긴 후 다시 s2에서 하나를 출력한다.
      return s2.peek();
     }
     
     void moveAll() {   #moveAll를 만들어, null예외처리해결
       if (s1에 값이 남아 있다면) {
          for (남아 있는 만큼 반복) {
              s2.push(s1.pop());
           }
       }
     }




# 실행코드
MyQueue queue = new MyQueue();
while (총 q만큼 반복) {
  if (q=1) queue.enqueue(); -> 각 쿼리별 실행처리 1:입력 2:빼기 3:출력
  else if (q=2) queue.dequeue();
  else if (q=3) queue.print();
```


## 코드
```java
import java.io.*;
import java.util.*;

class MyQueue {
    Stack<Long> s1 = new Stack<>();
    Stack<Long> s2 = new Stack<>();
    
    public void enqueue(Long value) {
        s1.push(value);
    }
    
    public Long dequeue() {
        if(s2.size()==0) {
            int count = s1.size();
            for (int i=0; i<count; i++) {
                Long value = s1.pop();
                s2.push(value);
            }
        }
        return s2.pop();
    }
    
    public Long print() {
        if(s2.size()!=0) {
            return s2.peek();
        } else {
            moveAll();
            return s2.peek();
        } 
    }
    
    public void moveAll() {
        if (s1.size() > 0 ) {
            int count = s1.size();
            for (int i=0; i<count; i++) {
                Long value = s1.pop();
                s2.push(value);
            }
        }
    }
}


public class Solution {

    public static void main(String[] args) throws IOException {
        /* Enter your code here. Read input from STDIN. Print output to STDOUT. Your class should be named Solution. */
        Scanner sc = new Scanner(System.in);
        
        int queries = sc.nextInt();
        
        MyQueue queue = new MyQueue();
        while(queries-- > 0) {
            int q = sc.nextInt();
            if (q == 1) {
                queue.enqueue(sc.nextLong());
            } else if (q == 2) {
                queue.dequeue();
            } else if (q == 3) {
                System.out.println(queue.print());
            }
           
        }
        
    }
}
```
