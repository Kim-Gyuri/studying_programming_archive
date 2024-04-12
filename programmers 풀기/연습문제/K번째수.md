## K번째수

### 문제
배열 array의 i번째 숫자부터 j번째 숫자까지 자르고 정렬했을 때, k번째에 있는 수를 구하려 합니다. <br><br>

예를 들어 array가 [1, 5, 2, 6, 3, 7, 4], i = 2, j = 5, k = 3이라면
+ array의 2번째부터 5번째까지 자르면 [5, 2, 6, 3]입니다.
+ 1에서 나온 배열을 정렬하면 [2, 3, 5, 6]입니다.
+ 2에서 나온 배열의 3번째 숫자는 5입니다.

배열 array, [i, j, k]를 원소로 가진 2차원 배열 commands가 매개변수로 주어질 때,  <br> commands의 모든 원소에 대해 앞서 설명한 연산을 적용했을 때 나온 결과를 배열에 담아 return 하도록 solution 함수를 작성해주세요.

### 풀이
```java
import java.util.Arrays;

public class Main {
    public static void main(String[] args) {
        int[] array = {1,5,2,6,3,7,4};
        int[][] commands = {{2,5,3}, {4,4,1},{1,7,3}};
        System.out.println(Arrays.toString(solution(array, commands))); // [5, 6, 3]
    }

    private static int[] solution(int[] array, int[][] commands) {
        int[] answer = new int[commands.length]; // 결과 담을 곳

        // 입력
        for (int index=0; index<commands.length; index++) {
            int i = commands[index][0] - 1; // 자를 범위의 시작 인덱스
            int j = commands[index][1] - 1; // 자를 범위의 끝 인덱스
            int k = commands[index][2] - 1; // 찾고 싶은 값의 인덱스

            int[] temp_arr = new int[j-i+1]; // 자른 부분을 배열로 보관한다.
            int cur = 0; // 자른 부분 배열의 인덱스로 사용한다.
            // 자른 부분을 배열로 값 넣는다.
            for (int l=i; l<=j; l++) { // 입력 배열의 인덱스 i~j 부분을 범위로
                temp_arr[cur] = array[l];
                cur++; // (다음 넣을 인덱스이므로 +1 해준다.)
            }
            Arrays.sort(temp_arr); // 정렬한 후에
            answer[index] = temp_arr[k]; // k 위치의 값을 얻는다.
        }
        // 모든 탐색을 마치고 결과 배열을 반환한다.
        return answer;
    }
}
```
