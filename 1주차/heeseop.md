# 19장 프로토타입

자바스크립트는 객체지향언어 이며 자바스크립트를 이루고 있는 거의 모든 것이 객체다.

## 19.1 객체지향 프로그래밍

객체지향 프로그래밍은 실체를 인식하는 철학적 사고를 프로그래밍에 접목하려는 시도에서 시작하며 실체는 특징이나 성질을 나타내는 속성(attribute/poperty)를 가지고 있다.

속성중에서 필요한 속성만을 추려내어 표현하는 것을 추상화(abstraction)라 한다.

```javascript
const circle = {
  radius: 5, // 프로퍼티
  
  getDiameter() { // 메서드
    return 2 * this.radius;
  }
};

console.log(circle.radius); // 5
console.log(circle.getDiameter) // 10
```

위의 예시처럼 객체지향 프로그래밍은 객체의 상태를 나타내는 프로퍼티와 상태 데이터를 조작할 수 있는 메서드를 하나의 논리적인 단위로 묶은 복합적인 자료구조라고 할 수 있다.

## 19.2 상속과 프로토타입

상속(ingeritance)은 객체지향 프로그래밍의 핵심 개념으로, 어떤 객체의 프로퍼티 또는 메서드를 다른 객체가 상속받아 그대로 사용할 수 있는것을 말한다.

```javascript
function Circle(radius) {
  this.radius = radius;
}

// 프로토타입을 사용해 상위(부모) 객체 역할을 하는
// Circle.prototype의 모든 프로퍼티와 메서드를 상속받는다
Circle.prototype.getArea = function () {
  return Math.PI * Math.pow(this.radius, 2);
}

const myCircle = new Circle(10);
console.log(myCircle.getArea());
```

## 19.3 프로토타입 객체

### 19.3.1 `__proto__` 접근자 프로퍼티

모든 객체는 `__proto__` 접근자 프로퍼티를 통해 자신의 프로토타입, 즉 `[[prototype]]` 내부에 간접적으로 접근할 수 있다.

`__proto__` 접근자 프로퍼티를 코드 내에서 직접 사용하는 것은 권장하지 않는다.

### 19.3.2 함수 객체의 prototype 프로퍼티

함수 객체만이 소유하는 prototype 프로퍼티는 생성자 함수가 생성할 인스턴스의 프로토타입을 가리킨다.

non-constructor인 화살표 함수와 ES6 메서드 축약표현으로 정의한 메서드는 prototype 프로퍼티를 소유하지 안으며 프로토타입도 생성하지 않는다.

|구분|소유|값|사용 주체|사용 목적|
|---|---|---|---|---|
|`__proto__` 접근자 프로퍼티|모든 객체|프로토타입의 참조|모든 객체|객체가 자신의 프로토타입에 접근 또는 교체하기 위해 사용|
|prototype 프로퍼티|constructor|프로토타입의 참조|생성자 함수|생성자 함수가 자신의 생성할 객체(인스턴스)의 프로토타입을 할당하기위해 사용|

## 19.5 프로토타입의 생성 시점

프로토타입은 생성자 함수가 생성되는 시점에 더불어 생성된다.

### 19.5.1 사용자 정의 생성자 함수와 프로토타입 생성 시점

생성자 함수로서 호출할 수 있는 함수, 즉 constructor는 함수 정의가 평가되어 함수 객체를 생성하는 시점에 프로토타입도 더불어 생성된다.

### 19.5.2 빌트인 생성자 함수와 프로토타입 생성 시점

> ### 전역객체란?
전역 객체는 코드가 실행되기 이전 단계에서 자바스크립트 엔진에 의해 생성되는 특수한 객체이다. 브라우저 내에서는 window, node.js 내에서는 global 객체를 가리킨다.

이처럼 객체가 생성되기 이전에 생성자 함수와 프로토타입은 이미 객체화되어 존재하며 이후 생성자 함수 또는 리터럴 표기법으로 객체를 생성하면 프로토타입은 생성된 객체의 `[[prototype]]` 내부 슬롯에 할당된다.

## 19.6 객체 생성 방식과 프로토타입의 결정

객체는 다음과 같은 다양한 생성 방법이 있다.

- 객체 리터럴
- Object 생성자 함수
- 생성자 함수
- Object.create 메서드
- 클래스(ES6)

### 19.6.1 객체 리터럴에 의해 생성된 객체의 프로토타입

```javascript
const obj = { x: 1 };
```

객체 리터럴에 의해 생성된 obj 객체는 Object.prototype을 프로토타입으로 갖게 된다.

