
# 1071. Greatest Common Divisor of Strings
공통된 문자열로 str1, str2를 나누었을때 '' 인경우의 공통된 문자열 찾기

# 접근법
str1, str2 의 문자열 중 작은 것을 기준으로 나눌 수 있는 최소한의 문자열을 점진적으로 대입해본뒤 최대 길이의 largestDivideString를 return

```js run
 const gcdOfStrings = function (str1, str2) {
        const lessString = str1.length > str2.length ? str2 : str1;
        const bigString = str1.length > str2.length ? str1 : str2;
        let largestDivideString = "";
        for (let i = 1; i <= lessString.length; i++) {
          const word = lessString.substr(0, i);
          const splittedWord = bigString.split(word);
          if (
            splittedWord.every((v) => v === "") &&
            lessString.split(word).every((v) => v === "")
          ) {
            largestDivideString = word;
          }
        }
        return largestDivideString;
      };};
```

# 200. Number of Islands
DFS로 풀어봤습니다.

## 접근법
이 문제는 섬의 개수를 구하는 문제입니다.

1. grid를 순회하면서 땅(1)을 찾는다.
2. 땅을 찾으면 섬 개수 + 1 을 해주고 DFS를 통해 연결된 땅을 모두 탐색한다.
3. 조건에 따라 땅을 탐색하고 땅이 맞다면 땅(1)을 물(0)로 변경해준다.
4. 1을 통해 땅을 찾으면 2,3을 반복. 
5. 섬 개수 반환.

```js run
 const numIslands = function (grid) {
        let count = 0;

        function callDFS(grid, i, j) {
          if (
            i < 0 ||
            i >= grid.length ||
            j < 0 ||
            j >= grid[i].length ||
            grid[i][j] == "0"
          ) {
            return;
          }

          grid[i][j] = "0";

          callDFS(grid, i + 1, j); // down
          callDFS(grid, i - 1, j); // up
          callDFS(grid, i, j + 1); // right
          callDFS(grid, i, j - 1); // left
        }

        for (let i = 0; i < grid.length; i++) {
          for (let j = 0; j < grid[i].length; j++) {
            if (grid[i][j] == "1") {
              count += 1;
              callDFS(grid, i, j);
            }
          }
        }

        return count;
      };
```

# 1145. Binary Tree Coloring Game


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
 * @param {number} n
 * @param {number} x
 * @return {boolean}
 */
const btreeGameWinningMove = (root,n,x)=> {
        let first = 0;
        let second = 0;
        let left = true;
        let right = true;
        function DFS(v) {
          if (v > n) return;
          else {
            if (left === true && v > 3) first++;
            if (right === true && v > 3) second++;
            DFS(v * 2);
            if (v === 2) left = false;
            DFS(v * 2 + 1);
            if (v === 3) right = false;
          }
        }
        DFS(1);
        return first !== second;
      }
```
