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

# 2/16(목) 
## 104. Maximum Depth of Binary Tree (EASY)

트리의 깊이 구하는 문제 (가장 깊은)

### 문제 접근법
왼쪽부터 탐색하며 하위 노드가 없을 때 0을 리턴

자식 노드를 모두 탐색하고 둘중 큰 값 + 1 을 리턴 

![image](https://user-images.githubusercontent.com/59095793/219292142-fd90679c-773c-4e6b-81af-ff73e47d302f.png)

```
/**
 * Definition for a binary tree node.
 * function TreeNode(val, left, right) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.left = (left===undefined ? null : left)
 *     this.right = (right===undefined ? null : right)
 * }
 */
/**
 * @param {TreeNode} root
 * @return {number}
 */
var maxDepth = function(root) {
    if(!root) return 0

    return Math.max(maxDepth(root.left), maxDepth(root.right)) + 1
};
```

# 2/17(금) 
## 783. Minimum Distance Between BST Nodes (EASY)

트리의 각 노드 값들의 가장 작은 차이를 구하는 문제

### 문제 접근법

BST의 특징은 왼쪽은 루트 기준 작은 수, 오른쪽은 루트 기준 큰 수를 가진다.

그리고 전위 순회를 통해 트리를 탐색하면 트리의 값들을 오름차순으로 얻을 수 있다.

이것을 이용해서 배열에 오름차순 정렬된 트리의 값들을 구한다.

그리고 이중 포문을 순회하면서 가장 작은 차이를 가지는 값을 구한다.

```
/**
 * Definition for a binary tree node.
 * function TreeNode(val, left, right) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.left = (left===undefined ? null : left)
 *     this.right = (right===undefined ? null : right)
 * }
 */
/**
 * @param {TreeNode} root
 * @return {number}
 */
var minDiffInBST = function(root) {
    const tmp = []
    
    const dfs = (root) => {
        if(!root) return 
        
        
        dfs(root.left)
        tmp.push(root.val)
        dfs(root.right)
    }
    
    dfs(root)
    let min = Infinity

    for(let i=0; i<tmp.length; i++){
        for(let j=i+1; j<tmp.length; j++){
            min = Math.min(min, tmp[j] - tmp[i])
        }
    }
    
    return min
};
```

# 2/18(토) 
## 226. Invert Binary Tree (EASY)

트리를 반대로 뒤집는 문제

### 문제 접근법

그냥 뒤집는다
이때, 탐색중 노드가 없다면 null을 리턴해줘야 에러가 발생하지 않는다.

```
/**
 * Definition for a binary tree node.
 * function TreeNode(val, left, right) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.left = (left===undefined ? null : left)
 *     this.right = (right===undefined ? null : right)
 * }
 */
/**
 * @param {TreeNode} root
 * @return {TreeNode}
 */
var invertTree = function(root) {
    if(!root) return null
    
    const left = invertTree(root.left)
    const right = invertTree(root.right)
    
    root.left = right
    root.right = left
    
    return root
   
};
```

## 추가 문제 

## 2028. Find Missing Observations (MEDINUM)

rolls 의 합과 n개의 조합의 합의 평균이 mean인 n개의 조합을 찾는 문제이다.  (정확히 일치 해야한다.)

이때 n 은 1 ~ 6 의 범위를 갖는다

### 문제 접근법


우선 불가능한 경우를 먼저 리턴해준다.
1. n개의 조합이 모두 가장 큰수를 가지지만 평균 값이 mean 보다 작은 경우
2. n개의 조합이 모두 가장 작은수를 가지지만 평균 값보다 큰 경우

그리고 평균값이 mean이 되기 위한 수를 구한다

그리고 그 수를 이용하여 조합을 찾는다.


조합 찾는 예시

n = 5, k = 13 

mod = 3

val = 2

```
/**
 * @param {number[]} rolls
 * @param {number} mean
 * @param {number} n
 * @return {number[]}
 */
var missingRolls = function(rolls, mean, n) {
    const sum = rolls.reduce((a,b)=>a+b,0), len = rolls.length + n

    if((sum + (6 * n)) / len  < mean || (sum + (1 * n)) / len > mean) return []

    // (sum + [?]) /  len === mean 이 되는데 필요한 값
    let k = (len * mean) - sum 

    const res = [] 

    let mod = k%n
    const val = Math.floor(k/n)
    for(let i=0; i<n; i++){
        if(mod){
            res.push(val+1)
            mod--
        }
        else res.push(val)    
    }
    
    return res
};

```

## 240. Search a 2D Matrix II (MEDINUM)

matrix에서 target을 찾는 문제이다, 만약 있다면 true, 없다면 false를 리턴한다.

### 문제 접근법

row, col 모두 오름 차순 정렬되 있어서 이진 탐색으로 풀었다

처음에는 각 row, col을 이진 탐색하여 문제를 풀었지만 꽤 많은 시간이 소요되었기 때문에

솔루션을 보고 다시 풀이

같은 이진 탐색이지만 



```
/**
 * @param {number[][]} matrix
 * @param {number} target
 * @return {boolean}
 */
// const rowBinarySearch = (matrix, col, start, end, target) => {
//   let left = start, right = end;

//   while (left <= right) {
//     const mid = (left + right) >> 1;
//     if (matrix[col][mid] === target) return true;
//     if (matrix[col][mid] > target) right = mid - 1;
//     else left = mid + 1;
//   }

//   return false;
// };

// const colBinarySearch = (matrix, row, start, end, target) => {
//   let left = start, right = end;

//   while (left <= right) {
//     const mid = (left + right) >> 1;
//     if (matrix[mid][row] === target) return true;
//     if (matrix[mid][row] > target) right = mid - 1;
//     else left = mid + 1;
//   }

//   return false;
// };

// var searchMatrix = function(matrix, target) {
//   let start = 0;
//   const m = matrix.length, n = matrix[0].length - 1;

//   for (let row = 0; row < m; row++) {
//     const rowSearch = rowBinarySearch(matrix, row, start, n, target);
//     const colSearch = colBinarySearch(matrix, row, start, m - 1, target);

//     if (rowSearch || colSearch) return true;
//     start++;
//   }

//   return false;
// };

var searchMatrix = function(matrix, target) {
 let row = matrix.length - 1,
        col = 0,
        yMax = matrix[0].length;
    

    while (row > -1 && col < yMax) {
        if (matrix[row][col] === target) return true;
        if (target < matrix[row][col]) row--;
        else col++;
    }

    return false;
}
```
