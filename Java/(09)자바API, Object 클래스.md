# 자바API, Object 클래스

### Deprecated란 무엇인가?

- 디프리케이티드는 더이상 사용하지 않을때 명시한다.
- 컴파일 시 Warning을 띄어준다.

### Object 클래스란 무엇인가?

- Object 클래스는 모든 클래스의 부모 클래스이다.
- Object 클래스의 메소드를 통해서 모든클래스의 기본적인 행동을 정의할 수 있다.

### Object 클래스의 메소드란 무엇인가?

- 크게 객체를 처리하기위한 메소드와 쓰레드를 위한 메소드로 나뉜다.
- 객체를 처리하는 메소드로는 clone, finalize, getClass, toString, hashCode가 있다.
- 쓰레드 처리를 위한 메소드르는 notify, wait가 있다.

### toString 메소드에 대해서 설명하면?

- 호출하는 방법으론 println, 더하기 연산, 직접 호출이 있다.
- 결과로는 패키지이름 클래스이름 과 @ 그리고 객체의 해쉬코드값을 출력한다.
- 보통은 각 객체에 오버라이딩해서 사용한다.

### equals 메소드에 대해서 설명하면?

- 비교연산자로는 객체의 주소값을 비교한다.
- 각각의 성분이 동일한지 오버라이딩해서 사용한다.
- equals 메소드를 hashCode메소드도 오버라이딩 해줘야한다.
