# HTTP Header
```shell
[시작라인(Start Line)] - 요청 또는 응답의 목적을 설명하는 첫 줄
[해더 (Header)] - 청/응답에 대한 부가 정보를 담은 (K-V) 쌍


[본문(Body)] - 실제 데이터 
```
* 클라이언트와 서버가 서로 요청과 응답의 메세지를 주고 받을 때에 그 `메세지의 정보(성격, 부가 정보, 처리 방법...)를` 알려주는 <U>메타 데이터 영역</U>
* `HTTP 메세지 본문(body)`에 담긴 데이터에 대해 설명하거나, 통신을 위한 약속 정보가 담겨져 있음
* HTTP의 특징인 `비연결지향(Stateless)`으로 인해 매번 요청의 필요한 정보를 헤더에 넣어서 보냄

| 목적       | 설명                            |
| -------- | ----------------------------- |
| 요청 정보 제공 | 어떤 데이터 원하는지, 어떤 포맷으로 받고 싶은지 등 |
| 인증       | 토큰, 세션 쿠키, 인증 타입              |
| 데이터 설명   | MIME 타입, 인코딩 형식, 길이           |
| 캐싱/압축    | 요청 결과를 저장하거나 압축할 수 있음         |
| 연결 설정    | keep-alive, close 같은 옵션       |

<br></br>

# 1. HTTP Request Header
* 클라이언트가 서버에 요청을 보낼 때, 요청에 대한 추가적인 정보를 담는 헤더
* **요청 헤더 예시:** Firefox로 developer.mozilla.org에 접속했을 때 
```shell
GET /home.html HTTP/1.1

# 요청을 보낼 도메인 주소
Host: developer.mozilla.org 

# 클라이언트의 브라우저 및 OS 정보.
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.9; rv:50.0) Gecko/20100101 Firefox/50.0

# 클라이언트가 받고 싶은 MIME 타입(콘텐츠 유형).
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8

# 선호하는 언어 설정.
Accept-Language: en-US,en;q=0.5

# 클라이언트가 수용 가능한 압축 방식들.
Accept-Encoding: gzip, deflate, br

# 사용자가 이전에 방문한 페이지(이 요청을 유발한 페이지).
Referer: https://developer.mozilla.org/testpage.html

# TCP 연결을 요청 후에도 유지할지 여부.
Connection: keep-alive

# 브라우저가 HTTPS를 선호한다는 신호.
Upgrade-Insecure-Requests: 1

# 조건부 요청용 헤더.

If-Modified-Since: Mon, 18 Jul 2016 02:36:04 GMT
# 서버 리소스가 이 ETag와 다를 경우에만 응답. 같으면 304로 응답함.
If-None-Match: "c561c68d0ba92bbeb8b0fff2a9199f722e3a621a"

# 캐시된 리소스를 사용할 최대 시간 (초 단위).
Cache-Control: max-age=0
```

## 1.1. Host
* 요청을 보낼 호스트를 나타냄
* <U>주로 도메인 네임으로 명시</U>되며, 포트 번호가 포함될 수 있음
* 요청 헤더에 반드시 명시 되어 있어야 하는 필드

## 1.2. User-Agent
```shell
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.9; rv:50.0) Gecko/20100101 Firefox/50.0
# 서버 측에서는 이 요청이
# Mac OS X 10.9에서 Firefox 50.0 브라우저를 사용하는 사용자의 HTTP 요청
# 임을 파악할 수 있음 
```
* HTTP 요청을 시작하는 클라이언트 측의 프로그램
    * 이 프로그램 안에는 요청을 보낸 <U>클라이언트 측의 브라우저, 운영체제(OS), 렌더링 엔진 등 환경 정보를 포함</U>함
* 서버 측에선 `User-Agent를 참고`하여 호환되는 콘텐츠를 제공함

## 1.3. Referer
```shell
# 이전에 접속해 있던 URL
Referer: https://developer.mozilla.org/testpage.html
# 음 여기서 https://developer.mozilla.org 로 이동하나 보군
HOST: developer.mozilla.org 
```
* 클라이언트가 요청을 보낼 때에 머무르고 있던(이전) URL이 명시 됨
* 서버에서는 Referer의 표기된 정보로 어떤 경로로 유입되었는지 추적하거나, 출처 검사에 사용 가능
* 보안상 민감한 정보가 URL에 포함되면 Referer를 통해 유출될 수 있음 


## 1.4. Authorization
```shell
 Authorization: <type> <credentials>
```
* 클라이안트의 인증 정볼를 담는 헤더
* 인증 정보를 담기 위해서 `인증 타입(type)`과 인증을 위한 `정보 (credentials)`가 차례로 명시됨

