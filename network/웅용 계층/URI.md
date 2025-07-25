# URI
![alt text](<../설명사진/[응용 계층] URI, URL, URN의 관계.png>)
* Uniform Resource Identifier의 약자로 네트워크 상에서 <U>자원을 식별하기 위해 사용되는 표준 형식</U>
    * 여기서의 **자원(resource):** 네트워크 상에서 메시지를 통해 주고 받는 대상
    * 이 자원의 종류는 크게 `URL`, `URN`이 있음
 
<br></br>

 ## URI의 구성 요소
```shell
scheme://[user[:password]@]host[:port]/path[?query][#fragment]
```
| 구성 요소         | 설명                       | 예시                         |
|------------------|----------------------------|------------------------------|
| `scheme`         | 사용 프로토콜              | `http`, `https`, `ftp`, `mailto`  |
| `user:password@` | 인증 정보        | `user:pass@`                 |
| `host`           | 서버 주소                  | `www.google.com`             |
| `port`           | 포트 번호 (생략 가능)      | `:80`, `:443`                |
| `path`           | 자원의 경로                | `/search`, `/images/logo.png`|
| `?query`         | 추가 파라미터              | `?q=chatgpt&lang=ko`         |
| `#fragment`      | 문서 내 위치               | `#section1`, `#footer`       |


<br></br>

 ## URL
* 자원의 실제 `위치(location)`를 지정하여, 해당 자원에 어떻게 접근할 수 있는지를 알려줌

```shell
scheme://[user[:password]@]host[:port]/path[?query][#fragment]
```

| 구분           | 구성 요소         | 설명                                       |
|----------------|------------------|--------------------------------------------|
| **기본 구성**   | `scheme`         | 사용 프로토콜 (ex. http, https, ftp 등)    |
|                | `authority`      | 자원의 권한 정보를 나타내는 블록          |
|                | `path`           | 자원의 경로를 나타냄                       |
|                | `?query`         | 클라이언트가 서버로 전달하고자 하는 데이터 |
|                | `#fragment`      | 문서 내 특정 위치를 가리킴                 |


| `authority` 구성 요소 | 설명                                      |
|------------------------|-------------------------------------------|
| `user:password@`       | 사용자 인증 정보 (사용자 이름과 비밀번호) |
| `host`                 | 서버 주소 또는 IP 주소                     |
| `port`                 | 포트 번호 (기본 포트는 생략 가능)         |
 <br></br>

 ## URN
 * 자원의 고유한 이름을 저공함
 * 자원의 위치나 접근 방법 이런 것에 대해서는 무관