### 19.6.2 Object 생성자 함수에 의해 생성된 객체의 프로토타입

마찬가지로 생성자 함수에 의해 생성된 객체 역시 Object.prototype이다.

```javascript
const obj = new Object();
obj.x = 1;
```

### 19.6.3 생성자 함수에 의해 생성된 객체의 프로토타입

new 연산자와 함께 생성자 함수를 호출하여 인스턴스를 생성하면 다른 객체 생성 방식과 마찬가지로 추상 연상 OrdinaryObjectCreate가 호출된다.이떄, 생성자 함수에 의해 생성되는 객체의 프로토타입은 생성자 함수의 prototype 프로퍼티에 바인딩되어 있는 객체다.

```javascript
function Person(name) {
  this.name = name;
}

const me = new Person('Lee')
```

## 19.7 프로토타입 체인

```javascript
function Person(name) {
  this.name = name;
}

Person.prototype.sayHello = function () {
  console.log(`Hello ${this.name}`);
}

const me = new Person('Jeong');

// hasOwnProperty는 Object.prototype의 메서드다.
console.log(me.hasOwnProperty('name')); // true

Object.getPrototypeOf(me) === Person.prototype; // true
```

me 객체는 Person.prototype 뿐만 아니라 Object.prototype도 상속받는다.

자바스크립트는 객체의 프로퍼티(메서드 포함)에 접근하려고 할 때 해당 객체에 접근하려는 프로퍼티가 없다면 `[[prototype]]` 내부 슬롯의 참조를 따라 자신의 부모 역할을 하는 프로토타입의 프로퍼티를 순차적으로 검색한다. 이를 프로토타입 체인이라 한다. 프로토타입 체인은 자바스크립트가 객체지향 프로그래밍의 상속을 구현하는 매커니즙이다.

프로토타입 체인은 상속과 프로퍼티 검색을 위한 매커니즘이며 스코프 체인은 식별자 검색을 위한 매커니즘 이라고 할 수 있다.

## 19.8 오버라이딩과 프로퍼티 섀도잉

오버라이딩이란 상위 클래스가 가지고 있는 메서드를 하위 클래스가 재정의하여 사용하는 방식이다.

## 19.9 프로토타입의 교체

### 19.9.1 생성자 함수에 의한 프로토타입의 교체

```js
const Person = (function () {
  function Person(name) {
    this.name = name;
  }
  
  // 생성자 함수의 prototype 프로퍼티를 통해 프로토타입을 교체
  Person.prototype = {
    sayHello() {
      console.log(`Hello, ${this.name}`);
    }
  };
  
  return Person;
}());

const me = new Person('Jeong');

console.log(me.constructor == Person); // false
console.log(me.constructor == Object); // true
```

프로토타입을 교체하면 constructor 프로퍼티와 생성자 함수 간의 연결이 파괴된다.

```js
const Person = (function () {
  function Person(name) {
    this.name = name;
  }
  
  // 생성자 함수의 prototype 프로퍼티를 통해 프로토타입을 교체
  Person.prototype = {
    // constructor 프로퍼티와 생성자 함수 간의 연결을 설정
    constructor: Person,
    sayHello() {
      console.log(`Hello, ${this.name}`);
    }
  };
  
  return Person;
}());

const me = new Person('Jeong');

console.log(me.constructor == Person); // true
console.log(me.constructor == Object); // false
```
프로토타입으로 교체한 객체 리터럴에 constructor 프로퍼티를 추가하여 프로토타입의 constructor 프로퍼티를 되살린다.

### 19.9.2 인스턴스에 의한 프로토타입의 교체

```js
function Person(name) {
  this.name = name;
}

const me = new Person('Jeong');

// 프로토타입으로 교체할 객체
const parent = {
  sayHello() {
    console.log(`Hello ${this.name}`);
  }
};

// me 객체의 프로토타입을 parent 객체로 교체한다.
Object.setPrototypeOf(me, parent);

// 위 코드는 아래의 코드와 동일하게 동작한다.
// me.__proto__ = parent;

me.sayHello(); // Hello Jeong

console.log(me.constructor === Person) // false
console.log(me.constructor === Object) // true
```

앞의 19.9.1절과 같이 프로토타입을 교체하면 constructor 프로퍼티와 생성자 함수 간의 연결이 파괴된다.

