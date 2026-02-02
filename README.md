![image](https://user-images.githubusercontent.com/53042858/229412295-350acc39-ee8d-4c2e-92c8-72943ff354ac.png)

<h2>contents</h2>

- 1장 들어가기
- [2장 객체 생성과 파괴](src/main/java/com/effectiveJava/chapter2/README.md)
- [3장 모든 객체의 공통 메서드](src/main/java/com/effectiveJava/chapter3/README.md)
- [4장 클래스와 인터페이스](src/main/java/com/effectiveJava/chapter4/README.md)
- [5장 제네릭](src/main/java/com/effectiveJava/chapter5/README.md)
- 6장 열거 타입과 애너테이션
- 7장 람다와 스트림
- 8장 메서드
- 9장 일반적인 프로그래밍 원칙
- 10장 예외
- 11장 동시성
- 12장 직렬화

## Chapter 2. Creating and Destroying Objects

이 장은 객체의 생성과 파괴에 관한 내용을 다룬다: 언제 그리고 어떻게 객체를 생성할지, 언제 그리고 어떻게 객체 생성을 피할지, 객체가 적시에 파괴되도록 보장하는 방법, 그리고 파괴 전에 수행해야 하는 정리 작업을
관리하는 방법을 설명한다.

- [Item 1: Consider static factory methods instead of constructors](item1/README.md)
- [Item 2: Consider a builder when faced with many constructor parameters](item2/README.md)
- [Item 3: Enforce the singleton property with a private constructor or an enum type](item3/README.md)
- [Item 4: Enforce noninstantiability with a private constructor](item4/README.md)
- [Item 5: Prefer dependency injection to hardwiring resources](item5/README.md)
- [Item 6: Avoid creating unnecessary objects](item6/README.md)
- [Item 7: Eliminate obsolete object references](item7/README.md)
- [Item 8: Avoid finalizers and cleaners](item8/README.md)
- [Item 9: Prefer try-with-resources to try-finally](item9/README.md)

## Chapter 3. Methods Common to All Objects

- [Item 10: Obey the general contract when overriding equals](item10/README.md)
- [Item 11: Always override hashCode when you override equals](item11/README.md)
- [Item 12: Always override toString](item12/README.md)
- [Item 13: Override clone judiciously](item13/README.md)
- [Item 14: Consider implementing Comparable](item14/README.md)

## Chapter 4. Classes and Interfaces

- [Item 15: Minimize the accessibility of classes and members](item15/README.md)
- [Item 16: In public classes, use accessor methods, not public fields](item16/README.md)
- [Item 17: Minimize mutability](item17/README.md)
- [Item 18: Favor composition over inheritance](item18/README.md)
- [Item 19: Design and document for inheritance or else prohibit it](item19/README.md)
- [Item 20: Prefer interfaces to abstract classes](item20/README.md)
- [Item 21: Design interfaces for posterity](item21/README.md)
- [Item 22: Use interfaces only to define types](item22/README.md)
- [Item 23: Prefer class hierarchies to tagged classes](item23/README.md)
- [Item 24: Favor static member classes over nonstatic](item24/README.md)
- [Item 25: Limit source files to a single top-level class](item25/README.md)

## Chapter 5. Generics

- [Item 26: Don’t use raw types](item26/README.md)
- [Item 27: Eliminate unchecked warnings](item27/README.md)
- [Item 28: Prefer lists to arrays](item28/README.md)
- [Item 29: Favor generic types](item29/README.md)
- [Item 30: Favor generic methods](item30/README.md)
- [Item 31: Use bounded wildcards to increase API flexibility](item31/README.md)
- [Item 32: Combine generics and varargs judiciously](item32/README.md)
- [Item 33: Consider typesafe heterogeneous containers](item33/README.md)

## Chapter 6. Enums and Annotations

- [Item 34: Use enums instead of int constants](item34/README.md)
- [Item 35: Use instance fields instead of ordinals](item35/README.md)
- [Item 36: Use EnumSet instead of bit fields](item36/README.md)
- [Item 37: Use EnumMap instead of ordinal indexing](item37/README.md)
- [Item 38: Emulate extensible enums with interfaces](item38/README.md)
- [Item 39: Prefer annotations to naming patterns](item39/README.md)
- [Item 40: Consistently use the Override annotation](item40/README.md)
- [Item 41: Use marker interfaces to define types](item41/README.md)

## Chapter 7. Lambdas and Streams

- [Item 42: Prefer lambdas to anonymous classes](item42/README.md)
- [Item 43: Prefer method references to lambdas](item43/README.md)
- [Item 44: Favor the use of standard functional interfaces](item44/README.md)
- [Item 45: Use streams judiciously](item45/README.md)
- [Item 46: Prefer side-effect-free functions in streams](item46/README.md)
- [Item 47: Prefer Collection to Stream as a return type](item47/README.md)
- [Item 48: Use caution when making streams parallel](item48/README.md)

## Chapter 8. Methods

- [Item 49: Check parameters for validity](item49/README.md)
- [Item 50: Make defensive copies when needed](item50/README.md)
- [Item 51: Design method signatures carefully](item51/README.md)
- [Item 52: Use overloading judiciously](item52/README.md)
- [Item 53: Use varargs judiciously](item53/README.md)
- [Item 54: Return empty collections or arrays, not nulls](item54/README.md)
- [Item 55: Return optionals judiciously](item55/README.md)
- [Item 56: Write doc comments for all exposed API elements](item56/README.md)

## Chapter 9. General Programming

- [Item 57: Minimize the scope of local variables](item57/README.md)
- [Item 58: Prefer for-each loops to traditional for loops](item58/README.md)
- [Item 59: Know and use the libraries](item59/README.md)
- [Item 60: Avoid float and double if exact answers are required](item60/README.md)
- [Item 61: Prefer primitive types to boxed primitives](item61/README.md)
- [Item 62: Avoid strings where other types are more appropriate](item62/README.md)
- [Item 63: Beware the performance of string concatenation](item63/README.md)
- [Item 64: Refer to objects by their interfaces](item64/README.md)
- [Item 65: Prefer interfaces to reflection](item65/README.md)
- [Item 66: Use native methods judiciously](item66/README.md)
- [Item 67: Optimize judiciously](item67/README.md)
- [Item 68: Adhere to generally accepted naming conventions](item68/README.md)

## Chapter 10. Exceptions

- [Item 69: Use exceptions only for exceptional conditions](item69/README.md)
- [Item 70: Use checked exceptions for recoverable conditions and runtime exceptions for programming errors](item70/README.md)
- [Item 71: Avoid unnecessary use of checked exceptions](item71/README.md)
- [Item 72: Favor the use of standard exceptions](item72/README.md)
- [Item 73: Throw exceptions appropriate to the abstraction](item73/README.md)
- [Item 74: Document all exceptions thrown by each method](item74/README.md)
- [Item 75: Include failure-capture information in detail messages](item75/README.md)
- [Item 76: Strive for failure atomicity](item76/README.md)
- [Item 77: Don’t ignore exceptions](item77/README.md)

## Chapter 11. Concurrency

- [Item 78: Synchronize access to shared mutable data](item78/README.md)
- [Item 79: Avoid excessive synchronization](item79/README.md)
- [Item 80: Prefer executors, tasks, and streams to threads](item80/README.md)
- [Item 81: Prefer concurrency utilities to wait and notify](item81/README.md)
- [Item 82: Document thread safety](item82/README.md)
- [Item 83: Use lazy initialization judiciously](item83/README.md)
- [Item 84: Don’t depend on the thread scheduler](item84/README.md)

## Chapter 12. Serialization

- [Item 85: Prefer alternatives to Java serialization](item85/README.md)
- [Item 86: Implement Serializable with great caution](item86/README.md)
- [Item 87: Consider using a custom serialized form](item87/README.md)
- [Item 88: Write readObject methods defensively](item88/README.md)
- [Item 89: For instance control, prefer enum types to readResolve](item89/README.md)
- [Item 90: Consider serialization proxies instead of serialized instances](item90/README.md)
