## Item 1: Consider static factory methods instead of constructors

클래스가 클라이언트에게 인스턴스를 제공하는 전통적인 방법은 public constructor를 제공하는 것이다. 또 다른 기법은 public static factory method를 제공하는 것이다. Static
factory method는 단순히 클래스의 인스턴스를 반환하는 static method다.

**예시: Boolean 클래스**

```java
public static Boolean valueOf(boolean b) {
    return b ? Boolean.TRUE : Boolean.FALSE;
}
```

**주의**: Static factory method는 Design Pattern의 Factory Method 패턴과는 다르다.

### Static Factory Method의 장점

#### 장점 1: 이름을 가질 수 있다

Constructor와 달리 static factory method는 이름을 가질 수 있다. Constructor의 매개변수만으로는 반환될 객체를 잘 설명하지 못하는 경우, 잘 선택된 이름을 가진 static
factory는 사용하기 쉽고 클라이언트 코드의 가독성도 높다.

**예시:**

- `BigInteger(int, int, Random)` constructor보다 `BigInteger.probablePrime`이라는 static factory method가 더 명확하다

클래스는 주어진 시그니처를 가진 constructor를 하나만 가질 수 있다. 프로그래머들은 이 제약을 피하기 위해 매개변수 순서만 다른 두 개의 constructor를 제공하곤 하는데, 이는 나쁜 생각이다.
사용자는 어떤 constructor가 어떤 것인지 기억할 수 없다.

Static factory method는 이름을 가지므로 이런 제약이 없다. 같은 시그니처의 여러 constructor가 필요한 경우, constructor를 static factory method로 교체하고 차이를
강조하는 신중하게 선택된 이름을 사용하라.

#### 장점 2: 호출될 때마다 새 객체를 생성할 필요가 없다

Constructor와 달리 static factory method는 호출될 때마다 새 객체를 생성할 필요가 없다. 이는 immutable class(Item 17)가 미리 생성된 인스턴스를 사용하거나, 생성된
인스턴스를 캐싱하여 반복적으로 제공함으로써 불필요한 중복 객체 생성을 피할 수 있게 한다.

`Boolean.valueOf(boolean)` method가 이 기법을 잘 보여준다: 이 method는 절대 객체를 생성하지 않는다. 이 기법은 Flyweight 패턴과 유사하며, 동등한 객체가 자주 요청되는
경우, 특히 생성 비용이 큰 경우 성능을 크게 향상시킬 수 있다.

**Instance-controlled class:**
반복된 호출에서 같은 객체를 반환하는 static factory method의 능력은 클래스가 어느 시점에 어떤 인스턴스가 존재하는지 엄격하게 제어할 수 있게 한다. 이렇게 하는 클래스를
instance-controlled라고 한다.

Instance control을 하는 이유:

- Singleton(Item 3) 또는 noninstantiable(Item 4)을 보장
- Immutable value class(Item 17)가 동등한 두 인스턴스가 존재하지 않음을 보장: `a.equals(b)` if and only if `a == b`
- 이는 Flyweight 패턴의 기초이며, Enum type(Item 34)이 이 보장을 제공한다

#### 장점 3: 반환 타입의 하위 타입 객체를 반환할 수 있다

Constructor와 달리 static factory method는 반환 타입의 하위 타입 객체를 반환할 수 있다. 이는 반환될 객체의 클래스를 선택하는 데 큰 유연성을 제공한다.

**응용 - Interface-based framework:**
이 유연성의 한 가지 응용은 API가 구현 클래스를 public으로 만들지 않고도 객체를 반환할 수 있다는 것이다. 이런 방식으로 구현 클래스를 숨기면 매우 간결한 API가 만들어진다. 이 기법은
interface-based framework(Item 20)에 적합하며, interface가 static factory method의 자연스러운 반환 타입을 제공한다.

**Java Collections Framework 예시:**

- Java 8 이전에는 interface가 static method를 가질 수 없었다
- 관례상 `Type`이라는 이름의 interface를 위한 static factory method는 `Types`라는 noninstantiable companion class(Item 4)에 배치했다
- Java Collections Framework는 45개의 유틸리티 구현을 가지며, 거의 모든 구현이 `java.util.Collections`라는 하나의 noninstantiable class의 static
  factory method를 통해 export된다
- 반환되는 객체의 클래스는 모두 nonpublic이다

Collections Framework API는 45개의 별도 public class를 export했을 때보다 훨씬 작다. API의 크기뿐만 아니라 개념적 무게(프로그래머가 API를 사용하기 위해 마스터해야 하는
개념의 수와 난이도)도 감소했다.

**Java 8 이후:**

- Java 8부터 interface가 static method를 포함할 수 있는 제약이 제거되었다
- 따라서 일반적으로 interface를 위한 noninstantiable companion class를 제공할 이유가 거의 없다
- 그러나 이러한 static method 뒤의 구현 코드 대부분을 별도의 package-private class에 두는 것이 여전히 필요할 수 있다
- Java 8은 interface의 모든 static 멤버가 public이어야 하기 때문이다
- Java 9는 private static method를 허용하지만, static field와 static member class는 여전히 public이어야 한다

#### 장점 4: 입력 매개변수에 따라 반환 객체의 클래스가 달라질 수 있다

선언된 반환 타입의 하위 타입이면 어떤 클래스든 반환할 수 있다. 반환 객체의 클래스는 릴리스마다 달라질 수도 있다.

**EnumSet 예시 (Item 36):**

```java
// EnumSet은 public constructor가 없고 오직 static factory만 있다
```

OpenJDK 구현에서:

