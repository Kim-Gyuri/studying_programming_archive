[H-Index](https://school.programmers.co.kr/learn/courses/30/lessons/42747)를 풀자. <br>
코딩테스트 연습 > 정렬 문제다. <br><br>

### 문제
+ 조건을 만족하는 h값을 구해야 한다.
+ 조건: <br>  `어떤 과학자가 발표한 논문 n편 중, h번 이상 인용된 논문이 h편 이상이고 나머지 논문이 h번 이하 인용되었다.`

> 예

```
# 입력 
citations = [3,0,6,1,5]

우선 정렬해서 보면, [0,1,3,5,6]이 된다.

h=1 -> 1번 이상 인용된 논문이 4편 있다. -> 조건 불만족
h=3 -> 3번 이상 인용된 논문이 3편 있다. -> "조건 만족"
h=5 -> 5번 이상 인용된 논문이 2편 있다. -> 조건 불만족



결과로 H-Index는 3이 된다.
```

<br>

### 풀이
` 논문 n편 중, h번 이상 인용된 논문이 h편 이상이다.`(조건)을 만족하는 h값을 찾아야 한다. <br>
+ 입력된 배열을 오름차순 정렬한다.
+ h 값으로 "citations.length ~ 1" 범위 안에서, 루프를 돌며 찾는다. <br> 오름차순으로 정렬되어 있으므로, 뒤로 갈수록 h 값이 작아진다.  처음 찾은 게 최댓값이다.
> [참고한 것](https://youngest-programming.tistory.com/251)

<br>


### 코드
> 주석에 설명을 적어두었다.

```java
import java.util.Arrays;

class Main {
    public static void main(String[] args) {
        int[] citations = {3,0,6,1,5};
        System.out.println(solution(citations)); // result :3
    }
    private static int solution(int[] citations) {
        int answer = 0; // 결과를 담을 곳
        int n = citations.length; // 논문 총 몇 편이 있는지? = n

        // 오름차순 정렬을 한 후, 조건을 만족하는 h를 찾는다.
        Arrays.sort(citations);

        // 루프를 돌면서 h를 찾는데
        for (int i=0; i<n; i++) {
            int citation = citations[i];
            // h 값으로 (n~1) 범위 안에서 탐색한다.
            int h = n - i;
            // citations[i] >= h 인 경우가 가장 큰 h값이 된다.
            // (오름차순으로 정렬되어 있으므로, 뒤로 갈수록 h 값이 작아진다. 처음 찾은 게 최댓값이다.)
            if ( h <= citation) {
                answer = h;
                break;
            }
        }
        return answer;
    }
}
```
