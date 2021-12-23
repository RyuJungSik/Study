# 22. this

## 1. this 키워드

- 객체 → 상태를 나타내는 프로퍼티와 동작을 나타내는 메서드를 하나의 논리적인 단위로 묶은 복합적인 자료구조다.
- 메서드 → 자신의 속한 객체의 상태, 즉 프로퍼티를 참조하고 변경할 수 있어야 한다.
- 이때 메서드가 자신이 속한 객체의 프로퍼티를 참조하려면
- **자신이 속한 객체를 가리키는 식별자를 참조할 수 있어야 한다.**
- 객체 리터럴 방식으로 생성한 객체의 경우 메서드 내부에서 자신이 속한 객체를 가리키는 식별자를 재귀적으로 참조할 수 있다.

```jsx
const circle={
	radius :5,
	geDiameter(){
	//이 메서드가 자신이 속한 객체의 프로퍼티나 다른 메서드를 참조하려면
	//자신이 속한 객체인 circle을 참조할 수 있어야 한다.
	return 2*circle.radius
	}
}

console.log(circle.getDiameter()) //10
```

- getDiameter 메서드 내에서 메서드 자신이 속한 객체를 가리키는 식별자 circle을 참조하고 있다.
- 이 참조 표현식이 평가되는 시점은 getDiameter 메서드가 호출되어 함수 몸체가 실행되는 시점이다.
- 위 예제의 객체 리터럴은 circle 변수에 할당되기 직전에 평가된다.
- 따라서 getDiameter 메서드가 호출되는 시점에는 이미 객체 리터럴의 평가가 완료 되어 객체가 생성되었고 circle 식별자에 생성된 객체가 할당된 이후다.
- 따라서 메서드 내부에서 circle 식별자를 참조할 수 있다.
- 하지만 지가 자신이 속한 객체를 재귀적으로 참조하는 방식은 일방적이지 않고 바람직하지 않다.
- 생성자 함수 방식으로 인스턴스를 생성하는 경우 이상해진다..

```jsx
function Circle(radius){
	//이 시점에는 생성자 함수 자신이 생성할 인스턴스를 가리키는 식별자를 알 수 없다.
	???.radius=radius
}
```

- 생성자 함수 내부에서는 프로퍼티, 메서드를 추가하기 위해 자신이 생성할 인스턴스를 참조할 수 있어야 한다.
- 하지만 생성자 함수 방식으 객체 생성 방식은 생성자 함수가 정의한 이후 new 연산자와 함께 생성자 함수를 호출하는 단계가 추가로 필요하다.
- 즉, 생성자 함수로 인스턴스를 생성하려면 생성자 함수가 존재해야 한다.
- 생성자 함수를 정의하는 시점에는 아직 인스턴스를 생성하기 이전이므로 생성자 함수가 생성할 인스턴스를 가리키는 식별자를 알 수 없다. → this라는 특별한 식별자를 제공한다.
- **this →**
    - **자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 자기 참조 변수이다.**
    - **this를 통해 자신이 속한 객체 또는 자신이 생성할 인스턴스의 프로퍼티나 메서드를 참조할 수 있다.**
- 함수를 호출하면 arguments 객체와 this가 암묵적으로 함수 내부에 전달된다.
- 함수 내부에서 지역 변수처럼 사용할 수 있다.
- **단, this가 가리키는 값, 즉 this 바인딩은 함수 호출 방식에 의해 동적으로 결정된다.**
- this 바인딩 → 바인딩이란 식별자와 값을 연결하는 과정을 의미한다. 즉 this가 가리킬 객체를 바인딩 하는 것이다.

```jsx
const circle={
	radius :5,
	getDiameter(){
		return 2*this.radius
	}
};

//생성자 함수 this 사용
function Circle(radius){
	//this는 생성자 함수가 생성할 인스턴스를 가리킨다.
	this.radius=radius
	}
```

- **자바스크립트의 this는 함수가 호출되는 방식에 따라 this에  바인딩될 값, 즉 this바인딩이 동적으로 결정된다.**
- strict mode도 this 바인딩에 영향을 준다.
- this는 전역에서도 함수 내부에서도 참조할 수 있다.

