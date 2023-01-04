# 비동기 프로그래밍

## 이벤트 루프

> 이벤트 루프는 브라우저에 내장되어 있는 기능 중 하나다.

구글의 V8 자바스크립트 엔진을 비롯해서 대부분의 자바스크립트 엔진은 크게 2개의 영역으로 구분할 수 있다.

- 콜스택 (실행 컨텍스트가 추가되고 제거되는 스택 자료구조)
- 힙 (객체가 저장되는 메모리 공간)

비동기 처리에서 소스코드의 평가와 실행을 제외한 모든 처리는 자바스크립트 엔진을 구동하는 환경인 브라우저 또는 Node.js가 담당한다.

예를 들어서 `setTimeout`의 콜백 함수의 평가와 실행은 자바스크립트 엔진이 담당하지만 호출 스케줄링을 위한 타이머 설정과 콜백 함수의 등록은 브라우저 또는 Node.js가 담당한다. 이를 위해 브라우저 환경은 태스크 큐와 이벤트 루프를 제공한다.

- 태스크 큐 (비동기 함수의 콜백 함수 또는 이벤트 핸들러가 일시적으로 보관되는 영역)
- 이벤트 루프 (콜 스택에 실행 중인 실행 컨텍스트가 없다면 태스크 큐에 순차적으로 콜 스택으로 옮긴다.)

자바스크립트는 싱글 스레드 방식으로 동작하는데, 이때 싱글 스레드 방식으로 동작하는 것은 브라우저가 아니라 브라우저에 내장된 자바스크립트 엔진이라는 것에 주의하자.

만약 모든 자바스크립트 코드가 자바스크립트 엔진에서 싱글 스레드 방식으로 동작한다면 자바스크립트는 비동기로 동작할 수 없다.

- 자바스크립트 엔진 = 싱글 스레드
- 브라우저 환경 = 멀티 스레드

# AJAX

> Asynchronous JavaScript and XML

자바스크립트를 사용해서 브라우저가 서버에게 비동기 방식으로 데이터를 요청하고, 서버가 응답한 데이터를 수신해서 웹페이지를 동적으로 갱신하는 프로그래밍 방식

Ajax는 1999년 마이크로소프트가 개발한 XMLHttpRequest 객체를 기반으로 동작한다.

예전에는 HTTP 통신으로 html 전체를 받아서 새로 페이지를 이동한다면 다시 html을 받아서 리렌더링하는 방식으로 진행됐기 때문에 굉장히 비효율적이었다.

근데 ajax의 등작으로 서버로 html을 전부 받아서 리렌더링하는 것이 아니라 변경할 필요가 있는 부분만 부분적으로 렌더링 할 수 있게 됐다.

# JSON

> 클라이언트와 서버 간의 HTTP 통신을 위한 텍스트 데이터 포맷. 자바스크립트에서만 사용가능 한 것이 아니라 대부분의 프로그래밍 언어에서 사용 가능하다.

## JSON.stringify

> JSON 포맷의 데이터를 문자열로 바꾸는 자바스크립트 메소드

`JSON.stringify` 메서드는 배열도 문자열로 변환할 수 있다.

```js
const array = [
	{
		id: 1,
		text: 'one',
	},
	{
		id: 2,
		text: 'two',
	},
	{
		id: 3,
		text: 'three',
	}
];

// [{"id":1,"text":"one"},{"id":2,"text":"two"},{"id":3,"text":"three"}]
console.log(JSON.stringify(array));
```

## JSON.parse

> JSON 포맷의 문자열을 객체로 변환하는 자바스크립트 메서드

- 서버에서 클라이언트로 데이터를 제공하면 이 데이터는 문자열로 넘어온다.
- 이 문자열을 객체로 사용하려면 parse 메서드를 사용해야 한다.
- 배열이 JSON 포맷의 문자열로 변환되어 있으면 JSON.parse는 문자열을 배열로 변환한다.
- 배열의 요소가 객체인 경우에는 배열의 요소까지 객체로 전부 변환한다.

