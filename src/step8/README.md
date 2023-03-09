# 문제

```ts
/*
Exercise:
    Define type PowerUser which should have all fields from both User and Admin (except for type),
    and also have type 'powerUser' without duplicating all the fields in the code.
*/

interface User { ... }
interface Admin { ... }

type PowerUser = unknown;

type Person = User | Admin | PowerUser;

const persons: Person[] = [
    ...
    {
        type: 'powerUser',
        name: 'Nikki Stone',
        age: 45,
        role: 'Moderator',
        occupation: 'Cat groomer'
    }
];

function isAdmin(person: Person): person is Admin { ... }
function isUser(person: Person): person is User { ... }
function isPowerUser(person: Person): person is PowerUser { ... }

function logPerson(person: Person) {
    let additionalInformation: string = '';
    if (isAdmin(person)) {
        additionalInformation = person.role;
    }
    if (isUser(person)) {
        additionalInformation = person.occupation;
    }
    if (isPowerUser(person)) {
        additionalInformation = `${person.role}, ${person.occupation}`;
    }
    console.log(`${person.name}, ${person.age}, ${additionalInformation}`);
}

persons.filter(isAdmin).forEach(logPerson);
persons.filter(isUser).forEach(logPerson);
persons.filter(isPowerUser).forEach(logPerson);
```

# 풀이

```ts
type PowerUser = unknown;
```

- type 'powerUser' 필드를 갖고, `User`와 `Admin` 두 타입의 type필드를 제외한 모든 필드를 갖는 `PowerUser` 타입을 정의해야 한다.

<br/>

```ts
type PowerUser = { type: "powerUser" } & Omit<User, "type"> & Omit<Admin, "type">;
```

- intersections(&)과 Omit 유틸리티를 이용해 간단히 해결 가능하다.