```shell
Authorization: Basic YWRtaW46cGFzc3dvcmQ=
Authorization: Bearer eyJhbGciOi...
```
* **인증타입의 타입:** `Basic Type (사용자 ID와 비밀번호를 base64로 인코딩)`, `Bearer Type(JWT 등 토큰 기반 인증)`

```pgsql
[Authorization: Basic userId:password]
                  │
                  ▼
          ┌─────────────┐
          │   Base64    │
          │  Encoding   │
          └─────────────┘
                  │
                  ▼
[Authorization: Basic dXppOjEyMzQ=]
```
```pgsq
[Authorization: Bearer <token>]
                  │
                  ▼
        ┌────────────────────┐
        │   Token 생성 중...  │
        └────────────────────┘
                  │
       (토큰 생성 완료 후 삽입)
                  ▼
[Authorization: Bearer eyJhbGciOi...abc123def456ghi789]

```

<br></br>

# 2. HTTP Response Header
* 서버가 클라이언트에게 응답을 보낼 때, 응답에 대한 부가 정보를 담는 헤더
* `실제 데이터(body)`의 내용과 성격, 서버의 상태, 캐시, 인증 요구 등의 정보를 전달
* 클라이언트는 응답 헤더를 바탕으로 콘텐츠를 처리하거나, 사용자에게 메시지를 전달함
```shell
HTTP/1.1 200 OK

# 서버 정보
Server: Apache/2.4.1 (Unix)

# 콘텐츠 타입과 인코딩
Content-Type: text/html; charset=UTF-8
Content-Length: 138

# 캐싱 설정
Cache-Control: no-cache

# 요청에 대한 허용된 HTTP 메소드
Allow: GET, POST, HEAD

# 인증 필요 시 클라이언트에 요구
WWW-Authenticate: Basic realm="User Visible Realm"

# 요청 시 서버가 잠시 멈췄을 경우 클라이언트에게 재시도 시간 제시
Retry-After: 120

# 리소스 이동 시 새 위치 제공
Location: https://new.example.com/resource
```

## 2.1. Server
```shell
# Unix OS에서 동작하는 아파치  서버임
Server: Apache/2.4.1 (Unix)
```
* 요청을 처리한 서버 소프트웨어의 이름, 버전, 플랫폼 정보를 명시
* 보안상 이유로 숨기거나 간소화하는 경우도 있음

## 2.2. Allow
```shell
# 요청에 대한 허용된 HTTP 메소드 : GET, POST, HEAD
Allow: GET, POST, HEAD
```
* 클라이언트에게 <U>허용된 HTTP 목록</U>을 알려주기 위한 헤더
* 주로 `405 Method Not Allowed 응답`과 함께 사용됨

## 2.3.Retry-After
```shell
# 120초 이후에 가능 
Retry-After: 120
```
* 서버가 현재 요청을 처리할 수 없을 때, 클라이언트에게 다시 시도할 시점을 알림
* **두 가지 형식**  중 하나로 전달됨
    * 초 단위 숫자 (120초 후 재시도)
    * HTTP 날짜 포맷 (GMT 시각 기준)

## 2.4. Location
* 클라이언트가 요청한 `리소스의 새로운 위치(URL)`를 알려줌
* 주로 `3xx Redirect 응답 상태 코드`와 함께 사용됨

## 2.5. WWW-Authenticate
![alt text](<../설명사진/[응용 계층] HTTP 응답 헤더 WWW.png>)
```shell
# 인증 X -> 인증 정보를 보내달라는 의미
WWW-Authenticate: Basic realm="User Visible Realm"
```
* 클라이언트가 인증을 요구할 때에 서버가 보내는 <U>조건과 인증 방식을 설명하기 위한 헤더</U>
* `401 Unauthorized 응답`에 반드시 포함되어야 함
* `Basic`, `Bearer`, `Digest`, `OAuth `... 다양한 스킴 사용 가능


<br></br>

# 3. 공통 헤더
| 헤더 이름              | 설명                                                   |
| ------------------ | ---------------------------------------------------- |
| `Date`             | 메시지가 생성된 날짜와 시간을 나타냄                                 |
| `Connection`       | 클라이언트와 서버 간의 연결 관리 방식을 지정함 (`keep-alive`, `close` 등) |
| `Content-Length`   | 메시지 본문의 바이트 크기를 명시함                                  |
| `Content-Type`     | 본문의 데이터 형식(미디어 타입)을 지정함                              |
| `Content-Language` | 본문의 언어를 명시함                                          |
| `Content-Encoding` | 본문이 압축 등 어떤 인코딩으로 인코딩되었는지 명시함                        |
