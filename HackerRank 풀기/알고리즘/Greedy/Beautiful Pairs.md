## 문제
+ Determine and print the maximum possible number of pairwise disjoint beautiful pairs. (가능한 한 쌍으로 분리된 아름다운 쌍의 최대 수)
+ Note : Your task is to change exactly 1 element in B, so that the size of the pairwise disjoint beautiful set is maximum. ( pair 갯수가 N일 때 결과값에 1을 빼준다)


### 예시
```
# Sample Input 0
4                 :n 
1 2 3 4           :a[i]
1 2 3 3           :b[i]


# Sample Output 0
4
```

<br>

### 조건 <br>
+ 1 ≤ n ≤ 10³ 
+ 1 ≤ a[i],b[i] ≤ 10³ 


## 풀이
+ A에 있는 원소와 B 원소가 있으면, 이것은 "아름다운 쌍"을 만든다. 
+ 각 요소는 아름다운 쌍을 만들기 위해 한 번만 사용할 수 있습니다.

<br>

```
# Explanation 0

You are given
1 2 3 4           :a[i]
1 2 3 3           :b[i]

The beautiful set is (모든 경우의 수)
[(a[0],b[0]), (a[1],b[1]), (a[2],b[2]), (a[2],b[3])]

and maximum sized pairwise disjoint beautiful set is (index 번호가 안겹치게)
either [(a[0],b[0]), (a[1],b[1]), (a[2],b[2])] or [(a[0],b[0]), (a[1],b[1]), (a[2],b[3]).


We can do better. We change the  element of arrayB  from 3 to 4.
Now new B array is: B = [1,2,4,3]

1 2 3 4           :     a[i]
1 2 4 3           : new b[i]

and the pairwise disjoint beautiful set is
[(a[0],b[0]), (a[1],b[1]), (a[2],b[3]), (a[3], b[2]) ]. 
So, the answer is 4.


# Note
we could have also selected index 3 instead of index 2  (-> array B : 1 2 3 4 )
but it would have yeilded the same result. Any other choice of index is not optimal.
```

<br><br>

Your task is to change exactly 1 element in B <br>
```
 if (pairs == n)  :pairs -1 
 if (pairs != n)  :pairs +1 
``` 

+ 문제문에는 B에서 하나의 요소를 강제로 변경해야 한다고 적혀있다.
+ 그래서 만약 모든 요소가 일치한다면, 우리는 하나를 빼야 한다. 
+ 왜냐하면 B 요소 1개가 변경되고 우리의 일치점이 줄어들 것이기 때문이다.



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
     * Complete the 'beautifulPairs' function below.
     *
     * The function is expected to return an INTEGER.
     * The function accepts following parameters:
     *  1. INTEGER_ARRAY A
     *  2. INTEGER_ARRAY B
     */
    private static final int MAX_NUM = 1000;
    
    public static int beautifulPairs(int[] A, int[] B) {
    // Write your code here
        int[] bucketA = new int[MAX_NUM+1];
        
        for (int i: A) {
          bucketA[i]++;   
        }
    
        int n = B.length, pairs = 0;
        for (int i=0; i<n; i++) {
            if (bucketA[B[i]] > 0) {
                bucketA[B[i]]--;
                pairs++;
            }
        }
        
        return (pairs == n) ? --pairs : ++pairs;
    }
}

public class Solution {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine(), " ");
        
        int n = Integer.parseInt(st.nextToken());
        int[] a_arr = new int[n];
        st = new StringTokenizer(br.readLine(), " ");
        for (int i=0; i<n; i++) {
            a_arr[i] = Integer.parseInt(st.nextToken());
        }
        
        int[] b_arr = new int[n];
        st = new StringTokenizer(br.readLine(), " ");
        for (int i=0; i<n; i++) {
            b_arr[i] = Integer.parseInt(st.nextToken());
        }
        
        int ans = Result.beautifulPairs(a_arr, b_arr);
        System.out.println(ans);
        
    }
}
```
