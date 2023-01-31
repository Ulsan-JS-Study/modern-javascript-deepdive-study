
# 846. handOfStarights

# 접근법

```js run
      var isNStraightHand = function (hand, groupSize) {
        if (hand.length === 1 && groupSize === 1) return true;
        if (hand.length % groupSize !== 0) return false;
        let copiedAndSortedHand = [...hand].sort((a, b) => a - b);
        const toFindLength = copiedAndSortedHand.length / groupSize;
        for (let i = 0; i < toFindLength; i++) {
          let group = [];
          for (let j = copiedAndSortedHand.length - 1; j >= 0; j--) {
            if (group.length === groupSize) break;
            if (group.length === 0) {
              group.push(copiedAndSortedHand.pop());
              continue;
            }
            if (copiedAndSortedHand[j] + 1 === group[group.length - 1]) {
              group.push(copiedAndSortedHand.splice(j, 1)[0]);
            }
          }
        }
        return copiedAndSortedHand.length === 0 ? true : false;
      };

```

# 2115. NfindAllPossibleRecipes


```js run
/**
 * @param {character[][]} grid
 * @return {number}
 */
<script>
      var findAllRecipes = function (recipes, ingredients, supplies) {
        const output = [];
        const findCanMakeRecipe = (ingredient) => {
          console.log(ingredient);
          let correctedIngredients = 0;
          for (let j = 0; j < ingredient.length; j++) {
            if (
              supplies.includes(ingredient[j]) ||
              output.includes(ingredient[j])
            )
              correctedIngredients += 1;
            if (correctedIngredients === ingredient.length)
              output.push(ingredient);
          }
          return correctedIngredients === ingredient.length ? true : false;
        };
        for (let i = 0; i < recipes.length; i++) {
          let correctedIngredients = 0;
          if (output.includes(ingredients[i])) continue;
          for (let j = 0; j < ingredients[i].length; j++) {
            if (
              supplies.includes(ingredients[i][j]) ||
              output.includes(ingredients[i][j])
            )
              correctedIngredients += 1;
            if (recipes.indexOf(ingredients[i][j])) {
              if (
                findCanMakeRecipe(recipes[recipes.indexOf(ingredients[i][j])])
              )
                correctedIngredients += 1;
              continue;
            }
            if (correctedIngredients === ingredients[i].length)
              output.push(recipes[i]);
          }
        }
        return output;
      };
```

# 208 Implement Trie 

```js run
var Trie = function() {
    this.n = {};  
    this.isWord = false;
    return this;
};

/** 
 * @param {string} word
 * @return {void}
 */
Trie.prototype.insert = function(word) {
    let curr = this;
    for (let i = 0; i < word.length; i++) {
        if (!curr.n[word[i]]) {
            curr.n[word[i]] = new Trie(word[i]);
        }
        curr = curr.n[word[i]];
    }
    curr.isWord = true;
};

/** 
 * @param {string} word
 * @return {boolean}
 */
Trie.prototype.search = function(word) {
    let curr = this;
    for (let i = 0; i < word.length; i++) {
        let char = word[i];
        if (!curr.n[char]) {
            return false;
        }
        curr = curr.n[char];
    }
    return curr.isWord;
};

/** 
 * @param {string} prefix
 * @return {boolean}
 */
Trie.prototype.startsWith = function(prefix) {
    let curr = this;
    for (let i = 0; i < prefix.length; i++) {
        let char = prefix[i];
        if (!curr.n[char]) {
            return false;
        }
        curr = curr.n[char];
    }
    return true;
};

/** 

/** 
 * Your Trie object will be instantiated and called as such:
 * var obj = new Trie()
 * obj.insert(word)
 * var param_2 = obj.search(word)
 * var param_3 = obj.startsWith(prefix)
 */
```