- Underlying enum type의 크기에 따라 두 하위 클래스 중 하나의 인스턴스를 반환한다
- 64개 이하의 원소: `RegularEnumSet` 인스턴스 반환 (single `long`으로 지원)
- 65개 이상의 원소: `JumboEnumSet` 인스턴스 반환 (`long` array로 지원)

이 두 구현 클래스의 존재는 클라이언트에게 보이지 않는다. `RegularEnumSet`이 작은 enum type에 대한 성능 이점을 제공하지 않게 되면, 향후 릴리스에서 제거될 수 있다. 마찬가지로 성능에
유익하다면 세 번째 또는 네 번째 구현이 추가될 수 있다. 클라이언트는 factory에서 반환받는 객체의 클래스를 알지도 못하고 관심도 없다; 단지 `EnumSet`의 하위 클래스이기만 하면 된다.

#### 장점 5: 반환 객체의 클래스가 method를 작성할 시점에 존재하지 않아도 된다

이러한 유연한 static factory method는 service provider framework의 기초를 형성한다. 대표적인 예로 Java Database Connectivity API (JDBC)가 있다.

**Service provider framework란:**
Provider가 service를 구현하고, 시스템이 구현을 클라이언트에게 제공하며, 클라이언트를 구현으로부터 분리하는 시스템이다.

**Service provider framework의 세 가지 필수 컴포넌트:**

| 컴포넌트                      | 역할                                                     | JDBC 예시                      |
|---------------------------|--------------------------------------------------------|------------------------------|
| Service interface         | 구현을 나타냄                                                | Connection                   |
| Provider registration API | Provider가 구현을 등록하는 데 사용                                | DriverManager.registerDriver |
| Service access API        | 클라이언트가 service 인스턴스를 얻는 데 사용 (flexible static factory) | DriverManager.getConnection  |

**선택적 네 번째 컴포넌트:**

- Service provider interface: service interface의 인스턴스를 생성하는 factory 객체를 설명
- JDBC 예시: `Driver`
- Service provider interface가 없으면, 구현은 reflectively하게 인스턴스화되어야 한다 (Item 65)

**추가 정보:**

- Service provider framework 패턴에는 많은 변형이 있다
- 예: Service access API가 provider가 제공하는 것보다 더 풍부한 service interface를 클라이언트에게 반환 가능 (Bridge 패턴)
- Dependency injection framework(Item 5)는 강력한 service provider로 볼 수 있다
- Java 6부터 플랫폼에 범용 service provider framework인 `java.util.ServiceLoader`가 포함되어 있다
- JDBC는 `ServiceLoader`보다 먼저 나왔기 때문에 사용하지 않는다

### Static Factory Method의 단점

#### 단점 1: Public 또는 protected constructor가 없는 클래스는 하위 클래스를 만들 수 없다

예를 들어, Collections Framework의 편의 구현 클래스들은 하위 클래스를 만들 수 없다.

**장점으로 작용할 수도 있음:**

- 프로그래머가 상속 대신 composition을 사용하도록 장려한다 (Item 18)
- Immutable type에 필요하다 (Item 17)

#### 단점 2: 프로그래머가 찾기 어렵다

Static factory method는 API 문서에서 constructor처럼 명확히 드러나지 않는다. Constructor 대신 static factory method를 제공하는 클래스를 어떻게 인스턴스화하는지
알아내기 어려울 수 있다.

**해결 방법:**

- Class 또는 interface 문서에서 static factory에 주의를 환기시킨다
- 일반적인 명명 규칙을 따른다

### Static Factory Method의 일반적인 명명 규칙

| 메서드 이름                          | 의미                                                                                      | 예시                                                            |
|---------------------------------|-----------------------------------------------------------------------------------------|---------------------------------------------------------------|
| **from**                        | 단일 매개변수를 받아 해당 타입의 인스턴스를 반환하는 type-conversion method                                    | `Date d = Date.from(instant);`                                |
| **of**                          | 여러 매개변수를 받아 이들을 통합한 인스턴스를 반환하는 aggregation method                                       | `Set<Rank> faceCards = EnumSet.of(JACK, QUEEN, KING);`        |
| **valueOf**                     | `from`과 `of`의 더 자세한 대안                                                                  | `BigInteger prime = BigInteger.valueOf(Integer.MAX_VALUE);`   |
| **instance** 또는 **getInstance** | 매개변수로 명시한 인스턴스를 반환하지만, 같은 값을 갖는다고 보장하지 않음                                               | `StackWalker luke = StackWalker.getInstance(options);`        |
| **create** 또는 **newInstance**   | `instance` 또는 `getInstance`와 같지만, 매번 새로운 인스턴스를 생성함을 보장                                  | `Object newArray = Array.newInstance(classObject, arrayLen);` |
| **getType**                     | `getInstance`와 같지만, factory method가 다른 클래스에 있을 때 사용. `Type`은 factory method가 반환할 객체의 타입 | `FileStore fs = Files.getFileStore(path);`                    |
| **newType**                     | `newInstance`와 같지만, factory method가 다른 클래스에 있을 때 사용. `Type`은 factory method가 반환할 객체의 타입 | `BufferedReader br = Files.newBufferedReader(path);`          |
| **type**                        | `getType`과 `newType`의 간결한 대안                                                            | `List<Complaint> litany = Collections.list(legacyLitany);`    |

### 요약

Static factory method와 public constructor는 각각의 용도가 있으며, 상대적인 장점을 이해하는 것이 중요하다. 종종 static factory가 더 적합하므로, 먼저 static
factory를 고려하지 않고 무조건 public constructor를 제공하는 습관은 피하라.