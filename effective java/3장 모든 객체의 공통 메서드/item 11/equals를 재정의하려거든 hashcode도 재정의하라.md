#### equals를 재정의한 클래스 모두에서 hashcode도 재정의해야 한다.
그렇지 않으면 hashcode 일반 규약을 어기게 되어 해당 클래스의 인스턴스를 HashMap이나 HashSet 같은 컬렉션의 원소로 사용할 때 문제를 일으킬 것이다.

# 1. Object 명세의 3가지 규약
#### 일관성 있는 hashCode
equals 비교에 사용되는 정보가 변경되지 않았다면, 애플리케이션이 실행되는 동안 그 객체의 hashcode 메서든 몇 번을 호출해도 일관되게 항상 같은 값을 반환해야 한다. <br>
단, 애플리케이션을 다시 실행한다면 이 값이 달라져도 상관없다.

#### equals가 true라면 hashCode도 동일해야 한다.
equals(Object)가 두 객체를 같다고 판단했다면, 두 객체의 hashCode는 똑같은 값을 반환해야 한다.

#### equals가 false라도 hashCode는 같을 수 있다.
equals(Object)가 두 객체를 다르다고 판단했더라도, 두 객체의 hashCode가 서로 다른 값을 반환할 필요는 없다. <br>
단, 다른 객체에 대해서는 다른 값을 반환해야 해시 테이블의 성능이 좋아진다.

`즉, 논리적으로 같은 객체는 같은 해시코드를 반환해야 한다.`

## 예제
PhoneNumber 클래스의 인스턴스를 HashMap의 원소로 사용한다고 해보자. <br>
```java
import java.util.*;

public final class PhoneNumber {
    private final short areaCode, prefix, lineNum;

    public PhoneNumber(int areaCode, int prefix, int lineNum) {
        this.areaCode = rangeCheck(areaCode, 999, "area code");
        this.prefix   = rangeCheck(prefix,   999, "prefix");
        this.lineNum  = rangeCheck(lineNum, 9999, "line num");
    }

    private static short rangeCheck(int val, int max, String arg) {
        if (val < 0 || val > max)
            throw new IllegalArgumentException(arg + ": " + val);
        return (short) val;
    }

    @Override public boolean equals(Object o) {
        if (o == this)
            return true;
        if (!(o instanceof PhoneNumber))
            return false;
        PhoneNumber pn = (PhoneNumber)o;
        return pn.lineNum == lineNum && pn.prefix == prefix
                && pn.areaCode == areaCode;
    }


    public static void main(String[] args) {
        Map<PhoneNumber, String> m = new HashMap<>();
        m.put(new PhoneNumber(707, 867, 5309), "Jenny");
        System.out.println(m.get(new PhoneNumber(707, 867, 5309)));
    }
}
```

### PhoneNumber 클래스가 equals()는 재정의했지만 hashCode()는 재정의하지 않았다면?
m.get(new PhoneNumber(707,867, 5309))를 실행하면 "제니"를 찾지 못하고 null을 반환한다. <br>
왜냐하면, hashCode가 다르기 때문에 해시 버킷이 달라진다. <br><br>

### 두 번째 규약(equals가 true라면 hashCode도 동일해야 한다.)을 위반한 것이다.
두 개의 PhoneNumber 인스턴스가 사용되었다.  <br>
하나는 "Jenny"를 HashMap에 저장할 때 사용되었고, 다른 하나는 이를 꺼낼 때 사용되었다.  <br> <br>

PhoneNumber 클래스는 equals()는 재정의했지만 hashCode()는 재정의하지 않았기 때문에,  <br>
논리적으로 동치인 두 객체가 서로 다른 해시코드를 반환하게 된다. <br>
#### 이는 Object 명세의 두 번째 규약을 위반한 것이다.
그 결과, get() 메서드는 엉뚱한 해시 버킷에 접근하게 되고,  <br>
정상적으로 저장된 객체를 찾지 못한다.  <br> <br>

이 문제는 equals()와 일관되게 동작하는 적절한 hashCode() 메서드만 작성해주면 간단히 해결된다.


