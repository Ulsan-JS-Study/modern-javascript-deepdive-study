## 22.1 this 키워드

동작을 나타내는 메서드는 자신이 속한 객체의 상태, 즉 프로퍼티를 참조하고 변경할 수 있어야 한다. 애때, 메서드가 자신이 속한 객체의 프로퍼티를 참조하려면 먼저 자신이 속한 객체를 가리키는 식별자를 참조할 수 있어야 한다.

```js
const circle ={
  // 프로퍼티
  radius: 5,
  // 메서드
  getDiameter() {
    // 자신이 속한 객체 circle의 프로퍼티를 참조하고 있다.
    return 2 * circle.radius;
  }
}

console.log(circle.getDiameter()); // 10
```

this는 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 자기 참조 변수다. this를 통해 자신이 속한 객체 또는 자신이 생성할 인스턴스의 프로퍼티나 메서드를 참조할 수 있다.

단, this가 가리키는 값, 즉 this 바인딩은 함수 호출 방식에 의해 동적으로 결정된다.

```js
function Circle(radius) {
  // this는 생성자 함수가 생성할 인스턴스를 가리킨다.
  this.radius = radius;
}

Circle.prototype.getDiaveter = function () {
  // 역시 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
  return 2 * this.radius;
}

const circle = new Circle(5);
console.log(circle.getDiameter()); // 10
```

생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킨다.

자바스크립트의 this는 함수가 호출되는 방식에 따라 this에 바인딩될 값, 즉 this 바인딩이 동적으로 결정된다.

this는 객체의 프로퍼티나 메서드를 참조하기 위한 자기 참조 변수이므로 인반 함수의 this에는 undefined가 바인딩 된다. 즉, 일반 함수 내부에서는 this를 사용할 필요가 없다.

## 22.2 함수 호출 방식과 this 바인딩

this 바인딩(this에 바인딩될 값)은 함수 호출 방식, 즉 함수가 어떻게 호출되었는지에 따라 동적으로 결정된다. 주의할 것은 동일한 함수도 다양한 방식으로 호출할 수 있다는 것이다.

1. 일반 함수 호출
2. 메서드 호출
3. 생성자 함수 호출
4. Function.prototype.apply/call/bind 메서드에 의한 간접 호출

### 22.2.1 일반 함수 호출

기본적으로 this에는 전역 객체가 바인딩된다.

```js
function foo() {
  console.log("Hello world!", this); // window
  function bar() {
    console.log("Hi!", this); // window
  }
  
  bar();
}

foo();
```

이처럼 일반 함수로 호출된 모든 함수(중첩 함수, 콜백 함수 포함) 내부의 this에는 전역 객체가 바인딩된다.

메서드 내부의 중첩 함수나 콜백 함수의 this 바인딩을 메서드의 this 바인딩과 일치시키기 위한 방법은 다음과 같다.

```js
var value = 1;

const obj = {
  value: 100,
  foo() {
    // 화살표 함수 내부의 this는 상위 스코프의 this를 카리킨다.
    setTimeout(() => console.log(this.value, 100); // 100
  }
};

obj.foo();
```

### 2.22.2 메서드 호출

메서드 내부의 this에는 메서드를 호출한 객체, 즉 메서드를 호출할 때 메서드 이름 앞의 마침표(.) 연산자 앞에 기술한 객체가 바인딩 된다.

```js
const universe = {
  planet: 'Earth',
  getPlanet() {
    return this.planet;
  }
};

console.log(person.getPlanet()); // Earth
```

### 22.2.3 생성자 함수 호출

생성자 함수 내부의 this에는 생성자 함수가 (미래에)생성할 인스턴스가 바인딩 된다.

```js
function Universe(planet) {
  this.planet = planet;
  this.getPlanet = function () {
    return `${this.planet} is contained in solar system.`;
  }
}

const univ1 = new Universe('Venus');
const univ2 = new Universe('Neptune');

console.log(univ1.getPlanet());
// Venus is contained in solar system.
console.log(univ2.getPlane st());
// Neptune is contained in solar system.
```

## 25.1 클래스는 프로토타입의 문법적 설탕인가?

javascript는 C#과 같은 클래스 기반 객체지향 프로그래밍에 익숙한 프로그래머를 위해 ES6에서 도입한 새로운 객체 생성 메커니즘이다.

## 25.2 클래스 정의

```js
// 클래스 선언문
class Person {}

// 익명 클래스 표현식
class Person = class {};

// 기명 클래스 표현식
class Person = class MyClass {};
```

자바스크립트 내에서 클래스는 엄밀히 말하면 함수이다. 값처럼 사용할 수 있는 일급 객체이다.

```js
class Worldcup {
  // 생성자
  constructor(country) {
    // 인스턴스 생성 및 초기화
    this.country = country;
  }
  
  champion() {
    console.log(`the champion is ${this.name}!`);
  }
  
  static goldenball() {
    console.log("Leonel Messi");
  }
}

// 인스턴스 생성
const worldcup = new Worldcup("Argentina");

// 인스턴스의 프로퍼티 참조
console.log(worldcup.country); // Argentina

// 인스턴스의 메서드 참조
worldcup.champion();

// 인스턴스의 정적 메서드 호출
Worldcup.goldenball();
```

