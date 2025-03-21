# @Bean VS @Component

* `@Bean`과 `@Component` 스프링 컨테이너에 빈을 등록하는 데 사용되는 어노테이션임 
* 그러나 방법과 목적에서의 차이가 있음.

## `@Bean`

* 보통 스프링 설정 클래스의 메서드에서 사용됨
    * `@Configuration` 애너테이션이 붙은 클래스 안에서 사용됨
* 개발자가 직접 모든 코드를 작성하여 제어함
* `@Bean` 어노테이션이 붙어 있는 메서드의 반환 값은 `스프링 컨테이너`에 의해 관리 되는 빈이 됨


```java
@Configuration
public class AppConfig {

    @Bean // 이 빈은 이제 스프링 컨테이너가 관리할 거임
    public MyService myService() {
        return new MyServiceImpl();
    }
}
```


## @Component

* 모든 자바 클래스에서 사용이 가능함
* 사용성은 쉬운 편이며, 클래스 이름 위에 어노테이션만 붙이면 됨
* `@Component`이 붙은 클래스를 스프링이 자동으로 탐지하여 빈을 생성함
* `@Service`, `@Repository`, `@Controller`와 같은 특수화된 애너테이션의 기본 어노테이션임

```java
@Component //이제부터 Spring이 자동으로 스캔하고 빈으로 등록함
public class ExComponent {
    // ...
}
```

## 차이점

1. **사용 위치**:
   - `@Bean`: 메서드 레벨에서 사용
   - `@Component`: 클래스 레벨에서 사용

2. **등록 방식**:
   - `@Bean`: 개발자가 직접 메서드를 통해 빈을 정의
   - `@Component`: Spring이 클래스를 스캔하여 자동으로 빈을 탐지하고 등록

3. **사용 목적**:
   - `@Bean`: 외부 라이브러리나 프레임워크의 클래스를 빈으로 등록할 때 주로 사용
   - `@Component`: 애플리케이션 내에서 직접 작성한 클래스를 빈으로 등록할 때 사용


| 항목 | `@Component` | `@Bean` |
|------|------------|---------|
| **사용 위치 (Where?)** | 모든 Java 클래스에서 사용 가능 | 보통 스프링 설정 클래스의 메서드에서 사용 |
| **사용 편의성 (Ease of use)** | 매우 쉬움. 어노테이션만 추가하면 됨 | 직접 모든 코드를 작성해야 함 |
| **자동 주입 (Autowiring)** | 지원됨 - 필드, 세터, 생성자 주입 가능 | 지원됨 - 메서드 호출 또는 메서드 매개변수 사용 |
| **빈 생성 주체 (Who creates beans?)** | 스프링 프레임워크가 자동으로 생성 | 개발자가 직접 빈 생성 코드를 작성 |
| **추천 사용 사례 (Recommended For)** | 애플리케이션 코드에서 빈을 만들 때 사용 (`@Component`) | 1. 커스텀 비즈니스 로직 빈 생성<br>2. 서드파티 라이브러리 빈을 등록할 때 사용 (`@Bean`) |
| **클래스당 빈 개수 (Beans per class?)** | 하나 (싱글턴) 또는 여러 개 (프로토타입) 가능 | 하나 또는 여러 개 - 원하는 만큼 생성 가능 |
