# **POJO** VS **JavaBean** VS **SpringBean**

## 이 글을 적는 이유
Spring 독학을 위해 강의, 인터넷 검색, 문서를 보다가 자꾸 나오는 저 3개의 단어가 너무나도 궁금했었다. 해서 찾아보니 3개다 객체란다. 이해가 안되서 찾아보니.. 성질이 좀 다른 객체라는 것이다. 단어마다 차이를 두는 것은 차이가 명확하기에 나누는 것이므로 그 명확한 것을 위해 공부하고자 적게 되었다.

<br></br>

## POJO (Plain Old Java Obeject)

* 이름 그대로 순수한 늙은 자바 객체라는 말
* 즉 특정하게 제약을 두지 않는 자바의 오브젝트 라는 말임.
    * 프레임 워크, 라이브러리에 의존하지 않는 객체
    * 자바의 기본 문법으로 작성된 순수한 객체

```java
public class People {
    private String name;
    private int age;
    private String gender;

    public Person(String name, int age, String gender) {
        this.name = name;
        this.age = age;
        this.gender = gender;
    }

    // Getter & Setter 메서드
    public String getName() { return name; }
    public int getAge() { return age; }
    public String getGender() { return gender; }
   
    public void setName(String name) { this.name = name; } 
    public void setAge(int age) { this.age = age; }
    public void setGender(String gender) {this.gender = gender}
}
```

<br></br>

## Java Bean
* POJO에서 몇가지 규칙이 추가된 자바 객체
    * 개본 생성자(매개 변수X) 필수👌
    * 필드는 `private`, 접근은 `pubic get,set` 으로 
    * 직렬화가 가능해야함 `(Serializable)`
* 코드 재사용성과 캡슐화를 위했었음
```java
public class People {
    private String name;
    private int age;
    private String gender;

    // 기본 생성자
    public People() {}

    // 매개변수가 있는 생성자
    public People(String name, int age, String gender) {
        this.name = name;
        this.age = age;
        this.gender = gender;
    }

    // Getter & Setter 메서드
    public String getName() { return name; }
    public int getAge() { return age; }
    public String getGender() { return gender; }
   
    public void setName(String name) { this.name = name; } 
    public void setAge(int age) { this.age = age; }
    public void setGender(String gender) { this.gender = gender; }
}
```

<br></br>

## Spring Bean
* Spring 프레임 워크가 관리하는 객체
* `@Bean @Component @Service ..` 어노테이션으로 생성됨
* Spring의 IoC 컨테이너가 이 Bean의 생명 주기를 관리함
    * 기븐으로 싱글턴으로 관리함

```java
@Configuration
public class AppConfig {

    @Bean
    public People people() {
        return new People("John Doe", 30, "Male");
    }
}

public class App {
    public static void main(String[] args) {
        ApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);
        People people = context.getBean(People.class);

        System.out.println("Name: " + people.getName());
        System.out.println("Age: " + people.getAge());
        System.out.println("Gender: " + people.getGender());

//output:        "John Doe", 30, "Male"
    }
}
        
```