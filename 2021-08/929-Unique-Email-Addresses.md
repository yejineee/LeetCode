# [LeetCode] 929. Unique Email Addresses (in JS)

### 📖 문제

[929. Unique Email Addresses](https://leetcode.com/problems/unique-email-addresses/)

### 💡 Fact
- 유니크한 이메일의 수를 구하여 하므로, Set을 사용해야 한다.
- 같은 로컬이라도 도메인에 따라 다른 이메일이 된다. 그 역도 마찬가지이다.
- 로컬 파트의 첫 번째 '+' 이후로의 문자열들은 무시되므로, 첫 번째 '+' 이전의 문자열에만 관심이 있다.

### 🚎 접근
1. 이메일을 하나씩 순회하며 다음을 반복한다.
   1. 이메일을 '@'를 기준으로 로컬, 도메인 파트로 나눈다.
   2. 로컬 파트를 '+'로 나누고 첫 번째 부분만 가져온다.
   3. 2번에서 구한 문자열에서 '.'을 전부 제거한다.
   4. 3에서 구한 문자열과 '@', 도메인 파트를 합친 문자열을 set에 넣는다.
2. set의 크기를 반환한다.

### 🧭 복잡도
N : 이메일 배열의 크기
M : 이메일의 평균 길이

- 공간 복잡도 : O(N*M)
- 시간 복잡도 : O(N*M)
  - 모든 이메일들을 순회해야 한다 -> O(N)
  - 각 이메일을 split하려면 이메일의 전체 문자열을 순회해야 한다. -> O(M)

### 📝 코드

```javascript
/**
 * @param {string[]} emails
 * @return {number}
 */
const numUniqueEmails = function (emails) {
  const forwaredEmailSet = emails.reduce((set, email) => {
    const [local, domain] = email.split('@');
    const filteredLocal = local.split('+')[0].replace(/\./g, '');
    set.add(`${filteredLocal}@${domain}`);
    return set;
  }, new Set());
  return forwaredEmailSet.size;
};

```