```js
const array = [
	{
		id: 1,
		text: 'one',
	},
	{
		id: 2,
		text: 'two',
	},
	{
		id: 3,
		text: 'three',
	}
];

// 문자열이었다가...
// [{"id":1,"text":"one"},{"id":2,"text":"two"},{"id":3,"text":"three"}]
console.log(JSON.stringify(array));

/*
객체로 변환된 모씁
0: {id: 1, text: 'one'}
1: {id: 2, text: 'two'}
2: {id: 3, text: 'three'}
*/
console.log(JSON.parse(JSON.stringify(array)));
```

# XMLHttpRequest

> 자바스크립트를 이용해서 HTTP 요청을 전송하려면 XMLHttpRequest 객체를 사용해야 한다. XMLHttpRequest는 Web API이다. HTTP 전송과 응답 수신을 위한 다양한 메서드와 프로퍼티를 제공한다.

- Web API 라서 브라우저 환경에서만 정상적으로 동작한다.

```js
const xhr = new XMLHttpRequest();
```

## XMLHttpRequest 객체의 프로퍼티와 메서드

[XMLHttpRequest 객체의 프로퍼티와 메서드](https://developer.mozilla.org/ko/docs/Web/API/XMLHttpRequest)

## HTTP 요청 순서

1. XMLHttpRequest.prototype.open 메서드로 HTTP 요청을 초기화한다.
2. 필요에 따라 XMLHttpRequest.prototyp.setRequestHeader 메서드로 특정 HTTP 요청의 헤더 값을 설정한다.
3. XMLHttpRequest.prototype.send 메서드로 HTTP 요청을 전송한다.

```js
const xhr = new XMLHttpRequest();

xhr.open('GET', '/users');

xhr.setRequestHeader('content-type', 'application/json');

xhr.send();
```

### XMLHttpRequest.open 메서드

> 서버에 전송할 HTTP 요청을 초기화한다.

- HTTP 메서드는 GET, POST, PUT, PATCH, DELETE 등이 있다.

## XMLHttpRequest.send 메서드

> open 메서드로 초기화된 HTTP 요청을 서버에 전송한다.

## XMLHttpRequest.prototype.setRequestHeader

> setRequestHeader 메서드는 특정 HTTP 요청의 헤더 값을 설정한다.

- setRequestHeader 메서드는 반드시 open 메서드를 호출한 이후에 호출해야한다.
- `Content-type`은 요청 몸체에 담아 전송할 데이터의 MIME 타입의 정보를 표현한다.

### 대표적인 MIME 타입

- text: text/plain, text/html, text/css, text/javascript
- application: application/json, application/x-www-form-urlencode
- multipart: multipart/formed-data

```js
xhr.setRequestHeader('accept', 'application/json');
```

## HTTP 응답 처리

> 보내는건 ok, 받는건 어떻게 처리할까요?

서버가 전송한 응답을 처리하려면 `XMLHttpRequest` 객체가 발생시키는 이벤트를 캐치해야 한다. onreadystatechange, onload, onerror 같은 이벤트 핸들러 프로퍼티를 갖고, readyState 프로퍼티 값이 변경된 경우 발생하는 readystatechange 이벤트를 캐치해서 HTTP 요청을 처리할 수 있다.

또는 readystatechange 이벤트 대신 load 이벤트를 캐치해도 좋다. load 이벤트는 HTTP 요청이 성공적으로 완료된 경우 발생한다.


```js
var xhr = new XMLHttpRequest();
console.log('UNSENT', xhr.readyState); // readyState will be 0

xhr.open('GET', '/api', true);
console.log('OPENED', xhr.readyState); // readyState will be 1

xhr.onprogress = function () {
    console.log('LOADING', xhr.readyState); // readyState will be 3
};

xhr.onload = function () {
    console.log('DONE', xhr.readyState); // readyState will be 4
};

xhr.send(null);
```
