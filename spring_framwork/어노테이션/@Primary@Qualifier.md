# @Primary 
* 여러 개의 빈이 존재 할 때, 특정 빈을 우선적으로 자동 주입 하도록 지정하는 어노테이션
* 단일 값을 주입 해야하는 상황에서 우선 순위로 지정할 수 있음
* `@Primary` 이 붙은 빈은 여러 빈 후보 중에 **먼저 선택됨**
* `Component` 가 직접, 간접적으로 선언된 클래스나 @Bean이 붙은 메서드에 적용할 수 있음

```java
// Spring 코드 주석의 예시 코드
@Component
public class FooService {

    private FooRepository fooRepository;

    @Autowired
    public FooService(FooRepository fooRepository) {
        this.fooRepository = fooRepository;
    }
}

@Component
public class JdbcFooRepository extends FooRepository {

    public JdbcFooRepository(DataSource dataSource) {
        // ...
    }
}

@Primary  // HibernateFooRepository를 우선적으로 주입
@Component
public class HibernateFooRepository extends FooRepository {

    public HibernateFooRepository(SessionFactory sessionFactory) {
        // ...
    }
}
```

## @Primary 사용 시에 주의 해야할 점
* `@Primary` 는 컴포넌트 스캔이 활성화 된 경우에만 적용됨
* **자동 주입 과정**에만 해당 어노테이션은 영향을 줌.
    * 그런 이유로 수동 주입`@Qualifier`을 사용하게 되면 묻힐 수 있음
* `@Primary`와 `@Qualifier`는 함께 사용할 수 있으며 `@Qualifier`가 우선임


<br></br>

# @Qualifier
* 여러 개의 빈이 존재 할 때, 특정 빈을 우선적으로 자동 주입 하도록 지정하는 어노테이션
* `Autowired`를 사용할 때, 동일한 타입의 빈일 경우 어떤 빈을 먼저 주입 해야할지 스프링 입장에선 곤란한 상황이 옴
    * 이때 사용하는 것이 `@Qualifier("빈 이름")` 
        * 빈 이름으로 지정하여 주입 가능


<br></br>


# @Primary VS @Qualifier
| 구분       | @Qualifier | @Primary |
|------------|-----------|----------|
| 역할       | 특정한 이름의 빈을 주입할 때 사용 | 기본적으로 주입될 빈을 지정 |
| 적용 대상  | `@Autowired`와 함께 사용하여 특정 빈을 지정 | 동일한 타입의 여러 빈 중 하나를 기본값으로 설정 |
| 우선순위   | `@Primary`보다 우선 적용됨 | `@Qualifier`가 없을 때 적용됨 |
| 선언 방식  | `@Qualifier("beanName")` | `@Primary` |
| 사용 예시  | `@Autowired @Qualifier("v8Engine") private Engine engine;` | `@Primary @Component public class DefaultEngine implements Engine {}` |
