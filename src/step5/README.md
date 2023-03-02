# 문제

```ts
// Exercise:
//     Without duplicating type structures, modify filterUsers function definition so that we can
//     pass only those criteria which are needed, and not the whole User information as it is
//     required now according to typing.

//     Higher difficulty bonus exercise:
//         Exclude "type" from filter criterias.

interface User {
    type: 'user';
    name: string;
    age: number;
    occupation: string;
}

interface Admin { ... }

type Person = User | Admin;

const persons: Person[] = [ ... ];

const isAdmin = (person: Person): person is Admin => person.type === 'admin';
const isUser = (person: Person): person is User => person.type === 'user';

function logPerson(person: Person) { ... }

function filterUsers(persons: Person[], criteria: User): User[] {
    return persons.filter(isUser).filter((user) => {
        const criteriaKeys = Object.keys(criteria) as (keyof User)[];
        return criteriaKeys.every((fieldName) => {
            return user[fieldName] === criteria[fieldName];
        });
    });
}

console.log('Users of age 23:');

filterUsers(persons, { age: 23 }).forEach(logPerson);
```

# 풀이

```ts
filterUsers(persons, { age: 23 }).forEach(logPerson);
// Argument of type '{ age: number; }' is not assignable to parameter of type 'User'.
```

- `filterUsers` 함수의 `criteria` 매개변수의 type이 `User`인데 반해 호출시에 `User`의 일부 필드만 있는 `{ age: 23 }`을 인자로 넘겨서 나타나는 오류
- `User`의 일부 필드만 인자로 받을 수 있도록 수정하도록 한다.

<br/>

```ts
function filterUsers(persons: Person[], criteria: Partial<User>): User[] { ... }
```

- `Partial<Type>` 유틸리티를 사용
  - `Partial<User>`로 작성하여 `User`의 모든 프로퍼티를 선택적으로 만드는 타입을 구성한다.
