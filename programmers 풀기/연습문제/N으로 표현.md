[N으로 표현](https://school.programmers.co.kr/learn/courses/30/lessons/42895?language=java)를 풀자. <br>

### 문제
숫자 N을 가지고 결과값 number를 만드는데, 필요한 N의 갯수를 return해라
+ 단, 사용횟수는 최솟값이여야 한다.
+ 수식에는 괄호와 사칙연산만 가능하며  <br> 나누기 연산에서 나머지는 무시한다.

`입력` <br>
+ 숫자 N (N은 1 이상 9 이하)
+ 만들 숫자 number

`출력` <br>
+ N을 사용한 횟수 <br>
+ 만약 횟수가 8보다 크면 -1를 return한다.


> 예:

```
N = 5, number = 12
경우의 수는
12 = 5 + 5 + (5 / 5) + (5 / 5)   -> 횟수:6
12 = 55 / 5 + 5 / 5              -> 횟수:5
12 = (55 + 5) / 5	         -> 횟수:4

여기서 4가 가장 작다.
return 4; 
```

<br>

### 풀이
DP()를 구현하는 문제다.
+ DFS를 구현하여 모든 경우의 수를 구해 조건에 맞는 값을 반환하도록 풀었다.
+ 사칙연산(+, -, *, /) 경우의 수로 나눠 DFS 탐색할 때, (N, NN, NN...)를 연산하는 경우의 수도 고려해야 한다.

>[참고한 코드](https://youngest-programming.tistory.com/392)

  <br>

### 코드
> 설명은 주석으로 적었다.

 ```java
public class Main {
    static int result = 9; //사용횟수를 기록하는 곳 ,(i+1)번으로 했으니 최댓값인 9로 비교한다
    static int N,number; // dfs() 로직에 사용하기 위해 입력값을 static 변수로 선언
    public static void main(String[] args) {
        int N = 5;
        int number = 12;
        System.out.println(solution(N, number)); // return 4
    }
    private static int solution(int N, int number) {
        Main.N = N;
        Main.number = number;
        dfs( 0,0);

        //8번 순회했음에도 number를 못 찾는 경우 -1를 반환
        // (i+1)번으로 했으니 9로 비교함
        return result == 9 ? -1 : result;
    }

    // dfs() 구현: number를 만들 때까지 탐색한다.
    // 매개변수: count(사용횟수), pre_value(사칙연산값)
    private static void dfs(int count, int pre_value) {
        // 만약 횟수가 8보다 크면, return -1
        if (count > 8) {
            result = -1;
            return;
        }
        // 표현하고 싶은 숫자 number를 찾았고 && 최솟값 count인 경우, return count
        if (number == pre_value && result > count) {
            result = count;
            return;
        }

        // n2 변수: N, NN, NNN 숫자를 만들기 위한
        int n2 = N;
        // number를 가장 최소로 만드는 수를 구한다.
        // 사칙연산별로 경우 수로 나눠 dfs() 탐색을 한다.
        for (int i=0; i<8-count; i++) {
            // count+ (i+1)번 dfs()를 반복하며 찾는다.
            dfs(count+i+1, pre_value+n2);
            dfs(count+i+1, pre_value-n2);
            dfs(count+i+1, pre_value*n2);
            dfs(count+i+1, pre_value/n2);
            n2 += N * (Math.pow(10, i+1)); //여기서 pow()를 활용해 경우수를 만든다.
        }
    }
}
```
