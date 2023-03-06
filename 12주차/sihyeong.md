# 1539. Kth Missing Positive Number
a
정수 배열 arr이 주어질때 k 번째 수 찾는 문제 


```
/**
 * @param {number[]} arr
 * @param {number} k
 * @return {number}
 */
var findKthPositive = function(arr, k) {
    let idx = 0
    for(let i=1; i<=arr.at(-1); i++){
        if(i!==arr[idx]) k--
        else idx++
        if(idx === arr.length) {
            idx=i 
            break
        }
        if(k===0) return i
    }
    
    return idx + k
};
```
