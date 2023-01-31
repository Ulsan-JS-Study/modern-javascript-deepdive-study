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

# 208. Implement Trie (Prefix Tree)

```

var Trie = function() {
    this.root = new Map()
};

/** 
 * @param {string} word
 * @return {void}
 */
Trie.prototype.insert = function(word) {
    let cur = this.root;
  
    for (const char of word) {
        if (!cur.has(char)) { 
            cur.set(char, new Map()); 
        }

        cur = cur.get(char);
    }
  
    cur.set('end', true);
};

/** 
 * @param {string} word
 * @return {boolean}
 */
Trie.prototype.search = function(word) {
    let cur = this.root;
  
    for (const char of word) {
        console.log(cur)
        if (!cur.has(char)) { return false; }
        cur = cur.get(char);
    }

    return cur.has('end');
};

/** 
 * @param {string} prefix
 * @return {boolean}
 */
Trie.prototype.startsWith = function(prefix) {
    let cur = this.root;
    for (const char of prefix) {
        if (!cur.has(char)) { return false; }
        cur = cur.get(char);
    }
  
    return true;
};

/** 
 * Your Trie object will be instantiated and called as such:
 * var obj = new Trie()
 * obj.insert(word)
 * var param_2 = obj.search(word)
 * var param_3 = obj.startsWith(prefix)
 */
```
