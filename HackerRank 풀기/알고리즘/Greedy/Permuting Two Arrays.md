## 문제
입력받은 값이 조건에 맞으면 YES, 아닌 경우는 NO를 출력한다.

### Sample
```
# Input
2           q = 2
3 10        A[] and B[] size n = 3, k = 10
2 1 3       A = [2, 1, 3]
7 8 9       B = [7, 8, 9]
4 5         A[] and B[] size n = 4, k = 5
1 2 2 1     A = [1, 2, 2, 1]
3 3 3 4     B = [3, 3, 3, 4]



# Sample Output
YES
NO



# Explanation 
실행할 쿼리 수         q = 2
첫번째 쿼리의 조건     A[] and B[] size n = 3, k = 10
첫번째 쿼리의 배열들   A = [2, 1, 3],  B = [7, 8, 9]
...(다음 쿼리 조건들)

A[]는 오름차순 정렬, B[]는 내림차순 정렬을 하고
모든 원소가 A[i] + B[i] ≥ k를 만족하면 YES, 아니면 NO를 출력해주면 된다.



## q(1)
정렬처리 A = [1, 2, 3],  B = [9, 8, 7]
1 + 9 = 10  
2 + 8 = 10
3 + 7 = 10 
으로 모든 원소가 조건을 만족하므로 YES


## q(2)
정렬처리 A = [1, 1, 2, 2],  B = [4, 3, 3, 3]
1 + 4 = 5  
1 + 3 = 4
2 + 3 = 5
2 + 3 = 5
으로 불만족하므로 NO
```

## 코드
```java
    public static String twoArrays(int k, List<Integer> A, List<Integer> B) {
    // Write your code here
        int count = 0;
        A.sort(Comparator.naturalOrder());
        B.sort(Comparator.reverseOrder());
        for (int i=0; i<A.size(); i++) {
           if ((A.get(i)+B.get(i)) >= k) count += 1;
        }
        return (count == A.size()) ? "YES" : "NO";
    }

}
```
