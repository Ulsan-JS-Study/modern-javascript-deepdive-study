# 2/6 (월)
## 1470. Shuffle the Array (EASY)
[x1,x2,...,xn,y1,y2,...,yn] -> [x1,y1,x2,y2,...,xn,yn]

### 의사 코드
1. answer 배열 선언 및 초기화
2. for문을 중간값인 n 까지 순회
3. 순회 하면서 answer에 nums[i],nums[i+n] 푸시
4. 리턴 answer

```
/**
 * @param {number[]} nums
 * @param {number} n
 * @return {number[]}
 */
var shuffle = function(nums, n) {
    let answer = []
    for(let i = 0; i<n; i++){
        answer.push(nums[i],nums[i+n])
    }
    return answer 
};
```
