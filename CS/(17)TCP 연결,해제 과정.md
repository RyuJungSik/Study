### TCP 연결성립과정 - 3웨이 핸드쉐이크 과정

![image](https://user-images.githubusercontent.com/76714485/230780103-f62a7194-81a2-458d-b9db-d4eb64bfab5b.png)

- 1 . SYN단계 → 클라이언트는 서버에 클라이언트의 ISN을 담아 SYN을 보낸다.
- 2 . SYN + ACK단계 → 서버는 SYN수신 후 서버의 ISN과 승인번호로 클라이언트의 ISN+1을 ACK로 보낸다.
- 3 . ACK 단계 → 클라이언트는 ISN+1의 값을 ACK에 담아서 서버로 보낸다.

### ISN, SYN, ACK

- ISN
    - ISN은 TCP기반 데이터 통신에서 각각의 새 연결에 할당된 고유한 32비트 시퀀스 번호를 나타낸다.
    - TCP 연결을 통해 전송되는 다른 데이터 바이트와 충돌하지 않는 시퀀스번호를 할당하는 도움이 된다.
- SYN
    - 연결요청 플래그이다.
- ACK
    - 응답 플래그이다.

### 클라이언트와 서버의 상태

![image](https://user-images.githubusercontent.com/76714485/230780065-38e5a052-e659-412e-a5c7-5e755f501ab6.png)

- 클라이언트가 SYN을 보내면서 클라이언트는 SYN-SENT상태가 된다.
- 서버는 LISTEN상태에서 클라이언트한테 SYN을 받을 수 있다.
- 서버는 SYN을 받으면 SYN-RECEIVED상태가 되고 SYN+ACK를 클라이언트에 보낸다.
- 클라이언트는 SYN+ACK를 받고 ESTABLISHED 상태가 되고 ACK를 보낸다.
- 그후 서버는 ACK를 받고 ESTABLISHED상태가 된다.

---

### TCP 연결 해제 과정 - 4웨이 핸드쉐이크
![image](https://user-images.githubusercontent.com/76714485/230780114-9a94a7a2-9531-458f-88d9-5c097f831392.png)

- 1 . 클라이언트가 연결을 닫을때 FIN 세그먼트를 보낸다.
- 2 . 그후 클라이언트는 FIN_WAIT_1 상태로 들어가고 서버의 응답을 기다린다.
- 3 . 서버는 클라이언트로 ACK라는 승인 세그먼트를 보낸다.
- 4 . 서버는 그 후 CLOSE_WAIT상태에 들어간다.
- 5 . 클라이언트는 ACK 세그먼트를 받으면 FIN_WAIT_2상태가 된다.
- 6 . 서버는 LAST_ACK상태가 되며 클라이언트에 FIN세그먼트를 보낸다.
- 7 . 클라이언트는 TIME_WAIT 상태가 되고 다시 서버로 ACK보내서 서버는 CLOSED가 되고 클라이언트는 TIME_WAIT시간 후 CLOSED상태가 된다.

### TIME_WAIT란?

- 지연패킷이 발생했을 때 데이터 무결성을 해결하기위한 수단이다.
- 최대 세그먼트수면의 두배를 기다린다.
