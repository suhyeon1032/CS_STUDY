## 2.프로그래밍 패러다임
>프로그래밍 패터다임은 프로그래머에게 프로그래밍의 관점을 갖게 해주는 역할을 하는 개발 방법론이다.
프로그래밍 패러다임은 크게 선언형, 명령형으로 나누며, 선언형은 함수형이라는 하위 집합을 갖는다. 또한, 명령형은 다시 객체지향, 절차지향으로 나눈다.
<img src="https://thebook.io/img/080326/057.jpg" width="400">

### 2.1 선언형과 함수형 프로그래밍
<span style="color:tomato">선언형 프로그래밍</span>이란 무엇을 풀어내는가에 집중하는 패러다임이며, **프로그램은 함수로 이루어진 것이다.** 라는 명제가 담겨 있는 패러다임이기도 하다.
<span style="color:tomato">함수형 프로그래밍</span>은 선언형 패러다임의 일종이다. 
#### 순수 함수
출력이 입력에만 의존하는 것을 의미한다.
#### 고차 함수
고차 함수란 함수가 함수를 값처럼 매개변수로 받아 로직을 생성할 수 있는 것을 말한다.

<span style="color:gray">일급 객체</span>
고차 함수를 쓰기 위해서는 해당 언어가 일급 객체라는 특징을 가져야 하며 그 특징은 다음과 같다.

- 변수나 메서드에 함수를 할당할 수 있다.
- 함수 안에 함수를 매개변수로 담을 수 있다.
- 함수가 함수를 반환할 수 있다.

### 2.2 객체지향 프로그래밍
>객체지향 프로그래밍은 객체들의 집합으로 프로그램의 상호 작용을 표현하며 데이터를 객체로 취급하여 객체 내부에 선언된 메서드를 활용하는 방식을 말한다. 
설계에 많은 시간이 소요되며 처리 속도가 다른 프로그래밍 패러다임에 비해 상대적으로 느리다.

#### 객체지향 프로그래밍의 특징
객체지향 프로그래밍은 추상화, 캡슐화, 상속성, 다형성이라는 특징이 있다.

<span style="color:gray">추상화</span>
abstraction : 복잡한 시스템으로부터 핵심적인 개념 또는 기능을 간추려내는 것을 의미한다.
ex) 군인, 장교, 키180, 여친있음, 안경씀, 축구못함, 롤마스터티어 등의 특징을 가진 사람에게서 코드로 나타낼 때 일부분의 특징인 군인, 장교만 뽑아내거나 더 간추려 나타내는 것을 말한다.
<span style="color:gray">캡슐화</span>
encapsulation : 객체의 속성과 메서드를 하나로 묶고 일부를 외부에 감추어 은닉하는 것을 말한다.
<span style="color:gray">상속성</span>
inheritance : 상위 클래스의 특성을 하위 클래스가 이어받아서 재사용하거나 추가, 확장하는 것을 말한다. 
코드의 재사용 측면, 계층적인 관계 생성, 유지 보수성 측면에서 중요하다.
<span style="color:gray">다형성</span>
polymorphism : 하나의 메서드나 클래스가 다양한 방법으로 동작하는 것을 말한다. 
대표적으로 오버로딩, 오버라이딩이 있다.

<span style="color:tomato">오버로딩</span>
같은 이름을 가진 메서드를 여러 개 두는 것을 말한다. 
메서드의 타입, 매개변수의 유형, 개수 등으로 여러 개를 둘 수 있으며 컴파일 중에 발생하는 정적 다형성이다. 
```java
class Person {

    public void eat(String a) {
        System.out.println("I eat " + a);
    }

    public void eat(String a, String b) {
        System.out.println("I eat " + a + " and " + b);
    }
}

public class CalculateArea {

    public static void main(String[] args) {
        Person a = new Person();
        a.eat("apple");
        a.eat("tomato", "phodo");
    }
}
/*
I eat apple
I eat tomato and phodo
*/
```
매개변수의 개수에 따라 다른 함수가 호출되는 것을 알 수 있다.

<span style="color:tomato">오버라이딩</span>
주로 메서드 오버라이딩을 말하며 상위 클래스로부터 상속받은 메서드를 하위 클래스가 재정의하는 것을 의미한다.
```java
class Animal {
    public void bark() {
        System.out.println("mumu! mumu!");
    }
}

class Dog extends Animal {
    @Override
    public void bark() {
        System.out.println("wal!!! wal!!!");
    }
}

public class Main {
    public static void main(String[] args) {
        Dog d = new Dog();
        d.bark();
    }
}
```
부모 클래스는 mumu! mumu!로 짖게 만들었지만 자식 클래스에서 wal!!! wal!!!로 짖게 만들었더니 자식 클래스 기반으로 메서드가 재정의됨을 알 수 있다.

#### 설계 원칙
객체지향 프로그래밍을 설계할 때는 SOLID 원칙을 지켜주어야 한다.
S : 단일 책임 원칙
O : 개방-폐쇄 원칙
L : 리스코프 치환 원칙
I : 인터페이스 분리 원칙
D : 의존역전 원칙

<span style="color:gray">단일 책임 원칙</span>
SRP : Single Responsibility Principle 
모든 클래스는 각각 하나의 책임만 가져야 하는 원칙이다. 
ex) A라는 로직이 존재한다면 어떠한 클래스는 A에 관한 클래스여야 하고 이를 수정한다고 했을 때도 A와 관련된 수정이어야 한다.

<span style="color:gray">개방-폐쇄 원칙</span>
OCP : Open Closed Principle
유지 보수 사항이 생긴다면 코드를 쉽게 확장할 수 있도록 하고 수정할 때는 닫혀 있어야 하는 원칙이다.
즉, 기존의 코드는 잘 변경하지 않으면서도 확장은 쉽게 할 수 있어야 한다.

<span style="color:gray">리스코프 치환 원칙</span>
LSP : Liskov Substitution Principle
프로그램의 객체는 프로그램의 정확성을 깨뜨리지 않으면서 하위 타입의 인스턴스로 바꿀 수 있어야 하는 것을 의미한다.
클래스는 상속이 되기 마련이고 부모, 자식이라는 계층 관계가 만들어진다. 이때 부모 객체에 자식 객체를 넣어도 시스템이 문제없이 돌아가게 만드는 것을 말한다.

<span style="color:gray">인터페이스 분리 원칙</span>
ISP : Interface Segregation Principle
하나의 일반적인 인터페이스보다 구체적인 여러 개의 인터페이스를 만들어야 하는 원칙을 말한다.

<span style="color:gray">의존 역전 원칙</span>
DIP : Dependency Inversion Principle
자신보다 변하기 쉬운 것에 의존하던 것을 추상화된 인터페이스나 상위 클래스를 두어 변하기 쉬운 것의 변화에 영향박지 않게 하는 원칙을 말한다. 
상위 계층은 하위 계층의 변화에 대한 구현으로부터 독립해야 한다. 

### 2.3 절차형 프로그래밍
>- 로직이 수행되어야 할 연속적인 계산 과정으로 이루어져 있다.
- 일이 진행되는 방식으로 그저 코드를 구현하기만 하면 되기 때문에 코드의 가독성이 좋으며 실행 속도가 빠르다.
- 계산이 많은 작업 등에 쓰인다.
- 대표적으로 포트란을 이용한 개디 과학 관련 연산 작업, 머신 러닝의 배치 작업이 있다.
- 모듈화가 어렵고 유지 보수성이 떨어진다는 단점이 있다.

