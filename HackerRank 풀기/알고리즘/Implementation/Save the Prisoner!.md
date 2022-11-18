## 문제
+ input 1 Line :  the number of test cases.
+ input 2 Line : n m s ( the number of prisoners, the number of sweets, the chair number to start passing out treats at)
+ output : the chair number of the prisoner to warn.

### Sample
```
# Sample Input 0
2
5 2 1
5 2 2


# Sample Output 0
2
3



# Explanation
[5,2,1]
 start : 1
 Prisoners in seats numbered 1 and 2 get sweets. 
 Warn prisoner 2.


[5,2,2]
start : 2
 Prisoners in seats numbered 2 and 3 get sweets. 
 Warn prisoner 3.
------------------------------------------------------

# Sample Input 1
2
7 19 2
3 7 3


# Sample Output 1
6
3


# Explanation
[7,19,2]
start : 2
2,3,4,5,6,7, 1,2,3,4,5,6, 7,1,2,3,4,5,  6 ->6가 된다.


[3,7,3]
start : 3
3,1,2,3,1,2,3 -> 3이 된다.
```



## 풀이
```
[n,m,s] = [3,7,3]
start : 3
3,1,2(1 turn),  
3,1,2(2 turn),
3(give at seat 3)

They go around twice, and there is one more to pass out to the prisoner at seat 3.

# prisoner 좌석 = (m+s-1)%n;
자신 위치를 제외했을 때(-1를 해줌), n단위로 몇 바퀴를 돌았는지 계산해야 한다.

# if (prisoner == 0)
다시 자신 위치로 돌아온 것임, prisoner = n;이 된다.

# else (prisoner != 0) return prisoner;




  
# 제출한 코드  
  int prisoner = (m+s-1)%n;
        if (prisoner == 0) prisoner = n;
        return prisoner;
```       
       
        
