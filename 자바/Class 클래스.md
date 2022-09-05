+ 자바는 클래스와 인터페이스의 메타 데이터를 java.lang 패키지에 소속된 Class 클래스로 관리한다.
> 메타 데이터: 클래스의 이름,생성자 정보,필드 정보,메소드 정보를 말한다.

## Class 객체 얻기 getClass() forName()
+ 방법1,2은 객체 없이 클래스 이름만 가지고 class객체를 얻는 방법이다.
```java
//클래스로부터 얻는 방법
1. Class clazz = 클래스이름.class
2. Class clazz = Class.forName("패키지...클래스이름");


Class class = String.class
Class clazz = Class.forName("java.lang.String");
```

<br>

+ 방법3은 클래스로부터 객체가 이미 생성되어 있을 경우에 사용하는 방법이다.
+ 예를 들어, String 클래스의 Class객체는 다음과 같이 얻는다.
```java
3. Class clazz = 참조변수.getClass();

String str = "감자바";
Class clazz = str.getClass();
```

<br> <br>

### 예제: Class 객체 정보 얻기
+ 다음과 같이 객체정보를 출력할 수 있다.
```
Car
MyBook
whiteship
```


```java
import whiteship.MyBook;

public class Main {
    public static void main(String[]args) throws ClassNotFoundException {
        //방법1
        Class clazz1 = Car.class;

        //방법2
        Class clazz2 = Class.forName("whiteship.MyBook");

        //방법3
        MyBook book = new MyBook();
        Class clazz3 = book.getClass();

        System.out.println(clazz1.getName());
        System.out.println(clazz2.getSimpleName());
        System.out.println(clazz3.getPackage().getName());
    }
}
```

+ MyBook 클래스
```java
package whiteship;

@AnotherAnnotation("whiteship")
public class MyBook extends Book implements MyInterface{
}
```

## 클래스 경로를 활용해서 리소스 절대 경로 얻기
+ Class 객체는 해당 클래스의 파일 경로 정보를 가지고 있기 때문에 이 경로를 활용해서 다른 리소스 파일(이미지, XML, Property 파일)의 경로를 얻을 수 있다.
+ 이 방법은 UI 프로그램에서 많이 활용된다.

```java
        String path1 = clazz.getResource("richel.JPG").getPath();
        String path2 = clazz.getResource("images/richel.JPG").getPath();
```        
        