위의 예시에서 클래스의 프로퍼티, 메서드를 정의하고 new 연산자를 통해 인스턴스를 생성하고 생성된 인스턴스의 프로퍼티, 메서드를 호출해 보았다.

위의 정적 메서드는 일반 메서드와 다르게 인스턴스가 없어도 호출할 수 있는 메서드이다.

> 사실 클래스는 별게 없다. 함수보다 좀더 강력한 프레임을 갖추어 코드의 재상용을 효율적으로 줄여주는 역할을 한다. 또한 프레임에 어긋나는 규격외의 에러도 자동으로 처리해 준다. 하지만 클래스를 제대로 활용하기 위해서는 클래스가 가지는 규칙을 이해할 팰요가 있다.

## 25.2 - 25.7 메서드와 프로퍼티

### 25.5.1 constructor

constructor는 한국말로 번역하면 '토대'이다. 즉, 인스턴스를 생성하고 초기화하기 위한 특수한 메서드이다.(초기값 설정)

```js
class Person {
  constructor(name) {
    // 인스턴스 생성 및 초기화
    this.name = name;
  }
}
```

> 위의 constructor를 처음봤을땐 이해하기 힘들었다. 그러나 조금만 자세히 살펴보면 아주 간단하다. 먼저 앞서 22장에서 배운 this의 의미를 알아야 한다. this는 자신이 속한 객체의 한단계 상위 객체를 가리킨다. 즉 위의 코드에서 this는 Person을 가리킨다. 이제 코드가 눈에 보일거다. `Person.name = name` 즉, 위의 코드는 인스턴스에서 받은 name을 클래스 내에서 초기값으로 설정하는 로직이다.

위의 constructor를 통해 인스턴스의 프로퍼티와 메서드의 초기값을 할당해줄 수 있다.

## 25.8 상속에 의한 클래스 확장

상속에 의한 클래스 확장은 기존 클래스를 상속받아 새로운 클래스를 확장하여 정의하는 것이다.

```js
class ToDoList {
  consturctor(name) {
    this.name = name;
  }
  
  intro() {
    console.log(`this is ${this.name}'s to do list`);
  }
  
  studyAboutThis() {
    return "This";
  }
}

// 상속을 통한 클래스 확장
class studyMore extends toDoList() {
  studyAboutClass() {
    return "Class";
  }
}

const toDoList = new ToDoList("Jeong");
```

extend 키워드 앞에 상속할 클래스의 이름을, 그리고 extend 키워드 뒤에 상속 받을 클래스의 이름을 기입하면 위의 예제처럼 클래스끼리 합쳐진다.

이때 클래스는 프로토타입으로 연결되며 마치 하나의 클래스처럼 동작한다.

### 25.8.5 super 키워드

super 키워드는 슈퍼 클래스의 constructor를 똑같이 받아올때 사용한다.

쉽게 말하면 super는 자신을 상속한 클래스의 constructor를 가리킨다.

```js
class Base {
  constructor(a, b) {
    this.a = a;
    this.b = b;
  }
}

class Test extends Base {
  constructor(a, b, c) {
    super(a, b);
    this.c = c;
  }
}

const test = new Test(1, 2, 3);
console.log(test);
```

위의 예시코드를 보면 a, b의 constructor 값을 super를 통해 그대로 가져온것을 알 수 있다.

## 클래스를 이용한 간단한 toDoList

```js
const fs = require('fs');

class TodoList { // 클래스 선언
  constructor() {
    this.todos = []; // 초기값은 빈 배열 할당
  }

  addTodo(todo) {
    this.todos.push(todo); // push 메서드를 통해 데이터 값을 추가
  }

  removeTodo(todo) {
    const index = this.todos.findIndex(t => t.text === todo.text); // findIndex 메서드를 이용하여 일치하는 데이터 값을 찾는다.
    if (index !== -1) {
      this.todos.splice(index, 1); // 일치하는 데이터값을 제거한다.
    }
  }
}

const todoList = new TodoList(); // 인스턴스 생성

todoList.addTodo({text: 'learn javascript'}); // addTodo메서드를 통해 인스턴스에 데이터를 추가 1.
todoList.addTodo({text: 'learn nodejs'}); // addTodo메서드를 통해 인스턴스에 데이터를 추가 2.
todoList.addTodo({text: 'learn react'}); // addTodo메서드를 통해 인스턴스에 데이터를 추가 3

todoList.removeTodo({text: 'learn nodejs'}); // 데이터와 일치하는 값을 제거

fs.writeFile('./data.json', JSON.stringify(todoList.todos), (err) => { // 데이터를 로컬에 저장
  if (err) throw err;
  console.log('The file has been saved!');
});

console.log(todoList.todos);
```