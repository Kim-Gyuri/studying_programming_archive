## 문제
deque는 LinkedList, ArrayList로 구현할 수 있다. <br>
```
Deque deque = new LinkedList<>();
or
Deque deque = new ArrayDeque<>();
```

+ 주어진 List 크기 N, 끊어읽을 크기 M을 받는다.
+ M크기로 잘라서 Dequeue에 저장하고, unique num 경우의 수 최대값을 찾는다.

## 풀이
> 예시
```
# Sample Input
6 3
5 3 5 2 3 2


# Sample Output
3
```

<br> 

```
# Explanation
In the sample testcase, there are 4 subarrays of contiguous numbers.
s1 <5,3,5> - Has  2 unique numbers.
s2 <3,5,2> - Has  3 unique numbers.
s3 <5,2,3> - Has  3 unique numbers.
s4 <2,3,2> - Has  2 unique numbers.
```

<br><br>


+ set을 사용한다.
+ Dequeue를 poll한 값이 Dequeue에 포함되지 않으면 Set에서 제거한다.

```
for (int i = 0; i < n; i++) {
                int num = in.nextInt();
               
                deque.add(num); 
                set.add(num);
                
                if (deque.size() == m) {
                    if (set.size() > max) max = set.size();
                    int pollNumber = (int) deque.poll();
                    if (!deque.contains(pollNumber)) set.remove(pollNumber);
                }                
            }
```            
            


## 코드
```java
    import java.util.*;
    public class test {
        public static void main(String[] args) {
            Scanner in = new Scanner(System.in);
            Deque deque = new ArrayDeque<>();
            HashSet<Integer> set = new HashSet<>();
            
            int n = in.nextInt();
            int m = in.nextInt();
            int max = Integer.MIN_VALUE;

            for (int i = 0; i < n; i++) {
                int num = in.nextInt();
               
                deque.add(num);
                set.add(num);
                
                if (deque.size() == m) {
                    if (set.size() > max) max = set.size();
                    int pollNumber = (int) deque.poll();
                    if (!deque.contains(pollNumber)) set.remove(pollNumber);
                }                
            }
            System.out.println(max);            
        }
    }
```
