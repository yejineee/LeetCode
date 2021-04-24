# [LeetCode] (in JS)

### 📖 문제

[Brick Wall](https://leetcode.com/explore/challenge/card/april-leetcoding-challenge-2021/596/week-4-april-22nd-april-28th/3717/)

벽에 있는 벽돌의 width가 저장되어있는 2차원 배열이 주어진다.

벽을 수직으로 지나칠 때, 지나가게되는 가장 적은 수의 벽돌을 구하여라.

### 💡 Fact

- 벽돌의 가장자리를 지나는 경우만이, 가장 적은 수의 벽돌이 나올 수 있다.
- i번째 벽돌이 끝나는 곳의 길이는 [0, i] 범위에 있는 벽돌들의 width의 합이다.
- n의 길이에서 끝나는 벽돌의 수가 가장 많을 때, n의 길이를 가로지르는 선이 가장 적은 벽돌을 지나치게 된다.
- 가장 많은 벽돌을 지나치게 될 때는 수직선상에 있는 모든 벽돌을 지날 때이며, 이는 벽의 층의 갯수와 같다.
- [가장 많은 벽돌을 지날 때의 값]에서 [n의 길이에서 끝나는 벽돌의 수의 최댓값]을 뺀 값이 가로지를 수 있는 가장 적은 벽돌의 수이다.

### 🚎 접근

벽돌이 끝나는 길이를 key, 그에 해당되는 벽돌의 수를 value로 담는 Map인 countOfEdge를 선언한다.

row를 순회하면서, row가 시작하는 점에서 sumOfWidth를 0으로 초기화시켜준다. 해당 벽돌이 끝나는 길이는 자신의 width에 sumOfWidth를 더한 값이다. sumOfWidth를 key로 갖는 벽돌이 하나 더 생긴 것이므로, 해당 value에 1을 더하여 저장한다.

sumOfWidth의 value 중 가장 큰 값을 구하고, 그 값을 wall의 row의 갯수에서 뺀다.

### 🧭 복잡도

N은 wall의 col의 수이고, M은 wall의 row의 수.

- 공간 복잡도 : O(N)
- 시간 복잡도 : O(N\*M)

### 🧐 알게된 점

- `arr.slice(0, -1)`로 마지막 요소를 제외시킬 수 있다.

  항상 마지막 요소를 제외시키고 순회할 때, if문으로 처리하였다. 배열 자체를 마지막 요소를 제외시킨 후, 순회를 하면 코드를 더 깔끔하게 할 수 있다.

  slice의 end index에 음수를 넣으면, 뒤에서부터의 index이다. -1은 배열의 가장 마지막 요소의 index를 뜻한다. 따라서, `slice(0, -1)`은 가장 마지막 요소만 제외한 배열을 반환하게 된다.

- Map이 좋을지, array가 좋을지를 생각하자.

  edge의 값을 저장할 때, array로 하면 O(1)로 접근할 수 있으니 더 좋다고 생각하였다. 그러나, total width of wall 의 값이 커질 경우 array 중 일부만이 채워지게 될 수 있다. 이는 메모리 낭비로 이어지게 된다.

  같은 O(1)의 접근 시간을 가지면서, 메모리 낭비는 줄일 수 있는 Map이 이번 문제에서는 더 적합하였다.

  어떤 데이터구조가 좋은지를 고려하고 하자...!

### 📝 코드

```javascript
/**
 * @param {number[][]} wall
 * @return {number}
 */
var leastBricks = function (wall) {
  const totalHeight = wall.length;
  const countOfEdge = new Map();
  let maxEdgeCount = 0;

  wall.forEach((row) => {
    let sumOfWidth = 0;
    row.slice(0, -1).forEach((width) => {
      sumOfWidth += width;
      const count = (countOfEdge.get(sumOfWidth) || 0) + 1;
      countOfEdge.set(sumOfWidth, count);
      maxEdgeCount = Math.max(maxEdgeCount, count);
    });
  });
  return totalHeight - maxEdgeCount;
};
```
