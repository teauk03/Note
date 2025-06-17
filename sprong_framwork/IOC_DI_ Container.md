# IOC (Inversion Of Control)


**제어의 역전이란 무엇인가?**
* SW 개발의 디자인 원칙 중 하나
* 프로그램의 호름 즉 제어권을 개발자가 아니라 프레임워크나 컨테이너가 관리하도록 위임하는 패턴

<br></br>

**프레임워크 vs 라이브러리의 차이점**
* **프레임워크:**  개발자가 작성한 코드를 해당 도구가 코드의 흐름을 개발자 대신 제어해줌
* **라이브러리**: 개발자가 작성한 코드를 개발자가 코드의 흐름을 제어

<br></br>

# DI (Dependency Injection)

**의존성 주입이란 무엇인가?**
* 일단 의존성이란 한 객체가 자신을 제외한 다른 객체가 필요해서 불러오는 것을 의존성을 띈다고 함
    * class A가 class B의 객체가 필요할 경우 (A는 B에 의존함)
* OOP에서 객체간의 의존 관계를 외부에서 주입하는 방법
* 제어 역전을 구현하는 방법이기도 함
* 이러한 의존 관계는 정적인 관계와 동적인 관계로 분리해서 설계하는 것이 이롭다.

<br></br>

**정적 클래스 관계**

아래 코드로 `OrderServiceImpl` 클래스는 `DiscountPolicy` 클래스와 `MemberRepository`에 의존한다는 것을 알 수 있음
```java
import SpringStart.core.discount.DiscountPolicy;
import SpringStart.core.member.Member;
import SpringStart.core.member.MemberRepository;

public class OrderServiceImpl implements OrderService
```
* 클래스의 의존 관계가 명확히 정의 되어 있고 변경되지 않음
* 애플리케이션을 키지 않아도 코드를 보고 명확히 알 수 있음
* 이 관계성을 보고 실제 어떤 객체가 `OrderServiceImpl`에 들어갈지는 알 수 없음

**동적인 객체 의존 관계** 
* 애플리케이션이 실행되는 시점에 생성되는 객체의 참조가 연결된 의존 관계
    * Run Time 때에 외부에서 실제 구현 객체를 생성하고 클라이언트에게 전달해서 C - S의 의존관계가 성립되게 함



<br></br>

# IoC, DI 컨테이너
* 객체를 관리 (생성, 의존관계 연결..)해주는 컨테이너

**Ioc 컨테이너**
* 객체간의 제어 흐름을 이 컨테이너가 담당하도록 하는 제어 역전의 구현체
* 의존성 주입, 객체 생성, 생명 주기를 담당

**DI 컨테이너**
* 객체간의 의존성을 자동으로 결합해주는 컨테이너 (IoC의 구현체 중에 하나로 동작)
