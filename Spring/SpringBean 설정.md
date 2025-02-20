# Spring Bean 설정



## Spring Bean 설정 방법


### 1. Spring Context 실행

* 첫번째로 Spring Context를 실행해야함
* `AnnotationConfigApplicationContext` 클래스를 사용하여 Spring Context를 실행할 수 있음

```java
var context = new AnnotationConfigApplicationContext(HelloWorldConfiguration.class);
```

### 2. Spring에서 관리할 Bean 설정

**Spring에서 관리할 Bean을 설정하는 방법 2가지**
1. 설정 클래스(@Configuration)를 사용하여 Bean을 정의하는 방법 ✅
2. 설정 파일을 이용하는 방법



### 3. Bean 정의

* `@Configuration` 어노테이션을 사용하여 설정 클래스를 정의함
* `@Bean` 어노테이션을 사용하여 Bean을 정의함

```java
@Configuration
public class HelloWorldConfiguration {

    @Bean
    public String name() {
        return "Ranga!";
    }

    @Bean
    public int age() {
        return 15;
    }

    @Bean
    public Person person() {
        return new Person("Riva", 23, new Address("567", "Busan"));
    }

    @Bean
    public Person person2MethodeCall() {
        return new Person(name(), age(), address());
    }

    @Bean
    public Person person3Parameters(String name, int age, Address address3) {
        return new Person(name, age, address3);
    }

    @Bean(name = "address2")
    public Address address() {
        return new Address("123", "Seoul");
    }

    @Bean(name = "address3")
    public Address address3() {
        return new Address("789", "Suwon");
    }
}
```

### 4. Bean 검색 및 사용

* Spring Context에서 정의한 Bean을 검색하고 사용함.
* `getBean` 메서드를 사용하여 Bean을 검색함

```java
System.out.println(context.getBean("name"));
System.out.println(context.getBean("age"));
System.out.println(context.getBean("person"));
System.out.println(context.getBean("person2MethodeCall"));
System.out.println(context.getBean("person3Parameters"));
System.out.println(context.getBean("address2"));
System.out.println(context.getBean("address2", Address.class));
```

## 결론

