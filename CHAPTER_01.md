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
