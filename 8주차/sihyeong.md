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

# 2/9 (목)
## 2306. Naming a Company (HARD)
ideas에서 두 개의 idea를 선택한 후, 각각의 idea의 앞글자를 스왑 했을 때
기존 ideas에 없으면 카운트하는 문제.

### 문제 접근법
ideas = ["coffee", "donuts", "time", "toffee"]가 있을 때

각 idea의 첫 번째 문자를 스왑 한 결과가 중복되지 않으려면 suffix가 중복되지 않아야 합니다.
그래서 아래와 같이 idea의 첫 번째 글자를 기준으로 suffix를 그룹화 합니다.

c : [ "offee" ]

d : [ "onuts" ]

t : [ "ime", "offee"] 

그리고 키값을 순회 하면서 비교를 해주는데 이때 중복된 suffix가 있는지 체크하고 카운트해 줍니다.

```
/**
 * @param {string[]} ideas
 * @return {number}
 */
 
 // ["coffee","donuts","time","toffee"]
var distinctNames = function(ideas) {
    const map = new Map()
    
    // c : offee
    // t : offee, ime
    // d : onuts
    for(const idea of ideas) {
        const prefix = idea[0]
        const suffix = idea.slice(1)
        
        if(!map.has(prefix)) map.set(prefix, new Set())
        map.get(prefix).add(suffix)
    }
    
    const keys = [...map.keys()]
    let count = 0 
    
    
    for(let i=0; i<keys.length; i++){
        const frist = map.get(keys[i])
        
        for(let j=i+1; j<keys.length; j++){
            const second = map.get(keys[j])
            let sameCount = 0
            
            for(const suffix of frist){
                // c : offee, t:offee 중복은 스왑을해도 사용불가능함
                if(second.has(suffix)) sameCount++
            }
            
            // 2를 곱하는 이유 ("coffee","donuts"), ("donuts","coffee")
            count += 2 * (frist.size - sameCount) * (second.size - sameCount)
        }
    }
   
    return count
};
```

# 2/10 (금)
## 1162. As Far from Land as Possible (MEDINUM)
grid[i] === 0 // 물
grid[i] === 1 // 땅

물과 가장 멀리 떨어진 땅의 거리 찾는 문제

### 문제 접근법
동시에 탐색해야하기 때문에 BFS 사용하여 풀이
모든 땅에서 동시에 탐색을 진행하면서 최대 거리를 찾는다.
manhattan distance 사용 x 

### 의사 코드
1. 변수 queue에 배열을 초기화하고 grid를 순회하면서 땅의 좌표와 거리 0을 푸시해준다.
2. maxDistance를 나타내는 변수를 선언하고 -1로 초기화해준다.
3. BFS 탐색
    1. maxDistance 업데이트
    2. 4가지 방향으로 탐색을하며 grid가 유효한지 체크한다
        1. 이동한 좌표가 유효하다면 grid에서 해당 위치를 땅으로 변경하고 queue에 푸시해준다.
4. maxDistance를 리턴한다. 이때 maxDistance가 0이라면 육지 또는 물이 없다고 판단하여 -1을 리턴해준다.

```
/**
 * @param {number[][]} grid
 * @return {number}
 */
var maxDistance = function(grid) {
    const m = grid.length, n = grid[0].length
    const directions = [[-1,0],[0,1],[1,0],[0,-1]]
    
    const gridIsValid = (x,y) => {
        return x >= 0 && x < m && y >= 0 && y < n && grid[x][y] === 0
    }
    
    let queue = [] 
    
    // 땅의 위치를 찾아서 queue에 push
    for(let row=0; row<m; row++){
        for(let col=0; col<n; col++){
            if(grid[row][col] === 1) {
                queue.push([row,col,0])
            }
        }
    }
    
    let maxDistance = -1
    
    while(queue.length){
        const nextQueue = []
        for(const [x,y,distance] of queue){
            maxDistance = Math.max(maxDistance, distance)
            for(const [dx,dy] of directions){
                const nx = x + dx, ny = y + dy

                if(gridIsValid(nx,ny)){
                    grid[nx][ny] = 1
                    nextQueue.push([nx,ny,distance+1])
                }
            }
        }
        queue = nextQueue
    }
    
    return maxDistance === 0 ? -1 : maxDistance
};
```
