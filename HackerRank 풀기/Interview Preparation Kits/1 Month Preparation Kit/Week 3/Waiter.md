## 문제
```
You are a waiter at a party. There is a pile of numbered plates. Create an empty  array. 

At each iteration, i, 
remove each plate from the top of the stack in order.

Determine if the number on the plate is evenly divisible by the "i"th prime number.
If it is, stack it in pile B(i). 
Otherwise, stack it in stack A(i).

Store the values in  B(i) from top to bottom in 'answers'.

In the next iteration, do the same with the values in stack A(i). Once the required number of iterations is complete, 
store the remaining values in A(i) in 'answers',
again from top to bottom. 

Return the ans array.
```

> 해석
```
You are a waiter at a party.
There is a pile of numbered plates.

1. 빈 배열을 만든다.
순서대로 stack에서  plate를 제거한다.

접시 번호가 ith prime number로 나누어지는지 확인한다.
나누어진다 ->pile  b에 쌓기
아니다 -> stack a에 쌓기

b에 있는 값(위->아래)을 ans에 저장한다.


2. do the same with the values in stack A(i).
필요한 반복횟수를 만족하면 ans에 남아있는 a값들을 저장한다.
다시 반복한다.

3. return ans;
```

<br><br>

### Example
+ A = [2,3,4,5,6,7]
+ q = 3
> `1` A0에 모든 접시를 저장한다. A0 = [2,3,4,5,6,7] <br>
> `2` 빈 배열을 생성한다. ans = [] <br>
> `3` first loop, 2로 나누어지는 접시인지 확인한다. <br>
> 나누어지는 접시 B1 = [6,4,2] <br>
> 아닌 경우 A1 = [7,5,3] <br>
> <br> <br>
> `4` ans에 B1를 넣는다. ans = [2,4,6] <br>
> `5` second loop, A1 원소가 3으로 나누어지는 접시인지 확인한다. <br>
> 나누어지는 접시 B2 = [3] <br>
> 아닌 경우 A2 = [7,5] <br>
> <br> <br>
> `6` ans에 B2를 넣는다. ans = [2,4,6,3] <br>
> <br><br>
> `7` third loop, A2원소가 5로 나누어지는 접시인지 확인한다. <br>
> 나누어지는 접시 B3 = [5] <br>
> 아닌 경우 A3 = [7] <br>
> <br> <br>
> `8` ans에 B2 원소를 넣는다. ans = [2,4,6,3,5] <br>
> `9` 모든 loop를 끝냈다. A3에 남아있는 원소를 위->아래 순서로 ans에 넣는다. ans = [2,4,6,3,5,7] <br>
> `10` return ans;
  
<br> <br>
  
## Function Description
Complete the waiter function in the editor below. <br>

### Input Format
> int number[n]: the numbers on the plates <br>
> int q: the number of iterations

### Returns
int[n]: the numbers on the plates after processing  
  
### Sample 
```  
# Input
5 1
3 4 7 6 5
  

# Output
4
6
3
7
5   
```  

## 코드 스케치
```
    public static List<Integer> waiter(List<Integer> number, int q) {
    // Write your code here
        int prime = 2; 
        ArrayList<Integer> result = new ArrayList<>(); #결과를 반환할 리스트
        Stack<Integer> all = new Stack<Integer>();        # A0 [] :A0에 모든 접시를 저장한다.
        Stack<Integer> divisible = new Stack<Integer>();  # B1 [] :prime number로 나누어지는 접시를 넣는다.
        List<Integer> reverse = new ArrayList<Integer>(); # A1 [] :아닌 경우 접시를 넣는다.
        
        for (int i : number) { #stack에 순서대로 저장한다.
            all.push(i);
        }
        
        for (int j=0; j<q; j++) { #q간격으로
            reverse.clear();
            while(!all.empty()) {
                if (all.peek() % prime == 0) { #접시 번호가 소수(prime number)로 나누어지는지 확인한다.
                    divisible.push(all.pop()); # 나누어진다 -> divisible stack에 쌓아둔다.
                } else {
                    reverse.add(all.pop());    # 아닌 경우 -> reverse stack에 쌓는다.
                }
            }
            
            while(!divisible.empty()) {   # divisible 값을 위->아래 순서로 result Array에 저장한다.
                result.add(divisible.pop());
            }
            
            
            for (int plate : reverse) { 
                all.push(plate);
            }
            prime = nextPrime(prime); #다음 prime number를 받는다.
        }
        
        if (reverse.size()> 0) {      
            Collections.reverse(reverse); #reverse 원소를 뒤집기 정렬하여, 
            result.addAll(reverse);      #result Array에 남아있는 reverse 원소를 저장한다.
        }
        return result;                  #return ans(결과 result)
    }
    
    public static int nextPrime(int n) { #다음 prime number 정하는 
        n++;
        for (int i=2; i<n; i++) {
            if (n%i == 0) {
                n++;
                i=2;
            } else {
                continue;
            }
        }
        return n;
    }

}

```

## 코드
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

class Result {

    /*
     * Complete the 'waiter' function below.
     *
     * The function is expected to return an INTEGER_ARRAY.
     * The function accepts following parameters:
     *  1. INTEGER_ARRAY number
     *  2. INTEGER q
     */

    public static List<Integer> waiter(List<Integer> number, int q) {
    // Write your code here
        int prime = 2;
        ArrayList<Integer> result = new ArrayList<>();
        Stack<Integer> all = new Stack<Integer>();
        Stack<Integer> divisible = new Stack<Integer>();
        List<Integer> reverse = new ArrayList<Integer>();
        
        for (int i : number) {
            all.push(i);
        }
        
        for (int j=0; j<q; j++) {
            reverse.clear();
            while(!all.empty()) {
                if (all.peek() % prime == 0) {
                    divisible.push(all.pop());
                } else {
                    reverse.add(all.pop());
                }
            }
            
            while(!divisible.empty()) {
                result.add(divisible.pop());
            }
            
            
            for (int plate : reverse) {
                all.push(plate);
            }
            prime = nextPrime(prime);
        }
        
        if (reverse.size()> 0) {
            Collections.reverse(reverse);
            result.addAll(reverse);
        }
        return result;
    }
    
    public static int nextPrime(int n) {
        n++;
        for (int i=2; i<n; i++) {
            if (n%i == 0) {
                n++;
                i=2;
            } else {
                continue;
            }
        }
        return n;
    }

}

public class Solution {
    public static void main(String[] args) throws IOException {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        String[] firstMultipleInput = bufferedReader.readLine().replaceAll("\\s+$", "").split(" ");

        int n = Integer.parseInt(firstMultipleInput[0]);

        int q = Integer.parseInt(firstMultipleInput[1]);

        List<Integer> number = Stream.of(bufferedReader.readLine().replaceAll("\\s+$", "").split(" "))
            .map(Integer::parseInt)
            .collect(toList());

        List<Integer> result = Result.waiter(number, q);

        bufferedWriter.write(
            result.stream()
                .map(Object::toString)
                .collect(joining("\n"))
            + "\n"
        );

        bufferedReader.close();
        bufferedWriter.close();
    }
}
```
