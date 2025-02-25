# 1. ordinal()을 배열 인덱스로 사용하지 말자.
배열이나 리스트에서 원소를 꺼낼 때 ordinal 메서드로 인덱스를 얻는 코드가 있다. <br>
하지만 문제가 한가득이다.
### 예시
```java
class Plant {

    enum LifeCycle { ANNUAL, PERENNIAL, BIENNIAL }

    final String name; // 식물 이름
    final LifeCycle lifeCycle; // 생애주기

    Plant(String name, LifeCycle lifeCycle) {
        this.name = name;
        this.lifeCycle = lifeCycle;
    }

    @Override public String toString() {
        return name;
    }
}
```
식물을 간단히 나타낸 다음 클래스를 예로 살펴보자. <br>
식물들을 배열 하나로 관리하고, 이들을 생애주기(한해살이, 여러해살이, 두해살이)별로 묶었다. <br>

### 생애주기의 oridinal 값을 그 배열의 인덱스로 사용하려 할 때, 문제가 한가득이다.
문제점을 설명하기 위한 예제 코드는 다음과 같다. <br>
```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        List<Plant> garden = Arrays.asList(
                new Plant("Rose", Plant.LifeCycle.PERENNIAL),
                new Plant("Sunflower", Plant.LifeCycle.ANNUAL),
                new Plant("Carrot", Plant.LifeCycle.BIENNIAL)
        );

        // 생애주기별로 식물을 저장할 배열 (ordinal()로 인덱스를 사용)
        @SuppressWarnings("unchecked")
        List<Plant>[] plantsByLifeCycle = (List<Plant>[]) new List[Plant.LifeCycle.values().length];

        // 배열 초기화
        for (int i = 0; i < plantsByLifeCycle.length; i++) {
            plantsByLifeCycle[i] = new ArrayList<>();
        }

        // ordinal()을 인덱스로 사용하여 분류
        for (Plant plant : garden) {
            plantsByLifeCycle[plant.lifeCycle.ordinal()].add(plant);
        }

        // 결과 출력
        for (int i = 0; i < plantsByLifeCycle.length; i++) {
            System.out.printf("%s: %s%n", Plant.LifeCycle.values()[i], plantsByLifeCycle[i]);
        }
    }
}
```
##### 1. 배열은 제네릭과 호환되지 않아 비검사 형변환을 수행해야 한다. 
List<Plant>[] 배열을 생성할 때 제네릭 배열을 직접 생성할 수 없기 때문에 @SuppressWarnings("unchecked")로 경고를 억제해야 한다. <br>
이는 타입 안정성을 보장하지 못하므로 런타임 오류의 위험이 있다.

#### 2. 정수 기반 인덱스 사용의 위험하다.
ordinal()을 인덱스로 사용하면 열거형 상수의 순서가 변경되거나 새로운 상수가 추가될 경우, 잘못된 인덱스를 참조할 위험이 있다. <br>
예를 들어, LifeCycle에 새로운 항목이 추가되면 배열에 누락되거나 ArrayIndexOutOfBoundsException이 발생할 수 있다.


# 2. EnumMap : 열거 타입을 키로 사용
열거 타입을 키로 사용하도록 설계한 Map 구현체이다.

```java
import java.util.*;

import static java.util.stream.Collectors.groupingBy;
import static java.util.stream.Collectors.toSet;

public class Main {
    public static void main(String[] args) {

        Plant[] garden = {
                new Plant("Basil",    Plant.LifeCycle.ANNUAL),
                new Plant("Carroway", Plant.LifeCycle.BIENNIAL),
                new Plant("Dill",     Plant.LifeCycle.ANNUAL),
                new Plant("Lavendar", Plant.LifeCycle.PERENNIAL),
                new Plant("Parsley",  Plant.LifeCycle.BIENNIAL),
                new Plant("Rosemary", Plant.LifeCycle.PERENNIAL)
        };


        // EnumMap을 사용해 데이터와 열거 타입을 매핑한다.
        Map<Plant.LifeCycle, Set<Plant>> plantsByLifeCycle = new EnumMap<>(Plant.LifeCycle.class);

        for (Plant.LifeCycle lc : Plant.LifeCycle.values())
            plantsByLifeCycle.put(lc, new HashSet<>());

        for (Plant p : garden)
            plantsByLifeCycle.get(p.lifeCycle).add(p);

        System.out.println(plantsByLifeCycle);
    }
}
```
EnumMap을 사용하면, 더 짧고 명료하고 안전하고 성능도 원래 버전과 비등하다. <br>
장점은 다음과 같다.
+ 안전하지 않은 형변환 사용하지 않는다.
+ 배열 인덱스 계산 과정 오류 가능성도 없다.
+ 내부에서 배열을 사용해서 ordinal 배열만큼 성능이 나온다 <br> (낭비 되는 공간과 시간이 거의 없어 명확하고 안전하고 유지보수하기 좋다.)

