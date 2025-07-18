Object의 기본 toString 메서드가 우리가 작성한 클래스에 적합한 문자열을 반환하는 경우는 거의 없다. <br>
이 메서드는 PhoneNumber@adbbd처럼 단순히 `클래스_이름@진수로_표시한_해시코드`를 반환할 뿐이다. <br>
그래서 toString 메서드를 재정의해서 사용해야 한다. <br>

# 1. toString() 규약
+ 간결하면서 사람이 읽기 쉬운 형태의 유익한 정보를 반환해야 한다. <br> (PhoneNumber@adbbd보다는 010-123-1234으로 알려주는게 좋다.)
+ 모든 하위 클래스에서 이 메서드를 재정의하라.

### toString을 잘 구현한 클래스는 사용하기에 휠씬 즐겁고, 그 클래스를 사용한 시스템은 디버깅하기 쉽다.
toString 메서드는 객체를 println, printf, 문자열 연결 연산자(+), assert 구문에 넘길 때, 혹은 디버거가 객체를 출력할 때 `자동으로 불린다.` <br>
`직접 호출하지 않아도 어딘가에서 쓰인다.` (`오류 메세지 효율적 로깅을 위해 필요하다`)

### toString은 그 객체가 가진 주요 정보 모두를 반환하는게 좋다.
하지만 객체가 거대하거나 객체의 상태가 문자열로 표현하기에 적합하지 않는 경우는, 요약 정보를 담자.

# 2. toString() 문서화 포맷
## (1) toString을 구현할 때면 반환값의 포맷을 문서화할지 정해야 한다.
#### (전화번호나 행렬 같은) 값 클래스라면 문서화하기를 권한다.
포맷을 명시하면 그 객체는 표준적이고, 명확하고, 사람이 읽을 수 있게 된다. <br>

## (2) 명시한 포맷에 맞는 문자열과 객체를 상호 전환할 수 있는 정적 팩터리나 생성자를 함께 제공해주면 좋다.
Integer.toString(),  BigInteger, BigDecimal
```java
import java.math.BigInteger;

public class BigIntegerExample {
    public static void main(String[] args) {
        BigInteger original = new BigInteger("12345678901234567890");

        // 객체 → 문자열
        String str = original.toString();  // "12345678901234567890"

        // 문자열 → 객체
        BigInteger parsed = new BigInteger(str);

        System.out.println(original.equals(parsed));  // true
    }
}
```

## toString() 문서화 포맷의 장점
+ 표준적이고, 명확하고, 사람이 읽을 수 있다.
+ 값 입출력에 사용한다.
+ CSV 같이 데이터 객체 저장도 가능하다.

## toString() 문서화 포맷의 단점
포맷을 한번 명시하면 (그 클래스가 많이 쓰인다면) 평생 그 포맷에 얽매이게 된다. <br>
> toString()에 포맷을 명시하면 외부 코드가 그 포맷에 의존하게 되어, <br>
> "사소한 내부 구현"이 아닌 사실상의 "공개 API"가 되어버린다. <br>
> 따라서 함부로 포맷을 정형화해서는 안 된다.

## toString() 문서화 포맷을 하지 않았을 때
향후 릴리스에서 정보를 더 넣거나 포맷을 개선할 수 있는 유연성을 얻게 된다.
> 외부에 약속한 형식이 없기 때문에, 포맷을 바꿔도 다른 코드에 영향을 주지 않는다. <br>
>  향후 릴리스에서 더 많은 정보 추가, 포맷 개선 등을 자유롭게 할 수 있는 유연성을 확보할 수 있다.

## 포맷 명시 여부와 상관없이 toString()이 반환한 값에 포함된 정보를 얻어올 수 있는 API를 제공하자.
예를 들어, PhoneNumber클래스는 지역코드, 프리픽스, 가입자 번호용 접근자를 제공해야 한다. <br>
그렇지 않으면 toString의 반환값을 파싱하여 필요한 정보를 찾아야 한다. (toString의 반환값을 파싱하는 것은 별로임)
###  ❌ 비추천: 문자열 파싱으로 정보 추출
```java
PhoneNumber number = new PhoneNumber((short) 123, (short) 456, (short) 7890);
String str = number.toString(); // "(123) 456-7890"

// ❌ 비추천: 문자열 파싱으로 정보 추출
String area = str.substring(1, 4);
```


### 아래와 같이 정보를 얻을 수 있게 접근자를 제공해야 한다.
```java
public class PhoneNumber {
    private final short areaCode;
    private final short prefix;
    private final short lineNumber;

    public PhoneNumber(short areaCode, short prefix, short lineNumber) {
        this.areaCode = areaCode;
        this.prefix = prefix;
        this.lineNumber = lineNumber;
    }

    // ✅ toString(): 사람이 보기 좋게
    @Override
    public String toString() {
        return String.format("(%03d) %03d-%04d", areaCode, prefix, lineNumber);
    }

    // ✅ 별도 접근자 메서드 제공
    public short getAreaCode() {
        return areaCode;
    }

    public short getPrefix() {
        return prefix;
    }

    public short getLineNumber() {
        return lineNumber;
    }
}
```

# 3. toString()을 굳이 재정의하지 않아도 되는 경우
정적 유틸리티 클래스, 부분 열거 타입 (partial enum type)이 해당된다.
###  (1) 정적 유틸리티 클래스
상태(필드)가 없고, 인스턴스를 생성하지 않는 클래스를 말한다. <br>
ex: Math, Collections, Objects, Arrays, FileUtils 등

```java
public final class MathUtils {
    public static int square(int n) {
        return n * n;
    }
    // toString() 필요 없음
}
```
출력할 상태가 전혀 없으므로 toString()을 재정의할 이유가 없다.

### (2) 부분 열거 타입 (partial enum type)
동작 위주이며, 상태를 구체적으로 설명할 필요가 없을 경우를 말한다. <br>

```java
public enum Operation {
    PLUS, MINUS, TIMES, DIVIDE;
    // 특별한 포맷이 필요 없으면 기본 name() 사용
}
```
특별한 문자열 표현을 요구하지 않으면, enum.name() 또는 기본 toString()으로 충분하다.


## 하위 클래스들이 공통적으로 써야 할 문자열 표현이 있다면?
#### 상위 클래스에서 toString()을 구현해두고 하위 클래스들이 상속해서 사용하도록 합니다.
### 예시 : 추상 클래스
추상 클래스라면 toString을 재정의해줘야 한다. <br>

```java
public abstract class AbstractCollection<E> implements Collection<E> {
    @Override
    public String toString() {
        Iterator<E> it = iterator();
        if (!it.hasNext())
            return "[]";

        StringBuilder sb = new StringBuilder();
        sb.append('[');
        for (;;) {
            E e = it.next();
            sb.append(e == this ? "(this Collection)" : e);
            if (!it.hasNext())
                return sb.append(']').toString();
            sb.append(',').append(' ');
        }
    }
}
```

이 구현은 ArrayList, HashSet, LinkedList 등 하위 컬렉션들이 상속해서 그대로 사용한다. <br>
하위 클래스에서는 별도로 toString()을 재정의할 필요가 없다.

# 결론
객체의 주요 정보를 사람이 읽기 쉬운 형태로 제공하기 위해 toString()을 재정의하자. (디버깅하기 쉽게 해준다.)