# 2. hashcode 동작방법
## hashCode()의 역할은 무엇인지?
해시 기반 자료구조(HashMap, HashSet, Hashtable 등)에서 객체를 어느 해시 버킷(bucket)에 저장할지를 결정하는 데 사용된다. <br>
객체가 삽입되거나 검색될 때 먼저 `hashCode()`를 호출해 적절한 버킷을 찾고, 해당 버킷 안에서는 `equals()`로 `정확히 같은 객체인지 확인`한다.

## 좋은 해시 함수
논리적으로 `다른 객체는 가능하면 다른 해시코드를 반환해야 한다.` (명세 3번째 규약이다) <br>
즉, 해시 충돌(collision)을 최소화해야 한다.

####  해시 충돌이란?
> 서로 다른 객체가 같은 hashCode()를 반환하는 경우를 말한다. <br>
> 같은 버킷에 들어가게 되어, 성능 저하의 원인이 된다.

### 예시 : 최악의 hashcode 구현 - 사용금지
```java
@Override public int hashCode(){return 42;}
```
+ 모든 객체가 한 버킷에 있다.
+ 검색할 때마다 equals()를 모든 항목에 적용한다.
+  모든 객체에게 똑같은 값만 내어주므로 모든 객체가 해시 테이블의 버킷 하나에 담겨 마치 연결리스트(linked list)처럼 동작한다.
+  그 결과 평균 수행 시간이 O(1)인 해시 테이블이 O(n)으로 느려져서, 객체가 많아지면 도저히 쓸 수 없게 된다.

### 예시 : 좋은 해시 함수 (예: Objects.hash(...) 사용)
```java
@Override
public int hashCode() {
    return Objects.hash(areaCode, prefix, lineNum);
}
```
+ 서로 다른 객체는 서로 다른 해시코드를 가질 가능성이 높다.
+ 해시 충돌 감소 → 성능 유지

# 3. 좋은 hashcode를 작성하는 간단한 요령
좋은 hashcode를 만드는 요령을 살펴보자. <br>

### (1) 기본 원칙
`equals()를 재정의했다면, 반드시 hashCode()도 재정의해야 한다.` <br>
동등성을 결정하는 필드들만 사용하여 해시코드를 계산해야 한다. 

### (2)  필드의 해시코드 계산 방법
hashCode()를 구현할 때, 각 필드의 타입에 따라 적절한 방식으로 해시코드를 계산해야 한다.
#### 기본 타입 필드
박싱 클래스의 정적 메서드를 사용해 해시코드를 계산한다.
```
int → Integer.hashCode(f)

long → Long.hashCode(f)

double → Double.hashCode(f)

boolean → Boolean.hashCode(f)
```

#### 참조 타입 필드
+ equals()가 해당 필드를 재귀적으로 비교한다면, hashCode()도 재귀적으로 호출해야 한다. <br>
+ 계산이 복잡할 경우, 표준형(canonical representation)을 만들어 그 객체의 hashCode()를 사용한다. <br>
+ 필드의 값이 null인 경우 0을 사용한다.

#### 배열 필드
+ 배열이라면 핵심 원소 각각을 별도 필드처럼 취급하여 해시코드를 계산한다.
+ 핵심 원소가 없으면 0을 사용한다.
+ 모든 원소가 핵심 원소라면 Arrays.hashCode()를 사용하는 것이 편리하다.

### (3) 해시코드 누적 방법
각 필드에서 계산한 해시코드를 다음과 같이 누적하여 최종 result를 만든다.
```
result = 31 * result + fieldHashCode;
```

### (4) result를 반환한다.
```
return result;
```

# 4. Hashcode 작성 시 주의사항
### (1) hashCode() 작성 시 동치인 인스턴스에 대해 똑같은 해시코드를 반환할지 자문하자
hashcode를 다 구현했다면 이 메서드가 동치인 인스턴스에 대해 똑같은 해시코드를 반환할지 자문해보자. <br>

