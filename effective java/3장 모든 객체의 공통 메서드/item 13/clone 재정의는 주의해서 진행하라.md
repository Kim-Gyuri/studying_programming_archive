# Cloneable
Cloneable은 `복제해도 되는 클래스임을 명시하는` 용도의 Mixin interface이다. <br>
이 인터페이스는 `메서드를 하나도 정의하지 않으며`, 단지 Object 클래스의 clone() 메서드의 동작 여부에 영향만 준다.

##  Cloneable의 동작 방식
#### Cloneable을 구현하지 않은 객체
Object.clone()은 protected 메서드이며, Cloneable을 구현하지 않은 객체에서 호출하면 CloneNotSupportedException이 발생한다.
#### Cloneable을 구현한 클래스
반면, Cloneable을 구현한 클래스의 인스턴스에서 super.clone()을 호출하면,
해당 객체의 필드를 단순 복사한 새 객체(얕은 복사)를 반환한다.

## 정상적인 clone 동작을 위한 조건
외부에서 clone()을 안전하게 호출하고 싶다면, 다음 두 가지를 모두 만족해야 한다.
+ Cloneable 인터페이스 구현
+ clone() 메서드를 public으로 오버라이딩

### 하지만 이것만으로는 안전하지 않다.
Cloneable을 구현하고 clone()을 public으로 제공하더라도, 다음과 같은 심각한 한계가 있다. <br>
+ super.clone()은 얕은 복사만 수행 → 참조 필드가 공유됨
+ 상속 구조에서는 clone() 구현이 매우 취약하고 오류 발생 가능성 높음
+ CloneNotSupportedException이라는 불필요한 checked 예외

생성자 호출을 우회하기 때문에 불변식(invariant)이 깨질 위험이 있다.

## 1. Clone 메서드의 일반 규약
### (1) 클론 객체와 원본 객체는 서로 다른 객체이다.
```java
x.clone() != x   
```
+ 복사된 객체는 원본과 물리적으로 다른 객체여야 한다.
+ 즉, 동일한 참조가 아니어야 하며, 식별자(==) 비교 시 false여야 한다.

### (2) 클론 객체는 원본 객체와 동등한 값을 가져야 한다.
equals() 메서드를 오버라이드한 경우, 논리적으로 동등해야 한다.
```java
x.clone().equals(x)  
```
단, equals()를 재정의하지 않았다면 기본적으로 == 비교이므로 위 조건은 성립하지 않을 수 있다. <br>
따라서 이 조건을 충족하려면 equals()를 명시적으로 구현해야 한다.

### (3) 클론 객체의 클래스는 원본 객체와 동일해야 한다.
복사된 객체는 같은 클래스의 인스턴스여야 한다.
```java
x.clone().getClass() == x.getClass()   
```

## 2. Clone 메서드의 주의점
### clone()은 반드시 super.clone()을 호출해야 하는 건 아니다.
####  clone()은 생성자 연쇄(constructor chaining)과 비슷하다.
일반적으로 객체를 만들 때는 생성자에서 super()를 호출해서 상위 클래스까지 초기화를 이어간다. <br>
이것을 생성자 연쇄라고 한다. <br><br>

clone()도 비슷하게 super.clone()을 호출해서 상위 클래스의 복사 기능을 이어받는다. <br>
그래서 마치 생성자 연쇄처럼 보인다.
```java
@Override
public MyClass clone() {
    MyClass copy = (MyClass) super.clone(); // 상위 클래스의 복사
    return copy;
}
```

#### 생성자 연쇄와 clone()의 차이점
`생성자는 반드시 super()를 호출해야 하지만,` `clone()은 super.clone()을 생략해도 된다.` <br>
대신, clone()에서 직접 new 키워드로 새 객체를 만들어 반환해도 컴파일 에러는 나지 않는다. 
```java
@Override
public MyClass clone() {
    return new MyClass(this); // 생성자 직접 호출해서 복사
}
```
생성자 연쇄(Constructor chainging)와 살짝 비슷한 메커니즘이다.
즉, clone 메서드가  super.clone이 아닌, 생성자를 호출해 얻은 인스턴스를 반환해도 컴파일러는 불평하지 않을 것이다.

#### 만약 super.clone()을 호출하지 않고 직접 객체를 만들면, Cloneable을 구현한 의미가 사라진다.
특히 final 클래스라면 clone()을 상속받을 일도 없기 때문에, 굳이 Cloneable을 구현할 이유도 없다.


## 3. 상위 클래스를 제대로 상속했을 때 Cloneable 구현
#### (1) clone() 메서드를 올바르게 구현하려면 반드시 super.clone()을 호출해야 한다.
그래야 상위 클래스가 정의한 모든 필드도 제대로 복사된다.

#### (2) 불변(immutable) 클래스는 복사할 필요 없이 기존 인스턴스를 재사용할 수 있으므로, 대부분 clone()을 제공하지 않는 것이 바람직하다.

