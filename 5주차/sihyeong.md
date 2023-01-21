# 1071. Greatest Common Divisor of Strings
공통된 문자열로 str1, str2를 나누었을때 '' 인경우의 공통된 문자열 찾기

# 접근법
str1, str2 의 문자열 중 작은 것을 기준으로 prefix를 구해 str1, str2를 나눈다(replaceAll).

```
var gcdOfStrings = function(str1, str2) {
    let prefix = ''
    let ans = ''
    
    const minStr = str1.length> str2.length ? str2 : str1
    const maxStr = str1.length > str2.length ? str1 : str2
    
    for(const char of minStr){
        prefix += char 
        if(maxStr.replaceAll(prefix,'') === '' && minStr.replaceAll(prefix, '') === ''){
            ans = prefix.length > ans.length ? prefix : ans 
        } 
    }
    
    return ans 
};
```

# 200. Number of Islands
DFS, BFS 두가지 방법으로 풀어봤습니다.

## 접근법
이 문제는 섬의 개수를 구하는 문제입니다.

1. grid를 순회하면서 땅(1)을 찾는다.
2. 땅을 찾으면 섬 개수 + 1 을 해주고 DFS 또는 BFS를 통해 연결된 땅을 탐색한다.
3. 조건에 따라 땅을 탐색하고 땅이 맞다면 땅(1)을 물(0)로 변경해준다.
4. 1을 통해 땅을 찾으면 2,3을 반복. 
5. 섬 개수 반환.

```
/**
 * @param {character[][]} grid
 * @return {number}
 */
var numIslands = function(grid) {
    let count = 0 
    const directions = [[-1,0],[0,1],[1,0],[0,-1]]
    
    const isValid = (x,y) => {
        return x>=0 && y>=0 && x<grid.length && y< grid[0].length && grid[x][y]==="1"
    }
    
    const DFS(x,y){
         if(isValid(x,y)){
             grid[x][y] = 0
             directions.forEach((d)=> DFS(x+d[0],y+d[1]))

        }
     }
     
    const BFS = (i,j) => {
        let queue = [[i,j]]

        while(queue.length){
            const nextQueue = [] 

            for(const [x,y] of queue){
                for(const [dx, dy] of directions){
                    const nx = x + dx
                    const ny = y + dy
                    if(isValid(nx,ny)){
                        grid[nx][ny] = '0'
                        nextQueue.push([nx,ny])
                    }
                }
            }

            queue = nextQueue
        }
    }
    
    for(let i=0; i<grid.length; i++){
        for(let j=0; j<grid[0].length; j++){
            if(grid[i][j] === "1"){
                count++
                BFS(i,j) // BFS(i,j) DFS(i,j)
            }
        }
    }
    return count
};
```

# 1145 Binary Tree Coloring Game

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
 * @param {number} n
 * @param {number} x
 * @return {boolean}
 */
var btreeGameWinningMove = function(root, n, x) {
    let left, right; 
    const red = x
    const dfs = (node) => {
        if(!node) return 0 
        
        const l = dfs(node.left)
        const r = dfs(node.right)
        
        // red 하위 트리 개수 
        if(node.val === red){
            left = l
            right = r
        }
        
        return l + r + 1 
    }
    dfs(root)
    // 이진트리라서 절반을 비교한다.
    return Math.max(Math.max(left,right), n-left-right-1) > n/2
};
```
