# 문제

```ts
// Exercise:
//     Fix type errors in logPerson function.
//     logPerson function should accept both User and Admin
//     and should output relevant information according to
//     the input: occupation for User and role for Admin.

interface User {
    ...
    occupation: string;
}

interface Admin {
    ...
    role: string;
}

type Person = User | Admin;

const persons: Person[] = [ ... ];

function logPerson(person: Person) {
    let additionalInformation: string;
    if (person.role) {
        additionalInformation = person.role;
    } else {
        additionalInformation = person.occupation;
    }
    console.log(` - ${person.name}, ${person.age}, ${additionalInformation}`);
}
```

# 풀이

```ts
if (person.role) {
  additionalInformation = person.role;
}
```

- logPerson 함수 내의 if문에 type guard가 필요하다.

<br/>

```ts
if ("role" in person) {
  additionalInformation = person.role;
}
```

- `in` 연산자를 이용해 narrowing 해준다.
