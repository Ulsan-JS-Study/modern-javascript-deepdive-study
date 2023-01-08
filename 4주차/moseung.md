80번
```js run
 // var removeDuplicates = function(nums) {
      //     let test = {};
      //     let count = 0;
      //     let newNums = [];
      //     for(let i =0;i<nums.length;i++){
      //         if(test[nums[i]]&&test[nums[i]]<2){
      //             test[nums[i]]+=1;
      //             count++;
      //             newNums.push(nums[i])
      //         }else if(test[nums[i]]===undefined){
      //             test[nums[i]]=1;
      //             count++;
      //             newNums.push(nums[i])
      //         }else{
      //             nums.splice(i,1)
      //         }
      //     }
      //     for(let j = 0;j<nums.length-count;j++){
      //         newNums.push("_")
      //     }
      //     nums=newNums
      //     return count;
      // };
      var removeDuplicates = function (nums) {
        for (let i = nums.length - 1; i >= 0; i--) {
          let secPrev = nums[i - 2];
          if (secPrev === nums[i]) nums.splice(i, 1);
        }
      };
```

720번
```js run
var longestWord = function (words) {
        const sortedWords = words.sort();
        let output = sortedWords[0];
        for (let i = 0; i < sortedWords.length; i++) {
          if (
            i === sortedWords.length - 1 &&
            sortedWords[i].length !== sortedWords[i - 1].length
          ) {
            output === sortedWords[i];
            break;
          }
          if (
            sortedWords[i][0] === sortedWords[i + 1][0] &&
            sortedWords[i].length !== sortedWords[i + 1].length
          ) {
            output = sortedWords[i + 1];
            continue;
          }
          break;
        }
        return output;
      };
```

875번
```js run
function minEatingSpeed(piles, H) {
  function canEatAll(speed) {
    let time = 0;
    for (let p of piles) {
      time += Math.ceil(p / speed);
    }
    return time <= H;
  }

  let l = 0;
  let r = Math.max(...piles);  // when the max speed = biggest pile, it only needs 1h to eat each pile
  while (l < r) {
    const m = Math.floor((l + r) / 2);
    if (!canEatAll(m)) l = m + 1;
    else r = m;
  }
  return l;
}
```
