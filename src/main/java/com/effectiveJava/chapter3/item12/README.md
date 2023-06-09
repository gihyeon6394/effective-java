<h1>item 12. toString을 항상 재정의하라</h1>

> toString을 재정의한 클래스는 인스턴스를 보기 좋게 읽을 수 있다

<h3>toString의 일반 규약</h3>

> 간결하면서 사람이 읽기 쉬운 형태의 유익한 정보를 반환하라  
> **모든 하위클래스에서 이 메서드를 재정의하라!**

_단 상위클래스에서 적절히 재정의 했다면 안해도 된다_

Object의 toString이 적합한 문자열을 반환하는 경우는 거의 없음

```java
public class foo{
    public static void main (String[] args){
        PhoneNumber1 phoneNumber1 = new PhoneNumber1("SKT", "010-5678-1234");
        System.out.println(phoneNumber1.toString()); // com.effectiveJava.chapter3.item12.Item12$PhoneNumber1@68c4de73
        System.out.println(phoneNumber1); // com.effectiveJava.chapter3.item12.Item12$PhoneNumber1@68c4de73
    }
}
```

<h3>toString override의 좋은 점</h3>

- 디버깅이 쉬움 -> 클래스 사용성이 좋아짐
- 인스턴스는 개발자 의도와 다른 곳에서 쓰임
- 이 때 로깅이 의미없이 찍힐 수 있음

아래 메시지는 디버깅이 힘듬

~~~~
 System.out.println(phoneNumber1 +"에 연결할 수 없습니다.");
 // com.effectiveJava.chapter3.item12.Item12$PhoneNumber1@68c4de73에 연결할 수 없습니다.
~~~~

<h2>toStirng 재정의 하는 법</h2>

- 그 객체가 가진 주요 정보<sup>필드</sup> 를 모두 반환
- 반환값의 포맷을 문서화할지 말지 결정
- toString의 의도를 밝힌다

```java
public class PhoneNumber2{
    private final String telecom;
    private final String phoneNumber;
    // ...
    
    /**
     * 객체의 통신사 (telecom)와 전화번호 (phoneNumber)를 문자열로 제공한다
     * @return 예시
     * PhoneNumber2{telecom='SKT', phoneNumber='010-5678-1234'}
     */
    @Override
    public String toString() {
        return "PhoneNumber2{" +
                "telecom='" + telecom + '\'' +
                ", phoneNumber='" + phoneNumber + '\'' +
                '}';
    }
}
```


```java
public class foo{
    public static void main (String[] args){
        PhoneNumber2 phoneNumber2 = new PhoneNumber2("SKT", "010-5678-1234");
        System.out.println(phoneNumber2.toString());
        System.out.println(phoneNumber2);
        System.out.println(phoneNumber2 + "에 연결할 수 없습니다.");
    }
}
```

<h4>console</h4>

```console
PhoneNumber2{telecom='SKT', phoneNumber='010-5678-1234'}
PhoneNumber2{telecom='SKT', phoneNumber='010-5678-1234'}
PhoneNumber2{telecom='SKT', phoneNumber='010-5678-1234'}에 연결할 수 없습니다.
```
