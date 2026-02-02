# Item 24: Favor static member classes over nonstatic

## Nested Class의 종류

Nested class는 다른 클래스 내부에 정의된 클래스다. Nested class는 오직 자신을 감싸고 있는 클래스를 위해서만 존재해야 한다. 네 가지 종류의 nested class가 있다:

| 종류                     | Inner Class 여부 |
|------------------------|----------------|
| Static member class    | X              |
| Nonstatic member class | O              |
| Anonymous class        | O              |
| Local class            | O              |

## Static Member Class

Static member class는 가장 단순한 형태의 nested class다. 다른 클래스 내부에 선언된 일반 클래스로 생각하면 되며, enclosing class의 모든 멤버(private 포함)에 접근할 수
있다. Static member class는 다른 static 멤버들과 동일한 접근 규칙을 따른다.

### 일반적인 사용 사례

Public helper class로 자주 사용된다. 예를 들어, 계산기의 연산을 나타내는 enum을 고려해보자:

```java
// Calculator 클래스의 public static member class
class Calculator {
    public static enum Operation {
        PLUS, MINUS, ...
    }
}

// 클라이언트 사용
Calculator.Operation.PLUS
Calculator.Operation.MINUS
```

## Nonstatic Member Class

### Static vs Nonstatic의 구문적 차이

구문적으로는 `static` modifier의 유무만 다르지만, 두 종류는 매우 다르다.

### 핵심 차이점

Nonstatic member class의 각 인스턴스는 enclosing class의 인스턴스와 암묵적으로 연결된다. Nonstatic member class의 instance method 내에서는:

- Enclosing instance의 메서드를 호출할 수 있다
- Qualified this 구문을 사용해 enclosing instance의 참조를 얻을 수 있다

Nested class가 enclosing class의 인스턴스 없이 독립적으로 존재할 수 있다면, 반드시 static member class여야 한다.

### 연결(Association) 방식

Nonstatic member class 인스턴스와 enclosing instance 간의 연결은:

- 멤버 클래스 인스턴스가 생성될 때 확립된다
- 이후 수정할 수 없다
- 일반적으로 enclosing class의 instance method 내에서 nonstatic member class 생성자를 호출하여 자동으로 확립된다
- 드물게 `enclosingInstance.new MemberClass(args)` 표현식으로 수동 확립 가능하다

### 일반적인 사용 사례: Adapter 패턴

Nonstatic member class는 Adapter를 정의하는 데 자주 사용된다. Outer class의 인스턴스를 다른 클래스의 인스턴스처럼 보이게 한다.

**Map 인터페이스 구현 예시:**

- `Map`의 `keySet`, `entrySet`, `values` 메서드가 반환하는 collection view들을 구현할 때 사용

**Collection 인터페이스의 Iterator 구현:**

```java
// Nonstatic member class의 전형적인 사용
public class MySet<E> extends AbstractSet<E> {
    ... // 클래스의 대부분 생략
    
    @Override public Iterator<E> iterator() {
        return new MyIterator();
    }
    
    private class MyIterator implements Iterator<E> {
        ...
    }
}
```

## 중요: Static을 사용해야 하는 이유

**Enclosing instance에 접근할 필요가 없는 member class를 선언할 때는 반드시 `static` modifier를 추가하라.**

Static modifier를 생략하면:

- 각 인스턴스가 enclosing instance에 대한 숨겨진 외부 참조를 갖게 된다
- 이 참조를 저장하는 데 시간과 공간이 소요된다
- **더 심각하게는**, enclosing instance가 가비지 컬렉션 대상이 되어야 할 때도 유지될 수 있다 (Item 7)
- 결과적으로 메모리 누수가 발생할 수 있으며, 이는 치명적일 수 있다
- 참조가 보이지 않기 때문에 감지하기 어렵다

### Private Static Member Class의 사용

Private static member class는 enclosing class가 나타내는 객체의 컴포넌트를 표현하는 데 자주 사용된다.

**Map 구현의 Entry 예시:**

- `Map` 인스턴스는 키와 값을 연결한다
- 많은 `Map` 구현은 각 key-value 쌍마다 내부 `Entry` 객체를 가진다
- 각 entry는 map과 연결되어 있지만, entry의 메서드들(`getKey`, `getValue`, `setValue`)은 map에 접근할 필요가 없다
- 따라서 nonstatic member class를 사용하는 것은 낭비다
- Private static member class가 최선이다
- Entry 선언에서 실수로 `static` modifier를 생략하면:
    - Map은 여전히 작동하지만
    - 각 entry가 map에 대한 불필요한 참조를 포함하게 되어 공간과 시간을 낭비한다

### API 설계 시 주의사항

Public이나 protected member로 exported class의 일부인 경우, static과 nonstatic 중 올바르게 선택하는 것이 특히 중요하다. 이 경우 member class는 exported
API 요소이므로, 이후 릴리스에서 nonstatic에서 static member class로 변경하면 하위 호환성을 위배하게 된다.

## Anonymous Class

Anonymous class는 이름이 없다. Enclosing class의 멤버가 아니며, 다른 멤버들과 함께 선언되는 것이 아니라 사용 지점에서 동시에 선언되고 인스턴스화된다.

### 특징

- 표현식이 합법적인 코드의 모든 지점에서 허용된다
- Nonstatic context에서 발생할 때만 enclosing instance를 가진다
- Static context에서 발생하더라도, constant variable(상수 표현식으로 초기화된 final primitive 또는 string 필드) 외에는 static 멤버를 가질 수 없다

### 제약사항

- 선언된 지점 외에는 인스턴스화할 수 없다
- `instanceof` 테스트나 클래스 이름이 필요한 작업을 수행할 수 없다
- 여러 인터페이스를 구현하거나, 클래스를 확장하면서 동시에 인터페이스를 구현하도록 선언할 수 없다
- 클라이언트는 supertype에서 상속받은 멤버만 호출할 수 있다
- 표현식 중간에 나타나므로 짧게(약 10줄 이하) 유지해야 가독성이 유지된다

### 사용 사례

**과거 (Lambda 이전):**

- 작은 function object와 process object를 즉석에서 생성하는 선호 수단이었다
- 현재는 lambda가 선호된다 (Item 42)

**현재:**

- Static factory method 구현에 자주 사용된다 (Item 20의 `intArrayAsList` 참조)

## Local Class

Local class는 네 가지 nested class 중 가장 드물게 사용된다.

### 특징

- Local variable을 선언할 수 있는 거의 모든 곳에서 선언 가능하다
- 동일한 scoping 규칙을 따른다
- 다른 종류의 nested class들과 공통 속성을 가진다:
    - Member class처럼: 이름이 있고 반복적으로 사용 가능하다
    - Anonymous class처럼: nonstatic context에서 정의된 경우에만 enclosing instance를 가지며, static 멤버를 포함할 수 없다
    - Anonymous class처럼: 가독성을 해치지 않도록 짧게 유지해야 한다

## 요약 및 선택 가이드

| 상황                                                    | 선택                     |
|-------------------------------------------------------|------------------------|
| 단일 메서드 외부에서 접근 필요 또는 메서드 내부에 넣기에 너무 긴 경우              | Member class           |
| Member class의 각 인스턴스가 enclosing instance 참조 필요        | Nonstatic member class |
| Member class의 각 인스턴스가 enclosing instance 참조 불필요       | Static member class    |
| 메서드 내부에 속하고, 한 곳에서만 인스턴스 생성하며, 클래스를 특징짓는 기존 타입이 있는 경우 | Anonymous class        |
| 메서드 내부에 속하지만 위 조건에 해당하지 않는 경우                         | Local class            |