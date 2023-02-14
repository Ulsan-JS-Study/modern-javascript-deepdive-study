
# 2/6 (월)
## 1470. Shuffle the Array (EASY)
[x1,x2,...,xn,y1,y2,...,yn] -> [x1,y1,x2,y2,...,xn,yn]

### 의사 코드
1. answer 배열 선언 및 초기화
2. for문을 중간값인 n 까지 순회
3. 순회 하면서 answer에 nums[i],nums[i+n] 푸시
4. 리턴 answer
5. 솔직히 n도 필요없을듯 answer.length/2로 구해짐

```
/**
 * @param {number[]} nums
 * @param {number} n
 * @return {number[]}
 */
var shuffle = function(nums, n) {
    let answer = [];
    for(let i =0;i<n;i++){
        answer.push(nums[i])
        answer.push(nums[i+n])
    }
    return answer
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
    var output = 0
    var jump = 0
    var far = 0
    for(i=0;i<nums.length-1;i++){
        jump = Math.max(jump , nums[i] + i)
        if(i == far){
            far = jump
            output ++
        }
    }
    return output
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

그리고 키값을 순회 하면서 비교를 해주는데 이때 중복된 count가 있는지 체크하고 카운트해 줍니다.

```js run
var distinctNames = function (ideas) {
        let output = 0;
        const ideasMap = new Map();
        const suf = Array.from({ length: 26 }, () => new Set());
        for (const A of ideas) suf[A.charCodeAt(0) - 97].add(A.slice(1));
        for (i = 0; i < 26; i++) {
          if (suf[i].size === 0) continue;
          for (let j = i + 1; j < 26; j++) {
            let count = 0;
            for (let x of suf[i]) {
              if (suf[j].has(x)) {
                count += 1;
              }
            }
            if (suf[j].size === 0) continue;
            output += (suf[j].size - count) * (suf[i].size - count);
            //해당 set의 사이즈에서 count의 갯수만큼 뺴준값을 서로 곱해준다.
          }
        }
        return output * 2;
        //a ,b === b , a이므로 *2해준다.
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

### 의사 코드
1. 변수 queue에 배열을 초기화하고 grid를 순회하면서 땅의 좌표와 거리 0을 푸시해준다.
2. maxDistance를 나타내는 변수를 선언하고 -1로 초기화해준다.
3. BFS 탐색
    1. maxDistance 업데이트
    2. 4가지 방향으로 탐색을하며 grid가 유효한지 체크한다
        1. 이동한 좌표가 유효하다면 grid에서 해당 위치를 땅으로 변경하고 queue에 푸시해준다.
4. maxDis를 리턴한다. 이때 maxDis가 0이라면 육지 또는 물이 없다고 판단하여 -1을 리턴해준다.

```js run
var maxDistance = function (grid) { 
    let queue = []
    let containsZero = false
    for (let row = 0; row<grid.length; row++) {
        for (let column = 0; column<grid[0].length; column++) {
            if (grid[row][column] === 1) {
                queue.push([row,column,0])
            } else {
                containsZero = true
            }
        }
    }
    if (queue.length === 0 || !containsZero) {
        return -1
    }
    
    let maxDis  = -1
    let direction = [[0,1], [0,-1], [1,0], [-1,0]]
    while (queue.length > 0) {
        let cell = queue.shift()
        maxDis = Math.max(maxDis, cell[2])
        for (let i = 0; i<direction.length; i++) {
            let x = direction[i][0] + cell[0]
            let y = direction[i][1] + cell[1]
            if (x >= 0 && x < grid.length && y >= 0 && y < grid[0].length && grid[x][y] === 0) {
                queue.push([x, y, cell[2] + 1]);
                grid[x][y] = 1;
            }
        }
    }
    return maxDis
}
```
