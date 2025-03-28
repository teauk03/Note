# SOLID 원칙

* OOP에서 좋은 설계를 위한 5개의 원칙
    * **[S]** Single Responsibility Principle (단일 책임 원칙)
    * **[O]** Open/Closed Principle (개방/패쇄 원칙)
    * **[L]** Liskov Substitution Principle (리스코프 치환 원칙)
    * **[I]** Interface Segregation Principle (인터페이스 분리 법칙)
    * **[D]** Dependency Inversion Principle (의존 역전 법칙)
* 코드의 유지 보수를 유연하게 해줌
* OOP의 꽃이라고 할 수 있음

<br></br>

## 1. Single Responsibility Principle
* 클래스 하나에는 하나의 책임만 가져야하며, 그 책임이 완벽하게 수행 되어야 함.
* 클래스에서 내용을 변경할 때 그 이유가 하나여야 한다는 말임.
* 여러 메서드가 존재할 수 있으며, 그 메서드를 통해 수행되는 대표적인 업무는 하나이여야 함

<br></br>

**🚨 SRP를 위반한 경우**
```java
class user {
    private String email;
    private String password;

    public void saveUser() {
        // User 정보 저장 로직
    }

    public void login() {
        // login 로직
    }

}
```
1. 확장성, 유연성 부분에 문제가 생김
2. 코드가 복잡해 짐
3. 이들의 역할을 직관적으로 파악하기 힘들어짐
4. 불가피하게 클래스를 수정할 일이 많아짐
5. 클래스를 수정할때 다른 책임에 영향을 줄 수 있음
6. 여러개의 책임이 한 클래스에 있기 때문에, 테스트와 리펙터링에도 난이도가 올라감

<br></br>

**✅ SRP를 준수한 경우**
```java
class user {
    private String email;
    private String password;
    //  setter/getter and More
}

class userRepository {
    public void saveUser() {
        // User 정보 저장 로직
    }
}

class AuthenticationService {
    public void login() {
        // login 로직
    }
}   
```
1. 모듈화와 코드 유지보수에 유용함.
2. 각각의 클래스의 역할이 분명해져, 직관적으로 파악하기 쉬움
3. 수정사항이 생길때마다 유연하게 변경이 가능함 
4. 클래스가 책임별로 나누어져 있기에 코드 재활용성이 뛰어남



<br></br>
<br></br>

## 2. Open/Closed Principle
* SW의 구성요소(컴포넌트, 클래스, 모듈, 함수)가 확장할 때는 열려 있어야하고, 변경할 때는 닫혀 있어야 한다는 원칙
* 추가되는 기능들을 클래스 확장을 통해 손쉽게 구현하면서, 확장에 따른 클래스 수정은 최소화 하도록 하는 설계 기법
* 요구사항 변경, 추가사항이 발생할때  코드를 수정하지 않고도 기능을 확장할 수 있어야 함 (재사용에 용이)
* <U>추상화 사용을 통한 관계 구축</U>


<br></br>

**🚨 OCP를 위반한 경우**
```java

class PaymentProcessor {
    public void processPayment(String paymentType) {
        if (paymentType.equals("creditCard")){
            // 신용 카드 결제 로직 
        } else if (paymentType.equals.("cash")) {
            // 현금 결제 로직
        }
    }
}
```
신용카드의 은행사 마다 카드 결제를 확장할려면 수 많은 케이스를 한 클래스의 메서드에서 담당하게 됨
1. 유지 보수 난이도 상승
2. 기능 추가 상황에 난이도 상슴
3. 코드 복잡해짐
4. 기존 과정에 영향이 감

<br></br>

**✅ OCP 준수한 경우**
```java
public interface Payment {
    void process();
}

class KbBankCardPayment implements Payment {
    public void process() {
        // KB 국민 은행 결제 프로세스 
    }
}

class TossBankCardPayment implements Payment {
    public void process() {
        // Toss 은행 결제 프로세스 
    }
}

class CashPayment implements Payment {
    public void process() {
        // 현금 결제 프로세스 
    }
}

class PaymentProcessor {
    public void pricessPayment(Payment payment) {
        payment.process();
    }
}
```

OCP를 잘 지키면 이렇게 확장할 때 따로 수정할 필요 없이 클래스를 추가해서 확장하면 됨
*  기존 과정의 문제에서 자유로워짐


