## 문제
+ Given an array of n distinct integers, transform the array into a zig zag sequence by permuting the array elements.
+ A sequence will be called a zig zag sequence if the first k elements in the sequence are in increasing order <br> and the last k elements are in decreasing order, where k = (n+1)/2           
+ 지그재그 원소값 중 a[mid]은 가장 큰 값이다.

### 포인트
+ In this challenge, the task is to debug the existing code to successfully execute all provided test files. <br> (문제 코드를 수정해서 제출해라)
+ You can modify at most three lines in the given code. You cannot add or remove lines of code. <br> (최대 3줄 코드를 수정해주면된다. 따로 추가 작성 NO)


### Sample 
```
# Sample Input 0
1
7
1 2 3 4 5 6 7



# Sample Output 0
1 2 3 7 6 5 4



# Explanation
입력을 아래와 같이 정리할 수 있다.
t, number of test cases : 1
n, array size : 7
array elements : [1,2,3,4,5,6,7]

k = (n+1)/2이므로
k = (7+1)/2 = 4가 된다.
요소 4개까지는 오름차순이고, 그 다음은 내림차순으로 정렬하면 지그재그가 된다.
1 2 3 7 + 6 5 4 로 정렬하면 된다.
```


## 풀이
```
#1 가장 큰값은 a[mid]에 넣어야 한다.
#2 a[끝] 가운데로 스왑해야 하므로, 다음 a[끝]은 a[n-2]이 되어야 한다.
#3 오른쪽 끝(오름차순 기준) 포인터는 index 1씩 왼쪽으로 이동시켜야 한다.


# 코드 스케치
        Arrays.sort(a);             #우선 오름차순으로 정렬해준다.
        int mid = n/2; //수정 1     #index는 0부터 시작하므로 mid = n/2가 된다.
        int temp = a[mid];          #스왑해서 정렬한 리스트의 끝값으로 a[mid]을 만든다.
        a[mid] = a[n - 1];
        a[n - 1] = temp;
    
        int st = mid + 1;
        int ed = n - 2; //수정 2    #중간값(index: n-1)을 제외한 index가 end가 된다.
        while(st <= ed){            # 내림차순 정렬은, (지그재그의 중간 ~ 끝부분)
            temp = a[st];           # 중간값 다음 값인 a[mid+1]은 오른쪽 끝값과 스왑해서 뽑으면 된다. 
            a[st] = a[ed];
            a[ed] = temp;
            st = st + 1;
            ed = ed - 1; //수정 3    #계속 가장 오른쪽값을 왼쪽으로 스왑하도록 index를 바꿔준다.

```

<br><br>

```
# 배열을 이용해서 swap 하기

예: Integer[] arr = {1,2,3,4,5};

swap(arr, 0, 4); --> 결과: {5, 2, 3, 4, 1}



private static <T> void swap(T[] a, int f, int r) {
	  T temp;
    temp = a[f];
    a[f] = a[r];
    a[r] = temp;
}


배열과 인덱스를 인자로 넘겨주는 방법입니다.
arr에는 배열 그 자체가 아닌, 배열의 첫 번째 원소가 저장된 메모리 공간의 주소가 담겨 있습니다.
따라서 swap() 메서드 내에서  a[f] = a[r]  같은 연산을 수행하면 실제 배열의 값이 변경되게 됩니다.
```

> 참고 사이트 <br>
> [java swap](https://co1nam.tistory.com/21) <br>


## 코드 
```java
import java.util.*;
import java.lang.*;
import java.io.*;
import java.math.*;
public class Main {
    
    public static void main (String[] args) throws java.lang.Exception {
        Scanner kb = new Scanner(System.in);
        int test_cases = kb.nextInt();
        for(int cs = 1; cs <= test_cases; cs++){
            int n = kb.nextInt();
             int a[] = new int[n];
            for(int i = 0; i < n; i++){
                a[i] = kb.nextInt();
            }
            findZigZagSequence(a, n);
        }
   }
   
    public static void findZigZagSequence(int [] a, int n){
        Arrays.sort(a);
        int mid = n/2; //수정 1
        int temp = a[mid];
        a[mid] = a[n - 1];
        a[n - 1] = temp;
    
        int st = mid + 1;
        int ed = n - 2; //수정 2
        while(st <= ed){
            temp = a[st];
            a[st] = a[ed];
            a[ed] = temp;
            st = st + 1;
            ed = ed - 1; //수정 3
        }
        for(int i = 0; i < n; i++){
            if(i > 0) System.out.print(" ");
            System.out.print(a[i]);
        }
        System.out.println();
    }
}
```


