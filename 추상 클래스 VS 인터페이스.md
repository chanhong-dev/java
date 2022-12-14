## 추상 클래스 (abstract class)

### 미완성 설계도

- `abstract` 키워드를 붙이면 됨.
- 추상 클래스는 추상 메소드를 0개 이상 가지고 있다는 것을 제외하고 일반 클래스와 다르지 않음.
- 추상 메소드는 선언부는 있는데 구현부가 없는 메소드
자식 클래스에서 반드시 **오버라이딩**해야만 사용할 수 있는 메소드
- 추상 메소드를 포함하는 클래스는 반드시 초상 클래스여야 함.
- **다중 상속이 불가능**
- 상속하는 집한간에는 연관관계가 존재
- 상속을 위한 클래스이기 때문에 객체(인스턴스)를 생성할 수 없음.

```java
abstract class 클래스이름 {
    ...
    public abstract void 메서드이름();
}
```

- 추상 클래스를 상속받는 모든 클래스에서는 이 추상 메소드를 반드시 재정의 해야함.
→ 자식클래스에서 추상클래스의 모든 추상 메소드를 오버라이딩 하고 나서야 비로소 자식클래스의 인스턴스를 생성할 수 있음.
- 생성자와 필드, 일반 메소드 포함 가능

```java
abstract class Animal { abstract void cry(); }

class Cat extends Animal { void cry() { System.out.println("냐옹냐옹!"); } }
class Dog extends Animal { void cry() { System.out.println("멍멍!"); } }

 
public class Polymorphism02 {
    public static void main(String[] args) {
        // Animal a = new Animal(); // 추상 클래스는 인스턴스를 생성할 수 없음.

        Cat c = new Cat();
        Dog d = new Dog();

        c.cry();
        d.cry();
    }
}
```

### 추상 메소드의 목적

- 추상 메소드가 포함된 클래스를 상속받는 자식 클래스가 반드시 추상 메소드를 구현하도록 하기 위함.

---

## 인터페이스 (Interface)

### 기본 설계도

- 자바에서 클래스들이 구현해야하는 동작을 지정하는 용도로 사용되는 추상 자료형
- `interface` 키워드를 이용하여 선언
- **다중 상속**이 가능
- 상속하는 집한간에는 연관관계가 존재하지 않을 수 있음.
- 모든 멤버 변수는 `public static final` 이어야 하며, 이를 생략할 수 있음.
- 모든 메소드는 `public abstract` 이어야 하며, 이를 생략할 수 있음.
- 생략 시 컴파일러가 자동 추가

```java
// 접근 지정자는 public 만 가능 , class 설계도 이기 때문에 존재 목적이 "공개"
public interface 인터페이스이름 {
    public static final 타입 상수이름 = 값;
    ...
    public abstract 메소드이름(매개변수목록);
    ...
}
```

- 인터페이스는 오로지 추상 메소드와 상수만을 포함할 수 있음.
- 직접 인스턴스를 생성할 수 없음.
- 객체를 생성할 수 없기 때문에, 생성자를 가질 수 없음.
- Java7 까지는 추상 메소드로만 선언이 가능 
→ Java8 부터는 `디폴트 메소드`와 `정적 메소드` 선언 가능

> interface 도 Class, Enum, Annotation 처럼 `~.java` 파일로 작성되고, 
`~.class` 파일로 컴파일 된다.
> 

### 인터페이스의 구현

```java
class 클래스이름 extend 상위클래스이름 implements 인터페이스이름 { ... }
```

1. 단일 인터페이스 구현 클래스 
2. 다중 인터페이스 구현 클래스
3. 익명 구현 객체

### 인터페이스의 역할

- 객체를 어떻게 구성해야 하는지 정리한 설계도
- 객체의 교환성(또는 다형성)을 높여줌.

### 인터페이스의 장점

- 대규모 프로젝트 개발 시 일관되고 정형화된 개발을 위한 표준화 가능
- 클래스의 작성과 인터페이스의 구현을 동시에 진행할 수 있으므로, 개발시간 단축
- 클래스와 클래스 간의 관계를 인터페이스로 연결하며, 클래스마다 독립적인 프로그래밍 가능

---

## 추상 클래스와 인터페이스의 차이점

### 공통점

1. 메소드의 선언만 있고, 구현 내용이 없음
2. 객체 ( 인스턴스 ) 생성 불가
3. 상속받은 자식 클래스에서 반드시 추상 메소드를 구현하도록 강제 됨.

### 차이점

1. **접근자**
    1. 인터페이스에서 모든 변수는 `public static final`, 모든 메소드는 `public abstract` 임.
    2. 추상 클래스에서는 `static`이나 `final`이 아닌 필드를 가질 수 있고, `public`, `protected`, `private` 모두 가질 수 있음.
2. **다중 상속 여부**
    1. 인터페이스를 구현하는 클래스는 다른 여러개 인터페이스를 함께 구현할 수 있음.
    2. 자바에서는 다중 상속을 지원하지 않기 때문에 여러 추상 클래스를 상속할 수 없음.
3. **사용 의도**
    1. 추상 클래스는 객체들의 공통점을 찾아 추상화 시켜 놓은 것.
    부모 클래스가 가진 기능들을 확장 구현해야할 경우 사용
    2. 인터페이스는 클래스와 별도로 구현 객체가 같은 동작을 한다는 것을 보장하기 위해 사용
    공통된 기능
4. **적정한 사용 케이스**
    1. 추상 클래스
        1. 관련성 높은 클래스 간에 코드를 공유하고 싶은 경우
        2. 추상 클래스를 상속 받을 클래스들이 공통으로 가지는 메소드와 필드가 많은 경우
        3. `non-static`, `non-final` 필드 선언이 필요한 경우
    2. 인터페이스
        1. 서로 관련성 없는 클래스들이 인터페이스를 구현하게 되는 경우
        2. 다중 상속을 허용하고 싶은 경우
        3. 특정 데이터 타입의 행동을 명시하고 싶은데, 어디서 그 행동이 구현되는지는 신경쓰지 않는 경우

![image](https://user-images.githubusercontent.com/90807343/185111209-cfc361fd-4dd6-4262-bbb8-bfcabfebefa3.png)