```jsx
//this는 어디서든 참조 가능하다.
//전역에서 this는 전역 객체 window를 가리킨다.
console.log(this)//window

//일반 함수 내부에서는 this는 전역 객체 window를 가리킨다.
function square(number){
	console.log(this) //window
	return
}

//메서드 내부에서 this는 메서드를 호출한 객체를 가리킨다.
const person={
	name:'Lee'
	getName(){
	console.log(this)//{name:"Lee", getName:f}
	}
}

//생성자 함수 내부에서 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
function Person(name){
	this.name=name
	console.log(this)//Person {name : "Lee"}
}
```

- this는 일반적으로 객체의 메서드 내부 또는 생성자 함수 내부에서만 의미가 있다.
- string mode가 적용된 일반 함수 내부의 this에는 undefined가 바인딩 된다.

## 2. 함수 호출 방식과 this 바인딩

- **this 바인딩은 함수 호출 방식, 즉 함수가 어떻게 호출되었는지에 따라 동적으로 결정된다.**
- 렉시컬 스코프와 this 바인딩은 결정 시기가 다르다.
    - 렉시컬 스코프는 함수 정의가 평가되어 함수 객체가 생성되는 시점에 상위 스코프를 결정한다.
    - 하지만, this 바인딩은 함수 호출 시점에 결정된다.
- 함수 호출 방식은 다양하다.
    - 일반 함수 호출
    - 메서드 호출
    - 생성자 함수 호출
    - Function.prototype.apply/call/bind 메서드에 의한 간접 호출

```jsx
//this 바인딩은 함수 호출 방식에 따라 동적으로 결정된다.
const foo=function(){
	console.dir(this)
}

//1. 일반 함수 호출
//foo 함수 내부의 this는 전역 객체 window를 가리킨다.
foo() // window

//2. 메서드 호출
//foo 함수를 프로퍼티 값으로 할당하여 호출
//함수 내부의 this는 메서드를 호출한 객체 obj를 가리킨다.
const obj={foo}
obj.foo() //obj

//3. 생성자 함수 호출
//foo 함수를 new 연산자와 함꼐 생성자 함수로 호출
// foo 함수 내부의 this는 생성자 함수가 생성한 인스턴스를 가리킨다.
new foo() //foo{}

//4. Function.prototype.apply/call/bind 메서드에 의한 간접 호출
//foo 함수 내부의 this는 인수에 의해 결정된다.
const bar={name : 'bar'}
foo.call(bar) //bar
foo.apply(bar) //bar
foo.bind(bar)() //bar
```

### 2.1 일반 함수 호출

- **기본적으로 this는 전역 객체(global object)가 바인딩 된다.**

```jsx
function foo(){
	console.log(this) //window
	function bar(){
		console.log(this) //window
	}
	bar()
}
foo()
```

- **중첩함수를 일반 함수로 호출하면 함수 내부의 this에는 전역 객체가 바인딩 된다.**
- this는 객체의 프로퍼티나 메서드 참조하기 위한 참조 변수이므로 객체를 생성하지 않는 일반 함수에서 this는 의미가 없다.
- strict mode가 적용된 일반 함수 재부의 this에는 undefined가 바인딩 된다.

```jsx
function foo(){
'use strict'
	console.log(this) //undefined
	function bar(){
		console.log(this) //undefined
	}
	bar()
}
foo()
```

- 메서드 내에서 정의한 중첩함수도 일반 함수로 호출되면 중첩 함수 내부의 this에는 전역 객체가 바인딩 된다.

```jsx
// var 키워드로 선언한 전역 변수 value는 전역 객체의 프로퍼티다.
var value=1
//const 키워드로 선언한 전역 변수 value는 전역 객체의 프로퍼티가 아니다.

const obj={
	value:100,
	foo(){
	console.log(this) //{value : 100, foo : f}
	//메서드 내에서 정의한 중첩 함수
	function bar(){
		console.log(this) //window
	}
//메서드 내에서 정의한 중첩 함수도 일반 함수로 호출되면 중첩 함수
//내부의 this에는 전역 객체가 바인딩 된다.
bar()
}

}
```

