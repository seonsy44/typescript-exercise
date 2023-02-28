# 문제

```ts
//Exercise:
//    Type "Person" is missing, please define it and use
//    it in persons array and logPerson function in order to fix
//    all the TS errors.

interface User {
    name: string;
    age: number;
    occupation: string;
}

interface Admin {
    name: string;
    age: number;
    role: string;
}

type Person = unknown;

const persons: User[] /* <- Person[] */ = [
    {
        name: 'Max Mustermann',
        age: 25,
        occupation: 'Chimney sweep'
    },
    {
        name: 'Jane Doe',
        age: 32,
        role: 'Administrator'
    },
    ...
];

function logPerson(user: User) {
    console.log(` - ${user.name}, ${user.age}`);
}
```

# 풀이

- 타입 Person을 선언하여 해결하여야 한다.
- persons 배열을 살펴보면 각 객체에 `name`, `age`, `occupation` || `role` 이 key 값으로 있다.
  - User 인터페이스에 `role`이 없어서 에러가 발생한다.
- Person 타입을 type alias로 선언한다.
  - User 인터페이스와 Admin 인터페이스를 union type으로 묶어서 Person을 선언한다.
- User 타입으로 지정된 곳을 Person으로 변경한다.
