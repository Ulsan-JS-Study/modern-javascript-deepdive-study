## 1071번 문제

### 문제 요점
1. 앞의 문자열을 뒤의 문자열로 나눈다.
2. 모두 나누어 떨어지는 가장 큰수를 반환하면 된다.

### 테스트 케이스

```
Input: str1 = "ABCABC", str2 = "ABC"
Output: "ABC"
```

```
Input: str1 = "ABABAB", str2 = "ABAB"
Output: "AB"
```

```
Input: str1 = "LEET", str2 = "CODE"
Output: ""
```

### 나의 생각 정리
1. substring() 메서드를 이용하여 복잡한 서브스트링 메커니즘을 따로 짤 수고를 덜 수 있다.
2. 주어진 두개의 배열을 각각 앞뒤로 다르게 더하여 배열이 나누어 떨어지는지 확인할 수 있다.
3. 유클리드 알고리즘을 사용하여 최소공약수를 간단하게 구현할 수 있다. 귀납법으로 구현해보자.
4. 최소 공약수를 큰 문자열, 즉 앞의 문자열의 서브스트링을 반환하면 그것이 정답이다!

### 문제 풀이

```js
const gcdOfStrings = (str1, str2) => {

  // 문자열을 연결했을 때 서로 같은지 확인한다. 만약 다를시 ""을 리턴한다.
  if (str1 + str2 !== str2 + str1) {
      return "";
  }

  // 최대 공약수의 수만금 서브스트링의 길이를 리턴한다.
  return str1.substring(0, gcd(str1.length, str2.length));
}

// 유클리드 알고리즘을 이용한 방법. 재귀법을 사용하여 최대공약수를 구현한다.
function gcd(a, b) {
  if (b === 0) {
      return a;
  }

  // 최대공약수를 반환한다.
  return gcd(b, a % b);
}

console.log(gcdOfStrings("ABCABC", "ABC")); // "ABC"
console.log(gcdOfStrings("ABABAB", "ABAB")); // "AB"
console.log(gcdOfStrings("LEET", "CODE")); // ""
```

## 200번 문제

### 문제 요점
1. 1이 땅, 0이 물이라고 가정했을 때 섬의 개수를 반환하는 문제이다.
2. 1이 서로 이어져 있다면 같은 땅으로 간주한다.

### 테스트 케이스

```
Input: grid = [
  ["1","1","1","1","0"],
  ["1","1","0","1","0"],
  ["1","1","0","0","0"],
  ["0","0","0","0","0"]
]
Output: 1
```

```
Input: grid = [
  ["1","1","0","0","0"],
  ["1","1","0","0","0"],
  ["0","0","1","0","0"],
  ["0","0","0","1","1"]
]
Output: 3
```

### 나의 생각 정리

1. 반복문을 사용하여 배열을 순회한다. 핵심은 1이다.
2. 포인터를 만들자. 포인터가 육상에 방문, 배열속의 1을 순회하며 조회한다.
3. 

### 문제풀이

```js
function numIslands(grid) {
  let count = 0;

  // 일종의 포인터 역할을 수행한다. 포인터에 육지가 탐색될 경우 카운트에 1을 더한다.
  for (let i = 0; i < grid.length; i++) {
      for (let j = 0; j < grid[i].length; j++) {

        // 육지를 조회한다.
          if (grid[i][j] === '1') {
              dfs(grid, i, j);
              count++;
          }
      }
  }
  return count;
}

function dfs(grid, i, j) {
  if (i < 0 || i >= grid.length || j < 0 || j >= grid[i].length || grid[i][j] === '0') {
      return;
  }
  grid[i][j] = '0';
  dfs(grid, i - 1, j);
  dfs(grid, i + 1, j);
  dfs(grid, i, j - 1);
  dfs(grid, i, j + 1);
}
```

## 1145번 문제

### 문제 요점
1. 이진트리 탐색 기법을 활용한 게임이다.
2. 두개의 포인터 중 더 많은 탐색을 한 포인터가 이기는 게임이다. 나는 두번째 플레이어 이다.
3. 내가 승리한다면 true, 패배한다면 false를 반환하자

### 문제풀이

```js
function secondPlayerWins(root, n, x) {
    let xNode = findNode(root, x);
    let size = getSize(xNode);
    return size % 2 === 0;
}

function findNode(root, x) {
    if (!root) return null;
    if (root.val === x) return root;
    let left = findNode(root.left, x);
    let right = findNode(root.right, x);
    return left || right;
}

function getSize(node) {
    if (!node) return 0;
    return 1 + getSize(node.left) + getSize(node.right);
}
```