- 콜백 함수가 일반 함수로 호출된다면 콜백 함수 내부의 this에도 전역 객체가 바인딩 된다.
- 즉, 어떠한 함수라도 일반 함수로 호출되면 this에 전역 객체가 바인딩된다.

 

```jsx

var value=1

const obj={
	value:100,
	foo(){
	console.log(this) //{value : 100, foo : f}
	//콜백 함수 내부의 this에는 전역 객체가 바인딩 된다.
	setTimeout(function(){
		console.log(this)//window
		},100)
	}
}
```

- **이처럼 일반 함수로 호출된 모든 함수(중첩 함수, 콜백 함수 포함) 내부의 this에는 전역 객체가 바인딩 된다.**
- 중첩 함수 또는 콜백함수는 외부 함수를 돕는 헬퍼 함수의 역할을 한다.
- 따라서 외부 함수의 일부 로직을 대신하는 경우가 대부분이다.
- 그런데 위부 함수인 메서드와 중첩 함수 또는 콜백 함수의 this가 일치하지 않는다는 것은 중첩 함수 또는 콜백 함수를 헬퍼 함수로 동작하기 어렵게 한다.
- 메서드 내부의 중첩함수나 콜백 함수의 this 바인딩을 메서드의 this바인딩과 일치시키기 위한 방법은 다음과 같다.

```jsx
var value=1

const obj={
	value : 100,
	foo() :{
		//this 바인딩(obj)을 변수 that에 할당한다.
		const that=this
		//콜백 함수 내부에서 this 대신 that을 참조한다.
		setTimeout(function(){
			console.log(that.value)//100
		},100}
	}
}
```

- 위 방법 외에도 this를 명시적으로 바인딩 할 수 있다.
    - Function.prototype.apply
    - Function.prototype.call
    - Function.prototype.bind

```jsx
var value=1

const obj={
	value:100,
	foo(){
		//콜백 함수에 명시적으로 this를 바인딩한다.
		setTimeout(function(){
			console.log(this.value) //100
		}.bind(this),100)
	}
}
```

- 또한 화살표 함수를 사용해서 this 바인딩을 일치시킬 수도 있다.

```jsx
var value=1

const obj={
	value:100,
	foo(){
		//콜백 함수에 명시적으로 this를 바인딩한다.
		setTimeout(()=> console.log(this.value),100)
	}
}
```

### 2.2 메서드 호출

- 메서드 내부의 this에는 메서드를 호출한 객체가 바인딩 된다.
- 주의할 것은 메서드 내부의 this에는 메서드를 소유한 객체가 아닌 메서드를 호출한 객체에 바인딩 된다는 것이다.

```jsx
const person={
	name : 'Lee'
	getName(){
		//메서드 내부의 this는 메서드를 호출한 객체에 바인딩 된다.
		return this.name
		}
}

//메서드 getName을 호출한 객체는 person이다.
consoel.log(person.getName())//Lee
```