```js
function Person(name) {
  this.name = name;
}

const me = new Person('Jeong');

// 프로토타입으로 교체할 객체
const parent = {
  // constructor 프로퍼티와 생성자 함수 간의 연결을 설정
  constructor: Person,
  sayHello() {
    console.log(`Hello ${this.name}`);
  }
};

// 생성자 함수의 prototype 프로퍼티와 프로토타입 간의 연결을 설정
Person.prototype = parent;

// me 객체의 프로토타입을 parent 객체로 교체한다.
Object.setPrototypeOf(me, parent);

// 위 코드는 아래의 코드와 동일하게 동작한다.
// me.__proto__ = parent;

me.sayHello(); // Hello Jeong

console.log(me.constructor === Person) // false
console.log(me.constructor === Object) // true
```

## 19.10 instanceof 연산자

```js
객체 instanceof 생성자 함수
```

우변의 생성자 함수의 prototype에 바인딩된 객체가 좌변의 객체의 프로토타입 체인 상에 존재하면 ture로 평가되고, 그렇지 않은 경우에는 false로 평가된다.

생성자 함수의 prototype에 바인딩된 객체가 프로토타입 체인 상에 존재하는지 확인한다.

## 19.13 프로퍼티 존재 확인

### 19.13.1 in 연산자

```js
const person = {
  name: 'Lee',
  address: 'Seoul'
};

console.log('name' in person) // true
console.log('age' in person) // false

console.log(Reflect.has(person, 'name')) // true
console.log(Reflect.has(person, 'toString')) // true
```

in 연산자는 객체 내에 특정 프로퍼티가 존재하는지 여부를 확인한다.

ES6에서 도입된 Reflect.has 메서드는 in연산자와 동일하게 사용 가능하다.

## 19.14 프로퍼티 열거

### 19.14.1 for ... in 문

객체의 모든 프로퍼티를 순회하며 열거하려면 for ... in 문을 사용한다.

```js
const person = {
  name: 'Jeong',
  address: 'Ulsan'
  __proto__: { age: 20 }
};

for (const key in person) {
  console.log(key + ':' + person[key]);
}

// name: Jeong
// address: Ulsa
// age: 20
```

### 19.14.2 Object.keys/values/entries 메서드

```js
const person = {
  name: 'Jeong',
  address: 'Ulsan'
  __proto__: { age: 20 }
};

console.log(Object.keys(person)); // ["name", "address"]
console.log(Object.values(person)); // ["Jeong", "Ulsan"]
console.log(Object.entries(person)); // ["name", "address"], ["Jeong", "Ulsan"]
```


# 23장 실행 컨텍스트

실행 컨텍스트는 자바스크립트의 동작 원리를 담고 있는 핵심 개념이다.

## 23.1 소스코드의 타입
ECMAScript는 소스코드를 4가지 타입으로 구분한다. 전역코드, 함수코드, eval 코드, 모듈 코드.

우선 전역코드와 함수코드만 알아두자.

## 23.2 소스코드의 평가와 실행

소스코드를 실행하기 전에는 '소스코드 평가'와 '소스코드 실행'과정으로 나누어 처리한다.

아래와 같은 코드로 예시를 들겠다.

```js
var x;
x = 1;
```

1. 먼저 소스코드 평가 과정에서 var x;를 먼저 처리한다. 이때, undefined로 초기화 한다.
2. 이제 x의 값은 undefined이다. 소스코드 평가과정이 끝나게 되면 이제 소스코드 실행과정(런타임)으로 진행한다. x의 값에 1을 할당하여 최종적으로 x의 값은 1이 된다.

## 23.3 실행 컨텍스트의 역할

>이렇게 생각하면 쉽다.
자바스크립트는 쉽게 생각해서 '전역 코드'와 '함수 코드'로 나누어 져있다고 생각하면 쉽다. 즉, 함수 내의 영역은 전역 내에서 독립한 공간이라고 생각하자.

1. 전역 코드 평가
먼저 전역 코드의 선언문을 모두 실행한다.이떄, 값은 모두 할당되지 않았다.

2. 전역 코드 실행
앞서 전역 코드 평가에서 선언문은 모두 undefined이다. 런타임이 실행되어 선언문에 값을 할당하고 함수를 호출한다.

3. 함수 코드 평가
함수가 호출되면 함수 내에 있는 코드의 선언문을 모두 실행한다.역시 값은 아직 할당되지 않았다.

4. 함수 코드 실행
역시 런타임이 실행되어 아직 값이 할당되지 않는 선언문에 값을 할당한다.

실행 컨텍스트는 소스코드를 실행하는 데 필요한 환경을 제공하고 코드의 실행 결과를 실제로 관리하는 영역이다.

식별자와 스코프는 실행 컨텍스트의 렉시컬 환경, 코드 실행 순서는 실행 컨텍스트 스택으로 관리한다.

## 23.4 실행 컨텍스트 스택

