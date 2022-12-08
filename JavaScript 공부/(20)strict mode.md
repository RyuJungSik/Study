# 20. Strict mode

## 1. strict mode란?

```jsx
function foo(){
	x=10
}
foo()

console.log(x)
```

- foo 함수 내에서 선언하지 않은 x 변수에 값 10을 할당했다.
- x의 변수를 찾아야 x에 값을 할당할 수 있다.
- 때문에 자바스크립트 엔진은 x변수가 어디에서 선언되어있는지 스코프  체인을 통해 검색한다.
    - 자바스크립트 엔진은 foo함수의 스코프에서 x변수의 선언을 검색한다.
    - foo함수의 스코프에는 x변수의 선언이 없으므로 검색에 실패한다.
    - 자바스크립트엔진은 x변수를 검색하기 위해 foo함수의 컨텍스트의 상위 스코프에서 x변수의 선언을 검색한다.
- 전역 스코프에도 x 변수의 선언이 존재하지 않기 때문에 ReferenceError를 발생할거 같지만
    - **자바스크립트 엔진은 암묵적으로 전역 객체에 x 프로퍼티를 동적으로 생성한다.**
    - 이때 전역 객체의 x 프로퍼티는 마치 전역 변수처럼 사용할 수 있다.
    - **이러한 현상을 암묵적 전역이라 한다.**
- 개발자 의도와는 상관없이 발생한 암묵적 전역 → 오류를 발생할 수 있다.
    - 반드시 var, let, const 키워드를 사용하여 변수를 선언한 다음 사용해야 한다.
- ES5부터 strict mode가 추가되었다.
- strict mode → 자바스크립트 언어의 문법을 좀 더 엄격히 적용하여 오류를 발생시킬 가능성이 높거나 자바스크립트 엔진의 최적화 작업에 문제를 일으킬 수 있는 코드에 대해 명시적인 에러를 발생시킨다.
- ESLint 같은 린트 도구를 사용하여 strict mode와 유사한 효과를 얻을 수 있다.
    - 린트 도구는 정적 분석기능을 통해 소스코드를 실행하기전 문법적오류와 잠재적 오류를 찾아준다.
    - 뿐만아니라 코딩 컨벤션을 강제하기 때문에 더욱 강력한 효과를 얻는다.

## 2. strict mode의 적용

- strict mode를 적용하려면 전역의 선두 또는 함수 몸체의 선두에 'use strict'를 추가한다.

```jsx
'use strict'

function foo(){
	x=10 //referenceError : x is not defined
}
foo()
```

- 함수 몸체의 선두에 추가하면 해당 함수와 중첩 함수에 strict mode가 적용된다.

```jsx
function foo(){
	'use strict'
	x=10 //referenceError : x is not defined
}
foo()
```

- 코드 선두에 위치시키지 않으면 strict mode가 제대로 작동하지 않는다.

```jsx
function foo(){
	x=10 //에러 발생하지 않는다.
	'use strict'
}
foo()
```

## 3. 전역에 strict mode를 적용하는 것은 피하자

- 전역에 적용한 strict mode는 스크립트 단위로 적용된다.
- 스크립트 단위로 적용된 strict mode는 다른 스크립트에 영향을 주지 않고 해당 스크립트에 한정되어 적용된다.
    - strict mode와 non-strict mode .스크립트를 혼용하는 것은 오류를 발생시킬 수 있다.
    - 특히 서드파티 라이브러리를 사용하는 경우 라이브러리가 non-strict mode일 수 있다.
- 실행 함수로 스크립트 전체를 감싸서 스코프를 구분하고 즉시 실행함수의 선두에 strict mode를 적용한다.

```jsx
//즉시 실행 함수의 선두에 strict mode 적용
(function (){
	'use strict'
}())
```

## 4. 함수 단위로 strict mode를 적용하는 것도 피하자

- 어떤 함수는 strict mode를 적용하고 어떤 함수는 non-strict mode이면 바람직 하지 않다.
- 또한 모든 함수에 일일이 strict mode를 적용하는 것도 번거롭다.
- 또한 strict mode가 적용된 함수가 참조할 함수 외부의 컨텍스트에 strict mode에 적용하지 않는다면 이 또한 문제가 발생할 수 있다.
- **따라서 strict mode는 즉시 실행 함수로 감싼 스크립트 단위로 적용하는것이 바람직하다.**

## 5. strict mode가 발생시키는 에러

### 5.1 암묵적 전역

- 선언하지 않은 변수를 참조하면 ReferenceError가 발생한다.

```jsx
(fucntion(){
	'use strict'
	x=1
	console.log(x) //referenceError : x is not defined
}())
```

### 5.2 변수, 함수, 매개변수의 삭제

- delete 연산자로 변수, 함수, 매개변수를 삭제하면 SyntaxError가 발생한다.

```jsx
(function (){
	'use strict'
	var x=1
	delete x // SyntaxError : Delete of an unqualified identifier in strict mode.
	function foo(a){
		delete a; //SyntaxError : Delete of an unqualified identifier in strict mode.
	}
	delete foo //SyntaxError : Delete of an unqualified identifier in strict mode.
}())
```

### 5.3 매개변수 이름의 중복

- 중복된 매개변수 이름을 사용하면 SyntaxError를 사용한다.

```jsx
(function (){
	'use strict'
	//SyntaxError : Duplicate parameter name not allowed in this context
function foo(x,x){
	return x+x
}
console.log(foo(1,2))
}())
```

### 5.4 with 문의 사용

- with문을 사용하면 SyntaxError가 발생한다.
- with 문은 전달된 객체를 스코프 체인에 추가한다.
- with 문은 동일한 객체의 프로퍼티를 반복해서 사용할 때 객체 이름을 생략할 수 있어서 코드가 간단해지는 효과가 있지만 성능과 가독성에 나쁘므로 사용을 하지않는 것이 좋다.

```jsx
(function (){
	'use strict'
	//SyntaxError : strict mode code may not include a with statement
	with({x : 1}){
		console.log(x)
	}
}())
```

## 6. strict mode 적용에 의한 변화

### 6.1 일반 함수의 this

- strict mode에서 함수를 일반 함수로서 호출하면 this에 undefined가 바인딩 된다.
- 생성자 함수가 아닌 일반 함수 내부에서는 this를 사용할 필요가 없기 때문이다.
- 에러는 발생하지 않는다.

```jsx
(function (){
	'use strict'
	function foo(){
		console.log(this) //undefined
}
foo()

function Foo(){
	console.log(this) //Foo
{
new Foo()
}())
```

### 6.2 arguements 객체

- strict mode 에서는 매개변수에 전달된 인수를 재할당하여 변경하여도 arguments 객체에 반영되지 않는다.

```jsx
(function (a){
	'use strict'
	//매개변수에 전달된 인수를 재할당하여 변경
	a=2
	//변경된 인수가 arguments 객체에 반영되지 않는다.
	console.log(arguments) //{0 :1 , length :  1}
}(1))
```
