# 데이터포맷 JSON

### JSON은?

- JSON은 javaScript 객체 문법으로 구조화된 데이터를 표현하기 위한 표준 포맷이다.
- 타입으로는 undefined를 제외한 문자열, 숫자, 배열, 불리언 객체를 포함할 수 있다.
- Key : value 형태로 나타낸다.

### JSON 예시

```json
const a =
{
 "MusicList" : [
  {
   "name" : "ASong",
   "artist" : "AMan"
  },
 {
   "name" : "BSong",
   "price" : 1000
  }
 ]
}

console.log(a.MusicList[0].name);
console.log(a.MusicList[1]["price"]);
```

- 예시처럼 JSON안에는 배열도 가능하고, 객체의 타입이 달라도 된다.
- JS에서 JSON데이터 접근 시 대괄호 혹은 온점으로 접근가능하다.

### JSON 작성시 주의할점

- undefined사용불가하다.
- key - value 포맷 준수해야한다.
- 큰따옴표만 사용해야한다.

### JSON의 장점은?

- 텍스트 형태여서 읽기 쉽다.
- 프로그래밍 언어와 플랫폼에 독립적이여서 서로다른 시스템간에 객체를 교환하기 좋다.
- 주로 API, config파일에 활용된다.
