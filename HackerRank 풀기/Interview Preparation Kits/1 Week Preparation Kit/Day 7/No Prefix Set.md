## 문제
+ The set of strings is said to be a GOOD SET if no string is a prefix of another string.
+ Otherwise, print BAD SET on the first line followed by the string being checked.

### Sample
```
# Sample Input01
4
aab
aac
aacghgh
aabghgh


# Sample Output01
BAD SET
aacghgh




# Explanation
'aab' is a prefix of 'aabghgh', and aac' is prefix of 'aacghgh'. The set is a BAD SET. 
'aacghgh' is tested before 'aabghgh', so and it fails at 'aacghgh'.
```


## 풀이
+ TreeSet을 사용한다. 
> `HashSet이 아닌 TreeSet을 사용한 이유` <br>
> HashSet, TreeSet 모두 중복을 허용하지 않고 순서가 없는 컬렉션이다. <br>
> TreeSet은 이진탐색트리의 형태로 데이터를 저장하는 컬렉션이다. <br>
> 이진탐색트리 중에서도 성능을 향상시킨 '레드-블랙 트리'로 구현되어 있다. <br>
> 따라서 데이터의 추가, 삭제에는 시간이 걸리지만, 검색과 정렬이 뛰어나다는 장점이 있다.
> <br> <br>
> `TreeSet 주요 메소드` [TreeSet 참고 사이트](https://codedragon.tistory.com/5411) <br>


<br> <br>



+ 코드 스케치
```
TreeSet set = new TreeSet<>();                # treeSet 생성, words(List)원소를 1개씩 넣으면서 if()를 판단
for (String w : words) {                      # words List를 돌면서
  String next_w = words_to_check_ceiling(w);  # w와 같거나 긴 word를 검색한다.
  String prev_w = words_to_check_floor(w);    # w와 같거나 짧은 word를 검색한다.
  
  if (next_w가 null이 아니면서, next_w가 w로 시작하는 경우 
      혹은 prev_w가 null이 아니면서, prev_w가 w로 시작하는 경우) {
      print("BAD SET");    # prefix가 해당된 w를 찾았다면 "BAD SET"를 출력하고 해당 w를 출력한다.
      print(w);
  }
  
  words_to_check.add(w);  # w를 1개씩 넣으면서 if()루프를 돌린다.
}

print("GOOD SET");        # if()에 해당되지 않은 경우는 "GOOD SET"를 출력한다.
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
     * Complete the 'noPrefix' function below.
     *
     * The function accepts STRING_ARRAY words as parameter.
     */

    public static void noPrefix(List<String> words) {
    // Write your code here
        TreeSet<String> words_to_check = new TreeSet<>();
        for (String w : words) {
            String next_w = words_to_check.ceiling(w);
            String prev_w = words_to_check.floor(w);
            
            if ((next_w != null && next_w.startsWith(w)) || (prev_w != null && w.startsWith(prev_w))) {
                System.out.println("BAD SET");
                System.out.println(w);
                return;
            }
            words_to_check.add(w);
        }
        System.out.println("GOOD SET");
    }

}

public class Solution {
    public static void main(String[] args) throws IOException {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));

        int n = Integer.parseInt(bufferedReader.readLine().trim());

        List<String> words = IntStream.range(0, n).mapToObj(i -> {
            try {
                return bufferedReader.readLine();
            } catch (IOException ex) {
                throw new RuntimeException(ex);
            }
        })
            .collect(toList());

        Result.noPrefix(words);

        bufferedReader.close();
    }
}
```
