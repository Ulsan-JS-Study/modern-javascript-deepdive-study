## 1번 문제 

### 문제 요점
1. 스트링 중 가장 긴 서브스트링(substring)을 찾는 문제이다.
2. 단, 서브스트링은 중복을 허용하지 않는다.

### 테스트 케이스

```
Input: matches = [[1,3],[2,3],[3,6],[5,6],[5,7],[4,5],[4,8],[4,9],[10,4],[10,9]]
Output: [[1,2,10],[4,5,7,8]]
Explanation:
Players 1, 2, and 10 have not lost any matches.
Players 4, 5, 7, and 8 each have lost one match.
Players 3, 6, and 9 each have lost two matches.
Thus, answer[0] = [1,2,10] and answer[1] = [4,5,7,8].
```

```
Input: matches = [[2,3],[1,3],[5,4],[6,4]]
Output: [[1,2,5,6],[]]
Explanation:
Players 1, 2, 5, and 6 have not lost any matches.
Players 3 and 4 each have lost two matches.
Thus, answer[0] = [1,2,5,6] and answer[1] = [].
```

### 나만의 생각 정리
1. Set()을 이용하여 중복값을 쉽게 제거할 수 있다.
2. 문자열을 for문을 이용하여 순회하도록 하자.
3. 지목된 문자열이 Set()을 이용하여 중복값을 제거한 배열의 값과 비교해보도록 하자. 이때, indexOf나 has 메서드를 이용하면 문자열의 포함 여부를 알 수 있다.
4. 지목된 문자열을 기준으로 시작값과 끝값을 설정하면 편할 것 같다.

### 문제풀이
```js
const lengthOfLongestSubstring = s => {
  // new Set() 연산자를 통해 중복값을 없앤 새로운 배열을 생성한다.
  const nonPermitedDuplication = new Set(); // Set('배열의 길이') ['중복값을 제거한 배열']
  
  // 최대 길이 값의 초기 디폴트 값을 0으로 설정해준다.
  let maxLength = 0;

  // 각각 시작점과 마지막점을 가리키는 포인터를 생성한다. 초기 디폴트값은 역시 0으로 설정한다.
  let start = 0;
  let end = 0;

  // end - 1의 값이 s의 길이와 같이질때까지 while loop를 반복수행한다.
  while(end < s.length) {
    // 만약 중복값이 없는 배열과 순회중인 s와 비교하여 일치하는 데이터 값이 없다면 
    if (!nonPermitedDuplication.has(s[end])) {
      // 데이터 값을 중복값이 없는 배열에 추가한다.
      nonPermitedDuplication.add(s[end]);
      // 또한 end 포인터 값을 1 추가한다.
      end ++;
    // 반대의 경우
    } else {
      // 데이터 값을 중복값이 없는 배열에서 삭제시킨다.
      nonPermitedDuplication.delete(s[start]);
      // 또한 start 포인터 값을 1 추가한다.
      start ++;
    }
    
    // 마지막으로 maxLength와 end - start 를 비교하여 더 큰 값을 할당합니다.
    maxLength = Math.max(maxLength, end - start);
  }

  return maxLength;
}

console.log(lengthOfLongestSubstring('abcabcbb')); // 3
console.log(lengthOfLongestSubstring('bbbbb')); // 1
```

## 2번 문제

### 문제 요점
1. 배열 속에 다시 길이 2의 배열이 주어진다. 길이 2의 배열의 앞 항은 승자, 뒤 항은 패자를 가리킨다.
2. 이것을 총 나열하여 승자와 패자를 리턴해주면 된다. 리턴값은 배열로 구성하며 배열의 0번째 항은 한번도 패배하지 않은 플레이어들의 배열 집합을, 1번째 항은 한번만 패배한 플레이어들의 배열 집합을 리턴한다.

### 테스트 케이스

```
Input: matches = [[1,3],[2,3],[3,6],[5,6],[5,7],[4,5],[4,8],[4,9],[10,4],[10,9]]
Output: [[1,2,10],[4,5,7,8]]
Explanation:
Players 1, 2, and 10 have not lost any matches.
Players 4, 5, 7, and 8 each have lost one match.
Players 3, 6, and 9 each have lost two matches.
Thus, answer[0] = [1,2,10] and answer[1] = [4,5,7,8].
```

```
Input: matches = [[1,3],[2,3],[3,6],[5,6],[5,7],[4,5],[4,8],[4,9],[10,4],[10,9]]
Output: [[1,2,10],[4,5,7,8]]
Explanation:
Players 1, 2, and 10 have not lost any matches.
Players 4, 5, 7, and 8 each have lost one match.
Players 3, 6, and 9 each have lost two matches.
Thus, answer[0] = [1,2,10] and answer[1] = [4,5,7,8].
```

### 나만의 생각 정리
1. 배열의 첫번째 항은 승자, 두번째 항은 패자를 가리킨다. 여기서 승리 카운트는 따로 할 필요가 없을 것 같다. 승리와 상관없이 패배 수에 따라 리턴값이 달리지므로 핵심은 '패배'이다!
2. for ...in 이나 for ...of 문을 사용하면 편할 것 같다. 여기서 ES6 문법인 배열 할당문을 사용하면 가시성을 확보할 수 있다.
3. 역시 중복값을 피하기 위해 가장 간단한 new Set() 연산자를 사용할 예정이다.
4. 이제 패배를 한번도 하지않은 플레이어와 패배를 한번만 한 플레이어를 추려내야 한다. 추려낸 값을 각각 알맞게 내가 지정한 빈 배열에 데이터를 추가한다.
5. 빈 배열에 알맞은 데이터를 모두 추가하였다. 이를 리턴하면 끝!

### 문제풀이

```js
const findWinners = matches => {
  // 먼저 최종적으로 리턴할 배열을 만든다.
  const playerLosses = [];

  // 앞서 언급한대로 승리수는 리턴값에 영향이 없다. 패배 수만 활용하자.
  for (const[winner, loser] of matches) {
    // playerLosses에 loser를 추가한다. 이때, || 논리 연산자를 통해 playerLosses가 falthy 값을 가질경우
    // 0을 할당한다. 최종적으로 할당된 값에 +1을 해준다.
    playerLosses[loser] = (playerLosses[loser] || 0) + 1;
  }
  
  // new Set() 연산자를 통해 중복값을 제거한다.
  const undefeatedPlayers = new Set();
  const oneLossPlayers = new Set();

  for (const [winner, loser] of matches) {
    // loser 값이 1인 경우 loser에 데이터를 추가한다.
    if (playerLosses[loser] === 1) {
      oneLossPlayers.add(loser);
    }
    // loser 값이 없는 경우 winner에 데이터를 추가한다.
    if (!playerLosses[winner]) {
      undefeatedPlayers.add(winner);
    }
  }
  // 최종적으로 sort()를 통해 오름차순으로 정렬한다. 이때, 데이터는 모두 숫자값이므로
  // (a, b) => a - b를 콜백함수로 설정하여 숫자도 오름차순으로 정렬할 수 있다.
  const sortedUndefeatedPlayers = [...undefeatedPlayers].sort((a, b) => a - b);
  const sortedOneLossPlayers = [...oneLossPlayers].sort((a, b) => a - b);

  return [sortedUndefeatedPlayers, sortedOneLossPlayers];
}

console.log(matches = [[1,3],[2,3],[3,6],[5,6],[5,7],[4,5],[4,8],[4,9],[10,4],[10,9]]); // [[1,2,10],[4,5,7,8]]
console.log(matches = [[2,3],[1,3],[5,4],[6,4]]); // [[1,2,5,6],[]]
```