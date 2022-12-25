# Thread

### 자바에서 쓰레드에 대해서 설명하라.

- JVM이 시작되면 하나의 자바 프로세스가 시작된다.
- 작업을 동시 수행할때 다중 쓰레드를 실행하여 동시에 실행 할 수 있다.

### 쓰레드를 생성하는 방법은 무엇인가?

- 쓰레드를 생성하는 방법은 Runnable 인터페이스를 사용하거나 혹은 Thread 클래스를 사용하는 것 이다.
- 첫번째 과정은 Runnable을 구현한 클래스에 run()메소드를 구현 후 Thread.start()로 시작해 준다.
- Thread 클래스는 Thread 클래스를 상속한 클래스에 run()메소드를 오버라이드 후  객체 생성 후 start()로 시작해주면 된다.

### sleep()메소드는 무엇인가?

- 매개변수로 넘어온 값만큼 쓰레드를 대기하게 한다.
- 항상 try-catch로 묶어 주어야 한다.

### Thread 클래스의 주요 메소드는 무엇인가?

- getId(), getName()를 통해서 쓰레드 id와 name을 리턴 받는다.
- setName(), setDaemon()을 통해서 쓰레드 이름과 데몬유무를 설정할 수 있다.
- getStackTrace(), getState()을 통해 쓰레드이 스택정보와 상태를 확인할 수 있다.

### 데몬쓰레드란 무엇인가?

- setDaemon()메소드를 통해 데몬여부를 지정할 수 있다.
- 데몬 쓰레드는 해당 쓰레드의 수행 여부와 상관없이 JVM이 끝날 수 있다.

### synchronized란 무엇인가?

- 여러 쓰레드가 한 객체에 선언된 메소드에 접근하여 데이터 처리 시 값이 꼬이는 경우가 발생할 수 있다.
- 이러한 경우를 방지하기 위해서 synchronized를 선언한다.

### synchronized를 선언하는 방법은?

- 메소드 자체를 synchronized로 선언하는 synchronized methods와
- 메소드 내의 특정 문장만 synchronized로 감싸는 방법인 synchronized statments가 있다.

### synchronized 사용 시 유의점은?

- 여러 쓰레드에서 하나의 객체에 있는 인스턴스의 변수를 동시에 처리할 떄 발생하는 문제를 해결 하는 것이므로 변수가 선언된 객체가 다른 쓰레드에 공유할 일이 없으면 사용할 이유가 없다.

### 쓰레드의 상태를 통제하는 메소드는 무엇인가?

- 쓰레드의 상태를 확인하는 getState() 메소드
- 수행중인 쓰레드가 중지할 때 까지 대기하는 join() 메소드  등이 있다.

### 쓰레드의 상태는 무엇인가?

- 쓰레드의 상태로는 여러가지가 있다.
- 쓰레드 객체는 생성되었지만, 시작되지 않은 상태인 NEW
- 쓰레드가 실행중인 상태인 RUNNABLE
- 쓰레드가 실행 중지 상태이며, 모니터 락이 풀리기 기다리는 상태인 BLOCKED
- 쓰레드가 대기중인 상태인 WAITING
- 쓰레드가 종료된 상태인 TERMINATED 등이 있다.

### Object 클래스의 쓰레드 관련 메소드는 무엇인가?

- Object클래스에는 쓰레드를 통제하는 메소드가 몇개 있다.
- 다른 쓰레드가 Object 객체에 대한 notify() 메소드를 호출할 때 까지 현재 쓰레드를 대기하는 wait()메소드
- Object 객체의 모니터에 대기하고 있는 단일 쓰레드를 깨우는 notify()메소드가 있다.

### ThreadGroup이란 무엇인가?

- 쓰레드 그룹은 서로 관련된 쓰레드를 그룹으로 다루기 위한 것이다.
- 쓰레드 생성자를 통해서 그룹에 포함이 가능하다.
- 주요 메소드로는 activeCount()와 getThreadGroup()가 있다.