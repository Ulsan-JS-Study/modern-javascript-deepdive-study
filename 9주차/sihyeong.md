# 2/13 
## 1523. Count Odd Numbers in an Interval Range (EASY) (GREEDY)

low, high가 주어질때 범위내의 홀수 개수 구하는 문제

### 문제 접근법

low 부터 high 까지 for문을 순회하면서 홀수 개수 체크

```
/**
 * @param {number} low
 * @param {number} high
 * @return {number}
 */
var countOdds = function(low, high) {
    let ans = 0 
    for(let i=low; i<=high; i++){
      if(i%2 !== 0) ans++
    }
    return ans
};
```

하지만 이렇게 풀면 엄청난 시간이 소요된다.

low, high 모두 짝수인경우 와 아닌경우로 문제를 풀면 

low ~ high까지 순회할 필요 없기 때문에 훨씬 빠르다.

- testcase 1. 모두 짝수인경우 low=8, high=12 => (12 - 8) / 2
- testcase 2. 하나라도 홀수인경우 low=3, hight=7 => Math.floor((high-low)/2) + 1 

### 개선된 코드
```
/**
 * @param {number} low
 * @param {number} high
 * @return {number}
 */
var countOdds = function(low, high) {
    if(low%2==0 && high%2==0) return (high-low)/2
    return 1 + Math.floor((high-low)/2)
}; 
```

# 2/14(화) 
## 67. Add Binary (EASY)

이진 문자열 a,b가 주어질때 둘의 합을 구하는 문제

### 문제 접근법

이진 문자열을 10진수로 변경한 후 더하고 나서 다시 이진수로 변경

```
/**
 * @param {string} a
 * @param {string} b
 * @return {string}
 */
var addBinary = function(a, b) {
    return (parseInt(a,2) + parseInt(b,2)).toString(2)
};
```
하지만 a or b의 숫자가 엄청 크면 틀린 값이 나옵니다.
그래서 BigInt를 사용합니다.
![image](https://user-images.githubusercontent.com/59095793/218970430-2d47b9b8-d68b-4910-b9a8-b098f4e3a3db.png)

```
/**
 * @param {string} a
 * @param {string} b
 * @return {string}
 */
var addBinary = function(a, b) {
    return (BigInt(`0b${a}`) + BigInt(`0b${b}`)).toString(2)
};
```


# 2/15(수) 
## 989. Add to Array-Form of Integer (EASY)

num 배열의 숫자와 k 의 합 구하는 문제

### 문제 접근법

num 배열을 문자열로 바꾸고 숫자로 바꾼뒤에 k(숫자)와 더한후 다시 문자열로 바꾸고 배열화 한다!

하지만 num을 숫자로 바꿀때 숫자 범위가 커지면 숫자가 잘리기 때문에 BigInt로 변경해준다.
이때 하나만 BigInt로 변경하면 에러가 발생하기 때문에 k 또한 BigInt로 값을 변경해준뒤 나머지 과정을 진행한다.

```
/**
 * @param {number} low
 * @param {number} high
 * @return {number}
 */
var countOdds = function(low, high) {
    return (BigInt(num.join('')) + BigInt(k)).toString().split('')
};
```
