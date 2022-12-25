### 객체 리터럴

\- 자바스크립트는 원시 값을 제외한 나머지 값(함수, 배열, 정규 표현식 등)은 모두 객체이다.

\- 객체타입은 다양한 타입의 값을 하나의 단위로 구성한 복합적인 자료구조이다.

\- 객체는 0개 이상의 프로퍼티로 구성된 집합이며 키와 값으로 구성된다.

\- 모든 값은 프로퍼티 값이 될 수 있으며 함수도 가능하다. (함수가 프로퍼티의 값인 경우 메서드라고 부른다.)

\- {...} 중괄호 안에 프로퍼티를 정의 하며, 나열할 때는 쉼표(,)로 구분한다. 

```
var person = {
    name: "Lee", // 프로퍼티
    sayHello: function () { // 메서드
        console.log("Hello ~~")
}

console.log(typeof person) // object
console.log(person) // {name: "Lee", sayHello: f}
```

리터럴 : 사람이 이해할 수 있는 문자 또는 약속된 기호를 사용하여 값을 생성하는 표기법.

원시 값 : 변경 불가능한 값으로 string, number, bigint, boolean, unedfined, symbol, null 타입이 있다.

### 프로퍼티 

\- 나열할때는 쉼표(,)로 구분한다.

\- 프로퍼티 키는 식별자 역할을 하며, 식별자 네이밍 규칙을 따르지 않으면 반드시 따옴표를 사용해야 한다.

```
var person = {
    firstName: "Mark",
    'last-name': "Lee"
}
```

\- 프로퍼티 키를 동적으로 생성할 수 있다. 이때 대괄호(\[...\])로 묶어야 한다.

\- 프로퍼티 키를 중복 선언하면 나중에 선언한 프로퍼티가 먼저 선언한 프로퍼티를 덮어쓴다.

\- 프로퍼티 값에 접근하기 위해서는 마침표 또는 대괄호를 이용하여 접근한다.

식별자 네이밍 규칙을 따르지 않으면 반드시 대괄호를 이용하여 접근하며, 이때 키 값에 따옴표를 사용한다. (obj\['l-name'\])

\- 프로퍼티를 삭제하고 싶을 때는  delete를 사용한다.

```
var obj = {
    'last-name' : 'Lee',
    1 : 10
}

// 프로퍼티 키 동적 추가 
var key = 'hello'
console.log(obj.key) // 'hello'

// 프로퍼티 키 갱신
obj[key] = 'world' 
console.log(obj.key) // 'world'


// 식별자 네이밍을 따르지 않는 경우 접근
console.log(obj['last-name']) // 'Lee'
console.log(obj[last-name]) // ReferenceError: last-name is not defined

// 프로퍼티 키가 숫자인 경우, 대괄호를 사용하며 따옴표 유무는 상관 없음
console.log(obj[1]) // 10

// 객체에 프로퍼티 키가 없는경우
console.log(obj.age) // undefined

// 객체에서 프로퍼티 삭제
delete obj['last-name']
```

### ES6 객체 리터럴

\- 변수 이름과 프로퍼티의 키가 동일한 이름인 경우 프로퍼티 키를 생략 가능 하다.

```
const x = 1
const obj = {x}

console.log(obj) // {x: 1}
```

\- 객체 내부에서 계산된 프로퍼티 이름으로 프로퍼티 키를 동적 생성 가능하다.

```
// ES5 외부에서 동적 생성해야함
var prefix = 'prop';
var i = 0 

var obj = {} 

obj[prefix + '-' + ++i] = i

// ES6 내부에서 생성 가능

const prefix = 'prop'
let i = 0 

const obj = {
    [`${prefix}-${++i}`]: i
}
```

\- 메서드 축약 가능 

ES6 사양에서 메서드는 메서드 축약 표현으로 정의된 함수만을 의미한다. 

\- 축약 표현이 아닌 경우 일반함수이다.

\- 인스턴스를 생성할 수 없는 non-constructor이며, 생성자 함수로서 호출할 수 없다.

