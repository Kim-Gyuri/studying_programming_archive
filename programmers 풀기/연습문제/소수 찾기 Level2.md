[소수 찾기](https://school.programmers.co.kr/learn/courses/30/lessons/42839)를 풀자. <br>
코딩테스트 연습 > 완전탐색 문제다. <br><br>

### 문제
각 종이 조각에 적힌 숫자가 적힌 문자열 numbers가 주어졌을 때, <br>
종이 조각으로 만들 수 있는 소수가 몇 개인지 return 해라.
> 소수 : 1과 자신만을 약수로 가지는 수

`제한사항` <br>
+ numbers는 길이 1 이상 7 이하인 문자열입니다.
+"013"은 0, 1, 3 숫자가 적힌 종이 조각이 흩어져있다는 의미입니다.
+ 11과 011은 같은 숫자로 취급합니다.

<br>

### 풀이
+ 만들 수 있는 숫자 경우의 수를 구한다.
+ 재배열한 숫자를 저장할 ArrayList<> 선언한다.
+ 루프를 돌리며 Integer.parseInt() 했을 때, (011) 같은 숫자를 제거한다.
+ 소수를 판별하는 알고리즘 구현
> [참고한 것](https://dding9code.tistory.com/18)

<br>

### 코드
> 주석에 설명을 적어두었다. 

```java
import java.util.ArrayList;

public class Main {
    // 문자열(입력)에서부터 얻은 문자(숫자)를 넣을 곳
    static ArrayList<Integer> arr = new ArrayList<>();
    // 방문체크하기 위해, ( <numbers: 문자열>은 길이 1 이상 7 이하인 문자열이니까 크기 7로 )
    // -> 최대 7개의 수중에서 1,2...7개를 뽑는 모든 순열을 구할 수 있다.
    static boolean[] visited = new boolean[7];

    public static void main(String[] args) {
        String numbers = "011";
        System.out.println(solution(numbers)); // return 2
    }

    public static int solution(String numbers) {
        int answer = 0; // 결과를 담을 곳, (몇 개의 소수를 만드는지?)

        // 문자열(입력)에서 1개로 숫자를 만들 수 있는 경우의 수를 찾는다.
        for (int i=0; i<numbers.length(); i++) {
            dfs(numbers, "", i+1);
        }

        // 루프를 돌며, 만든 숫자 중에서 소수가 있다면 카운팅한다.
        for (int i=0; i<arr.size(); i++) {
            if (isPrime(arr.get(i))) answer++;
        }
        return answer;
    }

    // dfs() 구현: 문자열 -> 만들 수 있는 숫자 경우의 수를 구한다.
    // 매개변수 : 입력된 문자열, 문자열에서 만든 현재 숫자, (문자열길이가 index인 숫자를 만든다.)
    private static void dfs(String str, String cur_str, int index) {
        // 만든 문자열의 길이가 index인 경우의 수들을
        if (cur_str.length() == index) {
            // String -> Integer 변환하여
            int number = Integer.parseInt(cur_str);
            // "11 <-> 011" 과 같은 중복되는 수를 중복체크한다.
            if (!arr.contains(number)) {
                // 체크 완료한 숫자를 넣는다.
                arr.add(number);
            }
        }

        // 주어진 문자열 길이만큼 완전탐색을 한다.
        // 길이별 모든 순열을 구한다.
        for (int i=0; i<str.length(); i++) {
            // 방문하지 않은 경우
            if (!visited[i]) {
                visited[i] = true;
                cur_str += str.charAt(i);
                dfs(str, cur_str, index);
                visited[i] = false;
                // 현재 숫자말고 다른 숫자를 선택하는 경우, cur_str에서 위에서 방문한 숫자 지워주기
                cur_str = cur_str.substring(0, cur_str.length()-1);
            }
        }
    }

    // 소수인지 판단하는 로직
    private static boolean isPrime(int number) {
        // (0, 1)은 소수가 아니다.
        if (number < 2) return false;
        // 2부터 (number-1)까지의 모든 수를 확인하며
        // (제곱근까지만 확인하면 시간 복잡도 O(x^1/2)이 된다.)
        for (int i=2; i*i<number; i++) {
            // number가 해당 수로 나누어떨어지는다면, ->"소수가 아니다"
            if (number%i==0) return false;
        }
        return true;
    }
}
```
