## 문제
점수 높은 순으로 sort한 후, 동점이라면 이름순으로 sort한다.

> 예시
```
# Sample Input
5
amy 100
david 100
heraldo 50
aakansha 75
aleksa 150


# Sample Output
aleksa 150
amy 100
david 100
aakansha 75
heraldo 50
```

## 풀이
+ Arrays.sort() 
> 만약 기본타입 배열을 내림차순으로 정렬하고 싶다면 기본타입 배열을 래퍼 클래스로 만들어 Comparator를 두번째 인자에 넣어주어야 역순으로 정렬할 수 있다.

+ compareTo를 사용해 정렬기준을 둔다.
> 숫자의 비교 같은 경우는 단순히 크다(1), 같다(0), 작다(-1) 의 관한 결과값을 리턴해준다. 

> [compareTo 참고 사이트](https://mine-it-record.tistory.com/133)<br>


## 코드
```java
import java.util.*;

// Write your Checker class here
class Checker implements Comparator<Player> {
    public int compare(Player a, Player b) {
        if (a.score == b.score) {
            return a.name.compareTo(b.name);
        }
        return ((Integer) b.score).compareTo(a.score);
        
    }
}

class Player{
    String name;
    int score;
    
    Player(String name, int score){
        this.name = name;
        this.score = score;
    }
}

class Solution {

    public static void main(String[] args) {
        Scanner scan = new Scanner(System.in);
        int n = scan.nextInt();

        Player[] player = new Player[n];
        Checker checker = new Checker();
        
        for(int i = 0; i < n; i++){
            player[i] = new Player(scan.next(), scan.nextInt());
        }
        scan.close();

        Arrays.sort(player, checker);
        for(int i = 0; i < player.length; i++){
            System.out.printf("%s %s\n", player[i].name, player[i].score);
        }
    }
}
```
