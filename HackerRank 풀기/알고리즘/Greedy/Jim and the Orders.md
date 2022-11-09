## 문제
짐스 버거에는 배고픈 고객들이 줄을 잇고 있다.  <br>
각 고객에 대한 주문 번호와 준비 시간을 비교하여, 고객이 주문을 받는 순서를 출력한다. <br>
만약, 두 명 이상의 고객이 동시에 햄버거를 받는다면, 그들의 번호를 오름차순으로 출력한다. 

### Sample 
```
#Input 0
3
1 3
2 3
3 3


# Output 0
1 2 3



# Explanation 
order[1] = 1, prep[1] =3  --> 준비시간 t: 1+3= 4
order[2] = 2, prep[1] =3  --> 준비시간 t: 2+3= 5
order[3] = 3, prep[1] =3  --> 준비시간 t: 3+3= 6

준비시간 오름차순으로 정렬하면, order 1,2,3순으로 음식을 전달하면 된다.
```

## 제출한 코드 1
#### 풀이
```
(1)  Map<Integer, Integer> orderInfo = new HashMap<>();
Map<>에 주문번호와 준비시간을 저장한다.

(2) 정렬
- List를 정렬하는데, 오름차순으로 정렬할 경우, Map.Entry.comparingByValue()를 comparator로 넘겨준다.
- map(Map.Entry::getKey): map의 key값을 통해서 value를 리스트로 반환한다.
                .sorted(Map.Entry.comparingByValue())
                .map(Map.Entry::getKey) 
                .collect(Collectors.toList());
```

#### 코드
```java
class Result {
     //...
    public static List<Integer> jimOrders(List<List<Integer>> orders) {
    // Write your code here

        Map<Integer, Integer> customerOrders = new HashMap<>();
        int customer = 1;

        for (List<Integer> order : orders) {
            customerOrders.put(customer, order.get(0) + order.get(1));
            customer++;
        }

        return customerOrders
                .entrySet()
                .stream()
                .sorted(Map.Entry.comparingByValue())
                .map(Map.Entry::getKey)
                .collect(Collectors.toList());
    }
}

```

## 제출한 코드 2
#### 풀이
```
(1) 주문 정보에 대한 클래스를 따로 만든다.
class CustomerOrder implements Comparable<CustomerOrder> {...}
변수로 주문번호, 준비 시간를 갖고, compareTo()를 오버라이딩 해준다.
compareTo(): if 준비시간이 같다면 주문번호를 비교한다. else 준비시간을 비교한다.


(2) private static List<Integer> jimOrders(List<List<Integer>> orders) {..}
# 우선적으로 뽑힐 주문번호를 고른다는 의미로, 우선순위 큐를 사용했다. 
( 입력받은 값 orders을 주문 정보로 변환해 큐에 저장한다.)
 PriorityQueue<CustomerOrder> orderCheck = new PriorityQueue<>();

# 반환할 결과를 저장할 리스트를 선언한다. 
 List<Integer> results = new ArrayList<>();

# priorityQueue에 첫번째 값을 반환하고 제거한다.
(이미 CustomerOrder compareTo()로 정렬이 되었으므로, 값을 result에 순서대로 넣으면 된다.)
results.add(orderCheck.poll().orderId);
```

#### 코드
```java
import java.io.*;
import java.math.*;
import java.security.*;
import java.text.*;
import java.util.*;
import java.util.concurrent.*;
import java.util.function.*;
import java.util.regex.*;
import java.util.stream.*;
import static java.util.stream.Collectors.joining;
import static java.util.stream.Collectors.toList;

class CustomerOrder implements Comparable<CustomerOrder> {
    int orderId;
    int serveTime;
    CustomerOrder(int id, int time) {
        this.orderId = id;
        this.serveTime = time;
    }
    
    public int compareTo(CustomerOrder co) {
        if (this.serveTime == co.serveTime) return Integer.compare(this.orderId, co.orderId);
        else return Integer.compare(this.serveTime, co.serveTime);
    }
}

public class Solution {
    public static void main(String[] args) throws IOException {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        int n = Integer.parseInt(bufferedReader.readLine().trim());

        List<List<Integer>> orders = new ArrayList<>();

        IntStream.range(0, n).forEach(i -> {
            try {
                orders.add(
                    Stream.of(bufferedReader.readLine().replaceAll("\\s+$", "").split(" "))
                        .map(Integer::parseInt)
                        .collect(toList())
                );
            } catch (IOException ex) {
                throw new RuntimeException(ex);
            }
        });

        List<Integer> result = jimOrders(orders);

        bufferedWriter.write(
            result.stream()
                .map(Object::toString)
                .collect(joining(" "))
            + "\n"
        );

        bufferedReader.close();
        bufferedWriter.close();
    }
    
    private static List<Integer> jimOrders(List<List<Integer>> orders) {
        PriorityQueue<CustomerOrder> orderCheck = new PriorityQueue<>();
        List<Integer> results = new ArrayList<>();
        int customer = 1;
        for (List<Integer> order : orders) {
            orderCheck.add(new CustomerOrder(customer, order.get(0)+ order.get(1)));
            customer++;
        }
        
        for (int i=0; i<orders.size(); i++) {
            results.add(orderCheck.poll().orderId);
        }
        return results;
        
    }
}
```



