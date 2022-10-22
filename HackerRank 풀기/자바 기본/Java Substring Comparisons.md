## 문제
+ 입력받은 문자를 subString 길이 3로 나눠 List에 넣고, 가장 작은 길이, 가장 긴 길이를 출력한다.

<br>

+ Sample Input 0 
```
welcometojava
3
```

+ Sample Output 0
```
ava
wel
```

## 코드
```java
import java.util.Scanner;

public class Solution {

    public static String getSmallestAndLargest(String s, int k) {
        String smallest = "";
        String largest = "";
        
        // Complete the function
        // 'smallest' must be the lexicographically smallest substring of length 'k'
        // 'largest' must be the lexicographically largest substring of length 'k'
        java.util.List<String> a = new java.util.ArrayList<String>();
        
        for (int i=0; i<s.length()-k+1; i++) {
            a.add(s.substring(i, i+k));
        }
        java.util.Collections.sort(a);
        smallest = a.get(0);
        largest = a.get(a.size()-1);
        
        return smallest + "\n" + largest;
    }


    public static void main(String[] args) {
        Scanner scan = new Scanner(System.in);
        String s = scan.next();
        int k = scan.nextInt();
        scan.close();
      
        System.out.println(getSmallestAndLargest(s, k));
    }
}
```
