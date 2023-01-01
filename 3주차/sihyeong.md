# 비동기 
실행 컨텍스트 실행 컨텍스트를 알면 호이스팅과 클로저, 코드 실행 순서를 이해하는데 도움이 되기 때문에 정리합니다. 소스 코드의 타입 - 전역 코드 전역 변수를 관리하기 위해 최상위 스코

l-lsh.tistory.com](https://l-lsh.tistory.com/34)

자바스크립트 엔진은 싱글 스레드 방식으로 동작하며, 하나의 콜스택을 가집니다. 

그렇기 때문에 태스크들이 여러 개 있더라도 순차적으로 하나씩 태스크를 처리하는데 

이것을 동기 처리라고 합니다. 

**동기 처리:  현재 실행 중인 태스크가 종료할 때까지 다음에 실행될 태스크가 대기하는 방식**

\- 장점 : 태스크를 순서대로 하나씩 처리하기 때문에 실행 순서가 보장된다.

\- 단점 : 태스크가 하나씩 처리되기 때문에 앞선 태스크가 시간이 오래 걸리는 경우 블로킹이 발생한다.

블로킹 : 작업 중단

```
// setTimeout 유사 함수, delay 후 콜백 함수 실행(func)
function sleep(func, delay) {
    const delayUntil = Date.now() + delay
    
    while(Date.now() < delayUntil);
    func()
 }
 
 function foo(){
     console.log('foo')
 }
 
 function bar(){
     console.log('bar')
 }
 
 sleep(foo, 3 * 1000)
 
 bar() // 3초동안 블로킹이 발생한다.
```

하지만 브라우저가 동작하는 것을 보면 많은 태스크들이 동시에 처리되는것 처럼 느껴집니다. 

이유는 비동기 처리에서 소스코드의 평가와 실행을 제외한 모든 처리는 브라우저 또는 Node.js 에서 실행되기

때문입니다. 

예를 들면  아래와 같은 코드가 있을때, 

```
function foo(){
     console.log('foo')
 }
 
 function bar(){
     console.log('bar')
 }
 
 setTimeout(foo, 3*1000)
 bar() // 블로킹이 발생하지 않는다.
```

### 실행 순서

1\. 전역 코드가 평가 되어 전역 실행 컨텍스트가 생성되고 콜 스택( 실행 컨텍스트 스택)에 푸시된다.

2\. 전역 실행 컨텍스트 실행

3\. setTimeout 함수가 호출되고, setTimeout 함수의 함수 실행 컨텍스트가 생성되고 콜 스택에 푸시된다.

4\. setTimeout 함수가 실행되고, 콜 스택에서 팝된다.

이때, 브라우저에서 타이머가 만료를 기다리다가 만료되면 태스크 큐에 콜백함수를 푸시한다. (여기서는 foo) (병행 처리 )

5\. bar 함수가 호출되어 콜 스택에 푸시되고, 실행 종료 후 콜 스택에서 팝된다.

6\. 전역 실행 컨텍스트가 팝된다.

7\. 이때 콜 스택에 처리할 태스크가 없기 때문에 이벤트 루프에 의해 태스크 큐에서 대기 중인 foo가 콜 스택에 푸시된다.

8\. foo 실행 컨텍스트가 콜스택에 푸시되고, 실행 후 콜스택에서 팝된다. 

**태스크 큐**: 비동기 함수의 콜백 함수 또는 이벤트 핸들러가 일시적으로 보관되는 영역이다.

프로미스의 후속 처리 메서드(then, catch, finally)는 마이크로태스크 큐에 보관되며 우선순위는 마이크로태스크 큐가 더 높다.

**이벤트 루프**: 콜 스택을 관찰하다가 비어있으면, 태스크 큐에 대기 중인 함수를 체크하고 있다면 콜스택으로 이동시킨다.

이처럼 비동기 처리는 브라우저에서 병행 처리 되기 때문에 블로킹이 발생하지 않고 동시에 태스크를 수행하는 것처럼 보입니다.

**비동기 처리: 현재 실행 중인 태스크가 종료되지 않은 상태라 해도 다음 태스크를 곧바로 실행한다.**

\- 장점: 블로킹이 발생하지 않는다.

\- 단점: 태스크의 실행 순서가 보장되지 않는다. 

타이머 함수인 setTimeout, setInterval, HTTP 요청, 이벤트 핸들러는 비동기 처리 방식으로 동작한다.

# Ajax

### Ajax란? 

Ajax란 자바스크립트를 사용하여 브라우저가 서버에게 비동기 방식으로 데이터를 요청하고,

서버가 응답한 데이터를 수신하여 웹페이지를 동적으로 갱신하는 프로그래밍 방식을 말합니다. 

Ajax가 나오기 이전의 웹페이지는 html 태그로 시작해서 html 태그로 끝나는 완전한 HTML을

서버로부터 전송받아 웹페이지 전체를 처음부터 다시 렌더링 하는 방식으로 동작했습니다. 

그렇기 떄문에 다음과 같은 문제점들이 있었습니다.

1\. 변경할 필요가 없는 부분까지 포함된 완전한 HTML을 서버로부터 매번 다시 전송받기 때문에

    불필요한 데이터 통신이 발생한다.

2\. 매번 새로운 HTML을 전송 받기 때문에 깜빡이는 현상이 발생한다.

3\. 클라이언트와 서버와의 통신이 동기 방식으로 동작하기 때문에 서버로 부터 응답이 없을 때 블로킹 된다.

하지만 Ajax가 나온뒤로 위의 단점들을 해결할 수 있게 되었습니다.

1\. 변경에 필요한 데이터만 서버로부터 전송받기 때문에 불필요한 통신이 발생하지 않는다.

2\. 변경할 필요가 없는 부분은 다시 렌더링 하지 않기 때문에 깜박이는 형상이 발생하지 않는다.

3\. 클라이언트와 서버와의 통신이 비동기 방식으로 동작하기 때문에 블로킹이 발생하지 않는다.

## JSON

\- 클라이언트와 서버 간의 HTTP 통신을 위한 텍스트 데이터 포맷이며 대부분의 프로그래밍 언어에서 사용가능하다.

\- 객체 리터럴과 유사하게 키와 값으로 구성된 순수한 텍스트이다.

\- JSON의 키는 반드시 큰따옴표("")로 묶어야 한다. 

\- 값은 객체 리터럴과 같은 표기법을 그대로 사용가능하다. ( 모든 타입 가능 )

### JSON.stringify

객체를 JSON 포맷의 문자열로 변환한다.

클라이언트가 서버로 객체를 전송하려면 객체를 문자열화 해야하는데 이를 직렬화라고 한다. (배열도 가능)

```
const obj = {
    name: "Lee"
}

const arr = [{id:1}]

const json = JSON.stringify(obj)
const jsonArr = JSON.stringify(arr)

console.log(json) // {"name" : "Lee"}
console.log(jsonArr) // '[{"id":1}]'
```

### JSON.parse

서버로부터 클라이언트에게 전송된 JSON 데이터는 문자열이기 때문에 이를 객체로서 사용하려면 

JSON 포맷의 문자열을 객체화해야 한다. 이를 역직렬화라 한다.

```
const obj = {
    name: "Lee"
}

const json = JSON.stringify(obj)
const parsed = JSON.parse(json)
```

# REST API

## REST

REST란 HTTP를 기반으로 클라이언트가 서버의 리소스에 접근하는 방식을 규정한 아키텍쳐이다. 

\- REST의 기본원칙을 성실히 치킨 서비스 디자인을 RESTful이라고 표현한다.

\- REST API는 REST를 기반으로 서비스 API를 구현한 것을 의미한다. 

### REST API 구성

\- 자원 :  URL

\- 행위 :  HTTP 요청 메서드

\- 표현 :  페이로드 

### REST API 설계 원칙

1\. URL는 리소스를 표현해야 한다.

리소스를 식별할 수 있는 이름은 동사보다는 명사를 사용한다.

```
# bad
GET /getTodos/1

# good
GET /Todos/1
```

2\. 리소스에 대한 행위는 HTTP 요청 메서드로 표현한다.

요청 메서드는 크게 5가지가 있다. (GET, POST, PUT, PATCH, DELETE)

| HTTP 요청 메서드 | 종류 | 목적 | 페이로드 |
| --- | --- | --- | --- |
| GET | index / retrieve | 모든 / 특정 리소스 취득 | x |
| POST | create | 리소스 생성 | o |
| PUT | replace | 리소스의 전체 교체 | o |
| PATCH | modify | 리소스의 일부 수정 | o |
| DELETE  | delete | 모든 / 특정 리소스 삭제 | x |

### fetch를 사용하여 실습!

브라우저에 내장된 함수이기 때문에 따로 설치없이 바로 사용할 수 있기 때문에 fetch를 예시로 들겠습니다.

\- GET: 보낼 데이터가 없기 때문에, headers와 body 옵션이 필요 없다. 

```
// GET 요청
fetch('https://jsonplaceholder.typicode.com/posts')
  .then((response) => response.json())
  .then((json) => console.log(json));
```

\- POST: fetch(url, {method, headers, body})

-   method = HTTP 요청 메서드
-   headers = HTTP 헤더
    -   "Content-Type" : 요청 몸체에 담아 전송할 데이터의 타입 정보를 표현한다. 
-   body = 전송할 데이터 (페이로드)

```
// POST 
// 이때 페이로드값이 객체인경우 직렬화 한다음 전달한다.
fetch('https://jsonplaceholder.typicode.com/posts', {
  method: 'POST',
  body: JSON.stringify({
    title: 'foo',
    body: 'bar',
    userId: 1,
  }),
  headers: {
    'Content-type': 'application/json; charset=UTF-8',
  },
})
  .then((response) => response.json())
  .then((json) => console.log(json));
```

\- PUT : fetch(url, {method, headers, body})

```
// PUT
fetch('https://jsonplaceholder.typicode.com/posts/1', {
  method: 'PUT',
  body: JSON.stringify({
    id: 1,
    title: 'foo',
    body: 'bar',
    userId: 1,
  }),
  headers: {
    'Content-type': 'application/json; charset=UTF-8',
  },
})
  .then((response) => response.json())
  .then((json) => console.log(json));
```

\- PATCH : fetch(url, {method, headers, body})

```
// PATCH
fetch('https://jsonplaceholder.typicode.com/posts/1', {
  method: 'PATCH',
  body: JSON.stringify({
    title: 'foo',
  }),
  headers: {
    'Content-type': 'application/json; charset=UTF-8',
  },
})
  .then((response) => response.json())
  .then((json) => console.log(json));
```

\- DELETE : fetch(url,{method})

```
// DELETE
fetch('https://jsonplaceholder.typicode.com/posts/1', {
  method: 'DELETE',
});
```

# 프로미스

## 콜백 함수

콜백 함수는 ES6이전에 비동기 처를 위해 사용하던 하나의 패턴이지만, 다음과 같은 단점들이 있어 ES6에서 프로미스 패턴이 도입된다.

1\. 콜백 헬로 인해 가독성이 나쁘다.

비동기 처리 결과를 가지고 또다시 비동기 함수를 호출하여 콜백 함수 호출이 중첩되어 복잡도가 높아지는 현상

```
let g = 0
setTimeout(() => {g = 100;},0)
console.log(g) // 0
```

위의 코드에서 처럼 비동기 처리 결과를 변수에 할당하려고 하면 기대한 대로 동작하지 않는다.

setTimeout은 실행후 브라우저에서 타이머가 만료되면 콜백 함수를 태스크 큐로 이동한다, 그리고 콜 스택이 비어 있으면,

이때 태스크 큐에서 콜스택으로 이동하고 콜백함수가 실행된다.

그렇기 때문에 g = 100 이 되는 시점에는 이미 console.log(g)가 출력된 이후이다.

위와 같은 이유로 비동기 함수에 비동기 처리 결과에 대한 후속 처리를 수행하는 콜백함수를 전달하여 결과를 처리한다.

그리고 이것이 콜백 헬이다.

2\. 에러 처리의 한계

setTimeout 의 콜백함수가 실행되는 시점에는 이미 setTimeout이 콜 스택에서 제거된 상태이기 때문에 

setTimoue 함수의 콜백 함수가 발생시킨 에러는 catch블록에서 캐치되지 않는다. 

```
try{
    setTimeout(()=> { throw new Error('Error!')},1000)
} catch(e){
    // 에러 캐치 실패
    console.error('캐치한 에러', e)
}
```

## 프로미스

프로미스는 콜백 패턴이 가진 단점을 보완하기 위해 도입된 패턴입니다. 

### 프로미스 생성

new 연산자와 함께 호출하여 프로미스를 생성한다.

프로미스 (Promise 객체): 비동기 처리 결과와 처리 상태를 가지는 객체이다.

```
// resolve = 성공
// reject = 실패
const promise = new Promise((resolve, reject) => {
    if( 비동기 처리 성공){
        resolve('result')
    else{
        reject('failure reason')
    }
}
```

### 프로미스 상태

| 프로미스의 상태 정보 | 의미 | 상태 변경 조건 |
| --- | --- | --- |
| pending | 비동기 처리가 아직 수행되지 않은 상태 | 프로미스가 생성된 직후 기본 상태 |
| fulfilled | 비동기 처리가 수행된 상태(성공) | resolve 함수 호출 |
| rejected | 비동기 처리가 수행된 상태(실패) | reject 함수 호출 |

fulfilled, rejected 상태 (settled)에서는 더 이상 상태 변경이 안된다.

### 프로미스의 후속 처리 메서드

프로미스의 상태가 fulfilled 또는 rejected 상태일 때 사용하며 then, catch, finally가 있다. 

\- then

then 메서드는 두 개의 콜백 함수를 인수로 받는다.

첫 번째는 프로미스의 비동기 처리결과, 두 번째는 프로미스의 에러이다.

```
new Promise(resolve => resolve('fulfilled'))
    .then(v => console.log(v), e => console.log(e)) // fulfilled

new Promise((_, reject) => reject(new Error('rejected')))
    .then(v => console.log(v), e => console.error(e)) // Error: rejected
```

then 메서드는 언제나 프로미스를 반환한다.

만약 then 메서드의 콜백함수가 프로미스를 반환하면 그 프로미스를 반환하고

프로미스가 아닌 값을 반환하면, 그 값을 암묵적으로 resolve 또는 reject 하여 프로미스를 생성해 반환한다.

\- catch

catch 메서드는 한 개의 콜백 함수를 인수로 전달받으며, 프로미스가 rejected 상태인 경우에만 호출된다.

```
new Promise(_, reject) => reject(new Error('rejected')))
    .catch(e => console.log(e)); // Error: rejected
```

then과 동일하게 동작한다. 따라서 언제나 프로미스를 반환한다. 

\- finally

finally 메서드도 한 개의 콜백 함수를 인수로 전달받는다.

특이한 점은 프로미스의 성공(fulfilled) 또는 실패(rejected)와 상관없이 무조건 한번 호출되기 때문에

프로미스의 상태와 상관없이 공통적으로 수행해야 할 처리 내용이 있을 때 유용하다.

```
// 성공/실패 여부 상관없이 마지막에 한번 호출된다.
new Promise((resolve) => resolve('fulfilled'))
    .then(v => console.log(v) // fulfilled
    .finally(()=> console.log('finally')); //finally
```

언제나 프로미스를 반환한다.

### 프로미스의 에러 처리 

then 메서드의 두 번째 콜백 함수로 에러 처리를 하는 것보단 catch 메서드를 사용하는 것이 가독성이 좋고 명확하다.

### 프로미스 체이닝

then, catch, finally는 프로미스의 상태가 fulfilled 또는 rejected 상태일 때 사용가능하다. 

그리고 then, catch, finally는 모두 프로미스를 반환하기 때문에 

아래 코드처럼 then -> then -> catch처럼 연달아서 사용하는 것이 가능하다. 그리고 이것을 프로미스 체이닝이라고 한다.

프로미스 체이닝을 통해서 콜백 헬을 해결한다.

```
const url = 'https://jsonplaceholder.typicode.com';

fetch(`${url}/posts/1`)
    .then(({userId}) => fetch(`${url}/users/${userId}`))
    .then(userInfo => console.log(userInfo))
    .catch(err => console.error(err))
```

### 프로미스의 정적 메서드 

프로미스는 주로 생성자 함수로 사용되지만 객체이기 때문에 메서드를 가질 수 있습니다.

1\. Promise.resolve / Promise.reject

이미 존재하는 값을 래핑 하여 프로미스를 생성하기 위해 사용한다.

```
const resolvedPromise = Promise.resolve([1,2,3])
// promise이기 때문에 후속 처리 메서드를 사용할 수있다.
resolvedPromise.then(console.log) // [1,2,3]

//동일 하다
const resolvedPromise = new Promise(resolve => resolve([1,2,3])
resolvedPromise.then(console.log)
```

2\. Promise.all

여러 개의 비동기 처리를 모두 병렬 처리할 때 사용한다. 

만약 3개의 비동기 처리 함수가 각각 3초, 5초, 10초 가 걸리는 경우 각각 실행했을 때는 총 18초가 소요됩니다.

하지만 Promise.all을 사용하면 약 10초만 소요됩니다. 

\- 프로미스 요소를 갖는 배열 등의 이터러블을 인수로 전달한다.

\-  Promise.all가 종료되는 시간은 가장 늦게 fulfilled 상태가 되는 프로미스의 처리 시간보다 조금 더 길다.

\- 모든 프로미스가 fulfilled가 되면 모든 처리 결과를 배열에 저장해 새로운 프로미스를 반환한다.

\- 만약 첫 번째 프로미스가 가장 나중에 성공되어도 Promise.all 메서드는 첫 번째부터 차례대로 배열에 저장하기 때문에

   처리 순서가 보장된다.

\- 만약 인수로 전달받은 프로미스가 하나라도 rejected 상태가 되면 즉시 종료한다.

단, 각 비동기 처리는 서로 의존하지 않고 개별적으로 수행되어야만 합니다. (앞선 비동기 처리 결과를 사용하지 않는다.)

```
// 만약 requestData3 에서 rejected되면 약 10초에 종료된다.
const requestData1 = () => new Promise(resolve => setTimeout(() => resolve(1), 3000));
const requestData2 = () => new Promise(resolve => setTimeout(() => resolve(2), 5000));
const requestData3 = () => new Promise(resolve => setTimeout(() => resolve(3), 10000));

// 세 개의 비동기 처리를 병렬로 처리
Promise.all([requestData1(), requestData2(), requestData3()])
    .then(console.log) // [1, 2, 3]
    .catch(console.error);
```

3\. Promise.race

가장 먼저 처리되는 것을 반환한다.

\- Promise.all 메서드와 동일하게 프로미스를 요소로 갖는 배열 등의 이터러블을 인수로 전달받는다.

\- 가장 먼저 fulfilled 상태가 된 프로미스의 처리결과를 resolve 하는 새로운 프로미스를 반환한다.

\- 하나라도 rejected 상태가 되면 에러를 reject 하는 새로운 프로미스를 즉시 반환한다.

```
// 가장 먼저 처리되는것
Promise.race([
    new Promise(resolve => setTimeout(() => resolve(1), 3000)),
    new Promise(resolve => setTimeout(() => resolve(2), 2000)),
    new Promise(resolve => setTimeout(() => resolve(3), 1000))
 ])
    .then(console.log) // 3
    .catch(console.error)
```

4\. Promise.allsettled

\- 동일하게 프로미스를 요소로 갖는 배열 등의 이터러블을 인수로 전달받는다.

\- 비동기 처리 성공, 실패 상관없이 모두 settled 상태가 되면 인수로 받은 모든 프로미스들의 처리 결과를 배열로 반환한다.

settled ( 비동기 처리가 수행된 상태 즉 pending이 아니라 fulfilled 또는 rejected 상태 )

```
Promise.allSettled([
    new Promise(resolve => setTimeout(() => resolve(1), 2000)),
    new Promise((_,reject) => setTimeout(() => reject(new Error('Error!')), 1000))
]).then(console.log)

/*
[ 
    // 성공일 경우 비동기 처리 상태와, 결과를 리턴한다.
    {status: "fulfilled", value: 1},
    // 실패일 경우 비동기 처리 상태와, 에러를 나타내는 reason을 리턴한다.
    {status: "rejected", reason: Error: Error! at <anonymouse>3:54}
]
*/
```

### 마이크로태스크 큐

```
setTimeout(() => console.log(1), 0)

Promise.resolve()
    .then(() => console.log(2))
    .then(() => console.log(3))
```

위의 코드의 출력 순서는 2 -> 3 -> 1이다. 

이유는 프로미스의 후속 처리 메서드 (then, catch, finally)는 태스크 큐가 아닌 마이크로태스크 큐에 저장된다.

그리고 이벤트 루프에 의해 콜스택에 이동되는 우선순위는 마이크로태스크 큐가 태스크 큐보다 높기 때문에

마이크로태스크 큐의 함수들을 먼저 콜스택으로 이동시키고 실행한다. 

우선순위: 마이크로태스크 큐 > 태스크 큐

# 알고리즘 풀이 

## Longest Substring Without Repeating Characters
```
/**
 * @param {string} s
 * @return {number}
 */
var lengthOfLongestSubstring = function(s) {
    const map = new Map()
    let ans = 0 //aab
    let left = 0
    
    for(let i=0; i<s.length; i++){
        const char = s[i]
        if(map.has(s[i])){
            left = Math.max(map.get(s[i]), left)
        }
        ans = Math.max(ans, i + 1 - left) //0
        map.set(char, i+1)
    }
    
    return ans
};
```

## Find Players With Zero or One Losses
```
/**
 * @param {number[][]} matches
 * @return {number[][]}
 */
var findWinners = function(matches) {
    // [한번도 패배하지 않은 사람들, 딱 한번만 패매한 사람들]
     const total = new Map()
    
    for(const [winner, loser] of matches){
        total.set(winner, (total.get(winner)||0))
        total.set(loser, (total.get(loser)||0)+1)
    }
    
    const winner = [] , loser = []
    
    // answer[0] = 0, answer[1] = 1 그 외에는 조건에 부합하지 않는다. 
    for(const [key, value] of total){
        if(value === 0) winner.push(key)
        if(value === 1) loser.push(key)
    }
    
    // 오름차순 정렬
    winner.sort((a,b)=>a-b)
    loser.sort((a,b)=>a-b)
    
    return [winner, loser]
};
```
