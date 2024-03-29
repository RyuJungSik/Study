# API

### API의 의미는?

- API는 둘 이상의 컴퓨터 프로그램이 서로 통신하는 방법이자 컴퓨터 사이에 있는 중계 계층을 의미한다.
- 즉, 요청과 응답사이에 어떠한 데이터를 주고 받을 건지 등에 대한 방법이 정의된 중계계층이다.

### API의 장점은?

- 제공자는 DB관련 정보와 같은 서비스의 중요한 부분을 드러내지 않아도 된다.
- 사용자는 해당 서비스가 어떻게 구현되었는지 알 필요없이 필요한 정보만 받을 수 있다.
- open API 사용 시 개발에 간편성을 높힌다.
- 제공자는 내부 프로세스가 수정되어도 API는 수정이 안되게 할 수 있고, 사용자한테 영향을 주지 않고 변경을 할 수 있다.
- 제공자는 데이터를 한곳에 모을 수 있다.
- 제공자는 API를 이용해 제 3자가 만든 앱을 통해 데이터를 수집할 수 있다.

### API의 종류는?

- private : 내부적으로 사용된다. 주로 해시키를 하드코딩해서 이를 기반으로 서버간의 통신을 한다.
- public : 모든 사람이 사용 가능하다. 트래픽 방지를 위해 요청수 제한을 하기도 한다.
