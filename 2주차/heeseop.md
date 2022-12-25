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
console.log(univ2.getPlanet());
// Neptune is contained in solar system.
```