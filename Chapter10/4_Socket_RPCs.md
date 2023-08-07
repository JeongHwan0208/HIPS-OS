# Socket

- Client-Server System에서 통신하는 방법 중 하나

- `ID address`와 `포트(port) `로 식별

  <img src = "./img/img8.png" width = "50%">

- 자바가 제공하는 socket

  - 더 쉬운 인터페이스
  - `Socket` class : connection-oriented(TCP)
  - `DatagramSocket` class : connectionless(UDP) (방송, 브로드 캐스팅)
  - `MulticastSocket` class : multiple recipient(방송을 하는데 특정인한테만 방송하게 하는 것)





---



# RPC

- `RPC(Remote Procedure Call)` : 원격 프로시저 호출, 별도의 원격 제어를 위한 코딩 없이 다른 주소 공간에서 함수나 프로시저를 실행할 수 있게 하는 통신 기술
- 함수 vs  프로시저
  - 함수는 input에 대비한 out 발생을 목적
  - 프로시저는 결과 값에 집중하기 보단 명령 단위가 수행하는 절차를 목적
- 장점 
  - 고유 프로세스 개발에 집중이 가능
  - 프로세스 간 통신 기능을 비교적 쉽게 구현하고 정교한 제어 가능
- 단점
  - 호출 실행과 반환시간이 보장되지 않음
  - 보안이 보장되지 않음