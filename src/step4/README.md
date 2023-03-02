# 문제

```ts
// Exercise:
//     Figure out how to help TypeScript understand types in
//     this situation and apply necessary fixes.

interface User {
  type: "user";
  ...
}

interface Admin {
  type: "admin";
  ...
}

type Person = User | Admin;

const persons: Person[] = [ ... ];

function isAdmin(person: Person) {
  return person.type === "admin";
}

function isUser(person: Person) {
  return person.type === "user";
}

function logPerson(person: Person) {
  let additionalInformation: string = "";
  if (isAdmin(person)) {
    additionalInformation = person.role;
  }
  if (isUser(person)) {
    additionalInformation = person.occupation;
  }
  console.log(` - ${person.name}, ${person.age}, ${additionalInformation}`);
}
```

# 풀이

```ts
function isAdmin(person: Person) {
  return person.type === "admin";
}

if (isAdmin(person)) {
  additionalInformation = person.role;
  // Property 'role' does not exist on type 'User'.
}
```

- type guard가 필요하다.

<br/>

```ts
function isAdmin(person: Person): person is Admin {
  return person.type === "admin";
}
```

- `is` 키워드를 사용(`person is Admin` type predicate 구문 적용)
  - `isAdmin` 함수를 호출한 범위 내에서 `isAdmin(person)`의 리턴 타입을 `Admin`으로 좁힌다.
  - 즉, `isAdmin(person)`이 `true`를 반환한다면, type predicate 구문대로 person을 Admin타입으로 본다.
