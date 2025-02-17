## 비트 필드
비트별 OR를 사용해 여러 상수를 하나의 집합으로 모을 수 있으며, 이렇게 만들어진 집합을 비트 필드(bit field)라고 한다. <br>
```java
public class Text {
  public static final int STYLE_BOLD = 1 << 0;  // 1
  public static final int STYLE_ITALIC = 1 << 1 // 2
  ...
````

### 비트 필드의 문제점
비트 필드를 사용하면 비트별 연산을 사용해 합집합과 교집합 같은 집합 연산을 효율적으로 수행할 수 있지만 단점이 있다. <br>
#### 1. 비트 필드는 정수 열거 상수의 단점을 지닌다.
비트 필드는 결국 정수 값으로 관리되기 때문에, 컴파일 시점 타입 체크가 어렵고, 의미 없는 값이 들어갈 가능성이 있다. <br>
```java
public class Main {
    public static final int READ = 1 << 0;    // 0001
    public static final int WRITE = 1 << 1;   // 0010
    public static final int EXECUTE = 1 << 2; // 0100

    public static void setPermission(int permission) {
        System.out.println("Permission set: " + permission);
    }

    public static void main(String[] args) {
        setPermission(7);  // 의미 없는 값도 전달 가능 (0111)
    }
}
```
7과 같은 의미 없는 값이 전달되어도 컴파일 오류가 발생하지 않는다. <br><br>
#### 2. 비트 필드 값이 그대로 출력되면 단순한 정수 열거 상수를 출력할 때보다 해석하기 어렵다.
비트 필드를 사용하면 실제 값이 정수 형태로 출력되어 어떤 의미인지 바로 파악하기 어렵다. <br>
```java
public class Main {
    public static final int READ = 1 << 0;    // 0001
    public static final int WRITE = 1 << 1;   // 0010
    public static final int EXECUTE = 1 << 2; // 0100


    public static void main(String[] args) {
        int permission = READ | WRITE;  // 0011
        System.out.println(permission); // 출력: 3
    }
}
```
3이라는 숫자만 보면 READ와 WRITE 권한이 포함되어 있다는 것을 알기 어렵다.<br><br>
#### 3. 비트 필드 하나에 녹아 있는 모든 원소를 순회하기도 까다롭다.
비트 필드는 비트 연산을 통해 모든 원소를 확인해야 하므로, 열거형처럼 간단하게 순회할 수 없다. <br>
```java
public class Main {
    public static final int READ = 1 << 0;    // 0001
    public static final int WRITE = 1 << 1;   // 0010
    public static final int EXECUTE = 1 << 2; // 0100


    public static void main(String[] args) {
        int permissions = READ | WRITE | EXECUTE; // 0111

        for (int i = 1; i <= EXECUTE; i <<= 1) {
            if ((permissions & i) != 0) {
                System.out.println("Permission: " + i);
            }
        }
    }
}
```
```
Permission: 1
Permission: 2
Permission: 4
```
 비트 연산을 사용해야만 각 권한을 순회할 수 있다. <br><br>
#### 4. 최대 몇 비트가 필요한지를 API 작성 시 미리 예측하여 적절한 타입(보통은 int나 long)을 선택해야 한다.
비트 필드는 비트를 사용하여 값을 표현하므로, 사용할 비트 수를 미리 고려하고 적절한 자료형(int, long)을 선택해야 한다. <br>
```java
public class Main {
    public static final long READ = 1L << 0;
    public static final long WRITE = 1L << 1;
    public static final long EXECUTE = 1L << 2;
    // 추가적으로 필요한 권한이 많다면 long(64비트)으로도 부족할 수 있다.
}
```
만약 새로운 권한이 필요해 비트 수가 증가하면, 자료형을 변경하거나 구조를 수정해야 하는 번거로움이 있다. <br>

## EnumSet 클래스
정수 상수보다 열거 타입을 선호하는 프로그래머 중에도 상수 집합을 주고 받아야 할 때는 여전히 비트 필드를 사용하기도 한다. <br>
하지만 이제 더 나은 대안이 있다. <br>
java.util 패키지의 EnumSet 클래스는 열거 타입 상수의 값으로 구성된 집합을 효과적으로 표현해준다. <br><br>

#### 1. EnumSet 내부는 비트 벡터로 구현되었다.
```java
public enum Permission {
    READ, WRITE, EXECUTE, DELETE
}

EnumSet<Permission> permissions = EnumSet.of(Permission.READ, Permission.WRITE);
```
예를 들어, Permission 열거형에는 4개의 상수가 있으므로, 내부적으로는 다음과 같이 비트 벡터를 사용한다.<br>
```
READ = 0001
WRITE = 0010
EXECUTE = 0100
DELETE = 1000
```
EnumSet.of(Permission.READ, Permission.WRITE)는 내부적으로 0001 | 0010 = 0011과 같이 비트 연산으로 관리된다. <br><br>
#### 2. removeAll과 retainAll 같은 대량 작업은 (비트 필드를 사용할 때 쓰는 것과 같은) 비트를 효율적으로 처리할 수 있는 산술 연산을 써서 구현했다.
```java
EnumSet<Permission> allPermissions = EnumSet.allOf(Permission.class);
EnumSet<Permission> removePermissions = EnumSet.of(Permission.READ, Permission.WRITE);

allPermissions.removeAll(removePermissions);  // 비트 AND NOT 연산으로 빠르게 처리
```
EnumSet은 내부적으로 비트 벡터 기반이라, 대량 작업을 수행할 때도 비트 연산을 사용하여 효율적이다. <br>
+ removeAll은 내부적으로 비트 AND NOT 연산으로 빠르게 처리된다.
+ retainAll도 비트 AND 연산을 사용하여 지정된 요소만 빠르게 유지할 수 있다.

## 핵심정리
비트 필드는 비트 연산으로 인한 가독성과 안정성 문제를 초래한다. <br> 
EnumSet은 내부적으로 비트 벡터를 사용해 비트 필드와 유사한 효율성을 제공하면서도 열거형의 장점을 모두 누릴 수 있으므로, <br>
비트 필드 대신 EnumSet을 사용하자. 