### 예시: PhoneNumber의 clone() 구현
```java
@Override
public PhoneNumber clone() {
    try {
        return (PhoneNumber) super.clone();  // 얕은 복사
    } catch (CloneNotSupportedException e) {
        throw new AssertionError();  // clone()을 막으려면 Cloneable 미구현
    }
}
```
Object.clone()은 반환 타입이 Object이지만,
위 코드처럼 하위 클래스에서 반환 타입을 PhoneNumber로 좁힐 수 있다.

#### 자바는 공변 반환 타입을 지원하기 때문에 Object를 반환하던 메서드를 PhoneNumber처럼 더 구체적인 타입으로 재정의할 수 있다.
자바는 공변 반환 타입을 지원하기 때문에, Object를 반환하던 메서드를 PhoneNumber처럼 더 구체적인 타입으로 재정의할 수 있다. <br>
>  `공변 반환 타입`이란? <br>
> 부모 클래스 메서드는 Object를 반환하지만, 자식 클래스에서는 더 구체적인 타입으로 반환해도 된다.

## 4. 가변 객체에서 clone()을 사용할 경우 주의점
```java
public class Stack implements Cloneable {
    private Object[] elements;
    private int size;

    @Override
    public Stack clone() {
        try {
            Stack result = (Stack) super.clone();  // 얕은 복사
            result.elements = elements.clone();   // 배열 깊은 복사
            return result;
        } catch (CloneNotSupportedException e) {
            throw new AssertionError();
        }
    }
}
```
#### 왜 배열을 따로 복사해야 할까?
super.clone()은 얕은 복사(shallow copy)를 수행한다. <br>
즉, 복사된 객체의 elements 필드는 원본 객체가 참조하는 동일한 배열 객체를 참조한다. <br><br>

이렇게 되면 원본 객체의 배열을 수정할 경우, <br>
복제 객체도 영향을 받게 되어 예상치 못한 버그가 발생할 수 있다. <br><br>

#### 따라서 가변 객체 필드는 별도로 깊은 복사를 해야 한다.
위 코드처럼 elements.clone()을 호출해 배열 자체를 복제한다. <br>
그래야 복사된 객체는 완전히 독립적인 상태를 유지할 수 있다.

#### clone()의 역할과 책임
clone()은 원본 객체를 전혀 훼손하지 않고, <br>
복제된 객체가 자신의 불변식(invariant)과 상태를 제대로 유지하도록 해야 한다. <br><br>

즉, 생성자처럼 새 객체를 만들어 반환하는 역할을 하며, <br>
복사본이 원본과 독립적으로 동작하도록 보장하는 것이 중요하다. <br>


## 5. 배열의 복제
+ 배열은 clone() 메서드를 사용하라고 권장한다.
+ 배열은 자바에서 clone() 기능을 제대로 지원하는 거의 유일한 객체 타입이다.

따라서 배열 복사가 필요할 때는 직접 반복문을 쓰지 말고, 배열의 clone()을 활용하는 것이 가장 쉽고 안전하다.
```java
int[] original = {1, 2, 3};
int[] copy = original.clone();  // 완전한 복제본 생성
```


## 6. HashTable에서 복제 (복잡한 가변 객체 복제)
### HashTable 내부 구조
+ `버킷(bucket)` 배열이다.
+ 각 버킷은 `키-값 쌍`을 담는 연결 리스트의 첫 번째 노드(Entry)를 참조한다.

### (1) 버킷 배열 복제
`super.clone()` 또는 `elements.clone()`처럼 버킷 배열 자체는 `복제하지만 배열 안에 있는 연결 리스트(Entry) 노드들은 원본 객체와 동일한 참조`를 가진다. <br>
얕은 복사 문제가 발생한다.

### (2) 연결 리스트(버킷 내부 Entry) 복사 필요
각 버킷마다 연결 리스트를 새로운 노드들로 복사해야 원본과 독립적 복제본이 된다.

### 3) 연결 리스트 복사 방식
HashTable의 복제를 제대로 하려면, <br>
먼저 buckets 필드를 새로운 버킷 배열로 초기화하고, <br>
그다음 원본 테이블에 담긴 모든 (key, value) 쌍을 순회하면서 <br> 복제본 테이블의 put(key, value) 메서드를 호출해 내용을 복사한다.  <br>
<img width="400" height="400" alt="image" src="https://github.com/user-attachments/assets/6d0caac9-9a71-4289-bb04-253cb94e4091" />

#### (3-1) 재귀 호출 방식
연결 리스트 복제를 재귀적으로 구현한다.
+ 장점: 코드 간결
+ 단점: 호출 스택이 계속 쌓여 스택 오버플로우 위험하다.

