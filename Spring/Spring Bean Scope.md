# Spring Bean Scope

* `Spring Bean`: Spring 프레임 워크가 관리하는 객체

* `Scope`: Spring Bean이 존재할 수 있는 범위 

즉 Spring 컨테이너에서 Spring Bean이 어떻게 생성되고, 얼마나 유지되며, 어디서 사용할 수 있는지 설정하는 것을 말함.
* 기본적으로 스프링은 싱글톤 스코프(Singleton Scope)를 사용하게 함. (변경 가능)


## Bean Scope 종류

### 1. Singleton
- 기본 스코프
- Spring Ioc 컨테이너당 하나의 인스턴스만 생성
- Spring 컨테이너가 실행 될 때 빈이 생성되고, 컨테이너가 종료될 때 까지 유지 됨
- **예시 코드**:
```java
@Scope("singleton") // 생략 가능
@Component
class NomalClass {

}
```

### 2. Prototype
- Spring 컨테이너에서 빈을 요청할 때마다 새로운 인스턴스가 생성됨
    - 컨테이너에서 생성하지만 생성 후 Spring이 관리 하지 않음 
    - `@PreDestroy` 같이 라이프 사이클 콜백이 호출 되지 않음
    - 사용한 객체는 개발자가 직접 관리 해야함
- Spring IoC 컨테이너당 여러개의 인스턴스가 있을 수 있음
- **예제**:
```java
@Scope("prototype")
@Component
class PrototypeClass {

}

```

### 3. Request
- HTTP 요청당 하나의 인스턴스가 생성됨
- 주로 웹 애플리케이션에 활용됨
- HTTP 요청마다 새로운 빈이 필요할 때 사용함
- 요청이 끝나면 Spring 프레임 워크가 빈을 자동으로 제거함
- **예제**:
```java
@Component
@Scope(value = WebApplicationContext.SCOPE_REQUEST, proxyMode = ScopedProxyMode.TARGET_CLASS)
public class MyBean {
    // 요청별 상태를 저장하는 필드 및 로직
}
```


<br></br>

# Prototype vs Singleton (Spring Bean Scope) 비교표

| 항목             | Prototype (프로토타입)                                  | Singleton (싱글톤)                                  |
|-----------------|------------------------------------------------|------------------------------------------------|
| **인스턴스 수** | 요청할 때마다 새로운 인스턴스 생성               | Spring IoC 컨테이너당 하나의 인스턴스 생성 |
| **빈의 재사용** | 매번 새로운 객체 반환                            | 동일한 객체를 계속 재사용                   |
| **기본 스코프** | ❌ (기본 아님, `@Scope("prototype")` 설정 필요) | ✅ (기본 스코프, `@Scope("singleton")` 생략 가능) |
| **라이프사이클** | 빈을 반환한 후 Spring이 관리하지 않음           | 애플리케이션 종료 시까지 Spring이 관리       |

