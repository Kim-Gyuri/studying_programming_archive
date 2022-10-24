## 문제
+ 학생 정보(id,firstName,CGPA) sort하기 
+ CGPA 우선비교, CGPA가 같으면 이름순(알파벳), 이름이 같다면 ID 비교

```
# Sample Input
5
33 Rumpa 3.68
85 Ashis 3.85
56 Samiha 3.75
19 Samara 3.75
22 Fahim 3.76


# Sample Output
Ashis
Fahim
Samara
Samiha
Rumpa
```

## 풀이
`Comparable, Comparator를 이용한 ArrayList 정렬` <br>
+ Collections.sort 메소드는 두 번째 인자로 Comparator 인터페이스를 받을 수 있다.
+ Comparator 인터페이스의 compare 메소드를 오버라이드 하면 된다.
+ compareTo()는 자기 자신을 기준으로 상대방과의 차이 값을 비교하여 반환  

```
            public int compare(Student a, Student b) {
                if (a.getCgpa() < b.getCgpa()) return 1;  #a가 크면 양수 반환
                if (a.getCgpa() > b.getCgpa()) return -1; #a가 작으면 음수 반환
        
                if (a.getFname() != b.getFname()) {
                    return a.getFname().compareTo(b.getFname()); #a가 크면, 양수반환
                }
                return a.getId() - b.getId();
```                

> 예시
[참고 사이트](https://st-lab.tistory.com/243) <br>


## 코드
```java
import java.util.*;

class Student{
    private int id;
    private String fname;
    private double cgpa;
    public Student(int id, String fname, double cgpa) {
        super();
        this.id = id;
        this.fname = fname;
        this.cgpa = cgpa;
    }
    public int getId() {
        return id;
    }
    public String getFname() {
        return fname;
    }
    public double getCgpa() {
        return cgpa;
    }
}

//Complete the code
public class Solution
{
    public static void main(String[] args){
        Scanner in = new Scanner(System.in);
        int testCases = Integer.parseInt(in.nextLine());
        
        List<Student> studentList = new ArrayList<Student>();
        while(testCases>0){
            int id = in.nextInt();
            String fname = in.next();
            double cgpa = in.nextDouble();
            
            Student st = new Student(id, fname, cgpa);
            studentList.add(st);
            
            testCases--;
        }
        
        studentList.sort(new Comparator<Student>() {
            public int compare(Student a, Student b) {
                if (a.getCgpa() < b.getCgpa()) return 1;
                if (a.getCgpa() > b.getCgpa()) return -1;
        
                if (a.getFname() != b.getFname()) {
                    return a.getFname().compareTo(b.getFname());
                }
                return a.getId() - b.getId();
            }
        });
      
          for(Student st: studentList){
            System.out.println(st.getFname());
        }
    }
}
```
