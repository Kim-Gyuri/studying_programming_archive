## 문제
+ Goodland는 선을 따라 일정한 간격을 가진 많은 도시들이 있는 나라이다.
+ 인접한 도시들 사이의 거리는 1 단위이다.
+ 입력으로 '도시 수' '공장분포 범위' '각 도시의 건축지로서의 적합성 리스트'를 받는다.
+ return, 전체 도시 목록에 전기를 공급하는 데 필요한 (최소) 발전소의 수를 출력해라.
+ 만약, 수행할 수 없다면 -1을 반환한다.

### Sample 1
```
k = 3
arr = [0,1,1,1,0,0,0]

도시 3곳이 적합한데, city index 1,2,3이 해당된다.
# 만약 arr[2]에 세운다면, arr[0] -> arr[4]까지 공급 가능하다.
# arr[6]까지 제공하기 위해서는, city index 4,5,6 중에서 공장을 더 세워야 한다.
# 그래서 return -1.
```

<br> 

### Sample 2
```
k = 6 2
arr = [0 1 1 1 1 0]

도시 4곳이 적합한데, city index 1,2,3,4가 해당된다.
# 만약 arr[1] arr[4]에 세운다면, 전체 도시에 공급 가능하다.
# return 2.
```

## 코드 스케치
+ reachable [index] < k, k보다 작은 index에는 전력공급 가능하다.
+ 순차적으로 index 0부터 탐색해야 한다.

```
        // Write your code here
        int res = 0;        #공장을 세울 수 있는 도시의 수
        int index = 0;      #현재 index 
        int n = arr.size(); #전체 도시 수

        while (index < n) { #도시 n곳을 탐색하면서,
            boolean checked = false; #해당 도시를 탐색했는지 체크
            for (int i = index+(k-1); (i>=index-k+1) && (i>=0); i--) { #k칸 범위 조건으로 루프를 돈다.
                # index 0일 때 -> i = 1
                # index 3일 때 -> i = 4
                if (i < n) { 
                    if (arr.get(i) == 1) { #공장 세울 수 있는 도시라면, 
                        res++; 
                        index = i+k;       #k 단위로 도시를 탐색
                        checked = true;
                        break;
                    }
                }
            }
            if (!checked) return -1;
        }
        return res;
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
    public static int pylons(int k, List<Integer> arr) {
        // Write your code here
        int res = 0;
        int index = 0;
        int n = arr.size();

        while (index < n) {
            boolean checked = false;
            for (int i = index+(k-1); (i>=index-k+1) && (i>=0); i--) {
                if (i < n) {
                    if (arr.get(i) == 1) {
                        res++;
                        index = i+k;
                        checked = true;
                        break;
                    }
                }
            }
            if (!checked) return -1;
        }
        return res;
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

        int result = Result.pylons(k, arr);

        bufferedWriter.write(String.valueOf(result));
        bufferedWriter.newLine();

        bufferedReader.close();
        bufferedWriter.close();
    }
}
```
