# 문제

```ts
/*
Exercise:
    Remove UsersApiResponse and AdminsApiResponse types and use generic type ApiResponse in order to specify API
    response formats for each of the functions.
*/

interface User { ... }
interface Admin { ... }
type Person = User | Admin;

const admins: Admin[] = [ ... ];
const users: User[] = [ ... ];

type ApiResponse<T> = unknown;

type AdminsApiResponse = (
    {
        status: 'success';
        data: Admin[];
    } |
    {
        status: 'error';
        error: string;
    }
);

function requestAdmins(callback: (response: AdminsApiResponse) => void) {
    callback({
        status: 'success',
        data: admins
    });
}

type UsersApiResponse = (
    {
        status: 'success';
        data: User[];
    } |
    {
        status: 'error';
        error: string;
    }
);

function requestUsers(callback: (response: UsersApiResponse) => void) {
    callback({
        status: 'success',
        data: users
    });
}

function requestCurrentServerTime(callback: (response: unknown) => void) {
    callback({
        status: 'success',
        data: Date.now()
    });
}

function requestCoffeeMachineQueueLength(callback: (response: unknown) => void) {
    callback({
        status: 'error',
        error: 'Numeric value has exceeded Number.MAX_SAFE_INTEGER.'
    });
}

function logPerson(person: Person) { ... }

function startTheApp(callback: (error: Error | null) => void) { ... }
startTheApp( ... );
```

# 풀이

```ts
type ApiResponse<T> = unknown;

type AdminsApiResponse =
  | {
      status: "success";
      data: Admin[];
    }
  | {
      status: "error";
      error: string;
    };

type UsersApiResponse =
  | {
      status: "success";
      data: User[];
    }
  | {
      status: "error";
      error: string;
    };
```

- `AdminsApiResponse` 타입과 `UsersApiResponse` 타입을 동시에 대체할 수 있는 `ApiResponse` 타입을 작성해야 한다.

<br/>

```ts
type ApiResponse<T> = (
    {
        status: 'success';
        data: T;
    } |
    {
        status: 'error';
        error: string;
    }
);

function requestAdmins(callback: (response: ApiResponse<Admin[]>) => void) { ... }
function requestUsers(callback: (response: ApiResponse<User[]>) => void) { ... }
function requestCurrentServerTime(callback: (response: ApiResponse<number>) => void) { ... }
function requestCoffeeMachineQueueLength(callback: (response: ApiResponse<number>) => void) { ... }
```

- 제네릭을 사용해 data 타입을 동적으로 받는다.
- `ApiResponse` 타입을 사용하는 곳에 적절한 타입을 넘겨준다.
