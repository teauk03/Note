# IP
* 네트워크 계층의 가장 핵심적인 `인터넷 프로토콜 (Internet Protocol)`
* 인터넷 네트워크 상에서 각각의 장치를 식별하기 위한 고유한 논리 주소

<br></br>

# 종류
## 1. **IPv4**
* 가장 많이 사용함
* **형태:** xxx.xxx.xxx.xxxx 
    * x로 표시된 0~255 범위의 10진수를 옥텟(octet) 이라고 함
* 약 43억 개의 주소를 제공함 (32bit)
### 1.1. IPv4 헤더 구조
![alt text](<../설명사진/IPv4 패킷 헤더.png>)
* 데이터를 네트워크에서 전송할 떄, 이와 같은 헤더 정보를 붙여서 패킷을 만듬
* 이 헤더는 수신자가 누구며 어디서 왔고 어떤 방식으로 처리해야하는지 등을 표시하는 주소 
라벨임

| 필드                    | 설명                                                                                                                                                                       |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **버전 (4bit)**         | IP 버전 (IPv4는 항상 4)                                                                                                                                                       |
| **헤더 길이 (4bit)**      | IPv4 헤더의 전체 길이 (4바이트 단위)                                                                                                                                                 |
| **서비스 유형 (8bit)**     | 데이터의 우선순위나 처리 방식 지정 (QoS)                                                                                                                                                |
| **패킷 길이 (16bit)**     | 전체 IP 패킷의 크기 (헤더 + 데이터 포함)                                                                                                                                               |
| **식별자 (16bit)**       | 패킷을 구분하는 고유 ID (조각화 관련)                                                                                                                                                  |
| **플래그 (3bit)**        | 패킷 조각화 관련 설정 <br> ↳ **3개의 비트 중 2개를 주요하게 사용함**  <br> ▸ **DF (Don't Fragment):** 조각화를 **금지**함. <br> ▸ **MF (More Fragments):** 이 패킷 뒤에 **더 많은 조각이 있음**을 의미. 마지막 조각은 MF = 0 |
| **단편화 오프셋 (13bit)**   | 조각화된 패킷들의 순서를 맞추기 위한 오프셋 값                                                                                                                                               |
| **TTL (8bit)**        | Time To Live – 패킷이 네트워크를 몇 번 통과할 수 있는지 (hop:패킷이 호스트나 라우터에 전달되는 수), 0이 되면 폐기                                                                                                                 |
| **프로토콜 (8bit)**       | 상위 계층 프로토콜 지정 (예: 6 = TCP, 17 = UDP)                                                                                                                                     |
| **헤더 체크섬 (16bit)**    | 헤더의 오류 검출용                                                                                                                                                               |
| **송신지 IP 주소 (32bit)** | 발신자의 IP 주소                                                                                                                                                               |
| **수신지 IP 주소 (32bit)** | 수신자의 IP 주소                                                                                                                                                               |
| **옵션 (0\~40byte)**    | 특별한 목적에 따라 사용, 대부분 비워둠                                                                                                                                                   |
| **패딩**                | 옵션 필드를 32bit 단위로 맞추기 위한 채움                                                                                                                                               |
| **데이터**               | 실제 전송하고자 하는 내용 (ex. TCP, UDP, 애플리케이션 데이터 등)                                                                                                                              |

<br></br>

## 2. **IPv6**
* IPv4의 주소 부족 문제를 해결하기 위해 만들어짐
* **형태:** 2001:0db8:85a3:0000:0000:8a2e:0370:7334 (16진수, 총 128bit)
* 숫자로 세기 힘들 정도의 거의 무한한 수의 주소를 제공함
### 2.1. IPv6 헤더 구조
![alt text](<../설명사진/IPv6 헤더.png>)
* 효율성과 속도를 고려하여 고정된 40byte 크기의 헤더 구조를 가짐
* IPv4에서 사용하던 옵션, 조각화 등의 기능은 확장 헤더로 분리함

| 필드                                  | 크기     | 설명                                          |
| ----------------------------------- | ------ | ------------------------------------------- |
| **버전 (Version)**                    | 4bit   | IP 프로토콜 버전 (항상 6)                           |
| **트래픽 클래스 (Traffic Class)**         | 8bit   | 패킷의 우선순위 지정 (QoS 기능)                        |
| **플로우 라벨 (Flow Label)**             | 20bit  | 같은 흐름(연결)에 속한 패킷을 식별                        |
| **페이로드 길이 (Payload Length)**        | 16bit  | 실제 데이터(payload) + 확장 헤더 길이                  |
| **다음 헤더 (Next Header)**             | 8bit   | 다음에 오는 헤더의 종류 (ex: TCP=6, UDP=17, 확장헤더도 가능) |
| **홉 제한 (Hop Limit)**                | 8bit   | TTL 역할. 라우터를 몇 번 통과할 수 있는지 제한               |
| **송신지 IP 주소 (Source Address)**      | 128bit | 송신자의 IPv6 주소                                |
| **수신지 IP 주소 (Destination Address)** | 128bit | 수신자의 IPv6 주소                                |

<br></br>

# 기능

1. **IP 주소 지정**
    * IP 주소를 바탕으로 송수신 대상을 지정하는 것
2. **IP 단편화**
    * 전송하고자 하는 패킷의 크기가 최대 전송 단위보다 클 경우 크기 이하의 복수 패킷으로 나누는 것
    * 이 최대 크기를 `MTU`라고 부름
