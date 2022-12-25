# Socket

### Socket을 이용해서 TCP통신하는 방법은 무엇인가?

- 소켓 통신을 위해서는 데이터를 받는 서버와 전송하는 클라이언트 이렇게 두가지의 소스가 필요하다.
- 먼저 서버에서는
- ServerSocket 객체를 생성하여 데이터를 받을 객체를 생성한다.
- 그 후 해당 객체의 ServerSocket메소드인 accept()를 통해서 원격호출을 대기상태로 만든 후 연결이 완료되면 Socket 객체를 리턴받는다.
- 그후 리턴받은 Socket객체의 메소드인 getInputstream()메소드를 통해 InputStream객체를 리턴받는다. 단 데이터를 전송하려면 getOutPutStream() 메소드를 통해 OutputStream 객체를 전달하면된다.
- 마지막으로 Socket 객체와 ServerSocket 객체 모두  close()메소드를 통해 종료하면된다.
- 클라이언트 측에서는
- 먼저 Socket 객체를 IP와 포트를 설정하여서 서버와 동일하게 설정하여 객체를 생성한다.
- 그후 getOutPutStram()메소드를 통해  OutputStream객체를 생성 하여 데이터를 서버에 전송한다.
- 마지막으로 Socket 객체를 close()메소드로 종료해준다.

### Socket을 이용해서 UDP통신하는 방법은 무엇인가?

- 우선 서버측에서는
- DatagramSocket 객체 포트설정과 함께 생성한다.
- 그 후 데이터를 받을 DatagramPacket객체를 생성한다.
- 그후 생성한 DatagramSocket 객체의 receiver()메소드를 통해 데이터를 받은 후 DatagramPacket 객체에 데이터를 담든다.
- close()통해 서버를 종료한다.
- 클라이언트 측에서는
- 우선 DatagramSocket객체를 생성한다.
- 그후 InetAddress클래스를 통해 서버의 ip를 설정해준다.
- 그후 전송하 데이터인 DatagramPacket 패킷을 생성해준후 send()메소드를 통해 전송해준다.
- close()메소드를 통해 소켓 연결올 종료한다.
