# 2/13 
## 1523. Count Odd Numbers in an Interval Range (EASY) (GREEDY)

low, high가 주어질때 범위내의 홀수 개수 구하는 문제

### 문제 접근법

low 부터 high 까지 for문을 순회하면서 홀수 개수 체크

```js run
/**
 * @param {number} low
 * @param {number} high
 * @return {number}
 */
var countOdds = function (low, high) {
        let count = 0;
        for (let i = low; i <= high; i++) {
          if (i % 2 === 1) count += 1;
        }
        return count;
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

```js run
/**
 * @param {string} a
 * @param {string} b
 * @return {string}
 */
 
 var addBinary = function (a, b) {
        const binaryA = "0b" + String(a);
        const binaryB = "0b" + String(b);
        return (BigInt(binaryA) + BigInt(binaryB)).toString(2);
      };
```
하지만 a or b의 숫자가 엄청 크면 틀린 값이 나옵니다.
그래서 BigInt를 사용합니다.
![image](https://user-images.githubusercontent.com/59095793/218970430-2d47b9b8-d68b-4910-b9a8-b098f4e3a3db.png)


# 2/15(수) 
## 989. Add to Array-Form of Integer (EASY)

num 배열의 숫자와 k 의 합 구하는 문제

### 문제 접근법

num 배열을 문자열로 바꾸고 숫자로 바꾼뒤에 k(숫자)와 더한후 다시 문자열로 바꾸고 배열화 한다!

하지만 num을 숫자로 바꿀때 숫자 범위가 커지면 숫자가 잘리기 때문에 BigInt로 변경해준다.
이때 하나만 BigInt로 변경하면 에러가 발생하기 때문에 k 또한 BigInt로 값을 변경해준뒤 나머지 과정을 진행한다.

```js run
/**
 * @param {number} low
 * @param {number} high
 * @return {number}
 */
 var addToArrayForm = function (num, k) {
        return String(BigInt(num.join("")) + BigInt(k))
          .split("")
          .map(Number);
      };
```

# 2/16(목) 
## 104. Maximum Depth of Binary Tree (EASY)

트리의 깊이 구하는 문제 (가장 깊은)

### 문제 접근법
왼쪽부터 탐색하며 하위 노드가 없을 때 0을 리턴

자식 노드를 모두 탐색하고 둘중 큰 값 + 1 을 리턴 

![image](https://user-images.githubusercontent.com/59095793/219292142-fd90679c-773c-4e6b-81af-ff73e47d302f.png)

```js run
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
var maxDepth = function (root) {
        let isFinish = true;
        let answer = 1;
        let treeLength = root.length;
        while (isFinish) {
          treeLength = treeLength / 2;
          if (treeLength >= 1) {
            answer += 1;
            continue;
          }
          isFinish = false;
        }
        return answer;
      };
      // 위에 방법은 만약에 배열로 주어지면 2의 제곱근으로 수학적으로 푸는방법
      // 아래는 트리노드로 탐색해서 값을 구하는 방법
      var maxDepth = function (root) {
        if (root == null) return 0;
        let leftDepth = maxDepth(root.left);
        let rightDepth = maxDepth(root.right);
        return Math.max(leftDepth, rightDepth) + 1;
      };
      
```

# 2/17(금) 
## 783. Minimum Distance Between BST Nodes

트리 노드의 최소차를 구하는 문제 (가장 깊은)

### 문제 접근법
왼쪽부터 탐색하며 값들을 value배열에 push

value배열을 for문으로 탐색하며 값의 차를 구하고 min을 return

```js run
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
    const values = traverseInOrder(root);
    console.log(values)
  let minDiff = Infinity;

  for (let i = 1; i < values.length; i++) {
    minDiff = Math.min(values[i] - values[i - 1], minDiff);
  }

  return minDiff;
};

function traverseInOrder(root, values = []) {
  if (!root) return values;

  traverseInOrder(root.left, values);
  values.push(root.val);
  traverseInOrder(root.right, values);

  return values;
};
      
```

