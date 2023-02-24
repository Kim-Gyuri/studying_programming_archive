# Stack으로 괄호 유효성 체크 코드 구현하기
괄호 (), {}, [] 세가지를 이용하여 괄호가 알맞게 열리고 닫혔는지 판단하는 함수를 구현해보자.

## 풀이 포인트
 "열린 괄호는 stack에 담고, 닫힌 괄호를 비교하여 stack에서 뺀다." <br>
 입력받은 문자열 String 객체가 가지고 있는 캐릭터 배열 순회를 끝냈을 때, stack에 괄호가 남아 있다면 "짝이 맞지 않다"로 판단한다.
 

### 입력과 출력
```
# 입력된 괄호의 유효성을 출력한다.

(){}[]            # true
{(})}){)}{(}{)})( # false
{([])}            # true
{[}]              # false
```        

## 코드
```java
package hackerrank;

import java.io.IOException;
import java.util.Stack;


public class Main {
    public static void main(String[] args) throws IOException {
        System.out.println(solution("(){}[]"));             //true
        System.out.println(solution("{(})}){)}{(}{)})("));  //false
        System.out.println(solution("{([])}"));             //true
        System.out.println(solution("{[}]"));               //false
    }

    private static boolean solution(String s) {
        if (s.length()%2 != 0) return false;
        Stack<Character> stack = new Stack<>();
        for (int i=0; i<s.length(); i++) {
            switch (s.charAt(i)) {
                //닫힌 괄호를 비교하여 뺀다.
                case ')' :
                    if (stack.peek() == '(') stack.pop();
                    break;
                case '}':
                    if (stack.peek() == '{') stack.pop();
                    break;
                case ']':
                    if (stack.peek() == '[') stack.pop();
                    break;
                default:
                    stack.push(s.charAt(i)); //열린 괄호를 stack에 담는다.
                    break;
            }
        }
        return stack.empty();
    }

}
```
