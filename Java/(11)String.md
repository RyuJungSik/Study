# String

### String 클래스 선언문은 어떻게 되어있는가?

- public final class String extends Object이다
- 또한 implements Serializable, Comparable<String>, CharSequence 3개의 인터페이스를 선언한다.

### String에 상속된 인터페이스에 대해서 각각 설명하면?

- Serializable은 따로 구현할 메소드는 없고, 선언 시 파일로 저장 혹은 다른 서버에 전송 가능한 상태가 된다.
- Comparable는 compareTo메소드를 선언해서 객체를 비교한다.
- CharSequence는 해당 클래스가 문자열을 다루기 위한 클래스 라는것을 명시적으로 나타낸다.

### String클래스의 생성자에 대해서 설명하라.

- String의 생성자는 매우 많이 있다. 그중 가장 자주 사용하는것은 String(byet[] bytes) 와 String(byte[] bytes, Charset charset)이다.
- 그 외에는 생성자 대신 따옴표로 묶어 생성한다.

### String 을 byte로 변환하는 법은?

- getBytes()메소드를 사용한다.
- 안정성을 위해서 매개변수로 캐릭터 셋을 명시하기도 한다.
- 존재하지 않는 종종 캐릭터 셋의 이름을 지정하는 경우 예외가 발생하 try-catch를 동반해서 사용하면 효율적이다.

### 캐릭터 셋이란 무엇인가?

- 특수문자, 한글과 같은 경우 고유의 캐릭터 셋을 갖는다.
- 한글을 처리하기 위해서 요즘은 UTF-16이 가장 보편적이다.

### 객체의 널 체크란 무엇인가요?

- 객체의널이란 초기화가 되어있지 않으며, 어떤 메소드도 사용할 수 없다는 것을 의미합니다.
- 널인 객체의 메소드를 호출하면 예외를 발생시킵니다.
- 동등 연사자를 통해 널인지 체크해주는 것이 중요하다.

### 문자열이 같은지 비교하는 메소드는 무엇인가?

- 크게 equals로 시작하는 메소드, compareTo로 시작하는 메소드, contentEquals로 시작하는 메소드 이렇게 3개의 메소드가 잇다.
- equals는 콘스턴스 풀과 상관없이 해당 문자열의 동일함을 비교해준다.
- compareTo는 알파벳 순으로 순서값 만큼 리턴해준다.
- contentEquals는 CharSequence와 StringBuffer이 String 객체와 같은지 비교하는데 사용된다.

### String 메소드를 아는대로 말해보아라.

- 해당 문자열이 있는지 확인하는 contain()
- 특정 문자열의 위치를 찾는 indexOf()
- 특정 위치의 값을 추출하는 charAt()
- 문자열의 일부 값을 잘라내는 substring()
- 문자열을 배열로 나누는 split()
- 앞 뒤 공백을 제거해주는 trim()
- 문자열 내용을 변경해주는 replace()
- 대소문자를 변경해주는 toLowerCase(), toUpperCase()
- 기본 자료형을 문자열로 변경해주는 valueOf()가 있다.

### String대신 StringBuffer과 StringBuilder을 사용하는 이유는?

- String는 immutable하다 그래서 더하는 작업을 할시 쓰레기값이 생성된다.
- 하지만 StirngBuffer, StringBuilder을 사용 시 문자열을 더해도 새로운 객체를 생성하지 않아 메모리에 이점을 취할 수 있다.
- 현재 자바같은경우 String 더하기 연산시 자동으로 StringBuilder로 변환해준다. 하지만 for과 같은 루프에서는 변환을 해주지 않아서 명시해서 사용해줘야 한다.
