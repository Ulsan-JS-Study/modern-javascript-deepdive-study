# 846. Hand of Straights

```
/**
 * @param {number[]} hand
 * @param {number} groupSize
 * @return {boolean}
 */
var isNStraightHand = function(hand, groupSize) {
    const len = hand.length
    if(len % groupSize !== 0) return false 
    
    const map = new Map()
    hand.forEach(num => map.set(num, (map.get(num)||0)+1))

    hand.sort((a,b) => a-b)

    for(const num of hand){
        if(!map.has(num)) continue

        let curr = 0

        while( curr < groupSize) {
            const nextNum = num + curr
            if(!map.has(nextNum)) return false 
                
            map.set(nextNum,map.get(nextNum)-1)
            
            if(map.get(nextNum) === 0) map.delete(nextNum)
        
            curr++
        }
    }
    
    return true
};  
```
