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

# 2/7 (화)
## 904. Fruit Into Baskets (MEDIUM)
2개의 바구니에 과일을 최대 몇 개 담을 수 있는지 확인하는 문제.
이때 각 바구니에는 한 가지 타입의 과일을 담을 수 있으며
각 과일의 타입은 fruits[i]이다.
ex) [1,2,3,4,1]의 fruits가 있다면 여기서 타입의 개수는 4개입니다. 

### 의사 코드
1. 과일의 타입과 개수를 기록할 basket, window의 시작과 끝을 나타내는 left, right, 최댓값 max를 초기화해 줍니다.
2. fruits.length까지 fruits 배열을 순회한다.
   1. basket에 과일의 타입과 개수를 할당해 준다.
   2. basket의 사이즈가 2보다 큰 경우 즉 과일 타입의 개수가 2개 이상인 경우 while문을 통해 하나가 될 때까지 왼쪽(sliding window의 시작 부분)에서 지워준다.
   3. max 업데이트
3. return max

```
/**
 * @param {number[]} fruits
 * @return {number}
 */
var totalFruit = function(fruits) {
    const basket = new Map()
    let left = 0, right;
    let max = 0 
    
    for(right=0; right < fruits.length; right++){
        basket.set(fruits[right], (basket.get(fruits[right])||0)+1)
        
        // type이 2개 이상이면 2개가 될때까지 지워준다.
        while(basket.size > 2){
            basket.set(fruits[left], basket.get(fruits[left]) - 1)
            
            if(basket.get(fruits[left]) === 0){
                basket.delete(fruits[left])
            }
            left++
        }
        
        max = Math.max(max, right - left + 1)
    }
    
    return max
};
```

# 2/8 (수)
## 45. Jump Game II (MEDIUM)
nums[n-1] 에 도달하는데 필요한 최소 점프 횟수를 구하는 문제.
nums[i]는 최대 점프 거리를 나타내며, 테스트케이스에서 반드시 nums[n-1]에 도달할 수 있는 케이스를 준다.

```
/**
 * @param {number[]} nums
 * @return {number}
 */
var jump = function(nums) {
    // @@@무조건 nums[n-1] 에 도달하도록 테스트 케이스가 주어진다. 
    let ans = 0 
    let end = 0 
    let far = 0
    
    for(let i=0; i<nums.length-1; i++){
        far = Math.max(far, nums[i] + i) // 현재 자기위치에서 갈수 있는 최대 거리
        
        // 현재 위치가 end이면 점프 횟수 추가 
        if(i === end){ 
            ans++
            end = far
        }
    }
    
    return ans
};
```
