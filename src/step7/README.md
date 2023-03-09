# 문제

```ts
/*
Exercise:
    Implement swap which receives 2 persons and returns them in the reverse order. The function itself is already
    there, actually. We just need to provide it with proper types. Also this function shouldn't necessarily be limited to just
    Person types, lets type it so that it works with any two types specified.
*/

interface User { ... }
interface Admin { ... }

function logUser(user: User) { ... }
function logAdmin(admin: Admin) { ... }

const admins: Admin[] = [ ... ];
const users: User[] = [ ... ];

function swap(v1, v2) {
  return [v2, v1];
}

function test1() {
  console.log("test1:");
  const [secondUser, firstAdmin] = swap(admins[0], users[1]);
}

function test2() {
  console.log("test2:");
  const [secondAdmin, firstUser] = swap(users[0], admins[1]);
}

function test3() {
  console.log("test3:");
  const [secondUser, firstUser] = swap(users[0], users[1]);
}

function test4() {
  console.log("test4:");
  const [firstAdmin, secondAdmin] = swap(admins[1], admins[0]);
}

function test5() {
  console.log("test5:");
  const [stringValue, numericValue] = swap(123, "Hello World");
}

[test1, test2, test3, test4, test5].forEach((test) => test());
```

# 풀이

```ts
function swap(v1, v2) {
  return [v2, v1];
}
```

- `swap` 함수의 각 매개변수에 타입을 지정해줘야 한다.
- 'it works with any two types specified'라는 조건이 있으므로 `v1`, `v2`에는 `User` 타입, `Admin` 타입 이외의 다른 타입이 될 수도 있어야 한다.

```ts
function swap<T1, T2>(v1: T1, v2: T2): [T2, T1] {
  return [v2, v1];
}
```

- 매개변수의 타입을 리턴 타입으로 다시 사용한다.
  - 제네릭을 사용하여 해결한다.