```js
const x = 1;

function foo() {
  const y = 2;
  
  function bar() {
    const z = 3;
    console.log(x + y + z);
  }
  bar();
}

foo(); // 6
```

자료구조에서 스택의 원리를 이해한다면 아주 쉽게 이해할 수 있다. 스택은 가장 최상위에 있는 데이터만 조작이 가능하다.

empty
전역
전역 | foo
전역 | foo | bar
전역 | foo
전역
empty

실행 컨텍스트 스택의 최상위에 존재하는 실행 컨텍스트는 언제나 현재 실행중인 코드의 실행 컨텍스트 이다.

## 23.5 렉시컬 환경

>렉시컬 환경의 경우 너무 깊게 파고들 필요는 없다고 생각한다. 생각보다 복잡하다. 따라서 var과 let, const와의 동작방식 차이(객체 환경 레코드, 선언적 환경 레코드), 함수 호출시의 렉시컬 환경에 대해서만 알아보자.

렉시컬 환경은 식별자와 식별자에 바인딩된 값, 그리고 상위 스코프에 대한 참조를 가록하는 자료구조로 실행 컨텍스트를 구성하는 컴포넌트이다.

## 23.6 실행 컨텍스트의 생성과 실별자 검색 과정

```js
var x = 1;
const y = 2;

function foo(a) {
  var x = 3;
  const y = 4;
  
  function bar(b) {
    const z = 5;
    console.log(a + b + x + y + z);
  }
  bar(10);
}

foo(20);
```

### 23.6.1 전역 객체 생성
전역 객체는 전역코드가 평가되기 이전에 생성된다.

### 23.6.2 전역 코드 평가

1. 전역 실행 컨텍스트 생성
2. 전역 렉시컬 환경 생성
2.1 전역 환경 레코드 생성
2.1.1 객체 환경 레코드 생성
2.1.2 선언적 환경 레코드 생성
2.2 this 바인딩
2.3 외부 렉시컬 환경에 대한 참조 결정

앞서 실행 컨텍스트 스택을 배웠다. 가장 먼저 실행 컨텍스트 스택에 전역 실행 컨텍스트를 푸쉬한다. 이것을 전역 렉시컬 환경을 생성하고 바인딩 한다. 전역 환경 레코드에서 전역 프로퍼티와 메서드를 관리한다. 전역 환경 레코드는 크게 2가지, 객체 환경 레코드와 선언적 환경 레코드로 나뉜다. 쉽게 설명하자면 객체 환경 레코드는 var과 함수 선언문, 선언적 환경 레코드는 let, const를 관리한다.

> ### var과 let, const와의 차이점
여기서 재미있는 점이 객체 환경 레코드는 "선언 단계"와 "초기화 단계"를 동시에 진행하여 전역 렉시컬 환경에 바인딩 한다는 것이다. 즉,  코드 실행 단계 이전에도 참조할 수 있으며 이때의 값은 초기화 값인 undefined이다. 이것이 호이스팅이 발생하는 기본 원리이다.
이와달리 let, const같은 키워드는 객체 환경 레코드가 아닌 선언적 환경 레코드에 등록된다.(그리고 전역 렉시컬 환경과 바인딩 된다.) 이곳은 "선언 단계"와 "초기화 단계"가 분리되어 진행된다. 즉, 코드 실행 단계 이전에 참조할 수 없으며 참조시 에러가 난다.

```js
console.log(a); // undefined
console.log(b); // Error

var a = 1;
const b = 1;
```

이후 this 바인딩을 통해 전역 코드에서 this를 참조하면 바인딩되어 있는 객체가 반환된다. 외부 렉시컬 환경은 전역 환경 자체가 최상위 환경이기 때문에 참조할게 없다. 즉, null값이 자동으로 할당된다.


### 23.6.3 전역 코드 실행

앞서 초기화한 선언문에 값을 할당할 차례이다. 이를 식별자 결정이라 한다. 식별자 결정을 위해 식별자를 검색할 때는 실행 중인 실행 컨텍스트에서 식별자를 검색하기 시작한다.

### 23.6.4 foo 함수 코드 평가

```js
foo(20); // 호출 직전
```

1. 함수 실행 컨텍스트 생성
2. 함수 렉시컬 환경 생성
2.1 함수 환경 레코드 생성
2.2 this바인딩
2.3 외부 렉시컬 환경에 대한 참조 결정

