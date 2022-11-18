## 문제
해당 player의 rank를 출력한다.

### 풀이
```
# Sample Input 1
7
100 100 50 40 40 20 10
4
5 25 50 120



# Sample Output 1
6
4
2
1


# Explanation
rank가 아래와 같을 때, player의 경기를 기록하면 된다.
1등 : 100
1등 : 100
2등 : 50
3등 : 40
3등 : 40
4등 : 20
5등 : 10


player - 5일 때
1등 : 100
1등 : 100
2등 : 50
3등 : 40
3등 : 40
4등 : 20
5등 : 10
6등 : 5


player - 25일 때
1등 : 100
1등 : 100
2등 : 50
3등 : 40
3등 : 40
4등 : 25
5등 : 20
6등 : 10
7등 : 5


player - 50일 때
1등 : 100
1등 : 100
2등 : 50
2등 : 50
3등 : 40
3등 : 40
4등 : 20
5등 : 10
6등 : 5


player - 120일 때
1등 : 120
2등 : 100
2등 : 100
3등 : 50
4등 : 40
4등 : 40
5등 : 20
6등 : 10


5점일 때는 6등 
25점일 때는 4등
50점일 때는 2등
120점일 때는 1등
```

## 풀이
```
# 코드 스케치
result = new ArrayList<>();     #등수를 기록할 리스트 생성
playerCount = 0, rankCount = 0  #player, rank array의 index를 표시

while (ranked, player 점수를 다 돌리면서) {
    if (player 점수 < rank 점수) {        #rank점수가 더 높을 때, player 등수는 rank.size - rankCount + 1
      result.add(rank.size  - rankCount + 1);     #(+1은 index가 0부터 시작해서)
      playerCount++;  #player 점수를 계속 본다.
    } else (player 점수 ≥ rank 점수) {
      if (ranked 점수를 모두 확인했을 때) {
          result.add(1); #그때 player 점수는 1등이 된다.
          playerCount++; # player 점수를 계속 본다.
      } else (ranked 점수를 확인 중일 때) {
          rankedCount++;  # ranked 점수를 계속 본다. 
      }
....
return result;






# 작성한 코드
        List<Integer> result = new ArrayList<>();
        int playerCount = 0;
        int rankCount = 0;
        ranked = ranked.stream()
                        .distinct()
                        .sorted(Comparator.naturalOrder())
                        .collect(Collectors.toList());
        
        while((playerCount < player.size()) && (rankCount < ranked.size())) {
            if (player.get(playerCount).compareTo(ranked.get(rankCount)) < 0) {
                result.add(ranked.size()- rankCount +1);
                playerCount++;
            } else {
                if (rankCount == (ranked.size()-1)) {
                    result.add(1);
                    playerCount++;
                } else rankCount++;
            }
        }                    
        return result;
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
     * Complete the 'climbingLeaderboard' function below.
     *
     * The function is expected to return an INTEGER_ARRAY.
     * The function accepts following parameters:
     *  1. INTEGER_ARRAY ranked
     *  2. INTEGER_ARRAY player
     */

    public static List<Integer> climbingLeaderboard(List<Integer> ranked, List<Integer> player) {
    // Write your code here
        List<Integer> result = new ArrayList<>();
        int playerCount = 0;
        int rankCount = 0;
        ranked = ranked.stream()
                        .distinct()
                        .sorted(Comparator.naturalOrder())
                        .collect(Collectors.toList());
        
        while((playerCount < player.size()) && (rankCount < ranked.size())) {
            if (player.get(playerCount).compareTo(ranked.get(rankCount)) < 0) {
                result.add(ranked.size()- rankCount +1);
                playerCount++;
            } else {
                if (rankCount == (ranked.size()-1)) {
                    result.add(1);
                    playerCount++;
                } else rankCount++;
            }
        }                    
        return result;
    }
        
}

public class Solution {
    public static void main(String[] args) throws IOException {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        int rankedCount = Integer.parseInt(bufferedReader.readLine().trim());

        List<Integer> ranked = Stream.of(bufferedReader.readLine().replaceAll("\\s+$", "").split(" "))
            .map(Integer::parseInt)
            .collect(toList());

        int playerCount = Integer.parseInt(bufferedReader.readLine().trim());

        List<Integer> player = Stream.of(bufferedReader.readLine().replaceAll("\\s+$", "").split(" "))
            .map(Integer::parseInt)
            .collect(toList());

        List<Integer> result = Result.climbingLeaderboard(ranked, player);

        bufferedWriter.write(
            result.stream()
                .map(Object::toString)
                .collect(joining("\n"))
            + "\n"
        );

        bufferedReader.close();
        bufferedWriter.close();
    }
}
```