- 메서드는 프로퍼티에 바인딩된 함수다.
- 즉 person객체의 getName 프로퍼티가 가리키는 함수 객체는 person 객체에 포함된것이 아니라 독립적으로 존재하는 별도의 객체다.
- getName프로퍼티가 함수 객체를 가리키고 있을 뿐이다.
    
    ![22-1](https://user-images.githubusercontent.com/76714485/147217980-2a0dde9f-d394-4193-9980-3dfa4b7856e7.png)

    
    
- 따라서 getName 프로퍼티가 가리키는 함수 객체, 즉 getName 메서드는 다른 객체의 프로퍼티에 할당하는 것으로 다른 객체의 메서드가 될 수도 있고 일반 변수에 할당하여 일반 함수로 호출될 수도 있다.

```jsx
const anotherPerson={
	name:'Kim'
}

//getName 메서드를 anoterPeson객체의 메서드로 할당
anotherPerson.getName=person.getName

//getName 메서드를 호출한 객체는 anotherPerson이다.
console.log(anoterPerson.getName())//Kim

//getName 메서드를 변수에 할당
const getName=person.getName;

//getName 메서드를 일반함수로 호출
console.log(getName()) //''
//일반 함수로 호출된 getName 함수 내부의 this.name은 브라우저 환경에서 window.name와 같다.
//브라우저 환경에서 window.name 기본값은 ''이다.
//node.js의 기본값은 undefined이다.
```

- 따라서 메서드 내부의 this는 프로퍼티 메서드를 가리키고 있는 객체와는 관계가 없고 메서드를 호출한 객체에 바인딩 된다.
    
    ![22-2](https://user-images.githubusercontent.com/76714485/147218010-ff1aedc7-e0bb-4eec-8a8a-44363fb4dd30.png)

    
- 프로토타입 메서드 내부에서 사용된 this도 일반 메서드와 마찬가지로 해당 메서드를 호출한 객체에 바인딩 된다.

```jsx
function Person(name){
	this.name=name
}

Person.prototype.getName=function(){
	return this.name
}

const me=new Person('Lee')

//getName 메서드를 호출한 객체는 me 다.
console.log(me.getName()) // 1. Lee
Person.prototype.name='Kim'

//getName 메서드를 호출한 객체는 Person.prototype이다.
console.log(Person.prototype.getName())// 2. Kim
```

- 1.의 경우 getName 메서드를 호출한 객체는 me다.
    - 따라서 getName 메서드 내부의 this는 me를 가리키며 this.name은 ‘Lee’이다.
- 2.의 경우 getName메서드를 호출한 Person.prototype이다.
    - Person.prototype도 객체이므로 직접 메서드를 호출할 수 있다.
    - 따라서 getName 메서드 내부의 this는 Person.prototype을 가리킨다.
        
        ![22-3](https://user-images.githubusercontent.com/76714485/147218048-027c1664-4a23-41f6-9b98-e5c5492478ff.png)


### 2.3 생성자 함수 호출

- 생성자 함수 내부의 this에는 생성자 함수가 생성할 인스턴스가 바인딩 된다.

```jsx
//생성자 함수
function Circle(radius){
	//생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
	this.radius=radius
	this.getDiameter=function() {
		return 2*this.radius
	}
}

const c1=new Circle(5)
const c2=new Circle(10)

console.log(c1.getDiameter())//10
console.log(c2.getDiameter())//20
```

- 생성자 함수 → 객체(인스턴스)를 생성하는 함수
- 만약 new연산자와 함꼐 생성자 함수를 호출하지 않으면 생성자 함수가 아니라 일반함수로 동작한다.

```jsx
//new 연산자와 함께 호출하지 않으면 생성자 함수로 동작하지 않는다. 즉, 일반적인 함수의 호출이다.
const c3=Circle(15)

//일반 함수로 호출된 Circle에는 반환문이 없다. 암묵적으로 undefined를 반환한다.
console.log(c3) //undefined

//일반 함수로 호출된 Circle 내부의 this는 전역 객체를 가리킨다.
console.log(radius) //15
```

### 2.4 Function.prototype.apply/call/bind 메서드에 의한 간접 호출

- apply, call, bind 메서드는 Function.prototype 의 메서드다. 즉 이들 메서드는 모든 함수가 상속받아 사용할 수 있다.
    

    ![22-4](https://user-images.githubusercontent.com/76714485/147218088-36007d93-a66e-4a3b-abf3-a798b1b21711.png)

- apply, call 메서드는 this로 사용할 객체와 인수 리스트를 인수로 전달받아 함수를 호출한다.

```jsx
function getThisBinding(){
	return this
	}

//this로 사용할 객체
const thisArg={a:1}

console.log(getThisBinding())//window

//getThisBinding 함수를 호출하면서 인수로 전달한 객체를  getThisBinding 함수의 this에 바인딩한다.
console.log(getThisBinding.apply(thisArg)) //{a :1}
console.log(getThisBinding.call(thisArg)) //{a :1}
```

- **apply와 call메서드의 본질적인 기능은 함수를 호출하는 것이다.**
- apply와 call 메서드는 함수를 호출하면서 첫 번째 인수로 전달한 특정 객체를 호출한 함수의 this에 바인딩한다.
- apply와 call 메서드는 호출할 함수에 인수를 전달하는 방식만 다를 뿐 동일하게 동작한다.

```jsx
function getThisBinding(){
	console.log(arguments)
	return this
}

//this로 사용할 객체
const thisArg={a:1}

//getThisBinding 함수를 호출하면서 인수로 전달한 객체를 getThisBinding 함수의 this에 바인딩한다.
//apply 메서드는 호출할 함수의 인수를 배열로 묶어 전달합니다.
console.log(getThisBinding.apply(thisArg,[1,2,3]))
//Arguments(3) [1,2,3, callee :f, Symbol(Symbol.iterator):f]
//{a:1}

//call 메서드는 호출할 함수의 인수를 쉼표로 구분한 리스트 형식으로 전달한다.
console.log(getThisBinding.call(thisArg,1,2,3))
//Arguments(3) [1,2,3, callee :f, Symbol(Symbol.iterator):f]
//{a:1}
```

- apply → 호출할 함수의 인수를 배열로 묶어 전달한다.
- call → 호출할 함수의 인수를 쉼표로 구분한 리스트 형식으로 전달한다.
- apply,call의 대표적인 용도는 → arguments 객체와 같은 유사 배열 메서드를 사용하는 경우다.
- arguments 객체는 배열이 아니기 떄문에 Array.prototype.slice같은 배열 메서드를 사용할 수 없으나 apply와 call 메서드를 이용하면 가능하다.

```jsx
function convertArgsToArray(){
	console.log(agruments)
	//arguments 객체를 배열로 변환
	//Array.prototype.slice를 인수없이 호출하려면 배열의 복사본을 생성한다.
	const arr=Array.prototype.slice.call(arguments)
	//const arr=Array.prototype.slice.apply(arguments)
	console.log(arr)

	return arr
}

converArgsToArray(1,2,3)//[1,2,3]

```

- Function.prototype.bind 메서드는 apply와 call 메서드와 달리 함수를 호출하지 않는다.
    - 다만 첫번째 인수로 전달한 값으로 this 바인딩이 교체된 함수를 새롭게 생성해 반환한다.

```jsx
function getThisBinding(){
	return this
}

//this로 사용할 객체
const thisArg= {a:1}

//bind 메서드는 첫번쨰 인수로 전달한 this 바인딩이 교체된 getThisBinding 함수를 새롭게 생성해 반환한다.
console.log(getThisBinding.bind(thisArg)) //getThisBinding
//bind 메서드는 함수를 호출하지 않으므로 명시적으로 호출해야 한다.
console.log(getThisBinding.bind(thisArg)())//{a:1}
```

- bind 메서드는 메서드의 this와 메서드 내부의 중첩 함수 또는 콜백 함수의 this가 불일치하는 문제를 해결하기 위해 유용하게 사용된다.

```jsx
const person={
	name : 'Lee'
	foo(callback){
		//1
		setTimeout(callback,100)
	}
}

person.foo(function(){
	console.log(this.name) //''
	//일반 함수로 호출된 콜백 함수 내부의 this.name은 브라우저 환경에서 window.name과 같다.
})

```

- 위 예제는 person. foo 의 콜백 함수는 외부 함수 person. foo를 돕는 헬퍼함수의 역할을 하기 때문에 외부 함수 person. foo 내부의 this와 콜백 함수 내부의 this가 상이하면 문맥상 문제가 발생한다.
- 따라서 콜백 함수 내부의 this를 외부 함수 내부의 this와 일치 시켜 주어야 한다.
    - bind메서드를 사용하여 this를 일치시킬 수 있다.

```jsx
const person={
	name : 'Lee'
	foo(callback){
		//bind메서드로 callback 함수 내부의 this 바인딩을 전달
		setTimeout(callback.bind(this),100)
		}
}

person.foo(function(){
	console.log(this.name) //Lee
})
```

## 최종

### 함수 호출 방식 → this 바인딩

- 일반 함수 호출 → 전역 객체
- 메서드 호출 → 메서드를 호출한 객체
- 생성자 함수 호출 → 생성자 함수가 미래에 생성할 인스턴스
- apply, call, bind → 메서드에 첫번쨰 인수로 전달한 객체