<br></br>
<br></br>


## 3. Liskov Substitution Principle
* 서브 타입은 언제나 기존(상위) 타입으로 교체할 수 있어야 한다는 원칙 
* OOP의 다형성을 활용한 확장을 목표로 함
    * 상위 클래스를 사용하는 코드에서 자식 클래스로 변경해도 정상적으로 동작해야 함.

<br></br>

**🚨 LSP를 위반한 경우**
```java

class Calculator {
    public int add(int a, int b) {
        return a + b;
    }

    public int sub(int a, int b) {
        return a - b;
    }
}

class AdvancedCalculator extends Calculator {
    public int mul(int a, int b) {
        return a * b;
    }

    public double div(int a, int b) {
        if (b == 0) {
            throw new IllegalArgumentException("0으로 나눌 수 없음");
        }
        return (double) a / b;
    }
}

class Main {
    public static void main(String[] args) {
        AdvancedCalculator calc01 = new AdvancedCalculator();
        Calculator calc02 = new AdvancedCalculator();

        System.out.println(calc01.add(2, 3)); // 5
        System.out.println(calc01.mul(2, 3)); // 6

        // 부모 타입으로 선언된 경우, mul()과 div() 사용 불가
        // System.out.println(calc02.mul(2, 3)); // 컴파일 에러
        // System.out.println(calc02.div(2, 3)); // 컴파일 에러
    }
}

// [LSP를 지키지 않은 코드]
// AdvancedCalculator 클래스는 Calculator 클래스를 상속 받는다.
// 객체를 만들때 Calculator 클래스 타입을 사용하게 되면 mul, div 메서드를 사용할 수 없음


```

* 상위 클래스를 사용하는 코드에서 하위 클래스로 바꿨을 때, 예상한 대로 동작하지 않을 수 있음.
* 다형성이 무너짐
* 확장성이 떨어짐

<br></br>

**✅ OCP를 준수한 경우**
```java
interface BasicArithmeticOperations {
    int add(int a, int b);
    int sub(int a, int b);
}

interface AdvancedArithmeticOperations extends BasicArithmeticOperations {
    int mul(int a, int b);
    double div(int a, int b);
}

class BasicCalculator implements BasicArithmeticOperations {
    @Override
    public int add(int a, int b) {
        return a + b;
    }

    @Override
    public int sub(int a, int b) {
        return a - b;
    }
}

class AdvancedCalculator implements AdvancedArithmeticOperations {
    @Override
    public int add(int a, int b) {
        return a + b;
    }

    @Override
    public int sub(int a, int b) {
        return a - b;
    }

    @Override
    public int mul(int a, int b) {
        return a * b;
    }

    @Override
    public double div(int a, int b) {
        if (b == 0) {
            throw new IllegalArgumentException("0으로 나눌 수 없음");
        }
        return (double) a / b;
    }
}

public class Main {
    public static void main(String[] args) {
        BasicArithmeticOperations basicCalc = new BasicCalculator();
        System.out.println(basicCalc.add(2, 3)); // 5
        System.out.println(basicCalc.sub(5, 2)); // 3

        AdvancedArithmeticOperations advCalc = new AdvancedCalculator();
        System.out.println(advCalc.add(5, 2)); // 7
        System.out.println(advCalc.sub(5, 2)); // 3
        System.out.println(advCalc.mul(5, 2)); // 10
        System.out.println(advCalc.div(4, 2)); // 2.0
    }
}

//[LSP를 준수한 코드]
// BasicArithmeticOperations 인터페이스는 덧셈, 뺄셈만 지원하는 인터페이스라는 것을 명시하고,
// ArithmeticOperations 인터페이스는 BasicArithmeticOperations 인터페이스를 상속 받아 사칙 연산이 가능하다는 인터페이스를 정의함
// 

```

* 각각의 기능에 요구에 맞게 `인터페이스`와 `클래스`를 <U>독립되게 나누어 설계</U>를 하면 서브 타입이 기존 타입에 요구하는 것을 모두 충족할 수 있음.


<br></br>
<br></br>


## 4. Interface Segregation Principle
* 클래스는 자신이 사용하지 않는 인터페이스의 의존 받지 말아야 한다는 설계 원칙
* 클라이언트 자신이 사용하지 않은 메서드에 의존하지 않도록 인터페이스를 분리함
* 하나의 일반적인 인터페이스보단 여러개의 구체적인 인터페이스로 구현함.


