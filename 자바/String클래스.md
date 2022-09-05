## String 생성자
+ 자바 문자열은 java.lang 패키지의 String클래스의 인스턴스로 관리한다.
+ 소스상에서 문자열 리터럴은 String 객체로 자동 생성되지만, String 클래스의 다양한 생성자를 이용해서 직접 String 객체를 생성할 수도 있다.
+ 어떤 생성자를 이용해서 String 객체를 생성할지는 제공되는 매개값의 타입에 달려 있다.

```java
//배열 전체를 String 객체로 생성
String str = new String(byte[] bytes);

//지정한 문자셋으로 디코딩
String str = new String(byte[] bytes, String charsetName);

//배열의 offset 인덱스 위치부터 length만큼 String 객체로 생성
String str = new String(byte[] bytes, int offset, int length);

//지정한 문자셋으로 디코딩
String str = new String(byte[] bytes, int offset, int length, String charsetName);
```

### 예제: 바이트 배열을 문자열로 변환
바이트 배열을 문자열로 변환하기
```java
public class Main {
    public static void main(String[] args) {
        byte[] bytes = {72, 101, 108, 108, 111, 32, 74, 97, 118, 97}; //Hello Java

        String str1 = new String(bytes);
        System.out.println(str1);

        String str2 = new String(bytes, 6, 4); //인덱스(6)에서 4개를 뽑기 --->74에서 4개 뽑기
        System.out.println(str2); //Java
    }
}
```

## String 메소드
![string 메소드](https://user-images.githubusercontent.com/57389368/188356445-37697cb8-c367-4c24-995d-5d570f451c37.JPG) <br>

### 문자 추출 charAt()
+ 주어진 인덱스의 문자를 리턴한다. (인덱스란 0부터 시작)

```java
public class Main {
    public static void main(String[] args) {
        String ssn = "010624-1230123";
        char sex = ssn.charAt(7); //7번 인덱스는 1이므로 남자
        switch (sex) {
            case '1':
            case '3':
                System.out.println("남자 입니다.");
                break;
            case '2':
            case '4':
                System.out.println("여자 입니다.");
                break;
        }
    }
}
```

<br> <br>

### 문자열 비교 equals()
+ 기본타입 변수의 값을 비교할 때는, == 연산자를 사용한다.
+ 문자열 비교할 때는 equals()를 사용한다.

+ String str1 = new String("신민철"); new 연산자로 생성된 String 객체
+ String str2 = "신민철"; 
+ (str1 == str2) 다른 객체를 참조
+ (str1.equals(str2)) 같은 문자열을 가짐
+ (str2 == str3) 같은 객체를 참조한다.
+ (str3.equals(str2)) 같은 문자열을 가짐

```java
public class Main {
    public static void main(String[] args) {
        String str1 = new String("신민철");
        String str2 = "신민철";
        String str3 = "신민철";

        if (str1 == str2) {
            System.out.println("같은 객체를 참조");
        } else {
            System.out.println("다른 객체를 참조");
        }
        if (str1.equals(str2)) {
            System.out.println("같은 문자열을 가짐");
        } else {
            System.out.println("다른 문자열을 가짐");
        }

        if (str2 == str3) {
            System.out.println("같은 객체를 참조");
        } else {
            System.out.println("다른 객체를 참조");
        }
        if (str3.equals(str2)) {
            System.out.println("같은 문자열을 가짐");
        } else {
            System.out.println("다른 문자열을 가짐");
        }
    }
}
```