#### (3-2) 반복자(iterator) 방식
재귀 대신 반복문을 사용해 연결 리스트 순회 및 복제한다.
+ 장점: 스택 오버플로 위험 없음, 안전함

#### (3-3) 고수준 API 사용 (put(key, value))
복제 시 직접 내부 구조를 복사하지 않고, put 메서드를 호출해 원본 상태를 새 객체에 재구성한다.
+ 단점
  +  복사 속도가 느림 (저수준 필드 복사 우회)
  +  Cloneable 아키텍처의 핵심인 필드 단위 복사를 하지 않는다.


## 7. clone()의 상속에서의 주의점
### (1) 하위 클래스에서 Cloneable 구현 여부를 선택하게 하기
상위 클래스에서는 Cloneable을 구현하지 않고, 하위 클래스가 필요할 경우 직접 구현하도록 한다. <br><br>

이렇게 하면 상위 클래스의 설계 유연성을 유지할 수 있고, <br>
하위 클래스가 복제를 필요로 할 때만 clone()을 활성화하게 만들 수 있다.

### (2) 기본적으로 clone()을 막아두고, 하위 클래스에서 재정의 못하게 하기
복제를 아예 금지하고 싶다면, clone()을 다음처럼 작성해 final로 막는다. <br>
```java
@Override
protected final Object clone() throws CloneNotSupportedException {
    throw new CloneNotSupportedException();
}
```

+ final: 하위 클래스에서 오버라이드(재정의) 못하게 함
+ throw: 복제를 시도하면 즉시 예외 발생 → 객체 복제 금지 명시

이 방법은 객체 복제를 설계적으로 완전히 차단하고 싶을 때 사용한다.


## 8. clone 재정의 방법
요약하자면, Cloneable을 구현하는 모든 클래스는 clone을 재정의해야 한다.
+ 이때 접근 제한자는 public으로 변경한다.
+ 반환 타입은 Object가 아닌 자신의 클래스 타입으로 바꾼다. (공변 반환 타입 활용)

### 구현 흐름
+ `super.clone()`을 가장 먼저 호출하여 `필드 복사`를 수행한다.
+ 그 다음, 복제된 객체 내의 `가변 객체 참조들을 전부 복사`하여 <br> `깊은 복사`가 이루어지도록 수정한다.
+ 복제본의 모든 참조 필드는, `원본과 동일한 객체를 참조하지 않도록` 해야 한다.

### 예외적으로 필드 수정을 생략해도 되는 경우
클래스가 `기본 타입 필드와 불변 객체(String 등) 참조만` 갖는 경우엔, <br>
별도로 필드를 수정하지 않아도 괜찮다.

### 주의할 점
필드가 비록 기본 타입이거나 불변이더라도, <br>
그 값이 일련번호, 고유 ID 같은 의미를 갖는 경우엔 <br>
 복제본에서 반드시 새로운 값으로 수정해줘야 한다.


## 9.  clone() 재정의의 대안
clone() 메서드를 재정의하는 대신, 다음 두 가지 방법을 사용하는 것이 더 안전하다.
+ 복사 생성자 (Copy Constructor)
+ 복사 팩터리 (Copy Factory)

### (1) 복사 생성자 (Copy Constructor)
자신과 같은 클래스의 인스턴스를 인자로 받아 복사본을 생성하는 생성자다.
```java
public class Yum {
    public Yum(Yum original) {
        // 원본의 필드를 복사
    }
}
```

### (2) 복사 팩터리 (Copy Factory)
정적 팩터리 메서드로 복사본을 생성한다.
```java
public class Yum {
    public static Yum newInstance(Yum original) {
        return new Yum(original); // 내부적으로 복사 생성자 호출
    }
}
```

### 복사 생성자 / 복사 팩터리의 장점
복사 생성자와 그 변형인 복사 팩터리는 Cloneable/clone 방식보다 나은 면이 많다.

| 항목                      | 설명                                                  |
| ----------------------- | --------------------------------------------------- |
| ✅ **위험한 객체 생성 매커니즘 회피** | `clone()`처럼 생성자 없이 객체를 생성하지 않음                      |
| ✅ **명확한 문서화**           | `Cloneable`처럼 문서화되지 않은 규약에 의존하지 않음                  |
| ✅ **`final` 필드와의 호환성**  | `final` 필드도 복사 생성자 안에서 안전하게 초기화 가능                  |
| ✅ **검사 예외 없음**          | `CloneNotSupportedException` 같은 불필요한 검사 예외를 던지지 않음  |
| ✅ **형변환 불필요**           | 반환 타입이 자신의 타입이라 다운캐스팅 필요 없음                         |
| ✅ **인터페이스 인수 허용**       | 자신이 구현한 인터페이스 타입으로 인자를 받을 수 있음 (→ **변환 생성자 / 팩터리**) |

# 결론
clone()은 피하고, 복사 생성자 또는 복사 팩터리를 제공하자.
