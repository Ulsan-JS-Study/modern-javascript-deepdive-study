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
