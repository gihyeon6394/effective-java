<h1>item 3. private 생성자나 열거 타입으로 싱글턴임을 보증하라</h1>

> 싱글턴을 만드는 가장 좋은 방법은 대부분 enum

<h4>싱글턴? <sup>singletone</sup></h4>
인스턴스를 오직 하나만 생성할 수잇는 클래스

<h2>싱글턴을 만드는 방법</h2>

- private 생성자
    - public static final 필드
    - 정적 팩터리 메서드
- **열거 타입**<sup>best!</sup>

<h3>public static final 필드</h3>

- private 생성자는 한번만 호출 됨 <sup>public static final 필드 초기화 시</sup>

```java
public class Hani {

    public static final Hani INASTANCE; 

    // 최초 한번 인스턴스 생성
    static {
        try {
            INASTANCE = new Hani();
        } catch (Exception e) {
            throw new RuntimeException(e);
        }
    }

    private Hani() throws Exception {

        if (INASTANCE != null) {
            throw new Exception("INSTANCE Already!");
        }
    }
}
```

<h3>정적 팩터리 메서드</h3>

- 장점
    - 해당 클래스가 싱글턴임이 API에 명백히 드러남
    - 간결함
    - 정적 팩터리 메서드를 수정해서 싱글턴이 아니게 변경할 수 있음
    - 제네릭 싱글턴 팩터리를 만들 수 있음
    - 정적 팩터리의 메서드 참조를 공급자로 사용할 수 있음

```java
public class Minzi {
    private static final Minzi INASTANCE = new Minzi(); 


    private Minzi() {
    }

    public static Minzi getInstance() {
        return INASTANCE;
    }
}

```

<h3>열거<sup>enum</sup> 타입</h3>

- 추가 노력 없이 직렬화 가능
- 리플렉션 공격에도 안전
- **대부분의 경우 싱글턴을 구현하는 가장 좋은 방법**
- 부자연스러움

```java
public enum Karina {
    INSTANCE;
}
```

