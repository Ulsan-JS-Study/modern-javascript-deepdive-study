## 2/20 (월)

### 35. Search Insert Position

nums와 target이 주어질때, target을 nums에 삽입할 위치 찾는 문제

#### 문제 접근법

for문을 순회하면서 해당 값이 target과 같거나 큰경우 리턴

만약 for문에서 찾지 못한 경우 nums 배열 값중 가장 큰 값이기 때문에 배열 길이를 리턴 

```
var searchInsert = function(nums, target) {
    for(let i=0; i<nums.length; i++){
        if(nums[i] === target || nums[i] > target) return i
    }
    return nums.length
};
```
