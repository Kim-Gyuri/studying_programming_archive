# public 클래스에서는 public 필드가 아닌 접근자 메서드를 사용하라
### 포인트
+ public 클래스는 가변 필드를 public으로 노출하면 안된다.
+ package-private 클래스나 private 중첩 클래스에서는 종종 (가변/불변) 필드를 노출하는 편이 나올 때도 있다. 

## 캡슐화의 이점을 제공하지 못하는 클래스
다음과 같이 예시 코드가 있다.
```java
/**
 * Point 클래스를 사용하여 좌표를 나타내고 있다.
 * 클래스의 필드가 public으로 선언되어 있어 외부에서 직접 접근이 가능하다.
*/
class Point {
        public double x;
        public double y;
}
```

### 1. API를 수정하지 않고는 내부 표현을 바꿀 수 없다.
#### Getter/Setter를 이용하면 표현 변경 가능하다.
+ 패키지 바깥에서 접근할 수 있는 클래스라면 접근자(getter)를 제공하자.
+ 클래스 내부 표현 방식을 언제든 바꿀 수 있다.
```java
public double getX() { return x; }

public double getY() { return y; }
```

### 2. 불변식을 보장할 수 없다.
#### 클라이언트가 직접 값을 변경할 수 있다.
```java
public static void main(String[] args) {
     Point point = new Point(1, 2);
     System.out.println(point.x); // 1
     
     point.x += 1; // 값 변경 가능
     System.out.println(point.x); // 2
}
```
 
### 3. 외부에서 필드에 접근할 때 부수 작업을 수행할 수도 없다.
1차원적인 접근만 가능하다.
> `1차원적인 접근` : point.x와 같이 필드에 직접 접근하는 것


추가 로직을 삽입할 수 없다.
> `부가적인 작업 불가능하다` : 필드에 접근할 때 특정한 작업(예: 유효성 검사, 로깅)을 수행할 수 없습니다.
```java
// 유효성 검사를 할 수 있는 방법이 없다.
point.x = -5.0; // 유효하지 않은 값 할당 (<-- 해결 방안 없음)

// 로깅을 할 수 있는 방법이 없다.
System.out.println("Current x value: " + point.x);
```


## 철저한 객체 지향 프로그래머는 캡슐화한 클래스를 만든다.
#### 1. 필드들을 모두 private으로 바꾼다.
#### 2. public 접근자 (getter), 변경자 (setter)를 추가한다.
getter/setter 메서드를 통해 언제든지 내부 표현을 바꿀 수 있다.
```java
/**
 * Point 클래스를 사용하여 좌표를 나타내고 있다.
 */
class Point {
    private double x;
    private double y;
   
    public Point(double x, double y) {
       this.x = x;
       this.y = y;
    }
    
    public double getX() {
        return x;
    }
    
    public double getY() {
        return  y;
    }
    
    public void setX(double x) {
        this.x = x;
    }
    
    public void setY(double y) {
        this.y = y;
    }
}
```

## package-private 클래스
+ 같은 패키지 내에서만 접근이 가능하다는 의미이다. 
+ 다른 패키지에서는 직접적으로 이 클래스에 접근할 수 없다.

## private 중첩 클래스
이 클래스를 포함하는 외부 클래스까지로 제한한다.

### 예시 코드 : OuterClass 내부의 private 중첩 클래스
아래는 Point 클래스를 OuterClass 내부의 private 중첩 클래스로 작성한 예시이다.
+ Point 클래스를 OuterClass 외부에서는 직접적으로 접근할 수 없게 만들었다. 
+ 외부에서는 OuterClass를 통해 Point 클래스에 접근하고 사용할 수 있다.
```java
public class OuterClass {

    private static class Point {
        private double x;
        private double y;

        // 생성자
        public Point(double x, double y) {
            this.x = x;
            this.y = y;
        }

        // Getter 메서드
        public double getX() {
            return x;
        }

        public double getY() {
            return y;
        }

        // Setter 메서드
        public void setX(double x) {
            this.x = x;
        }

        public void setY(double y) {
            this.y = y;
        }
    }

    // OuterClass의 다른 멤버들
    // ...
}

```

## 자바 플랫폼 라이브러리에서 필드를 노출시킨 사례
java.awt.package 패키지의 Point, Dimension 클래스다.


## 예시 : 불변 필드를 노출한 public 클래스
```java
public class Time {
               // 불변식 : 시간과 분은 더 이상 변경될 수 없다(final 필드).
	private static final int HOURS_PER_DAY = 24;
	private static final int MINUTES_PER_HOUR = 60;

	public final int hour;
	public final int minute;

	// 생성자에서 유효성 검사를 수행하여 불변식을 보장한다.
	public Time(int hour, int minute) {
		if (hour < 0 || hour > HOURS_PER_DAY) {
			throw new IllegalArgumentException("시간: " + hour);
		}
		if (minute < 0 || minute > MINUTES_PER_HOUR) {
			throw new IllegalArgumentException("분: " + minute);
		}
		this.hour = hour;
		this.minute = minute;
	}

	...
}
```

### 불변식은 보장할 수 있다.
#### 각 인스턴스가 유효한 시간을 표현함을 보장한다.
불변식은 "해당 클래스의 인스턴스가 항상 특정한 조건을 만족해야 함"을 나타내는 것이다. <br>
Time 클래스에서는 다음과 같은 불변식이 보장되어야 합니다. 
+ 시간의 유효성: 시간은 음수가 될 수 없고, 0부터 23까지의 값을 가져야 한다.
+ 분의 유효성: 분은 음수가 될 수 없고, 0부터 59까지의 값을 가져야 한다.

이러한 불변식은 클래스의 사용자에게 다음을 보장한다. <br>
+ 인스턴스의 상태 변화가 없음 : <br> Time 클래스의 인스턴스가 한 번 생성되면 시간과 분은 더 이상 변경될 수 없다(final 필드).
+ 유효한 시간 표현: <br> 생성자에서 시간과 분을 초기화할 때 유효성 검사를 수행하여 유효하지 않은 값이 들어오지 않도록 보장한다. <br> 이는 객체의 상태가 항상 유효한 시간을 나타낼 것임을 보장한다.

<br>

### 여전히 불변식 보장 외의 두가지 문제를 해결할 수 없다.
아래 2가지를 여전히 해결할 수 없다.
+ API를 변경하지 않고는 표현 방식을 바꿀 수 없다.
+ 필드를 읽을 때 부수 작업을 수행할 수 없다.






