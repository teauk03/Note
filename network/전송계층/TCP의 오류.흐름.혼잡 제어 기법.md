# TCP의 데이터 송수신 과정에서의 오류, 흐름, 혼잡 제어
![alt text](<../설명사진/[전송계층] TCP 통신 단계.png>)
* 이 페이지에선 **(2) 과정**을 좀 더 자세히 학습할 것
* TCP의 신뢰성을 보장하는 방법 즉, <U>오류를 어떻게 제어하고 처리하는지에 대해,,</U>

<br></br>

## 1. 오류 검출
![alt text](<../설명사진/[전송 계층] TCP 세그먼트 헤더 구조.png>)
* TCP의 세그먼트 헤더에선 체크섬이란 필드가 존재하는데, 손실된 여부만 표시됨
* 신뢰성을 보장하기 위해선 `두 가지`의 조건이 필요
    1. 송신된 세그먼트에 문제가 있음을 알 수 있어야 함
    2. 오류를 감지하면 다시 재전송할 수 있어야 함


### 1.1. 중복된 ACK 세그먼트를 수신했을 때
![alt text](<../설명사진/[전송 계층] 중복 ACK1.png>)
* 수신자는 세그먼트를 순서대로 받기를 원함
* 만약에 송신 중에 특정 세그먼트가 손실되면, 그 이후 세그먼트를 받아도 이전 번호의 ACK 응답만 계속 보냄 
    * 이 문제를 `중복 ACK`이라고 부름
    ![alt text](<../설명사진/[전송 계층] 중복 ACK2.png>)

<br></br>

### 1.2. 타임 아웃이 발생 했을 때
![alt text](<../설명사진/[전송 계층] TCO- 타임아웃.png>)
* 송신자가 세그먼트를 보냈지만 일정 시간 동안 수신 측에서 ACK을 받지 못하면 패킷이 손실 되었다고 판단하게 됨
    * TCP로 송수신 하는 호스트는 모두 `재전송 타이머`라는 값을 유지하게 됨
    * 여기서 일정 시간이 끝난 상황을 `타임 아웃(time out)`이라고 부름
* 이 경우 다시 해당 패킷을 재전송하는 매커니즘을 가짐


<br></br>

## 2. 검출된 오류를 통한 재전송 기법 (ARQ)
![alt text](<../설명사진/[전송 계층] ARQ 종류.png>)
* **ARQ 기법:** `ACK`과 `타임 아웃` 발생을 기반으로 문제를 진단, <U>재전송 매커니즘으로 TCP 통신의 신뢰성을 확보</U>하는 기법

<br></br>

### 2.1. Stop-and-Wait ARQ
*  가장 단순한 ARQ 방식
* 하나의 세그먼트를 보내고, 해당 세그먼트에 대한 ACK을 받을 때까지 기다림
* ACK이 도착하면 다음 세그먼트를 보내는 매커니즘을 사용
* ACK이 일정 시간 내 도착하지 않으면 그때 재전송함
* 굉장히 느림

<br></br>

### 2.2. Go-Back-N ARQ
![alt text](<../설명사진/[전송 계층] ARQ (Go-Back-N).png>)
* 여러개의 세그먼트를 윈도우 크기만큼 연속으로 전송이 가능한 방식
    * `파이프라이닝(pipelining)`: 연속해서 메세지를 전송하는 기술
* 수신자가 중간에 하나라도 손실되면 모든 세그먼트를 무시함
* 송신자는 손실된 세그먼트 다음으로 모든 데이터를 다시 전송하는 메커니즘을 사용
* 이런 특징 때문에 `Go-Back-N ARQ의 ACK 응답`을 `누적 확인 응답`이라고 부름
<br></br>

### 2.3. Selective Repeat ARQ
![alt text](<../설명사진/[전송 계층] Selective Repeat ARQ .png>)
* 선택적으로 재전송할 수 있는 방식을 사용한 ARQ
* `버퍼링`을 지원함
    * **버퍼링:** 수신자가 손실된 세그먼트만 지정해서 요청하고, 정산 수신된 세그먼트는 저장 
* **TCP가 실제로 동작하는 방식**

<br></br>

## 3. 흐름 제어 
* 송신자가 너무 빠르게 데이터를 보내어 수신자가 `수신 버퍼`가 넘치는 `버퍼 오버 플로우`를 일으키지 않도록 <U>데이터의 흐름을 조절하는 방식</U>
    * **수신 버퍼:** 수신된 세그먼트가 프로세스에 읽히기 전에 임시로 저장되는 공간
    * **버퍼 오버플러우(buffer overflow):** 수신 버퍼가 가득한 문제 상황


<br></br>

### 3.1. 슬라이딩 윈도우 (Sliding Window)
![alt text](<../설명사진/[전송 계층] 슬라이딩 윈도우 동작.png>)
* 송신자가 수신자의 처리 능력에 <U>맞춰서 데이터를 조절해서 보내</U>는 방식
* **윈도우(window):** 송신 호스트가 한번에 전송할 수 있는 세그먼트의 범위 

<br></br>

## 4. 혼잡 제어
![alt text](<../설명사진/[전송 계층] 혼잡 제어.png>)
* 네트워크를 통해 전송되는 데이터의 양이 많아질수록, 트래픽이 증가하게 되고 이로 인해 네트워크 장비가 처리 용량을 초과하게 되면 `혼잡(Congestion)`이 발생함
* 혼잡이 발생하면 패킷 지연, 손실, 재전송이 일어나 네트워크 성능이 저하됨
* TCP는 이러한 네트워크 혼잡 상황을 완화하고자 여러가지 <U>혼잡 제어 알고리즘</U>을 사용함
![alt text](<../설명사진/[전송 계층]. 혼잡 제어 알고리즘 표.png>)

<br></br>

### 4.1. 느린 시작 알고리즘 
![alt text](<../설명사진/[전송 계층]느린시작 알고리즘.png>)
* 처음부터 작은 크기의 혼잡 윈도우로 시작하여 혼잡이 없는 것으로 판단되면 <U>지수적으로 혼잡 윈도우의 크기를 증가 시키는 알고리즘</U>
    * **혼잡 윈도우란?:** 혼잡 없이 전송할 수 있을 법한 데이터의 양
* 혼잡 윈도우가 계속 증가하다가 혼잡한 상황이 발생하면 다음과 같은 방법 중에 하나를 선택하여 해결할려고 시도함
    1. **타임 아웃이 발생한 상황**
        * 혼잡 윈도의 값을 1로 지정, 혼잡 윈도의 값을 절반으로 초기환한 후 느린 시작 다시 시작
    2. **혼잡 윈도우 >= 느린 시작 임계치 일 경우**
        * 느린 시작을 종료함
        * 혼잡 회피를 수행
    3. **3번의 중복 ACK 발생 시**
        * 빠른 회복 수행

<br></br>

### 4.2. 혼잡 회피 알고리즘
* 느린 시작 알고리즘과는 다르게 <U>혼잡 윈도우 크기를 점진적으로 증가시켜 혼잡을 예방하는 알고리즘</U>
* 느린 시작 임계치를 넘어간 시점부터 혼잡 발생을 우려하여 조금씩 조심히 혼잡 윈도우를 증가 시킴

<br></br>

### 4.3. 빠른 회복 알고리즘
* 3번의 중복 ACK을 받으면 실행되는 알고리즘
* 느린 시작을 거치지 않고 바로 혼잡 회피 단계로 복귀함


