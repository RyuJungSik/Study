# flux 패턴

### flux 패턴 이란?

![image](https://user-images.githubusercontent.com/76714485/230365908-085e8bd0-a4ff-4a60-a4c2-a9d7f32836ae.png)

- MVC의 뷰와 컨트롤러의 관계가 복잡해지면서 버그 수정이나 데이터 흐름파악에 어려움이 생겼다.
- 즉, 데이터를 일관성 있게 뷰에 공유하기 어려웠다.
- 그래서, 데이터가 한방향으로만 흐르게하는 flux패턴이 등장했다.
- flux패턴은 action, dispatcher, store이라는 계층이 있다.
    - action은 마우스 클릭같은 이벤트를 의미하며 이벤트 발생 시 action에 관한 객체를 dispatcher에 전달한다.
    - dispatcher은 받은 action 객체 정보를 기반으로 어떤 행위를 할것인지 정한다. 보통 로직을 미리 설정한다.
    - store는 데이터, 상태를 담고있는 계층이다.

### flux 패턴 장점과 예시는?

- 장점으로는
    - 데이터의 일관성이 증대한다.
    - 버그를 찾기가 쉬워진다.
    - 단위테스팅이 쉬워진다.
- flux패턴 예시로는 redux등이 있다.
