## 문제
`입력` <br>
+ 괄호로 이루어진 입력을 받는다. 

`출력` <br>
+ 여는 괄호, 닫는 괄호 짝이 맞는 경우 true를 반환한다.

<br>

> 예시
```
#Sample Input
{}()
({()})
{}(
[]


#Sample Output
true
true
false
true
```

## 풀이
```
# createBracketsMap() 메소드
괄호 종류를 입력한다.
Map<String, String> = HashMap<String, String>.. -> 여는 괄호, 닫는 괄호를 짝으로 입력한다.


# 입력 받기
String data = sc.next().trim();  -> 앞뒤 공백을 제거하여, String을 받는다.
isBalanced()메소드를 호출하도록 한다.


# isBalanced() 메소드
공백처리 , if (data.equals("")) return true;
Deque 사용한다. Deque<String> stack = new ArrayDeque<>();

for(입력 받은 문자 길이 만큼) {
  String c = 철자1개씩 
  if ( 철자 = 닫는 괄호) {
     if (stack.isEmpty()) return false;
     String opening = stack.pop();
     if ( 닫는 괄호 !=  data의 철자 c) return false;
  else {
       stack.push(c);
  }
}
return stack.isEmpty();
data에서 철자가 
우선 여는 괄호라면 stack에 저장
닫는 괄호라면, stack.pop()을 하는데 pop()한 데이터가 닫는 괄호와 짝이라면 pass한다.
-> 이렇게 data.length()번 반복하다가 stack에 여는 괄호가 없이 비었다면 true를 반환한다.
```

> [deque를 사용하는 이유 참고 사이트](https://tecoble.techcourse.co.kr/post/2021-05-10-stack-vs-deque/) <br>


## 코드
```java
import java.util.*;
class Solution{
    final static Map<String, String> BRACKETS = createBracketsMap();
    final static Set<String> CLOSING = new HashSet<String>(Arrays.asList("]","}",")"));
	public static void main(String []argh)
	{
		Scanner sc = new Scanner(System.in);
		
		while (sc.hasNext()) {
			String data = sc.next().trim();
      System.out.println(isBalanced(data));
		}
	} 
    private static Map<String, String> createBracketsMap() {
        Map<String, String> map = new HashMap<String, String>();
        map.put("{", "}");
        map.put("[", "]");
        map.put("(", ")");
        return map;
    }
    
    public static boolean isBalanced(String data) {
        if (data.equals("")) return true;
        Deque<String> stack = new ArrayDeque<>();
        for (int i=0; i<data.length(); i++) {
            String c = data.substring(i, i+1);
            if (CLOSING.contains(c)) {
                if (stack.isEmpty()) return false;
                String opening = stack.pop();
                if (!BRACKETS.get(opening).equals(c)) return false;
            } else {
                stack.push(c);
            }
        }
        return stack.isEmpty();
    }
}
```