\- ES6 메서드는 자신을 바인딩한 객체를 가리키는 내부슬롯 \[\[HomeObject\]\]를 갖는다.

super 참조는 내부 슬롯 \[\[HomeObject\]\]를 사용하여 슈퍼클래스의 메서드를 참조하므로 super 키워드 사용가능하다.

```
const obj = {
    sayHi : function() {  // 일반 함수 
    	console.log("hi")
    },
    sayHello(){  // 메서드 
    	console.log("hello")
    }
}

// ES5 vs ES6 
new obj.sayHi() // sayHi {}
new obj.sayHello() // TypeErrorL obj.sayHello is not a constructor

obj.sayHi.hasOwnProperty('prototype') // true
obj.sayHello.hasOwnProperty('prototype') // false 

// ES6에서의 메서드는 (축약 표현) super 사용 가능

const base = {
   name: 'Lee',
   sayHi() {
       return `Hi! ${this.name}`
   }
}

const derived = {
    __proto__: base, // 부모 유전자 base
    sayHi(){
    	return `${super.sayHi()}. how are you doing?`
    }
}

console.log(derived.sayHi()); // Hi! Lee. how are you doing?
```

자바스크립트 this에 대해 정리합니다.

### this 정의

\- 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 자기 참조 변수다. 

\- this를 통해 자신이 속한 객체 또는 자신이 생성할 인스턴스의 프로퍼티나 메서들를 참조할 수 있다.

### this 사용

자바스크립트에서 this 바인딩은 함수 호출 방식에 의해 동적으로 결정된다.

\- 전역에서 this 

this는 어디서든 참조 가능하다.

그리고 전역에서의 this는 전역 객체 window를 가리킨다.

```
console.log(this) // window
```

\- 일반 함수에서 this

일반 함수에서 this를 사용 시 전역 객체 window를 가리킨다.

하지만 strict mode일 경우 undefined가 바인딩된다.

메서드 내부에서 일반함수로 호출되면 this는 전역객체를 가리킨다.

ES6부터 객체 안의 함수 프로퍼티 축약형은 메서드이다. 이외에는 일반 함수이다.

```
function fn(){
    console.log(this) // window
}

function fn2(){
    'use strict' 
    console.log(this) // undefined
}

const obj = {
    foo(){
        function bar() {
        	console.log(this) // window
        }
    }
}
```

\- 메서드에서 this 

메서드 내부에서 this를 호출하면 메서드를 호출한 객체를 가리킨다.

일반 함수로 호출된 모든 함수(중첩 함수, 콜백 함수) 내부의 this는 전역 객체가 바인딩된다.

```
var x = 'global' // window.x = 'global'

const obj = {
    x: 'local',
    foo(){
    	console.log(this) // {x: 'local', foo: f}
        console.log(this.x) // 100

        // 메서드 내에서 정의한 중첩 일반함수
        function bar(){
            console.log(this) // window
            console.log(this.x) // 'global'
        }
        bar()
        
        // 콜백 함수가 일반 함수로 호출되기 때문에 this는 window를 가르킨다.
        setTimeout(function(){
            console.log(this) // window
            console.log(this.x) // 'global'
        }, 100)
    },
}

obj.foo()
```

위의 setTimeout의 콜백함수 내부에서 this가 전역객체가 아닌 객체를 가리키려면 화살표 함수 또는 명시적으로 this 객체 전달(bind) 해주면 된다.

```
var x = 'global' // window.x = 'global'

const obj = {
    x: 'local',
    foo(){
    	// 콜백함수 화살표 함수로 호출
        setTimeout(()=>{
            console.log(this.x) // 'local'
        }, 1000)
        
        // 명시적으로 this 객체 전달
        setTimeout(function(){
			console.log(this.x) // 'local'
		}.bind(this),1000)
    },
}

obj.foo()
```

### 생성자 함수 this 호출 

\- 생성자 함수가 생성할 인스턴스가 바인딩된다.

```
function Circle(radius){
    this.radius = radius
	this.getDiameter = function () {
        return 2 * this.radius
    }
}

const circle1 = new Circle(5)

console.log(circle1.getDiameter()) // 10
```
