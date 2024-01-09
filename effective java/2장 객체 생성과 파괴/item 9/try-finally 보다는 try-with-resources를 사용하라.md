# try-finally 보다는 try-with-resources를 사용하라
### 포인트
+ close를 통해 회수해야 하는 자원을 다룰 때는 try-finally를 사용하는 대신 반드시 try-with-resources를 사용하자.  <br>보다 가독성 좋으며, 쉽고 정확하게 자원을 회수할 수 있다.
+ 또한 커스텀 자원을 회수해야 하는 경우 AutoCloseable 인터페이스를 구현하는 것을 잊지 말도록 하자.

<br>

## 자원이 닫힘을 보장하는 수단 try-finally의 단점
자바에는 close()를 호출해 직접 닫아줘야 하는 자원이 있다.
> InputStream, OutputStream, java.sql.Connection


### BufferedReader를 사용해준 뒤에 close()를 호출해 직접 닫아주는 코드다. 
하지만 이 방법대로 close()를 호출하게 되면 문제가 발생할 수 있다. <br>
BufferedReader는 사용 중 IOException이 발생할 수 있는데, <br>
만약 br.readLine() 메서드에서 IOException이 발생하게 되면 메서드가 종료되므로 close()가 호출되지 않고 stream이 메모리에 남아있게 된다. <br><br>
따라서 예외가 발생하더라도 자원을 닫을 수 있도록 해줘야 하는데, 전통적으로 try-finally 문을 사용해서 close 처리를 해주었다.
```java
    static String firstLineOfFile(String path) throws IOException {
        BufferedReader br = new BufferedReader(new FileReader(path));
        String result = br.readLine();
        br.close();
        return result;
    }
```

---
### 예제 코드 1: try-finally 방식
finally 블록은 try, catch 블록이 끝난 뒤 실행할 로직을 정의해 주는 블록이다. <br>
따라서 이제 IOException이 발생하게 되더라도 상위 메서드로 IOException 객체를 던져준 뒤 finally 메서드를 종료하게 된다. <br><br>
```java
package study.effectivejava.item9;

import java.io.*;

public class Main {
    /**
     * try-finally : 더 이상 자원을 회수하는 최선의 방책이 아니다.
     */
    static String firstLineOfFile(String path) throws IOException {
        BufferedReader br = new BufferedReader(new FileReader(path));
        try {
            // 여기서 IOException 예외가 발생할 수 있다.
            return br.readLine();
        } finally {
            // 닫을 때 또 다른 IOException 예외가 발생할 수 있다.
            br.close();
        }
    }

    public static void main(String[] args) throws IOException {
        String path = args[0];
        System.out.println(firstLineOfFile(path));
    }
}
```

---
### 예제 코드 2: 자원이 둘 이상이면, try-finally 방식은 너무 지저분하다.
제공된 코드에서 try-finally 블록이 중첩되어 자원 관리를 수행하고 있다. <br>
각 try-finally 블록은 각각의 자원 (InputStream과 OutputStream)을 닫아서 메모리 누수를 방지한다. <br> 
그러나 이러한 코드 구조는 자원이 둘 이상인 경우에 지저분해질 수 있는 몇 가지 단점이 있다. <br>
```java
package study.effectivejava.item9;

import java.io.*;

public class Copy {
    // 버퍼 크기 상수 정의
    private static final int BUFFER_SIZE = 8 * 1024;

    // 파일 복사 메서드 정의
    static void copy(String src, String dst) throws IOException {
        // 소스 파일을 읽기 위한 InputStream 열기
        InputStream in = new FileInputStream(src);
        try {
            // 대상 파일에 쓰기 위한 OutputStream 열기
            OutputStream out = new FileOutputStream(dst);
            try {
                // 버퍼 크기만큼의 바이트 배열 생성
                byte[] buf = new byte[BUFFER_SIZE];
                int n;

                // 소스 파일에 버퍼 크기만큼 읽어와 대상 파일에 쓰기
                while ((n = in.read(buf)) >= 0)
                    out.write(buf, 0, n);
            } finally {
                // 대상 파일의 OutputStream 닫기 (finally 블록 사용하여 안전하게 닫기)
                out.close();
            }
            // 소스 파일의 InputStream 닫기 (finally 블록 사용하여)
        } finally{
            in.close();
        }
    }

    public static void main(String[] args) throws IOException {
        // 명령행 인수에서 소스 및 대상 파일 경로 가져오기
        String src = args[0];
        String dst = args[1];

        // 파일 복사 메서드 호출
        copy(src, dst);
    }
}
```


