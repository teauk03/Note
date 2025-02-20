# 발전 배경
* 초기에는 엔터프라이즈 기능이 JDK 내부에 모두 들어 있었다고 함.
    * **엔터 프라이즈란?**: 웹 애플리케이션, 데이터 베이스 연동, 보안, 트랜잭션 관리 등 다양한 비지니스 시스템을 포괄하는 계념
* 그러나! 이 엔터프라이즈 기능이 시간이 지날 수록 많아지면서 포괄적인 하나의 덩어리 같은게 독립적인 엔터 프라이즈 플랫폼으로 분리가 되기 시작했음

<br></be>

# 발전 과정
## 1. J2EE (Java 2 Platfrom, Enterprise Edition)
* Java의 엔터프라이즈의 기능을 처음 공식화한 버전
* EJB, Serblets, JSP 등의 기술을 포함 했었음

➡️ 그러나 무시무시하게 복잡한 구조와 설정 방법 때문에 개발자들이 죽어 나갔다고 함

<br></br>

## 2. Java EE (Java Platform, Enterprise Edition)
* J2EE의 불편한 점을 개선한 버전
* 복잡성을 줄이고 직관적으로 변경 했다고 함
* EJB ➡️ CDI(Contexts and Dependency Injection) 변경하여 유연하게 개발하게끔 했음
* 라이브러리 패키지는 `javax.*` 을 사용했음

<br></br>

## 3. Jakarta EE
* 2017년도에 프로젝트가 이클립스 재단으로 변경되어 이름이 리뉴얼 됨
* Java EE에서 사용하던 `javax.* 패키지` ➡️  `jakarta.* 패키지` 로 변경
* 더 빠른 업데이트와 개방적인 오픈소스 생태계를 제공함

<br></be>

# 주요 jakarta EE 사양

| **Jakarta 기술** | **설명** |
|------------------|----------------------------------|
| **Jakarta Server Pages (JSP)** | 서버 사이드에서 HTML을 동적으로 생성하는 기술 |
| **Jakarta Standard Tag Library (JSTL)** | JSP에서 사용할 표준 태그 라이브러리 |
| **Jakarta Enterprise Beans (EJB)** | 엔터프라이즈 애플리케이션을 위한 비즈니스 로직 관리 |
| **Jakarta RESTful Web Services (JAX-RS)** | RESTful 웹 서비스를 쉽게 개발하는 API |
| **Jakarta Bean Validation** | 데이터 검증을 위한 표준 API |
| **Jakarta Contexts and Dependency Injection (CDI)** | 객체 간의 의존성을 쉽게 관리하는 기능 |
| **Jakarta Persistence (JPA)** | 데이터베이스와의 연동을 위한 ORM(Object Relational Mapping) |
