# @RequestParam

* `Spring MVC`에서 request(요청) 매개변수를 컨트롤러의 매개변수로 매핑하는 어노테이션 
    * 다른 말로 `Servlet 요청`의 파라미터를 컨트롤러의 인자로 바인딩 할 수 있다고 표현함
* 클라이언트 쪽에서 쿼리 스트링, 폼 데이터로 전송한 값을 컨트롤러에서 받을 때 사용함
![alt text](../설명사진/@RequestParam설명사진.png)

* **예시 코드**
```java
    // URL: /login?name=kto03
    @RequestMapping("login")
    public String gotoLoginPage(@RequestParam String name, ModelMap model) {
        model.put("name", name); 
        System.out.println("Request Parameter: " + name); 
        // 실제 프로덕션 환경에서는 sysout으로 로깅하지 않음
        return "login";
    }
```    
* 사용자가 `/login?name=kao`로 요청하면 컨트롤러에서의 매개 변수 또한 `name=kao`가 됨 

<br></br>

## @RequestParam의 속성

### 1. required 속성
#### 1.1.default
* `@RequestParam`의 기본값은 필수임 `(required=true)`
* 만약 매개변수가 전달되지 않는다면 `400 error(잘못된 요청)` 뜨게 됨
```java
// (required = true) 생략 가능(기본 값)
@GetMapping("/someurl")
public String getUser(@RequestParam(required = true) String username) {
    return "User: " + username;
}
// [URL]someurl?username=kiki → User: kiki
```

#### 1.2. 선택적 매개변수 
* 매개변수가 없어도 되게끔 변경할 수 있음. `(required = false)`
* 값을 제공하지 않아도 에러가 발생하지 않음. (기본 값을 설정해야함)
```java
@GetMapping("/someurl")
public String getUser(@RequestParam(required = false) String username) {
    return "User: " + (username != null ? username : "defaultusername");
}
//  [URL]someurl?username=John → User: John
//  [URL]/someurl → User: defaultusername (오류 없이 기본값으로 처리)
```


<br></br>


### 2. defaultValue 속성
* defaultValue 속성을 지정하여 매개변수에 대한 기본 값을 설정할 수 있음
* `required = false`와 같음
```java
@GetMapping("/someurl")
public String getUser(@RequestParam(defaultValue = "defaultusername") String username) {
    return "User: " + username;
}
```
<br></br>

## 다양한 매개변수 받는 방법

### 1. 여러개의 매개변수 받기
```java
 @GetMapping("/bible")
    public String getBibleVerses(
        @RequestParam("book") int book, // book과 
        @RequestParam("chapter") int chapter, // chapter을 인자로!!
        Model model) {

        }
```
### 2. List, 배열 다중 값 매핑
* 동일한 파라미터로 여러 데이터가 전달 되는 경우 List, 배열로 매핑할 수 있음
```java
@GetMapping("/numbers")
public String getNumbers(@RequestParam List<Integer> numbers, Model model) {
    model.addAttribute("numbers", numbers);
    return "numbers";
}
// [URL] /numbers?numbers=1&numbers=2&numbers=3 → List: [1, 2, 3]
```

```java
@GetMapping("/tags")
public String getTags(@RequestParam String[] tags, Model model) {
    model.addAttribute("tags", tags);
    return "tags";
}
// [URL] /tags?tags=java&tags=spring&tags=boot 
// → Array: ["java", "spring", "boot"]
```

### 2. Map으로 모든 매개변수 매핑
* 개수의 제한 없이 `Map`을 활용하여 K-V 방식으로 모든 파라미터를 매핑함
    * Key: 파라미터 name
    * Value: 파라미터 value
```java
@GetMapping("/userinfo")
    public String getUserInfo(@RequestParam Map<String, String> params) {
        return "Params: " + params.toString();
    }
// [URL] /userinfo?name=John&age=25
// → Params: {name=John, age=25}

```