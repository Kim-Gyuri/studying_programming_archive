## 문제
해당 array에 있는 숫자를 k번 swap하여 사전순으로 가장 큰 순열을 출력하라
> 1 ≤ n ≤ 10⁵ <br>
> 1 ≤ k ≤ 10⁹

### 예시
```
# Sample Input 0
STDIN             ->Function
-----               --------
5 1                  n = 5, k = 1
4 2 3 5 1            arr = [4, 2, 3, 5, 1]



# Sample Output 0
5 2 3 4 1



# Explanation 0
swap이 1번 가능하므로, 가장 큰 5와 4를 swap 해주면 된다.
4 2 3 5 1   -->5-2-3-4-1
```

<br>

```
# Sample Input 1
arr= [3 2 4 1 5]   k=3 

[3 2 4 1 5]에서 5-3를 swap:1 ---> 52413
[5 2 4 1 3]에서 4-2를 swap:2 ---> 54213
[5 4 2 1 3]에서 2-3를 swap:3 ---> 54312



# Sample Output 1
5 4 3 1 2
```

<br>

```
# Sample Input 2
arr=[1 2 3 4]  k=2

[1 2 3 4]에서 1-4를 swap:1  ---> 4231
[4 2 3 1]에서 2-3를 swap:2  ---> 4321


# Sample Output 2
4 3 2 1
```

## 풀이
#### 문제를 보고 조심해야 겠다고 생각한 부분
+ 입력 받은 List<>의 숫자는 1부터 ~n임을 주의하자.
+ swaq을 할 때, 숫자 위치가 바뀜을 확인해줘야 한다. 

<br>

#### 코드 구현 스케치는 다음과 같다.
###  indics 
```
# swap 횟수는 실제로 숫자의 위치를 바꿨을 때에만 카운트 해야 한다.
->인덱스를 체크하여 코드 최적화를 높인다?

// since numbers are 1 to N, make array size() + 1 and ignore 0th index
// 입력 List의 숫자가 1부터 N까지이므로 "배열 크기()+1"을 만들고 0번째 인덱스를 무시한다.
indices[]는 인덱스 번호를 저장한다.
# 예시 List: 1 2 3 4 인 경우에
indices[]는 다음과 같다.
index[1]: value=0
index[2]: value=1
index[3]: value=2
index[4]: value=3


# 해당 숫자의 인덱스 정보를 얻는 메소드
    public static int[] getIndices(List<Integer> arr) {
        int[] indices = new int[arr.size()+1];
        for (int i=0; i<arr.size(); i++) {
            int number = arr.get(i);
            indices[number] = i;
        }
        return indices;
    }
```    

<br><br>

### swap()
```
# swap 구현하기
List<>의 숫자는 indexFrom인덱스->indexTo인덱스 값으로 swap한다.
인덱스 정보 배열 indics[]도 swap하는데, indics[]의 인덱스는 List<>의 value가 된다.

    public static void swap(List<Integer> arr, int[] indices, int indexFrom, int indexTo) {
        int numFrom = arr.get(indexFrom);
        int numTo = arr.get(indexTo);
        arr.set(indexFrom, numTo);
        arr.set(indexTo, numFrom);
        
        indices[numFrom] = indexTo;
        indices[numTo] = indexFrom;
    }
```    

<br><br>

### largestPermutation(int k, List<Integer> arr) 
```  
# 구현 코드 
        int currentSwap = 0;              #현재 swap횟수
        int largestNum = arr.size();      #List에서 가장 큰 숫자
        int[] indices = getIndices(arr);  #List<>의 인덱스 정보를 저장할 배열
        
        for (int i=0; i<arr.size(); i++) {  
            if (currentSwap == k) {       #swap을 k번까지 했다면 종료시킨다.
                break;
            }
            if (arr.get(i) != largestNum) { #현재 숫자가 최대 숫자가 아니라면, swap처리해준다.
            
                swap(arr, indices,i, indices[largestNum]); 
                currentSwap++;  #swap 횟수를 +1 더해 반복한다.
            }
            largestNum--;  #최대숫자를 변경한다. 
        }
        return arr; #swap을 끝낸 List를 반환한다.
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
     * Complete the 'largestPermutation' function below.
     *
     * The function is expected to return an INTEGER_ARRAY.
     * The function accepts following parameters:
     *  1. INTEGER k
     *  2. INTEGER_ARRAY arr
     */
    
    public static List<Integer> largestPermutation(int k, List<Integer> arr) {
    // Write your code here
        int currentSwap = 0;
        int largestNum = arr.size();
        int[] indices = getIndices(arr);
        
        for (int i=0; i<arr.size(); i++) {
            if (currentSwap == k) {
                break;
            }
            if (arr.get(i) != largestNum) {
            
                swap(arr, indices,i, indices[largestNum]);
                currentSwap++;
            }
            largestNum--;
        }
        return arr;

    }
    
    public static int[] getIndices(List<Integer> arr) {
        int[] indices = new int[arr.size()+1];
        for (int i=0; i<arr.size(); i++) {
            int number = arr.get(i);
            indices[number] = i;
        }
        return indices;
    }

    public static void swap(List<Integer> arr, int[] indices, int indexFrom, int indexTo) {
        int numFrom = arr.get(indexFrom);
        int numTo = arr.get(indexTo);
        arr.set(indexFrom, numTo);
        arr.set(indexTo, numFrom);
        
        indices[numFrom] = indexTo;
        indices[numTo] = indexFrom;
    }
}

public class Solution {
    public static void main(String[] args) throws IOException {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        String[] firstMultipleInput = bufferedReader.readLine().replaceAll("\\s+$", "").split(" ");

        int n = Integer.parseInt(firstMultipleInput[0]);

        int k = Integer.parseInt(firstMultipleInput[1]);

        List<Integer> arr = Stream.of(bufferedReader.readLine().replaceAll("\\s+$", "").split(" "))
            .map(Integer::parseInt)
            .collect(toList());

        List<Integer> result = Result.largestPermutation(k, arr);

        bufferedWriter.write(
            result.stream()
                .map(Object::toString)
                .collect(joining(" "))
            + "\n"
        );

        bufferedReader.close();
        bufferedWriter.close();
    }
}
```
