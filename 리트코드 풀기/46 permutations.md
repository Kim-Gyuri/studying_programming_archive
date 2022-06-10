## 문제 설명
Given an array nums of distinct integers, return all the possible permutations. You can return the answer in any order.
+ 배열 입력 받고, 가능한 순열 경우의 수를 반환한다.
> `Constraints:` <br> 1 <= nums.length <= 6 <br> -10 <= nums[i] <= 10 <br> All the integers of nums are unique. 


### 문제를 어떻게 구현해야 할지?
```java
class Solution {
  public List<List<Integer>> permute(int[] nums) {
    List<List<Integer>> ret = new ArrayList<>(); //결과를 리턴한다.
    List<Integer> tmp = new ArrayList<>(); //임시 저장공간 선언
    backtrack(nums, ret, tmp); //backtrack() 호출한다.
    return ret; //결과 반환한다.
  }

  public void backtrack(int[] nums, List<List<Integer>> ret, List<Integer> tmp) {
    //base case -> if() tmp가 가득찼을 때, ret.add() 하기
    //recursion (순회), -> for (int nums[])
  }
}
```

### 순열
검색할 수 있는 모든 경우의 수를 탐색한다. 단 중복값은 안됨
+ 루프를 돌면서 탐색하는데 tmp에 상태값을 넣어주고, backtrack 하고 backtrack에서 빠져나오고 나서 tmp에서 빼준다.
+ if (중복값 처리) -> 넘기고 계속 탐색시킨다
+ base case : tmp가 가득찼다면 ret.add(new ArrayList<Integer>(tmp));
  

### 예시 시물레이션
> `input : 1,2,3 ` <br>
call 0 : add1 -> tmp(1) <br>
call 1 : skip1, add2 -> tmp(1,2) <br>
call 2 : skip1,2, add3 -> tmp(1,2,3) <br>
call 3 : base case, 123 -> ret(1,2,3) <br>
call 2 : remove 3, return tmp(1,2) <br>
call 1 : remove 2, add 3, -> return tmp(1,3) <br>
call 4 : skip 1, add 2, -> tmp(1,3,2) <br>
call 5 : base case, 132 -> ret(1,3,2) <br>
call 4 : remove 2, skip 3, return tmp(1,3) <br>
계속 반복된다.

### 코드
```java
 class Solution {
    public List<List<Integer>> permute(int[] nums) {
        List<List<Integer>> list = new ArrayList<>();
        backtrackList(list, new ArrayList<>(), nums);
        return list;
    }
    
    private void backtrackList(List<List<Integer>> list, List<Integer> tempList, int[] nums) {
        if (tempList.size() == nums.length) {  //base case, tmp가 가득 찼으면 list.add(tmp)
            list.add(new ArrayList<>(tempList));
        } else {
            for (int i=0; i<nums.length; i++) {  //recursion(재귀함수)
                if (tempList.contains(nums[i])) continue;
                tempList.add(nums[i]);
                backtrackList(list, tempList, nums);
                tempList.remove(tempList.size()-1);
            }
        }
    }
}
```                                               
                                                
