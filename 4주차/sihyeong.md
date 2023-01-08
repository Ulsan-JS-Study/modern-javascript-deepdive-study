# 80 
```
/**
 * @param {number[]} nums
 * @return {number}
 */
var removeDuplicates = function(nums) {
    const len = nums.length 
    if(len < 2) return len 
    
    let i = 0
    for(const num of nums){
        if(i < 2 || num > nums[i-2]){
            nums[i++] = num 
        }
    }
    
    return i
};
```

# 720

```
/**
 * @param {string[]} words
 * @return {string}
 */

var longestWord = function(words) {
    words.sort() // 중복된경우

    const wordList = new Set(words)
    let res = ''
    
    for(const word of words){
        let key = '' 
        let isValid = true
        
        for(const char of word){
            key += char
            if(!wordList.has(key)){
                isValid = false 
                break
            }
        }
        
        if(isValid && key.length > res.length) res = key 
    }
    
    return res
};
```

# 875
```
/**
 * @param {number[]} piles
 * @param {number} h
 * @return {number}
 */
var minEatingSpeed = function(piles, h) {
    let min = 1, max = Math.max(...piles)
    
    while(min <=max){
        const mid = Math.floor((min+max) /2)
         // 시간당 바나나 mid개수를 먹는 합을 구한다. 무조건 올림 (7 /3 = 3)
        const totoalTime = piles.reduce((a,b)=> a + Math.ceil(b/mid), 0)
        
        if(totoalTime <= h){
            max = mid - 1
        }else{
            min = mid + 1
        }
    }
    
    return min
};
```