```java
@Override public int hashCode(){
    int result = Short.hashCode(areaCode);        // 첫 필드 해시
    result = 31 * result + Short.hashCode(prefix);  // 두 번째 필드 해시 누적
    result = 31 * result + Short.hashCode(lineNum); // 세 번째 필드 해시 누적
    return result;
}
```
equals()는 areaCode, prefix, lineNum 이 모두 같아야 true를 반환한다. <br>
그래서 hashCode()도 이 세 필드를 모두 사용해서 해시값을 만든다. <br>
이렇게 하면 equals()가 true인 두 객체는 항상 같은 hashCode()를 갖게 된다.
> equals()에서 비교하는 필드는 모두 hashCode()에도 포함해야 <br> 동치 인스턴스 → 동일 해시코드 원칙을 지킬 수 있다.

### (2) equals() 비교에 사용되지 않은 필드는 반드시 hashCode() 계산에서 제외해야 한다.
#### ❌ 잘못된 예시: equals()에 사용되지 않는 필드까지 hashCode()에 포함한 경우
```java
public class Person {
    private final String name;
    private final int age;
    private final long id; // DB나 시스템에서 부여하는 고유 식별자

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (!(o instanceof Person)) return false;
        Person person = (Person) o;
        return name.equals(person.name) && age == person.age;
        // id는 비교 대상이 아님
    }

    @Override
    public int hashCode() {
        int result = name.hashCode();
        result = 31 * result + Integer.hashCode(age);
        result = 31 * result + Long.hashCode(id); // ❌ equals에 사용되지 않은 필드를 포함
        return result;
    }
}
```
`왜 문제가 될까?` <br>
`equals()는 name과 age만 비교하고, id는 무시한다.` <br><br>

그런데 hashCode()는 id까지 포함해서 계산하므로, <br>
`equals()로 같다고 판단되는 두 객체가 다른 hashCode()를 가질 수 있다.` <br><br>

이렇게 되면 Java에서 동치 객체는 반드시 같은 해시코드를 가져야 한다는 규칙을 어기게 되어, <br>
해시 기반 자료구조(HashSet, HashMap)에서 예상치 못한 오류가 발생할 수 있다. <br><br>

#### ✅ 올바른 예시: equals()에 사용된 필드만 사용
```java
@Override
public int hashCode() {
    int result = name.hashCode();
    result = 31 * result + Integer.hashCode(age);
    return result; // ✅ equals()에서 비교하는 필드만 포함
}
```
이렇게 하면 equals()가 true를 반환하는 객체들은 항상 동일한 hashCode() 값을 반환하므로,
계약을 잘 지키는 안전한 코드가 된다. <br><br>

### (3) 성능을 높이기 위해 해시코드 계산시 핵심 필드를 생략해서는 안된다.
속도는 빨라지지만 해시 품질이 나빠져, 해시테이블 성능을 떨어뜨릴 수 있다.

### (4) hashCode가 반환하는 값의 생성규칙을 API 사용자에게 자세히 공표하지 말자.
클라이언트 의존도가 낮아지고, 추후에 계산방식을 바꿀 수 있기 때문이다. <br>
현재 String, Integer 등 여러 자바 라이브러리에서 hashCode 메서드가 반환하는 정확한 값을 알려준다.

# 5. Object 클래스의 hash() 메서드
Object 클래스는 임의의 개수만큼 객체를 받아 해시코드를 계산해주는 정적 메서드인 hash를 제공한다. <br>
(장단점을 고려해보면) hash 메서드는 성능에 민감하지 않은 상황에서만 사용하자.

### 장점
hashCode 함수를 단 한 줄로 작성할 수 있다.

### 단점
속도가 느리다. <br>
(입력 인수를 담기 위한 배열이 만들어지고, 입력 중 기본 타입이 있다면 박싱과 언박싱도 거쳐야 하기 때문이다.)

