## 1. 디자인 패턴
> 디자인 패턴이랑 프로그램을 설계할 때 발생했던 문제점들을 객체 간의 상호 관계 등을 이용하여 해결할 수 있도록 하나의 "규약" 형태로 만들어 놓은 것은 의미한다.

### 1.1 싱글톤 패턴
> 싱글톤 패턴(singleton pattern)은 하나의 클래스에 오직 하나의 인스턴스만 가지는 패턴이다. 
하나의 클래스를 기반으로 여러 개의 개별적인 인스턴스를 만들 수 있지만 하나의 클래스를 기반으로 단 하나의 인스턴스를 만들어 이를 기반으로 로직을 만드는데 쓰이며 보통 데이터베이스 연결 모듈에 많이 사용한다.

#### 자바스크립트의 싱글톤 패턴
자바스크립트에서는 리터럴 {} 또는 new Object로 객체를 생성하게 되면 다른 어떤 객체와도 같지 않기 때문에 싱글톤 패턴을 구현할 수 있다.

<small>자바스크립트에서 싱글톤 패턴 만드는 방식</small>

  
```js
const obj = {
    a: 27
}
const obj2 = {
    a: 27
}
console.log(obj === obj2)
// false
```
<small><I>obj와 obj2는 다른 인스턴스를 가진다</I></small>

```js
class Singleton {
    constructor() {
        if (!Singleton.instance) {
            Singleton.instance = this
        }
        return Singleton.instance
    }
    getInstance() {
        return this.instance
    }
}
const a = new Singleton()
const b = new Singleton() 
console.log(a === b) // true
```
위의 코드는 Singleton.instance라는 하나의 인스턴스를 가지는 Singleton 클래스를 구현한 모습인다. a와 b는 하나의 인스턴스를 가진다.

#### 데이터베이스 연결 모듈
```js
const URL = 'mongodb://localhost:27017/kundolapp' 
const createConnection = url => ({"url" : url})    
class DB {
    constructor(url) {
        if (!DB.instance) { 
            DB.instance = createConnection(url)
        }
        return DB.instance
    }
    connect() {
        return this.instance
    }
}
const a = new DB(URL)
const b = new DB(URL) 
console.log(a === b) // true
```
DB.instance라는 하나으 인스턴스를 기반으로 a,b를 생성한다. 
이를 통해 데이터베이스 연결에 관한 인스턴스 생성 비용을 아낄 수 있다.
#### 자바에서의 싱글톤 패턴
```java
class Singleton {
    private static class singleInstanceHolder {
        private static final Singleton INSTANCE = new Singleton();
    }
    public static Singleton getInstance() {
        return singleInstanceHolder.INSTANCE;
    }
}

public class HelloWorld{ 
     public static void main(String []args){ 
        Singleton a = Singleton.getInstance(); 
        Singleton b = Singleton.getInstance(); 
        System.out.println(a.hashCode());
        System.out.println(b.hashCode());  
        if (a == b){
         System.out.println(true); 
        } 
     }
}
/*
705927765
705927765
true
*/
```
#### mongoose의 싱글톤 패턴
싱글톤 패턴은 Node.js에서 MongoDB 데이터베이스를 연결할 때 쓰는 mongose모듈에서 사용한다.
mongoose의 데이터베이스를 연결할 때 쓰는 connect()라는 함수는 싱글톤 인스턴스를 반환한다. 
#### MySQL의 싱글톤 패턴
Node.js에서 MySQL 데이터베이스를 연결할 때도 싱글톤 패턴이 사용된다.
메인 모듈에서 데이터베이스 연결에 관한 인스턴스를 정의하고 다른 모듈인 A 또는 B에서 해당 인스턴스를 기반으로 쿼리를 보내는 형식으로 쓰인다.

#### 싱글톤 패턴의 단점
싱글톤 패턴은 TDD(Test Driven Development)를 할 때 걸림돌이 된다. 
TDD를 할 때 단위 테스트를 주로 하는데, 단위 테스르는 테스트가 서로 독립적이어야 하며 테스트를 어떤 순서로든 실행할 수 있어야 한다.
하지만 싱글톤 패턴은 미리 생성된 하나의 인스턴스를 기반으로 구현하는 패턴이므로 각 테스트마다 '독립적인' 인스턴스를 만들기가 어렵다.

