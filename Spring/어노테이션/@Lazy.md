# 스프링의 지연 초기화와 즉시 초기화
https://github.com/teauk03/SpringStudy/blob/study05/study/src/main/java/com/study05/LazyInitializationLauncherApplication.java


### **지연 초기화(Lazy Initialization) vs 즉시 초기화(Eager Initialization) 비교**

| 항목 | 지연 초기화 (Lazy Initialization) | 즉시 초기화 (Eager Initialization) |
|------|----------------------------------|----------------------------------|
| **초기화 시점** | 애플리케이션에서 해당 빈이 처음 사용될 때 초기화됨 | 애플리케이션이 시작될 때 모든 빈이 초기화됨 |
| **기본 설정** | 기본이 아님 (NOT Default) | 기본 설정 (Default) |
| **코드 예시** | `@Lazy` 또는 `@Lazy(value=true)` | `@Lazy(value=false)` 또는 `@Lazy` 어노테이션이 없는 경우 |
| **초기화 중 오류 발생 시** | 오류가 발생하면 런타임 예외가 발생 | 오류가 발생하면 애플리케이션이 시작되지 않음 |
| **사용 빈도** | 거의 사용되지 않음 | 매우 자주 사용됨 |
| **메모리 소비** | 적음 (빈이 초기화될 때까지 메모리 사용 안 함) | 모든 빈이 시작 시 초기화되므로 더 많은 메모리 사용 |
| **추천 사용 사례** | 애플리케이션에서 거의 사용되지 않는 빈 | 대부분의 빈에 적용 |
