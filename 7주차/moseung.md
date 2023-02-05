
# 2091. handOfStarights

# 접근법

```js run
      var minimumDeletions = function (nums) {
        if (nums.length <= 0) return 0;
        if (nums.length == 1) return 1;
        let maxIdx = 0;
        let minIdx = 0;
        let minVal = nums[0];
        let maxVal = nums[0];
        for (let i = 1; i < nums.length; ++i) {
          if (minVal > nums[i]) {
            minVal = nums[i];
            minIdx = i;
          }

          if (maxVal < nums[i]) {
            maxVal = nums[i];
            maxIdx = i;
          }
        }

        let res = nums.length;
        const leftIdx = Math.min(minIdx, maxIdx);
        const rightIndex = Math.max(minIdx, maxIdx);

        res = Math.min(res, 1 + rightIndex); // left-left
        res = Math.min(res, nums.length - leftIdx); // right-right
        res = Math.min(res, leftIdx + 1 + nums.length - rightIndex); // left-right

        return res;
      };

```

# 1930. Unique Length-3 Palindromic Subsequences


```js run
 var countPalindromicSubsequence = function (s) {
        let palindromicMap = new Map();
        let answer = 0;

        const checkPalindromic = (word) => {
          if (word[0] === word[2]) {
            return true;
          }
          return false;
        };

        const checkDuplicate = (word) => {
          if (palindromicMap.has(word)) {
            return false;
          }
          palindromicMap.set(word);
          return true;
        };

        for (let i = 0; i < s.length - 2; i++) {
          for (let j = i + 1; j < s.length - 1; j++) {
            for (let k = j + 1; k < s.length; k++) {
              const word = `${s[i]}${s[j]}${s[k]}`;
              if (checkPalindromic(word) && checkDuplicate(word)) answer += 1;
            }
          }
        }
        return answer;
      };
```

# 1262 Implement Trie 

```js run
var maxSumDivThree = function (nums) {
        // there are 3 options for how the sum fit's into 3 via mod % 3
        // track those 3 options via indices in the dp array
        // dp[0] = %3 === 0
        // dp[1] = %3 === 1
        // dp[2] = %3 === 2
        let dp = new Array(3).fill(0);
        for (let num of nums) {
          for (let i of dp.slice(0)) {
            console.log(dp.slice(0));
            let sum = i + num;
            let mod = sum % 3;
            
            // on each pass, set the value of dp[mod] to the Math.max val
            dp[mod] = Math.max(dp[mod], sum);
            console.log(dp);
          }
        }
        return dp[0];
      };
      maxSumDivThree([3, 6, 5, 1, 8]);
```
