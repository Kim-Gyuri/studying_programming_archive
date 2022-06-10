### 문제 내용
>Given n pairs of parentheses, write a function to generate all combinations of well-formed parentheses. <br>
>n개의 괄호 쌍이 주어지면, 잘 형성된 괄호의 모든 조합을 생성하는 함수를 작성한다. <br>
> ``` Input: n = 3, Output: ["((()))","(()())","(())()","()(())","()()()"] ```

### 풀이
+  ` backtrack == recursion(재귀) + termination check ` <br>
+  ` 코드 스케치`
```java
List<String> ret = new ArrayList<>(); //결과 리턴
process(ret); //recursion
return ret;

public void process(int n, int numOpen, int numClosed, List<String> ret) {
  //termination check
  //recurse next
  process() : add open bracket (여는 괄호)
  process() : add closed bracket (닫는 괄호)
```
+ 코드 구체화
```java

//add open bracket
if (numOpen < n ) {
  process(); -> process(n, numOpen+1, str+"c",numClosed);
}

//add closed bracket
if (numClosed > numOpen) {
  process(n, numOpen, numClosed+1, str+"c");
}

//termination check
if (numOpen == n && numClosed == n) {
  ret.add(str);
  return;
}
```

### 코드
```java
class Solution {
    public List<String> generateParenthesis(int n) {
        List<String> ret = new ArrayList<>();
        process(n,0,0,"",ret);
        return ret;
    }
    
    public void process(int n, int numOpen, int numClosed, String str, List<String> ret) {
        if (numOpen == n && numClosed == n) {
            ret.add(str);
            return;
        }
        if (numOpen < n) {
            process(n, numOpen+1, numClosed, str+"(", ret);
        }
        if (numOpen > numClosed) {
            process(n, numOpen, numClosed+1, str+")", ret);
        }
    }
}
```