### try-finally는 자원을 닫을 때 또 다른 예외가 발생할 수 있다.
간단한 파일 입출력 코드에서 자주 사용되는 구조 중 하나는 try-finally 블록을 사용하여 자원을 안전하게 닫는 것이다.  <br>
그러나 이러한 구조에서는 자원을 닫을 때 또 다른 예외가 발생할 수 있습니다. 이러한 경우, 두 번째 예외가 첫 번째 예외를 삼켜버리는 현상이 발생할 수 있다. <br>

#### 예시 코드 1를 다시보면,
위의 코드에서 readLine 메서드 호출 시 파일을 읽는 도중에 IOException이 발생할 수 있다.  <br>
그런 다음 finally 블록에서 close 메서드를 호출하면서 또 다른 IOException이 발생할 수 있다.  <br>
이 상황에서는 어떻게 될까요?

+ 첫 번째 IOException이 발생하면 readLine 메서드에서 예외가 발생한다.
+ finally 블록이 실행되어 close 메서드가 호출됩니다. 그런데 이때 또 다른 IOException이 발생한다.
+ 두 번째 IOException이 첫 번째 IOException을 삼켜버리면서, 첫 번째 예외는 무시되고 두 번째 예외만이 유지된다.

#### 자원이 닫힘을 보장하는 수단 try-finally의 단점
+ 코드 가독성에있어서 지저분하다.
+ 두번째 예외가 첫번째 예외를 집어삼켜버려 실제 시스템에서 디버깅을 어렵게 한다.





## 자원 회수의 최선책 try-with-resources
이러한 try-finally 방식의 단점을 보완하기 위해 자바 7 버전부터는 try-with-resources가 도입되었다.  <br>
try-with-resources를 사용하기 위해서는 사용하는 자원이 AutoCloseable 인터페이스를 구현해야 한다. <br><br>
AutoCloseable 인터페이스는 close 메서드 하나만 정의한 인터페이스다.
```java
public interface AutoCloseable {
      void close() throws Exception;
}
```
### try-with-resources를 사용한 예제 코드
```java
package study.effectivejava.item9;

import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

public class TopLineWithDefault {
    static String firstLineOfFile(String path, String defaultVal) {
        try (BufferedReader br = new BufferedReader(
                new FileReader(path))) {
            return br.readLine();
        } catch (IOException e) {
            return defaultVal;
        }
    }

    public static void main(String[] args) throws IOException {
        String path = args[0];
        System.out.println(firstLineOfFile(path, "Toppy McTopFace"));
    }
}
```

### try-with-resources - 복수의 자원을 처리하는 예제 코드
```java
package study.effectivejava.item9;

import java.io.*;

public class Copy {
    // 버퍼 크기 상수 정의
    private static final int BUFFER_SIZE = 8 * 1024;

    /**
     * 복수의 자원을 처리하는 try-with-resources : 짧고 매혹적이다.
     */
    static void copy(String src, String dst) throws IOException {
        try (InputStream in = new FileInputStream(src);
             OutputStream out = new FileOutputStream(dst)) {
            byte[] buf = new byte[BUFFER_SIZE];
            int n;
            while ((n = in.read(buf)) >= 0)
                out.write(buf, 0, n);
        }
    }

    public static void main(String[] args) throws IOException {
        // 명령행 인수에서 소스 및 대상 파일 경로 가져오기
        String src = args[0];
        String dst = args[1];

        // 파일 복사 메서드 호출
        copy(src, dst);
    }
}
```

+ 읽기 쉽고 문제 진단에 유리하다. (보여줄 예외만 보존되고, 여러 개의 다른 예외가 숨겨질 수도 있다.)
+ catch를 이용해 try문을 중첩하지 않고도 다수의 예외 처리가 가능하다.
+ 숨겨진 예외도 버려지지 않고, 스택 추적 내역에 '숨겨졌다'(suppressed)는 꼬리표를 달고 출력된다.


<br>

### try-with-resources를 catch 절과 함께 쓰는 예제 코드
```java
package study.effectivejava.item9;

import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

public class TopLineWithDefault {
  
    static String firstLineOfFile(String path, String defaultValue) {
        try (BufferedReader br = new BufferedReader(
                new FileReader(path))) {
            return br.readLine();
        } catch (IOException e) {
            return defaultValue;
        }
    }

    public static void main(String[] args) {
        String path = args[0];
        System.out.println(firstLineOfFile(path, "Toppy McTopFace"));
    }
}
```

