<br></br>

**🚨 ISP를 위반한 경우**
```java

interface Printer {
    void print();
    void scan();
    void fax();
}

class MultiFunctionPrinter implements Printer {
    @Override 
    public void print() {
        // 프린트 로직
    }
    @Override 
    public void scan() {
        // 스캔 로직
    }
    @Override 
    public void fax() {
        // 팩스를 전송하는 로직
    }
}
// 프린트만 하는 Printer
class SimplePrinter implements Printer {
    @Override 
    public void print() {
        // 프린트 로직
    }
    // 필요 ❌
    @Override 
    public void scan() {
        // 스캔 로직
    }
    // 필요 ❌
    @Override 
    public void fax() {
        // 팩스를 전송하는 로직
    }    
}
```
![alt text](<설명 사진/ISP위반.png>)
* `MultiFunctionPrinter 클래스`에서는 `Printer 인터페이스`가 요구하는 모든 기능을 구현해야하므로 의존적임
* `SimplePrinter 클래스`에서는 `print()` 기능을 하나의 역할만 구현하면 되는데 불필요한 메서드까지 구현해야함
    * `SimplePrinter`은 자신이 사용하지 않는 인터페이스의 메서드에 의존 받고 있음.

<br></br>

**✅ ISP를 준수한 경우**
```java
interface Printer {
    void print();
}
interface Scanner {
    void scan();
}
interface Fax {
    void fax();
}

class MultiFunctionPrinter implements Printer, Scanner, Fax {
    @Override 
    public void print() { }
    @Override 
    public void scan() { }
    @Override 
    public void fax() { }
}

class SimplePrinter implements Printer {
    @Override 
    public void print() {}
}
```
![alt text](<설명 사진/ISP준수.png>)
* 인터페이스가 각각의 기능들(Print, Scan, Fax)로 나누어져 별도의 인터페이스로 나누어짐
* 각 기능에 맞게 확장하기에 유리하고, 불필요한 인터페이스에 의존하지 않아도 됨


<br></br>
<br></br>


## 5. Dependency Inversion Principle
* 고수준 모듈이 저수준 모듈에 의존해서는 안되게 설계하는 원칙
* 고수준 모듈과 저수준 모듈 둘다 추상화에 의존하게 함
* <U>클래스 간의 결합도를 낮추는 것</U>을 지향함
    * **저수준:** 구체적인 동작을 구현하는 모듈 (고수준이 필요로 하는 실제 기능)
    * **고수준:** 비지니스 로직, 저수준이 가지고 있는 동작을 제어하는 추상화된 로직을 제공하는 모듈
 <br></br> 

**🚨 DIP를 위반한 경우**
![alt text](<설명 사진/DIP 위반 코드.png>)

![alt text](<설명 사진/DIP 위반.png>)

* **고수준 모듈**인 `Smartphone 클래스`는 **저수준 모듈**인 `UsbCCharger, LightningCharger 클래스`에 직접적으로 의존함.
* 만약 새로운 충전기능인 무선 충전이라는 기능이 추가 되었을 경우, `Smartphone 클래스`에서도 추가적인 수정(추가)가 필요함

<br></br>

**✅ DIP를 준수한 경우**
```java
interface Charger {
    void charge();
}

class UsbCCharger implements Charger {
    @Override
    public void charge(){
        System.out.println("USB C로 충전");
    }
}

class LightningCharger implements Charger{
    @Override
    public void charge(){
        System.out.println("라이트닝으로 충전");
    }
}

class WirelessCharger implements Charger {
    @Override
    public void charge(){
        System.out.println("무선으로 충전");
    }
}


class SmartPhone {
    private Charger charger;

    public SmartPhone(Charger charger) {
        this.charger = charger;
    }

    public void charge() {
        charger.charge();
    }

}
```
![alt text](<설명 사진/DIP준수.png>)

* **고수준 모듈**인 `SmartPhone클래스`는 더 이상 **저수준 모듈**인 `UsbC, Lightning 클래스`에 의존하지 않고, **추상화된** `Charger 인터페이스`에 의존하게 됨.
* 확장성, 유연성이 증가 되고, 테스트하기 편해짐