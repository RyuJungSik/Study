# DI와 DIP

### DI란?
![image](https://user-images.githubusercontent.com/76714485/227937978-735fc583-5e67-4938-b02a-a789329059dd.png)

- DI란 의존성 주입으로 메인모듈이 직접 다른 하위 모듈에 대한 의존성을 주기보다는
- 중간에 의존성 주입자가 이부분을 가로채 메인 모듈이 간접적으로 의존성을 주입하는 방식이다.
- 메인 모듈과 하위모듈간의 의존성을 느슨하게만들어 모듈 교체에 유리하다.

### DI 적용하지 않은 코드와 적용한 코드 사례

- 적용하지 않은 코드
- 프로젝트모듈이(메인) BEdev와 FEdev에 의존하는 관계

```java
import java.util.*;

class BackendDeveloper { 
 public void writeJava() {
  System.out.println("hello java"); }
 }
class FrontEndDeveloper {
 public void writeJavascript() {
  System.out.println("hello js"); }
 }

public class Project {
 private final BackendDeveloper backendDeveloper; 
 private final FrontEndDeveloper frontEndDeveloper;

 public Project(BackendDeveloper backendDeveloper, FrontEndDeveloper frontEndDeveloper) {
  this.backendDeveloper = backendDeveloper;
  this.frontEndDeveloper = frontEndDeveloper; 
 }
 public void implement() 
  backendDeveloper.writeJava(); 
  frontEndDeveloper.writeJavascript();
 }
 public static void main(String args[]) {
  Project a = new Project(new BackendDeveloper(), new FrontEndDeveloper());
  a.implement();
 } 
}
```

- DI적용시 developer라는 모듈이 추가되며 developer는 프로젝트, BEdev, FEdev를 의존한다.

```java
import java.util.*;

interface Developer { 
 void develop();
}

class BackendDeveloper implements Developer { 
 @Override
 public void develop() { writeJava(); }
 public void writeJava() {System.out.println("hello java"); }
}

class FrontendDeveloper implements Developer {
 @Override
 public void develop() { writeJavascript();}
 public void writeJavascript() { System.out.println("hello js");} 
}

public class Project {
 private final List<Developer> developers;
 
 public Project(List<Developer> developers) { this.developers = developers;}
 
 public void implement() { developers.forEach(Developer::develop);}
 
 public static void main(String args[]) { 
  List<Developer> dev = new ArrayList<>(); 
  dev.add(new BackendDeveloper());
  dev.add(new FrontendDeveloper());
  Project a = new Project(dev); 
  a.implement();
 } 
}
```

### 의존관계 역전 원칙

- 의존성 주입할때 발적용된다.
- 상위모듈이 하위모듈에 의존해선 안되고 둘다 추상화에 의존해야 한다.
- 추상화는 세부사항에 의존해서는 안되고, 세부 사항은 추상화에 따라 달라져야 한다.

### 의존성 주입의 장점

- 외부 모듈에서 생성하여 집어넣는 구조이기 때문에 모듈 교체가 쉽다.
- 단위테스트, 마이그레이션이 쉬워진다.
- 애플리케이션 의존성 방향이 일관되어서 코드 추론이 쉬워진다.

### 의존성 주입의 단점

- 결국 모듈이 더 생기게 되면서 복잡도가 증가한다.
- 종속성 주입이 런타임에 일어나기 떄문에 컴파일때 종속성 주입에 관한 오류를 잡기 힘들다.
