# 15. let, const오 블록 레벨 스코프

## 1. var 키워드로 선언한 변수의 문제점

- ES5까지는 변수를 선언할 수 있는 유일한 방법은 var 키워드를 사용하는 것이었다.

### 1.1 변수 중복 선언 허용

```jsx
var x=1;
var y=1;

//var 키워드로 선언한 변수는 같은 스코프 내에서 중복 선언을 허용한다.
// 초기화문이 있는 변수 선언문은 자바스크립트 엔진에 의해 var 키워드가 없는 것처럼 동작한다.
var x=100;
// 초기화문이 없는 변수 선언문은 무시된다.
var y;

console.log(x);/100
console.log(y); //1
```

- 위 코드는 var키워드로 x y가 중복 선언되었다.
- var 키워드로 선언한 변수를 중복 선언하면 초기화문 유무에 따라 다르게 동작한다.
- 초기화문이 있는 변수 선언문은 자바스크립트 엔진에 의해 var 키워드가 없는 것처럼 동작하고 초기화문이 없는 변수 선언문은 무시된다.
- 위 코드는 의도치 않게 먼저 선언된 변수 값이 변경되는 부작용이 발생한다.

## 1.2 함수 레벨 스코프

- var 키워드로 선언한 변수는 오로지 함수의 코드 블록만을 지역 스코프로 인정한다.
- 따라서 함수 외부에서 var 키워드로 선언한 변수는 코드 블록 내에서 선언해도 모두 전역 변수가 된다.

```jsx
var x=1

if(true){
	//x는 전역 변수다. 이미 선언된 전역 변수 x가 있으므로, x 변수는 중복 선언된다.
	// 이는 의도치 않게 변수값이 변경되는 부작용을 발생시킨다.
	var x=10;
}

console.log(x); //10
```

- for 문의 변수 선언문에서 var 키워드로 선언한 변수도 전역 변수가 된다.

```jsx
var i=10;

// for문에서 선언한 i는 전역 변수이다. 이미 선언된 전역 변수 i가 있으므로 중복 선언 된다.
for (var i=0; i<5;i++){
	console.log(i) //0 1 2 3 4
}

//의도치 않게 i 변수의 값이 변경되었다.
conosel.log(i)//5
```

### 1.3 변수 호이스팅

- var 키워드로 변수를 선언하면 변수 호이스팅에 의해 변수 선언문이 스코프의 선두로 끌어 올려진 것처럼 동작한다.
- 즉 변수 호이스팅에 의해 var 키워드로 선언한 변수는 변수 선언문 이전에 참조할 수 있다.
- 단, 할당문 이전에 변수를 참조하면 언제나 undefined를 반환한다.

```jsx
//이 시점에는 변수 호이스팅에 의해 이미 foo변수가 언언 되었다.(1. 선언단계)
// 변수 foo는 undefined로 초기화된다.(2. 초기화 단계)
console.log(foo) //undefined

//변수에 값을 할당(3. 할당 단계)
foo=123;

console.log(foo); //123

//변수 선언은 런타임 이전에 자바스크립트 엔진에 의해 암묵적으로 실행된다.
var foo;
```

- 변수 선언문 이전에 변수를 참조하는 것은 변수 호이스팅에 의해 에러를 발생시키지는 않지만 프로그램의 흐름상 맞지 않을뿐더러 가독성을 떨어 뜨리고 오류를 발생시킬 여지를 남긴다.

## 2. let 키워드

- var 키워드의 단점을 보완하기 위해 ES6에서는 새로운 변수 키워드인 let과 const를 도입했다.

### 2.1 변수 중복 선언 금지

- var → 이름이 동일한 변수를 중복 선언하면 아무런 에러가 발생하지 않는다.
      → 이때 변수를 중복 선언하면서 값까지 할당했다면 의도치 않게 먼저 선언된 변수 값이 재할                 당되어 변경되는 부작용이 발생한다.
- let → let키워드로 이름이 같은 변수를 중복 선언하면 문법 에러가 발생한다.

```jsx
let bar=123;
//let이나 const 키워드로 선언된 변수는 같은 스코프 내에서 중복 선언을 허용하지 않는다.
let bar=456; //SyntaxError : Identifier 'bar' has already been declared
```

### 2.2 블록 레벨 스코프

- var → var로 선언한 변수는 오로지 함수의 코드 블록만을 지역 스코프로 인정하는 함수 레벨 스코프를 따른다.
- let → let 키워드로 선언한 변수는 모든 코드 블록(함수, if문, for문, while문, try/catch문 등)을 지역 스코프로 인정하는 블록 레벨 스코프를 따른다.

```jsx
let foo =1;
{
	let foo=2 //지역변수
	let bar=3 //지역변수
}
console.log(foo) //1
console.log(bar) //referenceError: bar is not defined
```

- let 키워드로 선언된 변수는 블록 레벨 스코프를 따른다.

