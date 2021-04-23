# [LeetCode] Count Binary Substrings (in JS)

### 📖 문제

[Count Binary Substrings](https://leetcode.com/explore/challenge/card/april-leetcoding-challenge-2021/596/week-4-april-22nd-april-28th/3718/)

string `s`를 모두 0이거나 모두 1로 그룹 지었을 때, 연속된 0과 연속된 1의 갯수가 같은 것이 몇 개인지 세어라.

- example
  ```
  Input: "10101"
  Output: 4
  Explanation: There are 4 substrings: "10", "01", "10", "01" that have equal number of consecutive 1's and 0's.
  ```

### 💡 Fact

"000111"은 "000"과 "111"로 그룹지을 수 있다. 각 그룹의 길이는 3이다. 여기서 생길 수 있는 연속된 0과 1의 갯수가 같은 것은 "000111", "0011", "01"로 총 3개이다.

"11100"은 "111"과 "00"으로 그룹지을 수 있다. 각 그룹의 길이는 3과 2이다. 여기서 생길 수 있는 연속된 0과 1의 갯수가 같은 것은 "1100", "10"으로 총 2개이다.

위의 예시를 통해 살펴보았을 때, **연속된 0의 수와 연속된 1의 수 중 작은 수**만큼 같은 수의 연속된 0과 1의 묶음이 생기는 것을 알 수 있다.

### 🚎 접근

연속된 0과 1을 그룹지었을 때의 각 그룹의 길이를 알아야 한다.

그룹은 정규식 `/1+|0+/g`을 사용하여 얻어내었다.

첫 번째 그룹부터 시작하여 마지막 그룹을 제외하고, 각 그룹은 바로 다음 그룹의 길이를 비교하여 더 작은 값들을 더해나가면 최종 결과를 얻을 수 있다.

### 🧭 복잡도

N은 s의 길이

- 공간 복잡도 : O(N)
- 시간 복잡도 : O(N)

### 🧐 알게된 점

Array.prototype.reduce()에서 초기값을 제공하지 않으면, 인덱스 1부터 시작하여 콜백함수를 실행한다. 초기값을 제공하면, 0번째 인덱스부터 콜백함수를 실행한다.

**예상치 못한 결과를 얻지 않기 위해 reduce()의 초기값을 꼭 설정하자!**

### 📝 코드

```javascript
var countBinarySubstrings = function (s) {
  const groupRegex = /1+|0+/g;
  const lenOfGroup = s.match(groupRegex).map((x) => x.length);
  const totalCount = lenOfGroup.reduce((count, len, i) => {
    if (i >= lenOfGroup.length - 1) {
      return count;
    }
    return count + Math.min(len, lenOfGroup[i + 1]);
  }, 0);
  return totalCount;
};
```
