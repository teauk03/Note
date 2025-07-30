# HTTP/0.9
* 초창기의 HTTP 버전 (1991년도에 도입)
* **사용가능한 메서드:** `GET/파일명`
* **요청 메세지:** 한줄 
* **응답:** HTML
* 헤더도 없고 상태코드도 없었음

<br></br>

# HTTP/1.0
* HTTP/0.9를 개선, 확장한 버전 (1996년도에 도입)
* **사용가능한 메서드:*  `GET`, `POST`, `HEAD`
* 상태 코드 도입 
* 응답 헤더 도입 (Content-Type, Content-Length, Date, ...)
```shell
// 요청 
GET /index.html HTTP/1.0
Host: www.example.com
User-Agent: Mozilla/5.0

// 응답
HTTP/1.0 200 OK
Content-Type: text/html
Content-Length: 1234

<html> ... </html>
```
* <U>매 요청마다 TCP 연결을 새로 열고 닫아야 함</U> (성능 저하)
    * 매 요청마다 이 과정을 반복해야함 `(3,4 Way Hand Shake)` ![alt text](<../설명사진/[TCP] 3,4wayhandshake.png>)
* Host 헤더 미지원 
 
 <br></br>

 # HTTP/1.1
* 가장 널리 사용되는 HTTP 버전 (1997년도 도입)
    * 지금까지도 기본으로 쓰임


| 추가된 기능                            | 설명                                                   |
| ----------------------------- | ---------------------------------------------------- |
| 지속 연결 (Persistent Connection) | `Connection: keep-alive`로 **한 TCP 연결에서 여러 요청/응답 처리** |
| 파이프라이닝 (Pipelining)           | 여러 요청을 병렬로 전송해 응답 시간 줄임                              |
| **Host 헤더 필수화**               | 하나의 IP로 여러 도메인 처리 가능                                 |
| **캐싱 기능 강화**                  | `Cache-Control`, `ETag`, `If-Modified-Since`         |
| **Chunked Transfer Encoding** | 콘텐츠 길이를 모를 때도 전송 가능                                  |
| 추가된 메서드                       | `PUT`, `DELETE`, `OPTIONS`, `TRACE`                  |
 
 <br></br>

# HTTP/2
* 성능 중심의 큰 개선 버전 (2015년 공식 표준화)
*  HTTP/1.1을 보완하고 개선하기 위해 만들어짐

| 추가된 기능                   | 설명                                |
| -------------------- | --------------------------------- |
| 바이너리 프레이밍 계층         | 요청/응답을 **텍스트 → 바이너리**로 전송         |
| 멀티플렉싱 (Multiplexing) | **하나의 연결에서 여러 요청/응답 병렬 처리**       |
| 서버 푸시 (Server Push)  | 서버가 클라이언트가 요청하지 않은 리소스를 **미리 전송** |
| 헤더 압축 (HPACK)        | 요청/응답 헤더를 효율적으로 압축하여 전송           |

 <br></br>

# HTTP/3
* 최신 HTTP 버전 (2022년 RFC 9114로 표준화)
* 기존의 TCP → QUIC(UDP 기반 전송 프로토콜)로 전환

| 추가된 기능             | 설명                                   |
| -------------- | ------------------------------------ |
| **QUIC 기반 전송** | TCP 대신 **UDP 위에 암호화된 QUIC 사용**       |
| 0-RTT 연결 설정    | 서버에 대한 **이전 연결 정보가 있으면 즉시 전송 가능**    |
| 진정한 멀티플렉싱      | HTTP/2의 **헤드 오브 라인 블로킹 문제 완전 해소**    |
| TLS 1.3 내장     | **보안 통신이 기본** (TLS 내장된 QUIC로 자동 암호화) |