### 예시 : PhoneNumber에서, 제공하는 hashCode를 사용할 때 성능이 살짝 아쉽다. 
```java
import java.util.Objects;

public class PhoneNumber {
    private final short areaCode;
    private final short prefix;
    private final short lineNum;

    public PhoneNumber(short areaCode, short prefix, short lineNum) {
        this.areaCode = areaCode;
        this.prefix = prefix;
        this.lineNum = lineNum;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (!(o instanceof PhoneNumber)) return false;
        PhoneNumber that = (PhoneNumber) o;
        return areaCode == that.areaCode &&
               prefix == that.prefix &&
               lineNum == that.lineNum;
    }

    // 1️⃣ Objects.hash() 사용 (간단하지만 성능 저하)
    @Override
    public int hashCode() {
        return Objects.hash(areaCode, prefix, lineNum);
    }

    /*
    // 2️⃣ 성능이 중요한 경우 직접 해시코드 구현
    @Override
    public int hashCode() {
        int result = Short.hashCode(areaCode);
        result = 31 * result + Short.hashCode(prefix);
        result = 31 * result + Short.hashCode(lineNum);
        return result;
    }
    */
}
```

# 6.캐싱전략
클래스가 불변이고 해시코드를 계산하는 비용이 크다면, 매번 새로 계산하기보다는 캐싱하는 방식을 고려해보자. <br>
###  즉시 계산 (해시 키로 자주 사용되는 경우)
이 타입의 객체가 주로 해시의 키로 사용될 것 같다면 <br> 인스턴스가 만들어질 때 해시코드를 계산해둬야 한다.
```java
public final class PhoneNumber {

    private final short areaCode;
    private final short prefix;
    private final short lineNum;
    private final int hashCode;  // final로 선언

    public PhoneNumber(short areaCode, short prefix, short lineNum) {
        this.areaCode = areaCode;
        this.prefix = prefix;
        this.lineNum = lineNum;
        this.hashCode = computeHashCode();  // 생성 시점에 즉시 계산
    }

    private int computeHashCode() {
        int result = Short.hashCode(areaCode);
        result = 31 * result + Short.hashCode(prefix);
        result = 31 * result + Short.hashCode(lineNum);
        return result;
    }

    @Override
    public int hashCode() {
        return hashCode;  // 저장된 값 반환
    }

    @Override
    public boolean equals(Object o) {
        if (o == this) return true;
        if (!(o instanceof PhoneNumber)) return false;
        PhoneNumber pn = (PhoneNumber) o;
        return pn.areaCode == areaCode &&
               pn.prefix == prefix &&
               pn.lineNum == lineNum;
    }
}
```

 
###  지연 초기화 (드물게 해시 키로 쓰이는 경우)
해시의 키로 사용되지 않는 경우, <br> hashCode가 처음 불릴 때 계산하는 지연 초기화(lazy initalization) 전략을 고려한다.
```java
public final class PhoneNumber {

    private int hashCode; // 기본값 0. 계산된 적이 없다는 신호로 사용.

    private final short areaCode;
    private final short prefix;
    private final short lineNum;

    public PhoneNumber(short areaCode, short prefix, short lineNum) {
        this.areaCode = areaCode;
        this.prefix = prefix;
        this.lineNum = lineNum;
    }

    @Override
    public int hashCode() {
        int result = hashCode;
        if (result == 0) { // 아직 계산되지 않았다면
            result = Short.hashCode(areaCode);
            result = 31 * result + Short.hashCode(prefix);
            result = 31 * result + Short.hashCode(lineNum);
            hashCode = result; // 계산 결과를 필드에 저장
        }
        return result; // 캐시된 값 반환
    }

    @Override public boolean equals(Object o) {
        if (o == this)
            return true;
        if (!(o instanceof PhoneNumber))
            return false;
        PhoneNumber pn = (PhoneNumber)o;
        return pn.lineNum == lineNum && pn.prefix == prefix
                && pn.areaCode == areaCode;
    }
}
```
PhoneNumber는 불변 클래스이기 때문에, hashCode() 값은 객체 생성 시 고정된다. <br>
그래서 한 번만 계산해서 저장하면, 그 이후론 계산 없이 빠르게 반환할 수 있다. <br>
특히 이 객체가 HashMap, HashSet의 키로 쓰일 경우, hashCode()가 반복 호출되기 때문에 효율성이 크게 향상된다.

# 결론
equals를 재정의할 때는 hashcode도 반드시 재정의해야 한다. <br>
그렇지 않으면 프로그램이 제대로 동작하지 않을 것이다.
 
