## 문제
+ Map을 활용해, 전화번호 부록을 생성한다.
+ 이름과 전화번호를 같이 담는다.

`입력` <br>
+ 1줄: 전화번호 부록 사이즈 
+ 2줄~: 이름 \n 전화번호
+ 다음줄: 검색할 이름

`출력` <br>
+ '이름=번호'로 출력한다.

<br>

> 예시
```
#Sample Input

3
uncle sam
99912222
tom
11122222
harry
12299933
uncle sam
uncle tom
harry



#Sample Output

uncle sam=99912222
Not found
harry=12299933
```

## 풀이
```
# 전화번호 부록 생성
Map<String,Integer> = HashMap<>.. -> 이름,번호를 담는다.


# 전화번호 부록 저장하기
-입력받기에서, scan 사용 시 
n 입력하고 enter키를 누르면 공백이 입력되는 오류가 있다. -> "간단히 in.nextLine()로 한 줄을 입력하면 해결된다."
		int n=in.nextInt();
    in.nextLine();
    while(n-- > 0) {
        String name = in.nextLine();
        int phone = in.nextInt();
        phoneBook.put(name, phone);
        in.nextLine();
    }
    


# 전화번호 검색
while(in.hasNext())는 가볍게 while(true)로 보면 되고,
삼항연산자를 사용해 결과를 출력한다.
이때, map.containsKey() 사용한다. 해당 query(이름)이 있다면 true를 반환하므로 전화번호를 출력하게 한다.
		while(in.hasNext()) {
			String query=in.nextLine();
      System.out.println(phoneBook.containsKey(query) ? query+"="+phoneBook.get(query) : "Not found");        
		}
```

> [scanner 사용시 입력오류 참고 사이트](https://gongstudyit.tistory.com/18) <br>
> [맵에서 키나 값이 있는지 확인하는 함수로 containsKey와 containsValue 참고 사이트](https://dpdpwl.tistory.com/138) <br>


## 코드
```java
//Complete this code or write your own from scratch
import java.util.*;
import java.io.*;

class Solution{
	public static void main(String []argh) {
  
    Map<String, Integer> phoneBook = new HashMap<>();
		Scanner in = new Scanner(System.in);
        
		int n=in.nextInt();
    in.nextLine();
    while(n-- > 0) {
        String name = in.nextLine();
        int phone = in.nextInt();
        phoneBook.put(name, phone);
        in.nextLine();
    }
     
		while(in.hasNext()) {
			String query=in.nextLine();
      System.out.println(phoneBook.containsKey(query) ? query+"="+phoneBook.get(query) : "Not found");        
		}
	}
}
```
