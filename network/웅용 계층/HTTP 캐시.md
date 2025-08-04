# HTTP 캐시 
![alt text](<../설명사진/[응용 계층] HTTP 캐시에 대하여.png>)
* 요청에 대한 정보를 `클라이언트(브라우저) or 중간 서버`가 <U>HTTP 응답을 임시로 저장</U>해두고, 같은 요청이 들어오면 서버에 다시 가지 않고 <U>저장된 응답을 재사용하는 방식</U>
* 정보를 임시로 저장하는 행위 자체를 `캐시(cache)`, `캐싱(caching)`한다라고 함
* 임시로 저장된 정보를 `캐시(cache)`라고도 함

| 위치      | 설명                                          |
| ------- | ------------------------------------------- |
| 브라우저 캐시 | 사용자의 로컬 디스크에 저장됨 (정적 리소스 위주)                |
| 프록시 캐시  | CDN, Reverse Proxy 같은 중간 서버가 응답을 저장         |
| 서버 캐시   | 애플리케이션 서버 내부 캐시  |

<br></br>

## 1. HTTP 캐시 관련 헤더
| 헤더              | 설명                           | 예시                                                |
| --------------- | ---------------------------- | ------------------------------------------------- |
| `Cache-Control` | 캐시 정책을 정의하는 핵심 헤더            | `public`, `private`, `no-cache`, `max-age=3600`  |
| `Expires`       | 응답이 만료되는 절대 시간 (HTTP/1.0 방식) | `Expires: Wed, 21 Oct 2025 07:28:00 GMT`          |
| `Pragma`        | 오래된 브라우저용 (거의 안 씀)           | `Pragma: no-cache`                                |


### 1.1. Cache-Control
* 가장 많이 사용하는 캐시 제어 헤더
* 다양한 지시어를 사용하여 캐시를 세밀하게 제어할 수 있음

| 디렉티브              | 설명                                        | 사용 예시 / 특징                         |
| ----------------- | ----------------------------------------- | ---------------------------------- |
| `max-age=초`       | **캐시 유효 시간**을 초 단위로 지정                    | `max-age=3600` → 1시간 동안 캐시 사용 가능   |
| `no-cache`        | **캐시에 저장은 가능하지만**, 매 요청마다 **서버 검증 필수**    | ETag/Last-Modified 기반 304 재검증      |
| `no-store`        | **캐시에 저장 금지**. 민감한 정보에 사용                 | 로그인 정보, 결제 정보 등 → 절대 저장 금지         |
| `must-revalidate` | 캐시가 만료되면 **반드시 서버 검증**, 실패 시 오류 반환        | 오프라인일 때라도 만료된 캐시 사용 못 함            |
| `public`          | **모든 사용자/프록시/CDN**가 캐시 가능                 | CDN, Reverse Proxy 등 중간 서버에서 캐시 가능 |
| `private`         | \*\*브라우저(개인 사용자)\*\*만 캐시 가능. 중간 서버는 캐시 불가 | 로그인 후 개인화된 페이지 등                   |
| `s-maxage=초`      | **프록시 캐시에만 적용되는 max-age**                 | CDN, 캐시 서버에서만 `max-age` 덮어쓰기 가능    |

### 1.2. Expires
```shell
Expires: Wed, 21 Oct 2025 07:28:00 GMT
```
* HTTP/1.0 방식으로, 응답이 언제 만료되는지 절대 시간을 명시
* 클라이언트에서는 현재 시각과 비교해서 만료 여부 판단
* 클라이언트와 서버의 시간 동기화 문제가 있을 수 있음
* `Cache-Control: max-age 지시어`에 의해 덮어쓰기 됨


### 1.3. Pragma
* 과거용 `no-cache`


<br></br>

## 2. HTTP 캐시 동작
```http
GET / HTTP/1.1
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,...
Accept-Encoding: gzip, deflate, br, zstd
Accept-Language: ko-KR,ko;q=0.9,en-US;q=0.8,en;q=0.7
👉 Cache-Control: no-cache
Connection: keep-alive
Cookie: ck_img_view_cnt=4; PHPSESSID=edb7a6643df73febe0b4cb6f18e4df43; ...
Host: www.dcinside.com
👉 Pragma: no-cache
Referer: https://www.google.com/
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) ...
```
* HTTP 캐시는 <U>최대한 서버에 요청을 안 보내면서도 최신 데이터를 유지하려는 구조</U>를 가짐
* 이를 위해 <U>먼저 캐시가 아직 유효한지 검사하고, 필요할 땐 서버에 검증 요청</U>을 보냄


### 2.1. 캐시 신선도 검사
![alt text](<../설명사진/[응용 계층] HTTP 캐시 신선도 검사.png>)
```http
GET / HTTP/1.1
Accept: text/html
Cache-Control: max-age=600 👉 응답은 최대 600초 동안 신선하다고 간주
``` 
* 클라이언트(브라우저나 중간 서버)가 캐시된 응답이 여전히 유효한지 스스로 판단하는 단계
* 서버까지 확인하지 않고 로컬에서 판단
    * 판단 기준은 `Expires`, `Cache-Control: max-age`, `Age` 등과 같이 **캐시 만료 시점 정보를 포함한 헤더**
* 만약 클라이언트가 보기엔 캐시가 "오래되지 않았다"고 판단되면, **서버 요청 없이 캐시를 그대로 사용**
* 신선하지 않다고 판단되면 다음 단계인 `검증`으로 넘어감


### 2.2. 검증

```http
GET / HTTP/1.1
Cache-Control: no-cache             👉 반드시 서버에 확인 요청
If-None-Match: "abc123etag"        👉 ETag 값 기반으로 검증
If-Modified-Since: Wed, 02 Aug 2025 12:00:00 GMT 👉 최종 수정 시각 기준 검증
```
* 클라이언트가 캐시된 응답이 신선하지 않다고 판단하거나, `Cache-Control: no-cache`처럼 서버 확인이 명시된 경우, 서버에 재요청을 보냄
* 이때 조건부 요청 헤더`(If-None-Match, If-Modified-Since)`를 포함해서 서버에 "이 캐시 아직 쓸 수 있음?"이라고 검증하는 과정
* 서버는 클라이언트의 조건을 보고, 자원의 상태에 따라 다음과 같이 응답함
    1. 요청 받은 자원이 변경됨
    ![alt text](<../설명사진/[응용 계층] HTTP 검증 (1) 요청 받은 자원이 변경됨.png>)
    2. 요청 받은 자원이 변경되지 않음
    ![alt text](<../설명사진/[응용 계층] HTTP 검증 (2) 요청 받은 자원이 변경되지 않음.png>)
    3. 요청 받은 자원이 삭제됨
    ![alt text](<../설명사진/[응용 계층] HTTP 검증 (3) 요청 받은 자원이 삭제되어 존재하지 않음.png>)


