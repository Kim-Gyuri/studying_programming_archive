## 문제
+ 가장 작은 수를 가진 type를 출력한다.
+ 단, 같은 수를 가진 경우가 있는 경우, 작은 type을 출력한다.

### Sample
```
# Sample Input 1
11
1 2 3 4 5 4 3 2 1 3 4


# Sample Output 1
3



# Explanation 1
The different types of birds occur in the following frequencies:
Type 1 :2 
Type 2 :2 
Type 3 :3  
Type 4 :3 
Type 5 :1 

Type 3,4가 빈도수가 가장 높다, 둘의 빈도수가 같으므로 낮은 Type인 3을 출력한다.
```

## 풀이
+ Map<type, frequency> :Map을 사용해 input을 포맷한다. 
+ Map의 key값을 가진 Set을 생성해, 루프를 돌려 value가 최대값인 key값을 찾는다.

```
# 코드 스케치
Map<Integer, Integer> storage = HashMap<>;
for (Integer i : arr) {
  if (이미 storage에 key값이 저장된 경우) storage.put(type, frequency+1);  #해당 빈도수 +1증가
  else (key값을 새로 새팅해야 하는 경우) storage.put(type, 1) #key값을 세팅해준다.
}

Set<Integer> keySet = storage.keySet(); #Map의 key값으로 구성된 Set을 생성.
for (Integer i : keySet) #type을 기준으로 arr를 돌려봤을 때,
   if (현재값 > 최대빈도수) { #if()문으로 최대값을 판단한다.
      count = 현재 key값; (i)
      최대빈도수 = 현재값;   
      
return count;      



# 작성한 코드
    // Write your code here
        int count = 0;
        int maxFreq = 0;
        Map<Integer, Integer> storage = new HashMap<>();
        for (Integer i : arr) {
            if (storage.containsKey(i)) storage.put(i, storage.get(i)+1);
            else storage.put(i, 1);
        }
        
        Set<Integer> keySet = storage.keySet();
        for (Integer i : keySet) {
            if (storage.get(i) > maxFreq) {
                count = i;
                maxFreq = storage.get(i);
            }
        }
        return count;
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
     * Complete the 'migratoryBirds' function below.
     *
     * The function is expected to return an INTEGER.
     * The function accepts INTEGER_ARRAY arr as parameter.
     */

    public static int migratoryBirds(List<Integer> arr) {
    // Write your code here
        int count = 0;
        int maxFreq = 0;
        Map<Integer, Integer> storage = new HashMap<>();
        for (Integer i : arr) {
            if (storage.containsKey(i)) storage.put(i, storage.get(i)+1);
            else storage.put(i, 1);
        }
        
        Set<Integer> keySet = storage.keySet();
        for (Integer i : keySet) {
            if (storage.get(i) > maxFreq) {
                count = i;
                maxFreq = storage.get(i);
            }
        }
        return count;
    }
}

public class Solution {
    public static void main(String[] args) throws IOException {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        int arrCount = Integer.parseInt(bufferedReader.readLine().trim());

        List<Integer> arr = Stream.of(bufferedReader.readLine().replaceAll("\\s+$", "").split(" "))
            .map(Integer::parseInt)
            .collect(toList());

        int result = Result.migratoryBirds(arr);

        bufferedWriter.write(String.valueOf(result));
        bufferedWriter.newLine();

        bufferedReader.close();
        bufferedWriter.close();
    }
}
```