자, 이제 foo함수를 맞닥뜨렸다. 아까 실행 컨텍스트 스택에서 전역 객체 내에서 함수를 만나게 된다면 새로운 함수 실행 컨텍스트를 생성한다고 했다. 사실, 위의 전역 객체와 다를게 없다. 기본적인 구조는 똑같으며 역시 함수 내의 프로퍼티와 메서드의 값을 초기화 하고 각각의 함수 환경 레코드에 등록한다.

이후 this 바인딩을 하고 외부 렉시컬 환경에 대한 참조를 결정한다. 13.5절 "렉시컬 스코프"에서 JS는 함수를 어디서 호출했는지가 아니라 어디서 정의했는지에 따라 상위 스코프를 결정한다고 했다. 즉, foo함수의 상위 스코프는 전역 렉시컬 환경이다.

### 23.6.5 foo 함수 코드 실행
역시 식별자 결정을 위해 실행 중인 실행 컨텍스트의 렉시컬 환경에서 식별자를 검색하기 시작한다.

### 23.6.6 bar 함수 코드 평가
앞서 실행한 레퍼토리가 모두 똑같다. 달라진점 하나는 외부 렉시컬 환경 참조가 foo 함수라는 것. 즉, 상위 스코프는 foo 함수이다.

### 23.6.7 bar 함수 코드 실행
여기서 `console.log(a + b + x + y + z);`가 실행되었다. 실행 중인 실행 컨텍스트의 렉시컬 환경에서 외부 렉시컬 환경에 대한 참조로 이어진다. 이제 검색할 차례다.

>참조 규칙
하위 스코프에서 상위 스코프를 참조하는 것은 가능하지만 상위 스코프에서 하위 스코프를 참조하는 것은 불가능 하다.

검색한 값을 찾았다면 이제 값을 평가하여 할당한다.

### 23.6.8 차례로 실행 컨텍스트 종료
이제 bar함수의 모든 값을 할당 하였으니 bar 함수 실행 컨텍스트를 제거한다. 여기서 실행 컨텍스트는 스택 형식으로 쌓이기 때문에 bar 함수 실행 컨텍스트 종료 전에 foo 함수 실행 컨텍스트나 전역 실행 컨텍스트를 종료할 수 없다.(스택 자료구조에서는 오직 pop, push만 사용가능하다.)

### 23.7 실행 컨텍스트와 블록 레벨 스코프
var 변수 선언은 `함수 레벨 스코프`이고 let, const는 `블록 레벨 스코프`이다. 즉, var을 쓰면 함수 외의 요소`for문, if문, 배열 등`에서 사용하면 전역 변수로 할당이 된다. 이는 효율적이지 않을 뿐만 아니라 예기치 않은 오류를 발생시킬 수 있다. 이 원리를 좀더 파고들면 아까 객체 환경 레코드와 선언 환경 레코드의 차이를 알아야 한다. 가장 큰 차이는 "선언 단계"와 "초기화 단계"에 있다고 앞에 설명하였다.(var은 동시에 let, const는 따로)

자 이제 이해가 되었을것이라고 생각한다. 이제 var과 let,const의 차이점과 var을 사용하지 말아야할 이유를 설명할 수 있어야 한다!

> 요약
자, 잘 생각해보자. 실행 컨텍스트에 앞서 우리는 코드가 총 4가지로 구분되고 그중에서 2가지를 주로 사용한다.(전역 코드, 함수 코드) 전역 컨텍스트를 먼저 실행하고 만약 함수를 실행하게 되면 새로운 함수 컨텍스트를 만들어 스택 형식으로 쌓는다. 핵심은 코드가 전역 코드, 함수 코드 둘밖에 없다는 것이다. '블록 코드'는 없다. 즉, var 키워드는 블록 내에서 선언이 되어도 객체 환경 레코드에서 관리되며 이는 전역 랙시컬 환경과 바인딩 되어 전역 코드로 실행된다. 이것이 var이 함수 레벨 스코프인 원리이다. 이와 달리 let, const 키워드는 객체 환경 레코드가 아닌 선언 환경 레코드에서 관리된다고 했다.

>또한, 참조는 한단계 상위 스코프를 하게된다. 만약 코드 구조가 전역-함수1-함수2 라고 가정해보자. 함수2는 함수1을 참조하고, 함수1은 전역을 참조한다. 이것은 다음장의 클로저를 설명할때 핵심이 되는 개념이니 꼭 이해해 두도록 하자.

>이제 JS엔진의 핵심이 보이는가? JS엔진의 핵심은 바로 '함수'위주로 돌아간다는 것이다. 실행 컨텍스트를 잘 이해하면 변수 호이스팅, 스코프, 클로저 등의 중요한 개념을 매우 쉽게 이해할 수 있을것이다!