#### EnumMap의 생성자가 받는 키 타입의 Class 객체는 `한정적 타입 토큰` (Bounded Type Token)의 예시이다. <br> 이를 통해 런타임에 제네릭 타입 정보를 유지하고, 타입 안전성을 보장한다.
> 제네릭은 `타입 소거`(Type Erasure)로 인해 런타임에 구체적인 타입 정보를 알 수 없다. <br>
> `Class<T>` 객체는 제네릭 타입의 런타임 정보를 제공하며, 이를 `타입 토큰`(Type Token)이라 한다. <br>
> EnumMap에서는 특정 enum 타입의 Class 객체를 인자로 받아, 해당 열거형의 모든 상수를 기반으로 맵을 최적화한다.

```java
// EnumMap 생성 시 LifeCycle.class를 전달한다. (한정적 타입 토큰 사용)
Map<Plant.LifeCycle, List<Plant>> plantsByLifeCycle = new EnumMap<>(Plant.LifeCycle.class);
```

# 3. 스트림 사용 시 EnumMap의 최적화 효과가 사라지는 문제점
스트림을 사용할 때 고유한 맵 구현체를 사용하기 때문에 EnumMap을 사용할 때 얻는 공간과 성능 이점이 사라진다는 문제가 있다.
```java
        System.out.println(Arrays.stream(garden)
                .collect(groupingBy(p -> p.lifeCycle))); // 고유한 맵 구현체


        System.out.println(Arrays.stream(garden)
                .collect(groupingBy(p -> p.lifeCycle,  // 원하는 맵 구현체 명시
                        () -> new EnumMap<>(Plant.LifeCycle.class), toSet())));
```

### 첫 번째 예제: HashMap 사용
 groupingBy()는 기본적으로 HashMap을 반환하기 때문에 EnumMap의 성능과 메모리 최적화를 활용할 수 없다.

### 두 번째 예제: EnumMap 명시적 사용
groupingBy()의 세 번째 인자로 맵 생성자를 전달하면 원하는 구현체(EnumMap)을 사용할 수 있다. <br>
이 방식으로 EnumMap의 성능과 메모리 최적화를 유지한다. <br><br><br>

## 다차원 관계 : 중첩 EnumMap으로 표현하라.
물질의 상전이(Phase Transition)를 EnumMap을 사용해 효율적으로 매핑할 수 있다.
```java
import java.util.*;
import java.util.stream.Stream;

import static java.util.stream.Collectors.*;

/**
 * Phase(상태): 물질의 세 가지 상태(고체, 액체, 기체)를 나타냅니다.
 * Transition(상전이): 특정 **상태(Phase)**에서 **다른 상태(Phase)**로 변화하는 과정을 나타냅니다.
 */

// 중첩 EnumMap으로 데이터와 열거 타입 쌍을 연결했다.
public enum Phase {

    SOLID, LIQUID, GAS;

    // 상전이와 방향 설정 (ex: MELT는 SOLID → LIQUID)
    public enum Transition {

        MELT(SOLID, LIQUID), FREEZE(LIQUID, SOLID),
        BOIL(LIQUID, GAS), CONDENSE(GAS, LIQUID),
        SUBLIME(SOLID, GAS), DEPOSIT(GAS, SOLID);


        private final Phase from;
        private final Phase to;
        Transition(Phase from, Phase to) {
            this.from = from;
            this.to = to;
        }

        // 중첩 EnumMap 생성
        // groupingBy()와 toMap()을 사용해 출발 상태(from)와 도착 상태(to)를 키로 하는 중첩 EnumMap을 생성한다.
        private static final Map<Phase, Map<Phase, Transition>>
                m = Stream.of(values()).collect(groupingBy(t -> t.from,
                () -> new EnumMap<>(Phase.class),
                toMap(t -> t.to, t -> t,
                        (x, y) -> y, () -> new EnumMap<>(Phase.class))));

        // 상전이 조회 메서드
        public static Transition from(Phase from, Phase to) {
            return m.get(from).get(to);
        }
    }

    // 예시
    public static void main(String[] args) {
        for (Phase src : Phase.values()) {
            for (Phase dst : Phase.values()) {
                Transition transition = Transition.from(src, dst);
                if (transition != null)
                    System.out.printf("%s to %s : %s %n", src, dst, transition);
            }
        }
    }
}
```
```
SOLID to LIQUID : MELT 
SOLID to GAS : SUBLIME 
LIQUID to SOLID : FREEZE 
LIQUID to GAS : BOIL 
GAS to SOLID : DEPOSIT 
GAS to LIQUID : CONDENSE
```
groupingBy()와 toMap()을 사용해 `출발 상태(from)`와 `도착 상태(to)`를 키로 하는 **중첩 EnumMap**을 생성한다.


## 핵심정리
+ 배열의 인덱스를 얻기 위해 ordinal을 쓰는 것은 좋지 않다.
+ 대신 EnumMap을 사용하라.
+ 다차원 관계는 `EnumMap<..., EnumMap<...>>`으로 표현하라.
