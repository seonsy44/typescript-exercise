# 문제

```ts
// Given the data, define the interface "User" and use it accordingly.

type User = unknown;

const users: unknown[] = [ ... ];

function logPerson(user: unknown) {
    console.log(` - ${user.name}, ${user.age}`);
 }
```

# 풀이

- logPerson 함수의 user 인자의 타입이 unknown이라서 에러가 발생
- User를 type alias 혹은 interface로 선언하여 해결
  - 주석에 interface를 이용하라고 되어있음
- users 배열 내부 객체의 키와 값을 참고하여 User 타입 선언
- unknown을 User로 교체
