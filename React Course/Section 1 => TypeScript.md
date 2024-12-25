## Type Annotations and Inference

### Type Annotation

> التصريح الصريح لنوع المتغير (Explicitly specifying the type) بيضمن أمان النوع (Type Safety) وبيخلي الكود أسهل في القراءة.


```ts
let username: string;

const apiResponse = {
  id: 1,
  name: "Ammar",
  birthDate: "1020-10-20"
};
```

### Type Inference

> TypeScript بيقدر يتعرف على النوع من القيمة اللي اتخصصت للمتغير (Type Inference).


```ts
let age = 10; // inferred as number
```

---

## Handling `undefined` and `null`

```ts
let title;
console.log(title); // undefined

let title = null;
console.log(title); // null
```

> ال `undefined` هو القيمة الافتراضية للمتغيرات اللي متمتش تهيئتها، بينما `null` لازم يتم تعيينها بشكل صريح.


---

## Working with Objects

### Inline Object Types

```ts
const userProfile: { username: string; age: number } = {}; // Error: missing properties

const userProfile: { username: string; age: number } = {
  username: "Ammar",
  age: 30
}; // Works
```

### Using Type Aliases

```ts
type UserProfile = {
  username: string;
  age: number;
};

const user: UserProfile = {
  username: "Ammar",
  age: 30
};
```

> استخدام "Aliases" زي `type` بيحسن إعادة الاستخدام وبيخلي الكود أسهل لما بنعرف تركيبات معقدة.


---

## Arrays and Tuples

### Arrays

```ts
const numbers: number[] = [1, 2, 3, 4, 5];
```

### Tuples

> تعرف المصفوفات (Arrays) بأنواع وقيم معينة بتحسن الكود وتمنع الأخطاء.

```ts
const list: [number, string, { username: string }, boolean] =
  [1, "Ammar", { username: "Ammar" }, true];
```

---

## Spread and Destructuring

### Spread Operator

```ts
const userProfile = { username: "Ammar", age: 30 };
const newUser = { ...userProfile };
console.log(newUser); // { username: "Ammar", age: 30 }

const updatedUser = { ...userProfile, username: "Badr" };
console.log(updatedUser); // { username: "Badr", age: 30 }
```

### Destructuring

```ts
const { username, age } = userProfile;
console.log(username, age); // Ammar 30
```

---

## Interfaces and Extensions

### Interfaces

```ts
interface IUser {
  username: string;
  age: number;
}

const userProfile: IUser = { username: "Ammar", age: 30 }; // Works
```

### Extending Interfaces

```ts
interface IUser {
  username: string;
  age: number;
  isMarried: boolean;
}

interface INewUser extends IUser {
  address: string;
}

const newUser: INewUser = {
  username: "Ammar",
  age: 30,
  isMarried: false,
  address: "123 Main St"
};
```

---

## Type Utilities

### Union Types

```ts
type Status = "online" | "offline";
```

### Readonly Properties

```ts
interface IUser {
  username: string;
  readonly age: number;
}

const user: IUser = { username: "Ammar", age: 30 };
user.age = 40; // Error: cannot reassign readonly property
```

---

## Keyof and Index Signatures

### Keyof Operator

```ts
interface IUser {
  username: string;
  age: number;
  address: string;
}

type UserKeys = keyof IUser; // "username" | "age" | "address"

function getProperty(obj: IUser, key: UserKeys) {
  return obj[key];
}

const user: IUser = { username: "Ammar", age: 30, address: "Main St" };
console.log(getProperty(user, "username")); // "Ammar"
```

### Index Signatures

```ts
interface ICityDictionary {
  [city: string]: string;
}

const cities: ICityDictionary = {
  cairo: "Egypt",
  paris: "France",
};

cities["dubai"] = "UAE"; // Works
console.log(cities["dubai"]); // "UAE"
```

---

## Functions

### Rest Parameters

```ts
function sumNumbers(...numbers: number[]): number {
  return numbers.reduce((sum, num) => sum + num, 0);
}

console.log(sumNumbers(1, 2, 3)); // 6
```

### Function Overloading

```ts
function logMessage(message: string): void;
function logMessage(message: string): string {
  console.log(message);
  return message;
}
```

---

## Enums

```ts
enum Directions {
  Up,
  Right,
  Down,
  Left
}

console.log(Directions.Right); // 1
```

Enums can also assign custom values:

```ts
enum StatusCode {
  Success = 200,
  NotFound = 404,
  ServerError = 500
}

function handleResponse(status: StatusCode) {
  switch (status) {
    case StatusCode.Success:
      console.log("Success");
      break;
    case StatusCode.NotFound:
      console.log("Not Found");
      break;
    default:
      console.log("Error");
  }
}
```

---

## Generics

### Generic Functions

```ts
function logArg<T>(arg: T): T {
  return arg;
}

console.log(logArg<number>(42));
console.log(logArg<string>("Hello"));
```

### Swapping Values

```ts
function swap<T>(arg1: T, arg2: T): [T, T] {
  return [arg2, arg1];
}

const [a, b] = swap(10, 20);
console.log(a, b); // 20, 10
```

---

## Utility Types

### Partial

```ts
interface IUser {
  username: string;
  address: string;
}

function updateUser(user: IUser, updates: Partial<IUser>): IUser {
  return { ...user, ...updates };
}

const user: IUser = { username: "Ammar", address: "Main St" };
console.log(updateUser(user, { username: "Badr" }));
```