![Untitled](https://user-images.githubusercontent.com/76714485/133446245-347a1814-4f1f-45b6-afe8-9cc9051390f2.png)


### 2.3 변수 호이스팅

- let 키워드로 선언한 변수는 변수 호이스팅이 발생하지 않는 것처럼 동작한다.

```jsx
console.log(foo); //referenceError : foo is not defined
let foo
```

- 이처럼 let 키워드로 선언한 변수를 변수 선언문 이전에 참조하면 참조 에러가 발생한다.
- 

### var

- var
→ 런타임 이전에 자바스크립트 엔진에 의해 암묵적으로 "선언 단계"와  "초기화 단계"가 한번에 진행된다.
→ 선언 단계에서 스코프에 변수 식별자를 등록해 자바스크립트 엔진에 변수의 존재를 알린다.
→ 그리고 즉시 초기화단계에서 undefined로 변수를 초기화 한다.
→ 따라서 변수 선언문 이전에 변수에 접근해도 스코프에 변수가 존재하기 때문에 에러가 발생하지 않고 undefined를 반환한다.
→ 이후 변수 할당문에 도달하면 비로소 값이 할당된다.

    ```jsx
    // var 키워드로 선언한 변수는 런타임 이전에 선언 단계와 초기화 단계가 실행된다.
    // 따라서 변수 선언문 이전에 변수를 참조할 수 있다.
    console.log(foo) //undefined

    var foo
    console.log(foo) //undefined

    foo=1 //할당문에서 할당 단계가 실행된다.
    console.log(foo); //1
    ```

    ![Untitled 1](https://user-images.githubusercontent.com/76714485/133446269-362212a5-53da-4acb-b2aa-b88db19ff58a.png)


### let

- let
→ "선언 단계"와 "초기화 단계"가 분리되어 진행된다.
→ 런타임 이전에 자바스크립트 엔진에 의해 암묵적으로 선언 단계가 먼저 실행 되지만
→ 초기화 단계는 변수 선언문이 도달했을 때 실행된다.
→ 초기화 단계 가 실행되기 이전에 변수에 접근하려고 하면 참조 에러가 발생한다.
→ let 변수는 스코프의 시작 지점부터 초기화 단계 시작 지점까지 변수를 참조할 수 없다.
→ 스코프의 시작 지점부터 초기화 지점까지 변수를 참조할 수 없는 구간을 일시적 사각지대(TDZ)라 부른다.

```jsx
// 런타임 이전에 선언 단계가 실행된다. 아직 변수가 초기화 되지 않았다.
// 초기화 이전의 일시적 사각 지대에서는 변수를 참조할 수 없다.
console.log(foo); // referenceError :  foo is not defined

let foo; // 변수 선언문에서 초기화 단계가 실행된다.
console.log(foo) // undefined

foo=1 //할당문에서 할당 단계가 실행된다.
console.log(foo) //1
```

![Untitled 2](https://user-images.githubusercontent.com/76714485/133446288-995576b8-a8c6-4bc2-9309-1b3dbc0b1e97.png)


- 결국 let 키웓로 선언한 변수는 변수 호이스팅이 발생하지 않는 것처럼 보인다. 하지만 그렇지 않다.

```jsx
let foo =1;

{
	console.log(foo) //referenceError : cannot access 'foo' before initialization
	let foo=2; //지역변수
}
```

- let  가 호이스팅이 발생하지 않는다면 위 예제는 전역 변수 foo의 값을 출력해야 한다.
- 하지만 let 키워드로 선언한 변수도 여전히 호이스팅이 발생하기 때문에 참조 에러가 발생한다.
- 자바스크립트는 ES6에서 도입된 let, const 를 포함해서 모든 선언(var, let, const, function, function*, class)을 호이스팅한다.

### 2.4 전역 객체와 let

- var 키워드로 선언한 전역 변수와 전역함수, 그리고 선언하지 않은 변수에 값을 할당한 암묵전 전역은 전역 객체 window의 프로퍼티가 존재한다.

```jsx
//브라우저 버전

//전역 변수
var x=1;
//암묵적 전역
y=2;
//전역 함수
function foo() {}

// var 키워드로 선언한 전역 변수는 전역 객체 window의 프로퍼티다.
console.log(window.x) //1
// 전역 객체 window의 프로퍼티는 전역 변수처럼 사용할 수 있다.
console.log(x) //1

//암묵적 전역은 전역 객체 window의 프로퍼티다.
console.log(window.y) //2
console.log(y) //2

//함수 선언문으로 정의한 전역 함수는 전역 객체 window의 프로퍼티다.
console.log(window.foo); //f foo() {}
//전역 객체 window의 프로퍼티는 전역 변수처럼 사용할 수 있다.
console.log(foo) //f foo() {}
```

- let 키워드로 선언한 전역 변수는 전역 객체의 프로퍼티가 아니다.
- 즉 window.foo와 같이 접근할 수 없다.
- let 전역 변수는 보이지 않는 개념적인 블록 내에 존재하게 된다.

```jsx
//이 예제는 브라우저 환경에서 실행해야 한다.
let x=1

//let, const 키워드로 선언한 전역 변수는 전역 객체 window의 프로퍼티가 아니다.
console.log(window.x) //undefined
```

## 3. const 키워드

- const 키워드는 → 상수를 선언하기 위해 사용한다.
- const 키워드와 let 키워드는 대부분 동일하므로 let 키워드와 다른 점을 중심으로 살펴보자

### 3.1 선언과 초기화

- **const 키워드로 선언한 변수는 선언과 동시에 초기화 해야한다.**
- 그렇지 않으면 문법 에러가 난다.

```jsx
const foo=1
const foo2; //SyntaxError : Missing initializer in const declaration
```

- const 키워드로 선언한 변수는 let 키워드로 선언한 변수와 마찬가지로 블록 레벨 스코프를 가지며, 변수 호이스팅이 발생하지 않는 것처럼 동작한다.

```jsx
{
//변수 호이스팅이 발생하지 않는 것처럼 동작한다.
console.log(foo) //referenceError : cannot access 'foo' before initialization
const foo=1
console.log(foo) //1
}

//블록 레벨 스코프를 갖는다.
consoel.log(foo) //ReferenceError : foo is not defined
```

### 3.2 재할당 금지

- var 또는 let 키워드로 선언한 변수는 재할당이 자유로우나 const 키워드로 선언한 변수는 재할당이 금지된다.

```jsx
const foo=1
foo=2 //TypeError : Assigment to constant variable
```

### 3.3 상수

- const 키워드로 선언한 변수에 원시값을 할당한 경우 변수 값을 변경할 수 없다.
- 원시값은 변경 불가능한 값이므로 재할당없이 값을 변경할 수 있는 방법이 없기 때문이다.
- 이러한 특징을 이용해 const 키워드를 상수를 표현하는데 사용하기도 한다.
- 상수 → 재할당이 금지된 변수

```jsx
//세전 가격
let preTaxPrice=100

//세후 가격
//0.1의 의미를 명확히 알기 어렵기 때문에 가독성이 좋지 않다.
let after=preTaxPrice + (preTaxPrice * 0,1);
console.log(after) //110
```

- 코드 내에서 사용한 0.1은 어떤 의미로 사용했는지 명확히 알기 어렵다.
- 프로그램 내에서 세율은 바뀌지 않으며 고정된 값을 사용해야 한다.
- const 키워드로 선언된 변수에 원시값을 할당한 경우 
→ 원시값은 변경할 수 없는 값이고
→ const 키워드에 의해 재할당이 금지되므로 할당된 값을 변경할 수 있는 방법이 없다.
- 일반적으로 상수의 이름은 대문자로 선언해 상수임을 명확히 나타낸다.
여런단어시 _ 로 구분해서 스테이크 케이스로 표현하는것이 일반적이다.

```jsx
// 세율을 의미하는 0.1은 변경할 수 없는 상수로서 사용될 값이다.
// 변수 이름을 대문자로 선언해 상수임을 명확히 한다.
const TAX_RATE=0.1

//세전 가격
let preTaxPrice=100

//세후 가격
//0.1의 의미를 명확히 알기 어렵기 때문에 가독성이 좋지 않다.
let after=preTaxPrice + (preTaxPrice * TAX_RATE);
console.log(after) //110
```

### 3.4 const 키워드와 객체

- const 키워드로 선언된 변수에 객체를 할당한 경우, 값을 변경할 수 있다.

```jsx
const person={
	name:'Lee'
};

//객체에는 변경 가능한 값이다. 따라서 재할당없이 변경이 가능하다.
person.name='Kim'

console.log(person) //{name : "Kim"}
```

- const 키워드는 재할당을 금지할 뿐 "불변"을 의미하지 않는다.
- 즉, 새로운 값을 재할당하는 것은 불가능하지만 프로퍼티 동적 생성, 삭제, 프로퍼티 값의 변경을 통해 객체를 변경하는 것은 가능하다.
- 이때 객체가 변경되더라도 변수에 할당된 참조값은 변경되지 않는다.

## 4. var  VS  let  VS  const

- 변수 선언에는 기본적으로 const를 사용하고 let은 재할당이 필요한 경우에 한정해 사용하는 것이 좋다.
- const 키워드를 사용하면 의도치 않은 재할당을 방지하기 때문에 좀 더 안전하다.
- var와 let 그리고const 키워드는 다음과 같이 사용하는 것을 권장한다.
    - ES6를 사용한다면 var 키워드는 사용하지 않는다.
    - 재할당이 필요한 경웨 한정해 let키워드를 사용한다. 이때 변수의 스코프는 최대한 좁게 만든다.
    - 변경이 발생하지 않고 읽기 전용응로 사용하는 원시값과 객체에는 const키워드를 사용한다.
    → const 키워드는 재할당을 금지하므로 var, let 키워드보다 안전하다.
- const를 사용후 재할당이 필요하면 그때 let로 변경하자.
