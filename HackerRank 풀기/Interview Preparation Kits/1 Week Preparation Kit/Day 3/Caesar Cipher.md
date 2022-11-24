## 문제
+ 문자열 s를 k만큼 쉬프트 시켜라! 
+ 특수문자 제외, z를 넘어가면 다시 a로 돌아온다.
+ k 값이 100이하의 수 이므로 선처리로 26의 나머지값만 쉬프트 해준다.
+ 대문자 아스키값+k 했을때 소문자 영역에 들어갈 수 있으므로 기존 문자가 대문자인지 소문자인지 확인하는 if()문.

## 풀이
```
# 자바 문법
String
StringBuffer : 멀티 쓰레드용
StringBuilder : 단일스레드용

StringBuilder sb = new StringBuilder();
sb.append("문자열");   #문자열 연결
sb.toString();        #문자열을  String으로 변환

Character.isLetter(char);    #char가 문자라면 true반환
Character.isLowerCase(char); #char가 소문자라면 true반환
Character.isUpperCase(char); #char가 대문자라면 true반환




# 코드 스케치
k % 26; 
if (문자일 경우) {
  ascii = 문자 + k;
  if (영역을 벗어나는 경우) ascii-26;   #다시 a,A부터 시작하도록 한다.
  sb.append(ascii);
  } else {
    sb.append(c);
}
return sb.toString(); 




# 제출한 코드
        k = k%26;
        StringBuilder sb = new StringBuilder();
        
        for (Character c : s.toCharArray()) {
            if (Character.isLetter(c)) {
                int ascii = c+k;
                if ((Character.isLowerCase(c) && !Character.isLetter(ascii)) 
                || (Character.isUpperCase(c) && !Character.isUpperCase(ascii)) ) ascii -= 26;
                sb.append((char) ascii);
            } else {
                sb.append(c);
            }
        }
        return sb.toString();
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
     * Complete the 'caesarCipher' function below.
     *
     * The function is expected to return a STRING.
     * The function accepts following parameters:
     *  1. STRING s
     *  2. INTEGER k
     */

    public static String caesarCipher(String s, int k) {
    // Write your code here
        k = k%26;
        StringBuilder sb = new StringBuilder();
        
        for (Character c : s.toCharArray()) {
            if (Character.isLetter(c)) {
                int ascii = c+k;
                if ((Character.isLowerCase(c) && !Character.isLetter(ascii)) 
                || (Character.isUpperCase(c) && !Character.isUpperCase(ascii)) ) ascii -= 26;
                sb.append((char) ascii);
            } else {
                sb.append(c);
            }
        }
        return sb.toString();

    }

}

public class Solution {
    public static void main(String[] args) throws IOException {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        int n = Integer.parseInt(bufferedReader.readLine().trim());

        String s = bufferedReader.readLine();

        int k = Integer.parseInt(bufferedReader.readLine().trim());

        String result = Result.caesarCipher(s, k);

        bufferedWriter.write(result);
        bufferedWriter.newLine();

        bufferedReader.close();
        bufferedWriter.close();
    }
}
```


