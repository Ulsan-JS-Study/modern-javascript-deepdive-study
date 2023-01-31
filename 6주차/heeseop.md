## 846번 문제

### 문제 요점
1. 카드를 연속된 숫자로 재정렬 하는 문제이다.
2. 가지고 있는 카드의 숫자를 주어진 그룹사이즈의 숫자로 재정렬 하면 된다.

### 테스트 케이스

```
Input: hand = [1,2,3,6,2,3,4,7,8], groupSize = 3
Output: true
Explanation: Alice's hand can be rearranged as [1,2,3],[2,3,4],[6,7,8]
```

```
Input: hand = [1,2,3,4,5], groupSize = 4
Output: false
Explanation: Alice's hand can not be rearranged into groups of 4.
```

### 나의 생각 정리
1. %을 이용하면 편할 것 같다.
2. 그룹의 길이를 %로 나눈다. 나누어 떨어지면 true, 나누어 떨어지지 않는다면 false를 리턴한다.

### 테스트 케이스

```

```

### 문제 풀이

```js
function canRearrange(hand, groupSize) {
  const handLength = hand.length;
  let answer = null;

  handLength % groupSize === 0 ? answer = true : answer = false;
  handLength === groupSize ? answer = false : null;
  return answer;
}
```


## 208번 문제

### 문제 요점
1. 검색엔진에 실제로 적용되는 트리 메커니즘을 구현하는 문제이다.
2. 검색하는 단어의 중복값을 확인한다. 중복값을 확인하는 트리거를 설정하여 최종적으로 리턴한다.

### 테스트 케이스

```
Input
["Trie", "insert", "search", "search", "startsWith", "insert", "search"]
[[], ["apple"], ["apple"], ["app"], ["app"], ["app"], ["app"]]
Output
[null, null, true, false, true, null, true]

Explanation
Trie trie = new Trie();
trie.insert("apple");
trie.search("apple");   // return True
trie.search("app");     // return False
trie.startsWith("app"); // return True
trie.insert("app");
trie.search("app");     // return True
```

### 나의 생각 정리
1. 클래스를 이용하면 편할 것 같다.
2. 클래스 내에 컨스트럭터를 설정하여 자식단어(중복되는단어)와 트리거의 기본값을 설정한다.
3. 앞의 스터디에서 배운 서브스트링의 코드를 약간 활용하였다.
4. 

### 문제 풀이

```js
class TrieNode {
  constructor() {
      this.children = {};
      this.isEnd = false;
  }
}

class Trie {
  constructor() {
      this.root = new TrieNode();
  }

  // 기존의 서브스트링 로직을 활용하였다.
  insert(word) {
      let node = this.root;
      for (let i = 0; i < word.length; i++) {
          let char = word[i];
          if (!node.children[char]) {
              node.children[char] = new TrieNode();
          }
          node = node.children[char];
      }
      node.isEnd = true;
  }
  
  // 만약 서브스트링의 조건을 만족하지 못할 시 false를 반환한다.
  search(word) {
      let node = this.root;
      for (let i = 0; i < word.length; i++) {
          let char = word[i];
          if (!node.children[char]) {
              return false;
          }
          node = node.children[char];
      }
      return node.isEnd;
  }
  
  startsWith(prefix) {
      let node = this.root;
      for (let i = 0; i < prefix.length; i++) {
          let char = prefix[i];
          if (!node.children[char]) {
              return false;
          }
          node = node.children[char];
      }
      return true;
  }
}
```
