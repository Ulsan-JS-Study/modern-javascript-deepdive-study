## 80번 문제

### 문제 요점
1. 배열의 길이와 중복값을 2개까지만 허용하는 새로운 배열을 리턴한다.

### 테스트 케이스
```
Input: nums = [1,1,1,2,2,3]
Output: 5, nums = [1,1,2,2,3,_]
```

```
Input: nums = [0,0,1,1,1,1,2,3,3]
Output: 7, nums = [0,0,1,1,2,3,3,_,_]
```

### 나의 생각
1.

### 

```js
function removeDuplicates(nums) {
  let count = 0;
  const uniqueElements = new Set();
  nums = nums.filter(num => {
    if (!uniqueElements.has(num) || count < 2) {
      uniqueElements.add(num);
      count++;
      return true;
    }
    return false;
  });
  return nums;
}
```

## 720번 문제

### 문제 요점
1. 배열속의 단어를 다른 단어와 비교한다. 이때, 단어들의 조합으로 가장 긴 단어를 반환하는 값을 리턴하면 된다.
2. 단, 단어의 길이가 같은 단어끼리는 알파벳 순으로 정렬하여 앞에 위치하는 값을 리턴한다.

### 테스트 케이스

```
Input: words = ["w","wo","wor","worl","world"]
Output: "world"
```

```
Input: words = ["a","banana","app","appl","ap","apply","apple"]
Output: "apple"
```

### 나의 생각 정리
1. sort() 함수가 많이 사용될 것 같다.
2. 중복값을 제거한 배열을 만든다. 이 배열을 원래 배열과 순회하며 비교하면 좋을 것 같다.
3. 포인터를 기준으로 앞과 뒤의 값을 서로 비교한다. 이떄, 배열내에 중복되는 값이 있다면 데이터의 값이 더 긴쪽을 반환한다.
4. 데이터 값은 반드시 첫 번째 값부터 비교한다. 예를들어, 'banana'는 'a'를 포함하지만 시작이 'b'이므로 값에서 제외된다.

### 문제 풀이

```js
const longestWord = words => {
  // 단어를 알파벳 순으로 정렬한다.
  words.sort();

  // 또한 배열 속 중복된 값을 제거한다.
  const builtWords = new Set();

  // 초기 디폴트값을 ""으로 설정한다.
  let longest = "";

  // 배열 속을 순회한다.
  for (const word of words) {
    // 만약 순회한 값의 길이가 1이거나 중복값을 제거한 배열 속에 word의 맨 마지막 값을 제외한 값을 제외한 값이 있다면 중복값을 제거한 배열 속에 word값을 추가해 준다.
    if (word.length === 1 || builtWords.has(word.slice(0, -1))) {
      builtWords.add(word);

      // 또한 단어의 길이가 최장길이보다 크다면 최장길이를 단어값으로 할당한다.
      if (word.length > longest.length) {
        longest = word;
      }
    }
  }
  return longest;
}

console.log(longestWord(["w","wo","wor","worl","world"])); // world
console.log(longestWord(["a","banana","app","appl","ap","apply","apple"])); // apple
```
