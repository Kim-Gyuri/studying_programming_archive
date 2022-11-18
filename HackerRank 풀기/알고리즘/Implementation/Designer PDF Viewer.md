## 문제
+ input 1 line : 26개의 알페벳의 높이
+ input 2 line : 문자열
+ output : 강조된 문자열의 길이
+ 단, 모든 문자열의 너비는 1로 가정한다.

### Sample
```
# Sample Input 0
1 3 1 3 1 4 1 3 2 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5
abc


# Sample Output 0
9



# Explanation
a : 1
b : 3
c : 1
가장 긴 문자열은 b이다.
강조된 문자열 길이 = (문자열 개수 * 너비 * 가장 긴 문자열) 이므로
3 * 1 * 3 = 9가 된다.
```

## 풀이
```
# 코드 스케치
designerPdfViewer(List<Integer> h, String word)일 때,
일단 강조된 문자 String을 배열로 변형한다. word.toCharArray(); (비교를 편하게 하기 위해)

char[] word_arr = word.toCharArray(); #String -> char[]
int tallest = 0;                      #가장 긴 문자열의 길이를 저장할 변수
for (입력 받은 알파벳 길이만큼 돌면서) {
    int n = word_arr 원소의 길이       #97은 아스키 코드로 a이므로, -97를 해주면 길이를 알 수 있다.
    tallest = Math.max( tallest, n); 

return tallest * 1 * word.length();




# 작성한 코드
        char[] word_arr = word.toCharArray();
        int tallest = 0; int length=word.length();
        for (int i=0; i<length; i++) {
            tallest = Math.max(tallest, h.get(word_arr[i]-97));
        }
        return tallest*length;
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
     * Complete the 'designerPdfViewer' function below.
     *
     * The function is expected to return an INTEGER.
     * The function accepts following parameters:
     *  1. INTEGER_ARRAY h
     *  2. STRING word
     */

    public static int designerPdfViewer(List<Integer> h, String word) {
    // Write your code here
        char[] word_arr = word.toCharArray();
        int tallest = 0; int length=word.length();
        for (int i=0; i<length; i++) {
            tallest = Math.max(tallest, h.get(word_arr[i]-97));
        }
        return tallest*length;
    }

}

public class Solution {
    public static void main(String[] args) throws IOException {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        List<Integer> h = Stream.of(bufferedReader.readLine().replaceAll("\\s+$", "").split(" "))
            .map(Integer::parseInt)
            .collect(toList());

        String word = bufferedReader.readLine();

        int result = Result.designerPdfViewer(h, word);

        bufferedWriter.write(String.valueOf(result));
        bufferedWriter.newLine();

        bufferedReader.close();
        bufferedWriter.close();
    }
}
```