#### 의존성 주입
싱글톤 패턴은 사용하기 쉽고 광장히 실용적이지만 모듈 간의 결합을 강하게 만들 수 있다는 단점이 있다. 
이때 의존성 주입 (DI, Depedency Injection)을 통해 모듈간의 결합을 조금 더 느슨하게 만들어 해결할 수 있다.
<small>참고로 의존성이란 종속성이라고도 하며 A가 B에 의존성이 있다는 것은 B의 변경 사항에 대해 A 또한 변해야 된다는 것을 의미한다.</small>
![](https://velog.velcdn.com/images/daram9/post/49b5f018-e535-4897-8dcc-22a007e057ce/image.png)
위 그림처럼 메인 모듈(main mudule)이 직접 다른 하위 모듈에 대한 의존성을 주기보다는 중간에 의존성 주입자(dependency injector)가 이 부분을 가로채 메인 모듈이 간접적으로 의존성을 주입하는 방식이다.
이를 통해 메인 모듈(상위 모듈)은 하위 모듈에 대한 의존성이 떨어지게 된다. (디커플링이 된다라고 한다.)
##### 의존성 주입의 장점
모듈들을 쉽게 교체할 수 있는 구조가 되어 테스팅하기 쉽고 마이그레이션하기도 수월하다. 
구현할 때 추상화 레이어를 넣고 이를 기반으로 구현체를 넣어 주기 때문에 애플리케이션 의존성 방량이 일관되고, 애플리케이션을 쉽게 추론할 수 있으며, 모듈 간의 관계들이 조금 더 명확해진다.
##### 의존성 주입의 단점
모듈들이 더욱더 분리되므로 클래스 수가 늘어나 복잡성이 증가될 수 있으며 약간의 런타임 패널티가 생기기도 한다.
##### 의존성 주입 원칙
의존성 주입은 "상위 모듈은 하위 모듈에서 어떠한 것도 가져오지 않아야 한다. 또한 둘 다 추상화에 의존해야 하며, 이 때 추상화는 세부사항에 의존하지 말아야 한다." 라는 의존성 주입 원칙을 지켜주면서 만들어야 한다.

### 1.2 팩토리 패턴
>팩토리 패턴(Factory pattern)은 객체를 사용하는 코드에서 객체 생성 부분을 떼어내 추상화한 패턴이자 상속 관계에 있는 두 클래스에서 상위 클래스가 중요한 뼈대를 결정하고, 하위 클래스에서 객체 생성에 관한 구체적인 내용을 결정하는 패턴이다.

상위 클래스와 하위 클래스가 분리되기 때문에 느슨한 결합을 가지며 상위 클래스에서는 인스턴스 생성 방식에 대해 전혀 알 필요가 없기 때문에 더 많은 유연성을 갖게 된다.
객체 생성 로직이 따로 떼어져 있기 때문에 코드를 리팩터링하더라도 한 곳만 고칠 수 있게 되니 유지 보수성이 증가된다.
<img src="https://velog.velcdn.com/images/daram9/post/004c93d8-9997-462f-bd59-86d1b631b464/image.png" width="400">
예를 들어 라떼 레시피와 아메리타노 레시피, 우유 레시피라는 구체적인 내용이 들어 있는 하위 클래스가 컨베이어 벨트를 통해 전달되고, 상위 클래스인 바리스타 공장에서 이 레시피들을 토대로 우유 등을 생산하는 생산 공정을 생각하면 된다.
#### 자바스크립트의 팩토리 패턴
간단하게 new Object()로 패토리 패턴을 구현할 수 있다.
```js
const num = new Object(42)
const str = new Object('abc')
num.constructor.name;
str.constructor.name;
```
숫자나 문자열을 전달함에 따라 다른 타입의 객체를 생성한다. 
즉, 전달받은 값에 따라 다른 객체를 생성하며 인스턴스의 타입 등을 정한다.
<small>커피 팩토리를 기반으로 라떼 등을 생산하는 코드</small>
```js
class Latte {
    constructor() {
        this.name = "latte"
    }
}
class Espresso {
    constructor() {
        this.name = "Espresso"
    }
} 

class LatteFactory {
    static createCoffee() {
        return new Latte()
    }
}
class EspressoFactory {
    static createCoffee() {
        return new Espresso()
    }
}
const factoryList = { LatteFactory, EspressoFactory } 
 
class CoffeeFactory {
    static createCoffee(type) {
        const factory = factoryList[type]
        return factory.createCoffee()
    }
}   
const main = () => {
    // 라떼 커피를 주문한다.  
    const coffee = CoffeeFactory.createCoffee("LatteFactory")  
    // 커피 이름을 부른다.  
    console.log(coffee.name) // latte
}
main()

```
CoffeeFactory라는 상위 클래스가 중요한 뼈대를 결정하고 하위 클래스인 LatteFactory가 구체적인 내용을 결정하고 있다. 
참고로 이는 의존성 주입이라고도 볼 수 있다. 
또한, CoffeeFactory를 보면 static으로 createCoffee() 정적 메서드를 정의한 것을 알수 있는데, 정적 메서드를 쓰면 클래스의 인스턴스 없이 호출이 가능하여 메모리를 절약할 수 있고 개별 인스턴스에 묶이지 않으며 클래스 내의 함수를 정의할 수 있는 장점이 있다. 
#### 자바의 팩토리 패턴
```java
abstract class Coffee { 
    public abstract int getPrice(); 
    
    @Override
    public String toString(){
        return "Hi this coffee is "+ this.getPrice();
    }
}

class CoffeeFactory { 
    public static Coffee getCoffee(String type, int price){
        if("Latte".equalsIgnoreCase(type)) return new Latte(price);
        else if("Americano".equalsIgnoreCase(type)) return new Americano(price);
        else{
            return new DefaultCoffee();
        } 
    }
}
class DefaultCoffee extends Coffee {
    private int price;

    public DefaultCoffee() {
        this.price = -1;
    }

    @Override
    public int getPrice() {
        return this.price;
    }
}
class Latte extends Coffee { 
    private int price; 
    
    public Latte(int price){
        this.price=price; 
    }
    @Override
    public int getPrice() {
        return this.price;
    } 
}
class Americano extends Coffee { 
    private int price; 
    
    public Americano(int price){
        this.price=price; 
    }
    @Override
    public int getPrice() {
        return this.price;
    } 
} 
public class HelloWorld{ 
     public static void main(String []args){ 
        Coffee latte = CoffeeFactory.getCoffee("Latte", 4000);
        Coffee ame = CoffeeFactory.getCoffee("Americano",3000); 
        System.out.println("Factory latte ::"+latte);
        System.out.println("Factory ame ::"+ame); 
     }
} 
```
if ("Latte".equalsIgnoreCase(type))을 통해 문자열 비교 기반으로 로직이 구성된다.
이는 Enum 또는 Map을 이용하여 if문을 쓰지 않고 매핑해서 할 수도 있다.
<small><span style="color:gray">❕Enum
상수의 집합을 정의할 때 사용되는 타입이다. 
상수나 메서드 등을 집어넣어서 관리하며 코드를 리팩터링할 때 해당 집합에 관한 로직 수정 시 이 부분만 수정하면 되므로 코드 리팩터링 시 강점이 생긴다. </span></small>
### 1.3 전략패턴
>전략 패턴(strategy)은 정책 패턴(policy pattern)이라고도 하며, 객체의 행위를 바꾸고 싶은 경우 직접 수정하지 않고 전략이라고 부르는 캡슐화한 알고리즘을 컨텍스트안에서 바꿔주면서 상호 교체가 가능하게 만드는 패턴이다.

<img src="https://velog.velcdn.com/images/daram9/post/15007594-5d6d-4ad1-bb72-73b94de5f36f/image.png" width="300">
물건을 살 때 네이버페이, 카카오페이 등 다양한 방법으로 결제하듯 어떤 아이템을 살 때 LUNACard로 사는 것과 KAKAOCard로 사는 것을 구현한 예제이다. 결제 방식의 전략만 바꿔서 두 가지 방식으로 결제하는 것을 구현했다. 
#### 자바의 전략 패턴

```java
import java.text.DecimalFormat;
import java.util.ArrayList;
import java.util.List;
interface PaymentStrategy { 
    public void pay(int amount);
} 

class KAKAOCardStrategy implements PaymentStrategy {
    private String name;
    private String cardNumber;
    private String cvv;
    private String dateOfExpiry;
    
    public KAKAOCardStrategy(String nm, String ccNum, String cvv, String expiryDate){
        this.name=nm;
        this.cardNumber=ccNum;
        this.cvv=cvv;
        this.dateOfExpiry=expiryDate;
    }

    @Override
    public void pay(int amount) {
        System.out.println(amount +" paid using KAKAOCard.");
    }
} 

class LUNACardStrategy implements PaymentStrategy {
    private String emailId;
    private String password;
    
    public LUNACardStrategy(String email, String pwd){
        this.emailId=email;
        this.password=pwd;
    }
    
    @Override
    public void pay(int amount) {
        System.out.println(amount + " paid using LUNACard.");
    }
} 

class Item { 
    private String name;
    private int price; 
    public Item(String name, int cost){
        this.name=name;
        this.price=cost;
    }

    public String getName() {
        return name;
    }

    public int getPrice() {
        return price;
    }
} 

class ShoppingCart { 
    List<Item> items;
    
    public ShoppingCart(){
        this.items=new ArrayList<Item>();
    }
    
    public void addItem(Item item){
        this.items.add(item);
    }
    
    public void removeItem(Item item){
        this.items.remove(item);
    }
    
    public int calculateTotal(){
        int sum = 0;
        for(Item item : items){
            sum += item.getPrice();
        }
        return sum;
    }
    
    public void pay(PaymentStrategy paymentMethod){
        int amount = calculateTotal();
        paymentMethod.pay(amount);
    }
}  

public class HelloWorld{
    public static void main(String []args){
        ShoppingCart cart = new ShoppingCart();
        
        Item A = new Item("kundolA",100);
        Item B = new Item("kundolB",300);
        
        cart.addItem(A);
        cart.addItem(B);
        
        // pay by LUNACard
        cart.pay(new LUNACardStrategy("kundol@example.com", "pukubababo"));
        // pay by KAKAOBank
        cart.pay(new KAKAOCardStrategy("Ju hongchul", "123456789", "123", "12/01"));
    }
}
```
<small><span style="color:gray">❕컨택스트
프로그래밍에서의 컨텍스트는 상황, 맥락, 문맥을 의미하며 개발자가 어떠한 작업을 완료하는 데 필요한 모든 관련 정보를 말한다.</span></small>
#### passport의 전략 패턴
passport는 전략 패턴을 활용한 라이브러리로 Node.js에서 인증 모듈을 구현할 때 쓰는 미들웨어 라이브러리다.
여러가지 전략을 기반으로 인증할 수 있게한다. 서비스 내의 회원가입된 아이디와 비밀번호를 기반으로 인증하는 LocalStrategy 전략과 페이스북, 네이버 등 다른 서비스를 기반으로 인증하는 OAuth 전략 등을 지원한다. 
```js
var passport = require('passport')
    , LocalStrategy = require('passport-local').Strategy;

passport.use(new LocalStrategy(
    function(username, password, done) {
        User.findOne({ username: username }, function (err, user) {
          if (err) { return done(err); }
            if (!user) {
                return done(null, false, { message: 'Incorrect username.' });
            }
            if (!user.validPassword(password)) {
                return done(null, false, { message: 'Incorrect password.' });
            }
            return done(null, user);
        });
    }
));
```
passport.use()라는 메서드에 전략을 매개변수로 넣어서 로직을 수행한다.
### 1.4 옵저버 패턴
>옵저버 패턴(observer pattern)은 주체가 어떤 객체(subject)의 상태 변화를 관찰하다가 상태 변화가 있을 때마다 메서드 등을 통해 옵저버 목록에 있는 옵저버들에게 변화를 알려주는 디자인 패턴이다.

<img src="https://velog.velcdn.com/images/daram9/post/95365260-849d-4d22-b49b-2a7bdecba708/image.png" width="300">
주체란 객체의 상태 변화를 보고 있는 관찰자이고, 옵저버들이란 이 객체의 상태 변화에 따라 전달되는 메서드 등을 기반으로 추가 변화 사항이 생기는 객체들을 의미한다.

또한 주체와 객체를 따로 두지 않고 상태가 변경되는 객체를 기반으로 구축하기도 한다.

<img src="https://velog.velcdn.com/images/daram9/post/f4dfcbeb-4734-4271-9f0f-f305d1b26f0b/image.png
" width="300">
옵저버 패턴을 활용한 서비스로는 트위터가 있다.

옵저버 패턴은 주로 이벤트 기반 시스템에 사용하며 MVC 패턴에도 사용된다.
### 1.5 프록시 패턴과 프록시 서버
#### 프록시 패턴
>프록시 패턴은 대상 객체에 접근하기 전 그 접근에 대한 흐름을 가로채 대상 객체 앞단의 인터페이스 역학을 하는 디자인 패턴이다. 

<img src="https://velog.velcdn.com/images/daram9/post/d8eee955-081d-4758-a348-4d1c32d5d3cb/image.png" width="400">

이를 통해 객체의 속성, 변환 등을 보완하며 보안, 데이터 검증, 캐싱, 로깅에 사용한다. 
이는 프록시 객체로 쓰이기도 하지만 프록시 서버로도 활용한다. 
##### - 프록시 서버에서의 캐싱
<small><span style="color:gray">캐시 안에 정보를 담아두고, 캐시 안에 있는 정보를 요구하는 요청에 대해 다시 멀리 있는 원격 서버에 요청하지 않고 캐시 안에 있는 데이터를 활용하는 것을 말한다. 이를 통해 불필요하게 외부와 연결하지 않기 때문에 트래픽을 줄일 수 있다는 장점이 있다.</span> </small>
#### 프록시 서버
>프록시 서버는 서버와 클라이언트 사이에서 클라이언트가 자신을 통해 다른 네트워크 서비스에 간접적으로 접속할 수 있게 해주는 컴퓨터 시스템이나 응용 프로그램을 말한다.

##### <span style="color:gray">프록시 서버로 쓰는 ginx</span>
nginx는 비동기 이벤트 기반의 구조와 다수의 연결을 효과적으로 처리 가능한 웹 서버이며, 주로 Node.js 서버 앞단의 프록시 서버로 활용된다.
##### <span style="color:gray">프록시 서버로 쓰는 CloudFlare</span>
CloudFlare는 전 세계적으로 분산된 서버가 있고 이를 통해 어떠한 시스템의 콘텐츠 전달을 빠르게 할 수 있는 CDN 서비스다.

<img src="https://velog.velcdn.com/images/daram9/post/2c089eb1-6288-4c3b-b394-0269bb014a74/image.png
" width="400">

##### <span style="color:tomato">DDOS 공격 방어</span>
DDOS는 짧은 기간동안 네트워크에 많은 요청을 보내 네트워크를 마비시켜 웹 사이트의 가용성을 방해하는 사이버 공격 유형이다. CloudFlare는 의심스러운 트래픽, 특히 사용자가 접속하는 것이 아닌 시스템을 통해 오는 트래픽을 자동으로 차단해서 DDOS 공격으로부터 보호한다.
##### <span style="color:tomato">HTTPS 구축</span>
서버에서 HTTPS를 구축할 때 인증서를 기반으로 구축할 수도 있다. 
하지만 CloudFlare를 사용하면 별도의 인증서 설치 없이 좀 더 손쉽게 HTTPS를 구축할 수 있다.

이 모든 것이 웹 서버 앞단에 두어 '프록시 서버'로 쓰기 때문에 가능하다.

### 1.6 이터레이터 패턴
>이터레이터 패턴은 이터레이터를 사용하여 컬렉션의 요소들에 접근하는 디자인 패턴이다.
이를 통해 순회할 수 있는 여러 가지 자료형의 구조와는 상관없이 이터레이터라는 하나의 인터페이스로 순회가 가능하다.

### 1.7 노출모듈 패턴
>노출모듈 패턴은 즉시 실행 함수를 통해 private, public 같은 접근 제어자를 만드는 패턴을 말한다. 
자바스크립트는 private나 public 같은 접근 제어자가 존재하지 않고 전역 범위에서 스크립트가 실행된다. 그렇기 때문에 노출모듈 패턴을 통해 private와 public 접근 제어자를 구현하기도 한다. 

```js
const pukuba = (() => {
    const a = 1
    const b = () => 2
    const public = {
        c : 2, 
        d : () => 3
    }
    return public
})()
console.log(pukuba)
console.log(pukuba.a)
// { c: 2, d: [Function: d] }
```
a와 b는 다른 모듈에서 사용할 수 있는 변수나 함수인 private 범위를 가진다. 다른 모듈에서 접근할 수 없고 c와 d는 다른 모듈에서 사용할 수 있는 변수나 함수인 public 범위를 가진다. 
참고로 이 원리를 기반으로 자바스크립트 모듈 방식으로는 CJS(CommonJS) 모듈 방식이 있다. 

##### <span style="color:gray">public</span>
<small>클래스에 정의된 함수에서 접근 가능하며 자식 클래스와 외부 클래스에서 접근 가능한 범위</small>

##### <span style="color:gray">protected</span>
<small>클래스에 정의된 함수에서 접근 가능, 자식 클래스에서 접근 가능하지만 외부 클래스에서 접근 불가능한 범위</small>
##### <span style="color:gray">private</span>
<small>클래스에 정의된 함수에서 접근 가능하지만 자식 클래스와 외부 클래스에서 접근 불가능한 범위</small>
##### <span style="color:gray">즉시 실행 함수</span>
<small>함수를 정의하자마자 바로 호출하는 함수. 초기화 코드, 라이브러리 내 전역 변수의 충돌 방지 등에 사용한다.</small>

### 1.8 MVC 패턴
>MVC 패턴은 모델(Model), 뷰(View), 컨트롤러(Controller)로 이루어진 디자인 패턴이다.

<img src="https://velog.velcdn.com/images/daram9/post/bb60524c-6ad1-4abc-a9cb-6dbb380c66c6/image.png
" width="400">
애플리케이션의 구성 요소를 세 가지 역할로 구분하여 개발 프로세스에서 각각의 구성 요소에만 집중해서 개발할 수 있다.
재사용성과 확장성이 용이하다는 장점이 있고, 애플리케이션이 복잡해질수록 모델과 뷰의 관계가 복잡해지는 단점이 있다. 

#### 모델
모델은 애플리케이션의 데이터인 데이터베이스, 상수, 변수 등을 뜻한다.

#### 뷰
뷰는 inputbox, checkbox, textarea 등 사용자 인터페이스 요소를 나타낸다. 
즉, 모델을 기반으로 사용자가 볼 수 있는 화면을 뜻한다. 모델이 가지고 있는 정보를 따로 저장하지 않아야 하며 단순히 사각형 모양 등 화면에 표시하는 정보만 가지고 있어야 한다. 
또한, 변경이 일어나면 컨트롤러에 이를 전달해야 한다.

#### 컨트롤러
컨트롤러는 하나 이상의 모델과 하나 이상의 뷰를 잇는 다리 역할을 하며 이벤트 등 메인 로직을 담당한다. 
또한, 모델과 뷰의 생명주기도 관리하며, 모델이나 뷰의 변경 통지를 받으면 이를 해석하여 각각의 구성 요소에 해당 내용에 대해 알려준다.

### 1.9 MVP 패턴
>MVP 패턴은 MVC 패턴으로부터 파생되었으며 MVC에서 C에 해당하는 컨트롤러가 프레젠터(presenter)로 교체된 패턴이다.
<img src="https://thebook.io/img/080326/055_1.jpg" width="400">
뷰와 프레젠터는 일대일 관계이기 때문에 MVC 패턴보다 더 강한 결합을 지닌 디자인 패턴이라고 볼 수 있다. 

### 1.10 MVVM 패턴
>MVVM 패턴은 MVC의 C에 해당하는 컨트롤러가 뷰모델(View model)로 바뀐 패턴이다.
<img src="https://thebook.io/img/080326/055_2.jpg" width="600">
여기서 뷰모델은 뷰를 더 추상화한 계층이며, MVVM 패턴은 MVC 패턴과는 다르게 커맨드와 데이터 바인딩을 가지는 것이 특징이다. 뷰와 뷰모델 사이의 양방향 데이터 바인딩을 지원하며 UI를 별도의 코드 수정 없이 재사용할 수 있고 단위 테스팅하기 쉽다는 장점이 있다.

#### MVVM 패턴의 예: 뷰
MVVM 패턴을 가진 대표적인 프레임워크로는 뷰(Vue.js)가 있다. Vue.js는 반응형이 특징인 프런트엔드 프레임워크이다. 
함수를 사용하지 않고 값 대입만으로도 변수가 변경되며 양방향 바인딩, html을 토대로 컴포넌트를 구축할 수 있다는 점이 특징이다. 재사용 가능한 컴포넌트 기반으로 UI를 구축할 수 있으며 BMW, 구글, 루이비통 등에서 사용한다.
