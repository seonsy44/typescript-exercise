# 문제

```ts
// Exercise:
//     Fix typing for the filterPersons so that it can filter users and return User[] when personType='user' and return Admin[]
//     when personType='admin'. Also filterPersons should accept partial User/Admin type according to the personType.
//     `criteria` argument should behave according to the`personType` argument value. `type` field is not allowed in
//     the `criteria` field.

interface User {
    type: 'user';
    name: string;
    age: number;
    occupation: string;
}

interface Admin {
    type: 'admin';
    name: string;
    age: number;
    role: string;
}

type Person = User | Admin;

const persons: Person[] = [ ... ];

function logPerson(person: Person) {
    console.log(
        ` - ${person.name}, ${person.age}, ${person.type === 'admin' ? person.role : person.occupation}`
    );
}

function filterPersons(persons: Person[], personType: string, criteria: unknown): unknown[] {
    return persons
        .filter((person) => person.type === personType)
        .filter((person) => {
            let criteriaKeys = Object.keys(criteria) as (keyof Person)[];
            return criteriaKeys.every((fieldName) => {
                return person[fieldName] === criteria[fieldName];
            });
        });
}

const usersOfAge23 = filterPersons(persons, 'user', { age: 23 });
const adminsOfAge23 = filterPersons(persons, 'admin', { age: 23 });

usersOfAge23.forEach(logPerson);

adminsOfAge23.forEach(logPerson);
```

# 풀이

```ts
function filterPersons(persons: Person[], personType: string, criteria: unknown): unknown[] {
  return persons
    .filter((person) => person.type === personType)
    .filter((person) => {
      let criteriaKeys = Object.keys(criteria) as (keyof Person)[];
      return criteriaKeys.every((fieldName) => {
        return person[fieldName] === criteria[fieldName];
      });
    });
}
```

- `filterPersons` 함수 내에서 `No overload matches this call.` 에러메시지를 확인 할 수 있다.
  - 함수 오버로드를 통해 해결할 수 있다.

<br/>

```ts
function filterPersons(persons: Person[], personType: "user", criteria: Partial<Omit<User, "type">>): User[];
function filterPersons(persons: Person[], personType: "admin", criteria: Partial<Omit<Admin, "type">>): Admin[];
function filterPersons(persons: Person[], personType: string, criteria: Partial<Omit<Person, "type">>): Person[] {
  return persons
    .filter((person) => person.type === personType)
    .filter((person) => {
      let criteriaKeys = Object.keys(criteria) as (keyof Omit<Person, "type">)[];
      return criteriaKeys.every((fieldName) => {
        return person[fieldName] === criteria[fieldName];
      });
    });
}
```

- `personType`이 `user`이면 `User` 타입으로, `admin`이면 `Admin` 타입으로 오버로드 한다.
- 문제 조건에 '`type` field is not allowed in the `criteria` field'이 있으므로 Omit 유틸리티를 사용한다.

# 문제 외 팁

```ts
// Higher difficulty bonus exercise:

//     Implement a function `getObjectKeys()` which returns more convenient result for any argument given, so that you don't
//     need to cast it.

//     let criteriaKeys = Object.keys(criteria) as (keyof User)[];
//     --> let criteriaKeys = getObjectKeys(criteria);

const getObjectKeys = <T extends object>(obj: T) => Object.keys(obj) as (keyof T)[];

function filterPersons(persons: Person[], personType: "user", criteria: Partial<Omit<User, "type">>): User[];
function filterPersons(persons: Person[], personType: "admin", criteria: Partial<Omit<Admin, "type">>): Admin[];
function filterPersons(persons: Person[], personType: string, criteria: Partial<Omit<Person, "type">>): Person[] {
  return persons
    .filter((person) => person.type === personType)
    .filter((person) => {
      let criteriaKeys = getObjectKeys(criteria);
      return criteriaKeys.every((fieldName) => {
        return person[fieldName] === criteria[fieldName];
      });
    });
}
```

- 제네릭을 활용한 함수 `getObjectKeys`를 만들어 사용할 수 있다.
