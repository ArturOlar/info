### Roadmap
https://roadmap.sh/typescript

---

## TypeScript vs JavaScript
TypeScript is a superset of JavaScript that adds optional type annotations and other features such as interfaces, classes, and namespaces. JavaScript is a dynamically-typed language that is primarily used for client-side web development and can also be used for server-side development.

Here are a few key differences between TypeScript and JavaScript:

Types: TypeScript has optional type annotations while JavaScript is dynamically-typed. This means that in TypeScript, you can specify the data type of variables, parameters, and return values, which can help catch type-related errors at compile-time.
Syntax: TypeScript extends JavaScript syntax with features like interfaces, classes, and namespaces. This provides a more robust and organized structure for large-scale projects.
Tooling: TypeScript has better tooling support, such as better editor integration, type checking, and code refactoring.
Backwards Compatibility: TypeScript is fully compatible with existing JavaScript code, which means you can use TypeScript in any JavaScript environment.

---

## TypeScript vs JavaScript

---

## Install and Configure

### tsconfig.json

tsconfig.json is a configuration file in TypeScript that specifies the compiler options for building your project. It helps the TypeScript compiler understand the structure of your project and how it should be compiled to JavaScript. Some common options include:

```
target: the version of JavaScript to compile to.
module: the module system to use.
strict: enables/disables strict type checking.
outDir: the directory to output the compiled JavaScript files.
rootDir: the root directory of the TypeScript files.
include: an array of file/directory patterns to include in the compilation.
exclude: an array of file/directory patterns to exclude from the compilation.
```

### Compiler Options

---

## Running TypeScript

To run TypeScript code, you'll need to have a TypeScript compiler installed. Here's a general process to run TypeScript code:
- Write TypeScript code in a .ts file (e.g. app.ts)
- Compile the TypeScript code into JavaScript using the TypeScript compiler:
  - tsc app.ts
- Run the generated JavaScript code using a JavaScript runtime environment such as Node.js:
  - node app.js

### tsc
`tsc` is the command line tool for the TypeScript compiler. It compiles TypeScript code into JavaScript code, making it compatible with the browser or any JavaScript runtime environment.

`tsc` - this command will compile all TypeScript files in your project that are specified in your tsconfig.json file. If you want to compile a specific TypeScript file, you can specify the file name after the tsc command, like this: `tsc index.ts`

The tsc command has several options and flags that you can use to customize the compilation process. For example, you can use the --target option to specify the version of JavaScript to compile to, or the --outDir option to specify the output directory for the compiled JavaScript files.

### ts-node

---

## Typescript Types

TypeScript has several built-in types, including:
- `number`
- `string`
- `boolean`
- `any`
- `void`
- `null`, `undefined`
- `never`
- `object`
- `symbol`
- `Enumerated types (enum)`
- `Tuple types`
- `Array types`
- `Union types`
- `Intersection types`
- `Type aliases`
- `Type assertions`

You can also create custom types in TypeScript using interfaces, classes, and type aliases.

### Primitive Types

`number`

`string`

`boolean`

`void` - represents the return value of functions which don’t return a value. In JavaScript, a function that doesn’t return any value will implicitly return the value undefined. However, void and undefined are not the same thing in TypeScript.

`null`, `undefined` - JavaScript has two primitive values used to signal absent or uninitialized value: null (absent) and undefined (uninitialized). TypeScript has two corresponding types by the same names. How these types behave depends on whether you have the `strictNullChecks` option on. <br>
With `strictNullChecks` off, values that might be null or undefined can still be accessed normally, and the values null and undefined can be assigned to a property of any type. This is similar to how languages without null checks (e.g. C#, Java) behave. The lack of checking for these values tends to be a major source of bugs; TypeScript always recommend people turn strictNullChecks on if it’s practical to do so in the codebase. <br>
With strictNullChecks on, when a value is null or undefined, you will need to test for those values before using methods or properties on that value.

### Top Types

`unknown` - unknown is the type-safe counterpart of any. Anything is assignable to unknown, but unknown isn’t assignable to anything but itself and any without a type assertion or a control flow based narrowing. Likewise, no operations are permitted on an unknown without first asserting or narrowing to a more specific type.
```
function f1(a: any) {
  a.b(); // OK
}

function f2(a: unknown) {
  // Error: Property 'b' does not exist on type 'unknown'.
  a.b();
}
```

`any` - TypeScript has a special type, any, that you can use whenever you don’t want a particular value to cause typechecking errors. <br>
When a value is of type any, you can access any properties of it (which will in turn be of type any), call it like a function, assign it to (or from) a value of any type, or pretty much anything else that’s syntactically legal:

```
let obj: any = { x: 0 };
// None of the following lines of code will throw compiler errors.
// Using `any` disables all further type checking, and it is assumed
// you know the environment better than TypeScript.
obj.foo();
obj();
obj.bar = 100;
obj = 'hello';
const n: number = obj;
```

### Object Types

`Interface` - TypeScript allows you to specifically type an object using an interface that can be reused by multiple objects.

`Class` - In TypeScript, a class is a blueprint for creating objects with specific properties and methods.

`Enum`

`Array` - To specify the type of an array like [1, 2, 3], you can use the syntax number[]; this syntax works for any type (e.g. string[] is an array of strings, and so on). You may also see this written as Array<number>, which means the same thing, example: `const numbers: number[] = [1, 2, 3];`

`Tuple` - A tuple type is another sort of Array type that knows exactly how many elements it contains, and exactly which types it contains at specific positions.

```
type StringNumberPair = [string, number];

const pair: StringNumberPair = ['hello', 42];

const first = pair[0];
const second = pair[1];

// Error: Index out of bounds
const third = pair[2];
```

`Object` - To define an object type, we simply list its properties and their types. For example, here’s a function that takes a point-like object:

```
// The parameter's type annotation is an object type
function printCoord(pt: { x: number; y: number }) {
  console.log("The coordinate's x value is " + pt.x);
  console.log("The coordinate's y value is " + pt.y);
}

printCoord({ x: 3, y: 7 });
```

### Bottom Types

`never` -  The never type represents the type of values that never occur. For instance, never is the return type for a function expression or an arrow function expression that always throws an exception or one that never returns. Variables also acquire the type never when narrowed by any type guards that can never be true. <br>
The never type is a subtype of, and assignable to, every type; however, no type is a subtype of, or assignable to, never (except never itself). Even any isn’t assignable to never. <br>
Examples of functions returning never:

```
// Function returning never must not have a reachable end point
function error(message: string): never {
  throw new Error(message);
}

// Inferred return type is never
function fail() {
  return error('Something failed');
}

// Function returning never must not have a reachable end point
function infiniteLoop(): never {
  while (true) {}
}
```

### null, undefined, never, unknown

`null` - означає що значення відсутнє, тобто змінна явно вказана як порожня.

`undefined` - означає що значення не ініціалізоване, тобто змінна була оголошена, але їй не було присвоєно жодного значення.

```
let age: number | undefined;

console.log(age); // undefined
```

`never` - означає що значення ніколи не буде, тобто функція ніколи не поверне значення або змінна ніколи не отримає значення.

```
function throwError(message: string): never {
  throw new Error(message);
}

function infiniteLoop(): never {
  while (true) {}
}
```

Використовується для функцій, які ніколи не повертають результат. <br>
TypeScript це використовує для перевірки вичерпності (exhaustive checks).

```
type Shape = "circle" | "square";

function getArea(shape: Shape) {
  if (shape === "circle") return 10;
  if (shape === "square") return 20;

  // Якщо ми забули додати нову фігуру, TypeScript тут помітить помилку
  const _exhaustiveCheck: never = shape;
}
```

Використовуй в місцях, де функція або блок коду не має завершитись (throw, infinite loop, exhaustive switch).

`unknown` — це безпечна версія any.
TypeScript не знає, що це за тип, але змушує тебе перевірити його, перш ніж щось зробити.

```
let value: unknown = "Hello";

value = 42; // ок
value = { name: "Artur" }; // ок

// Але не можна просто так використовувати
// console.log(value.toFixed(2)); ❌ Помилка!

// Потрібна перевірка типу
if (typeof value === "number") {
  console.log(value.toFixed(2)); // ✅ ок
}
```

Якщо any — це "роби що хочеш", <br>
то unknown — це "спочатку перевір, а потім роби".

--- 

## Assertions

In TypeScript, the as keyword is used for type assertions, allowing you to explicitly inform the compiler about the type of a value when it cannot be inferred automatically. <br>
type assertion (або ствердження типу) — це одна з найважливіших концепцій у TypeScript, бо вона дозволяє сказати компілятору, який саме тип має значення, навіть якщо TS цього не може визначити сам. <br>
Type assertion — це спосіб сказати TypeScript-у: “Повір мені, я знаю, який це тип”.  Тобто, ти не змінюєш значення, а лише підказуєш компілятору, як його треба трактувати.

🧩 Приклад 1: Базове використання
```
let value: unknown = "Hello TypeScript";

// TypeScript не знає, що в value є метод length
console.log((value as string).length); // ✅ працює
```
`(value as string)` — це assertion.
Ми кажемо: “Я впевнений, що value — це string”.

🧩 Приклад 2: Альтернативний синтаксис (<>) <br>
Є два способи записати assertion:

```
// Спосіб 1 — сучасний
value as string;

// Спосіб 2 — старий (не працює в JSX)
<string>value;
```

В JSX-проєктах (React, Next.js тощо) завжди використовуй as, бо <string> сприймається як HTML-тег.

🧩 Приклад 3: DOM-елементи <br>
TypeScript часто не знає, який саме елемент ти отримав із DOM.

```
const input = document.querySelector("input");

// TS вважає, що це може бути Element або null
input?.value; // ❌ Property 'value' does not exist

// Але ми точно знаємо, що це HTMLInputElement
const inputEl = document.querySelector("input") as HTMLInputElement;
console.log(inputEl.value); // ✅ тепер все добре
```

🧩 Приклад 4: Звуження типу при роботі з API

```
type User = { id: number; name: string };

const data = JSON.parse('{"id": 1, "name": "Artur"}');

// TS думає, що data: any
// Ми стверджуємо, що це User
const user = data as User;

console.log(user.name); // ✅ "Artur"
```

🧩 Приклад 5: Type Assertion vs Type Casting (у JS)

Type assertion ≠ type casting.
Casting реально змінює тип даних у пам’яті,
а assertion — лише інструкція для компілятора.

```
let num = "123" as unknown as number; // ✅ TS вірить, що це число
console.log(num + 1); // "1231" (рядок, бо насправді не число)
```

Тобто assertion не змінює значення, воно лише “маскує” його тип.

⚠️ Важливо <br>
TypeScript не перевіряє, чи твоє твердження правильне.
Тобто ти можеш себе обдурити 👇
```
const value = "Hello" as number; // ❌ Логічно неправильно, але TS дозволяє з подвійним asse
```

Іще приклади:

```
let someValue: any = "Hello, TypeScript!";
let strLength: number = (someValue as string).length;

console.log(strLength); // Outputs: 18
```

### As Any

`any` is a special type in TypeScript that represents a value of any type. When a value is declared with the any type, the compiler will not perform any type checks or type inference on that value.

```
For example:

let anyValue: any = 42;

// we can assign any value to anyValue, regardless of its type
anyValue = 'Hello, world!';
anyValue = true;
```

### As Const

`as const` is a type assertion in TypeScript that allows you to assert that an expression has a specific type, and that its value should be treated as a read-only value.

For example:
```
const colors = ['red', 'green', 'blue'] as const;

// colors is now of type readonly ['red', 'green', 'blue']
```

Using as const allows TypeScript to infer more accurate types for constants, which can lead to improved type checking and better type inference in your code.

### Non Null Assertion

The non-null assertion operator (!) is a type assertion in TypeScript that allows you to tell the compiler that a value will never be null or undefined.

```
let name: string | null = null;

// we use the non-null assertion operator to tell the compiler that name will never be null
let nameLength = name!.length;
```

The non-null assertion operator is used to assert that a value is not null or undefined, and to tell the compiler to treat the value as non-nullable. However, it's important to be careful when using the non-null assertion operator, as it can lead to runtime errors if the value is actually `null` or `undefined`.

### satisfies Keyword

The `satisfies` operator lets us validate that the type of an expression matches some type, without changing the resulting type of that expression. <br>
Satisfies — це оператор перевірки відповідності типу, який каже: “Переконайся, що цей об’єкт відповідає певному типу, але не міняй його реальний тип”.

🔹 Приклад без satisfies
```
type User = {
    name: string;
    age: number;
};

const user: User = {
    name: "Artur",
    age: 25,
};
```

Все ок ✅
Але тепер якщо ти хочеш, щоб TS перевірив відповідність, але не звузив тип об’єкта, треба satisfies.

🔹 Приклад з satisfies
```
type User = {
    name: string;
    age: number;
};

const user = {
    name: "Artur",
    age: 25,
    role: "admin",
} satisfies User;
```

Тут важливо: <br>
TypeScript перевіряє, що user має принаймні властивості name і age (тип User).
Але при цьому user зберігає всі свої інші поля, і ти можеш їх використовувати: `console.log(user.role); // ✅ працює`

Без `satisfies` (якщо просто `: User`) — властивість `role` була б втрачена (TS би "обрізав" тип).

### У чому різниця між satisfies і as

| Особливість         | `as` (assertion)                       | `satisfies`                       |
| ------------------- | -------------------------------------- | --------------------------------- |
| **Що робить**       | Примушує TS повірити у тип             | Перевіряє відповідність типу      |
| **Перевірка**       | Не перевіряє, чи реально підходить тип | Перевіряє, чи значення відповідає |
| **Безпечність**     | Менш безпечне                          | Більш безпечне                    |
| **Збереження типу** | Втрачає точну форму значення           | Зберігає точну форму значення     |


🔹 Приклад із константами або enum-like об’єктами.

```
type Theme = "light" | "dark";

const colors = {
  light: "#fff",
  dark: "#000",
} satisfies Record<Theme, string>;
```

✅ Якщо ти забереш ключ "dark", отримаєш помилку:

```// ❌ Type '{ light: string; }' is missing the following properties from type 'Record<Theme, string>': dark```

---

## Type Inference

Type inference in TypeScript refers to the process of automatically determining the type of a variable based on the value assigned to it. This allows you to write code that is more concise and easier to understand, as the TypeScript compiler can deduce the types of variables without you having to explicitly specify them.

Here's an example of type inference in TypeScript: `let name = 'John Doe';`

In this example, the TypeScript compiler automatically infers that the type of the name variable is string. This means that you can use the name variable just like any other string in your code, and the TypeScript compiler will ensure that you don't perform any invalid operations on it.

---

## Type Compatibility

TypeScript uses structural typing to determine type compatibility. This means that two types are considered compatible if they have the same structure, regardless of their names.

Here's an example of type compatibility in TypeScript:

```
interface Point {
  x: number;
  y: number;
}

let p1: Point = { x: 10, y: 20 };
let p2: { x: number; y: number } = p1;

console.log(p2.x); // Output: 10
```

In this example, `p1` has the type `Point`, while `p2` has the type `{ x: number; y: number }`. Despite the fact that the two types have different names, they are considered compatible because they have the same structure. This means that you can assign a value of type `Point` to a variable of type `{ x: number; y: number }`, as we do with `p1` and `p2` in this example.

---

## Combining Types

In TypeScript, you can combine types using type union and type intersection.

### Type Union

The union operator `|` is used to combine two or more types into a single type that represents all the possible types. For example:

```
type stringOrNumber = string | number;
let value: stringOrNumber = 'hello';

value = 42;
```

### Type Intersection

The intersection operator & is used to intersect two or more types into a single type that represents the properties of all the types. For example:

```
interface A {
    a: string;
}

interface B {
    b: number;
}

type AB = A & B;
let value: AB = { a: 'hello', b: 42 };
```

### Type Aliases

A Type Alias in TypeScript allows you to create a new name for a type.

Here's an example:

```
type Name = string;
type Age = number;
type User = { name: Name; age: Age };

const user: User = { name: 'John', age: 30 };
```

In the example above, Name and Age are type aliases for string and number respectively. And User is a type alias for an object with properties name of type Name and age of type Age.

### keyof Operator

The `keyof` operator in TypeScript is used to get the union of keys from an object type. Here's an example of how it can be used:

```
interface User {
  name: string;
  age: number;
  location: string;
}

type UserKeys = keyof User; // "name" | "age" | "location"
const key: UserKeys = 'name';
```

`keyof` — це оператор TypeScript, який дозволяє отримати множину ключів об’єкта як union тип.

🔸 Приклад 1: базовий приклад
```
User = {
    name: string;
    age: number;
    isAdmin: boolean;
};

type UserKeys = keyof User;
```

👉 UserKeys тут буде: `type UserKeys = "name" | "age" | "isAdmin";`

🔸 Приклад 2: використання в коді
```
function getValue<T, K extends keyof T>(obj: T, key: K) {
    return obj[key];
}

const user = { name: "Artur", age: 25 };

const name = getValue(user, "name"); // ✅ OK, тип string
const age = getValue(user, "age");   // ✅ OK, тип number
// getValue(user, "email"); ❌ Error: "email" не існує в user
```

➡️ Таким чином, keyof дозволяє обмежити значення параметра key лише ключами об’єкта, що допомагає уникнути помилок.

🔸 Приклад 3: з індексними сигнатурами

```
type Dictionary = {
[key: string]: number;
};

type DictKeys = keyof Dictionary;
```

👉 DictKeys буде типом: `type DictKeys = string | number;`

📘 Чому так?
Бо в JS об’єктні ключі можуть бути або рядками, або числами (які автоматично конвертуються в рядки).

🔸 Приклад 4: keyof typeof

Іноді ми хочемо отримати ключі не типу, а конкретного об’єкта.
Для цього використовуємо typeof разом із keyof.

```
const COLORS = {
    red: "#ff0000",
    green: "#00ff00",
    blue: "#0000ff",
};

type ColorKeys = keyof typeof COLORS;
// "red" | "green" | "blue"

function getColor(name: ColorKeys) {
    return COLORS[name];
}

getColor("red"); // ✅
getColor("yellow"); // ❌ Error
```

🔸 Приклад 5: використання з mapped types

```
type User = {
    name: string;
    age: number;
};

type ReadOnlyUser = {
    readonly [K in keyof User]: User[K];
};

// еквівалентно:
type ReadOnlyUser = {
    readonly name: string;
    readonly age: number;
};
```

Тут `keyof User` дає `"name" | "age"`, і ми ітеративно проходимо всі ключі, створюючи новий тип.

🔸 Підсумок

| Оператор            | Опис                                                             |
| ------------------- | ---------------------------------------------------------------- |
| `keyof T`           | Повертає union тип усіх ключів об’єкта `T`                       |
| `typeof obj`        | Дає тип об’єкта (для роботи з реальними змінними)                |
| `keyof typeof obj`  | Дає union тип усіх ключів конкретного об’єкта                    |
| `K extends keyof T` | Обмежує змінну `K`, щоб вона могла бути лише одним із ключів `T` |

---

## Type Guards | Narrowing

Type guards are a way to narrow down the type of a variable. This is useful when you want to do something different depending on the type of a variable.

🔹 1. Що таке Narrowing (звуження типів)

Narrowing (звуження типів) — це процес, коли TypeScript "звужує" можливі типи змінної, виходячи з умов у коді.

Наприклад:

```
function printId(id: string | number) {
    if (typeof id === "string") {
        // Тут TypeScript знає, що id — string
        console.log(id.toUpperCase());
    } else {
        // А тут id — number
        console.log(id.toFixed(2));
    }
}
```

✅ TypeScript автоматично визначає, що всередині if ми працюємо лише з string, а в else — з number.

🔹 2. Що таке Type Guards (типові перевірки)

Type Guards — це спеціальні перевірки, які допомагають TypeScript зрозуміти, який саме тип даних зараз використовується.

Простіше кажучи: Type Guards — це те, що виконує narrowing.

🔸 Приклад 1. Type Guard через `typeof`
```
function log(value: string | number) {
  if (typeof value === "string") {
    console.log(value.toUpperCase());
  } else {
    console.log(value.toFixed(2));
  }
}
```
🧠 TypeScript бачить typeof value === "string" і звужує тип автоматично.

🔸 Приклад 2. Type Guard через instanceof

```
class Dog {
  bark() { console.log("Woof!"); }
}

class Cat {
  meow() { console.log("Meow!"); }
}

function speak(animal: Dog | Cat) {
  if (animal instanceof Dog) {
    animal.bark();
  } else {
    animal.meow();
  }
}
```
📘 instanceof працює для класів або об’єктів, створених через new.

🔸 Приклад 3. Type Guard через `in`

```
type Car = { drive(): void };
type Boat = { sail(): void };

function move(vehicle: Car | Boat) {
  if ("drive" in vehicle) {
    vehicle.drive();
  } else {
    vehicle.sail();
  }
}
```

🧩 `in` перевіряє наявність властивості у типі.
TypeScript після цієї перевірки "звужує" тип змінної.

🔸 Приклад 5. Narrowing з `== null`

```
function print(value?: string | null) {
  if (value == null) {
    console.log("No value");
  } else {
    console.log(value.toUpperCase());
  }
}
```
➡️ Тут `== null` перевіряє і `null`, і `undefined`, і тому TypeScript у `else` звужує тип до `string`.

🔸 Приклад 6. Discriminated Unions

TypeScript також вміє звужувати типи на основі "мітки" (tag).

```
type Square = { kind: "square"; size: number };
type Circle = { kind: "circle"; radius: number };

type Shape = Square | Circle;

function area(shape: Shape) {
  if (shape.kind === "square") {
    return shape.size ** 2;
  } else {
    return Math.PI * shape.radius ** 2;
  }
}
```

🔹 shape.kind — дискримінатор (ідентифікатор типу). <br>
🔹 TypeScript автоматично звужує тип на основі значення kind.

🧭 Підсумок

| Механізм              | Приклад                        | Для чого використовується |
| --------------------- | ------------------------------ | ------------------------- |
| `typeof`              | `typeof x === "string"`        | Примітиви                 |
| `instanceof`          | `obj instanceof Class`         | Об’єкти / класи           |
| `in`                  | `"prop" in obj`                | Наявність властивості     |
| Кастомний Type Guard  | `value is SomeType`            | Свої типи                 |
| Дискримінований Union | `if (shape.kind === "circle")` | Union із полем-міткою     |

---

## Functions

Functions are a core building block in TypeScript. Functions allow you to wrap a piece of code and reuse it multiple times. Functions in TypeScript can be either declared using function declaration syntax or function expression syntax.

### Typing Functions

In TypeScript, functions can be typed in a few different ways to indicate the input parameters and return type of the function.

Function declaration with types:
```
function add(a: number, b: number): number {
  return a + b;
}
```

Arrow function with types:
```
const multiply = (a: number, b: number): number => {
    return a * b;
};
```

Function type:
```
let divide: (a: number, b: number) => number;

divide = (a, b) => {
    return a / b;
};
```

### Function Overloading

Function Overloading in TypeScript allows multiple functions with the same name but with different parameters to be defined. The correct function to call is determined based on the number, type, and order of the arguments passed to the function at runtime.

```
function add(a: number, b: number): number;
function add(a: string, b: string): string;

function add(a: any, b: any): any {
  return a + b;
}

console.log(add(1, 2)); // 3
console.log(add('Hello', ' World')); // "Hello World"
```

---

## Interfaces

Interfaces in TypeScript provide a way to define a contract for a type, which includes a set of properties, methods, and events. It's used to enforce a structure for an object, class, or function argument. Interfaces are not transpiled to JavaScript and are only used by TypeScript at compile-time for type-checking purposes.

Here's an example of defining and using an interface in TypeScript:

```
interface User {
  name: string;
  age: number;
}

const user: User = {
  name: 'John Doe',
  age: 30,
};
```

### Types vs Interfaces

In TypeScript, both types and interfaces can be used to define the structure of objects and enforce type checks. However, there are some differences between the two.

Types are used to create a new named type based on an existing type or to combine existing types into a new type. They can be created using the type keyword. For example:
```
type Person = {
  name: string;
  age: number;
};

const person: Person = {
  name: 'John Doe',
  age: 30,
};
```

Interfaces, on the other hand, are used to describe the structure of objects and classes. They can be created using the interface keyword. For example:

```
interface Person {
    name: string;
    age: number;
}

const person: Person = {
    name: 'John Doe',
    age: 30,
};
```

🔹 1. Загальна ідея

| Keyword         | Для чого використовується                                                    |
| --------------- | ---------------------------------------------------------------------------- |
| **`interface`** | Для опису **структури об’єктів або класів** (контракту).                     |
| **`type`**      | Для **визначення типів будь-чого**: об’єктів, union’ів, tuple, функцій тощо. |

🔸 2. Приклад базової схожості

```
interface User {
  name: string;
  age: number;
}

type UserType = {
  name: string;
  age: number;
};
```

Обидва варіанти абсолютно однакові:<br>
✅ описують структуру об’єкта з полями name і age.<br>
TS не робить різниці під час перевірки типів.

🔹 3. Основні відмінності<br>
🧱 1. Розширення (Extending)<br>
🔸 Interface може розширювати інші інтерфейси або кілька одразу:
```
interface Person {
  name: string;
}

interface Employee extends Person {
  company: string;
}
```

🔸 Type використовує & (intersection):
```
type Person = { name: string };
type Employee = Person & { company: string };
```
✅ Обидва працюють, але синтаксис різний.

🧩 2. Merge Declaration (злиття інтерфейсів)<br>
Це фішка тільки interface.
```
interface User {
  name: string;
}

interface User {
  age: number;
}

const user: User = { name: "Artur", age: 25 };
```

✅TypeScript об’єднає обидва interface User в один:
```
// результат:
interface User {
  name: string;
  age: number;
}
```
❌ `type` так не вміє — якщо ти спробуєш повторно оголосити `type`, буде помилка:
```
type User = { name: string };
type User = { age: number }; // ❌ Error: Duplicate identifier
```
📘 Висновок:
`interface` добре підходить для API або бібліотек, де користувачі можуть дописувати поля через декларативне злиття.

⚙️ 3. `Union` і `Tuple` типи — тільки через `type`
```
type Status = "active" | "inactive" | "banned";

type Pair = [string, number];
```
❌ Таке з interface зробити не можна.

🧠 4. Використання з Utility Types <br>
type зручніший, коли потрібно комбінувати, модифікувати чи створювати складні типи:

```
type User = { name: string; age: number };
type PartialUser = Partial<User>; // { name?: string; age?: number }
```
Хоча interface теж можна передавати у utility типи, але type краще для складних випадків.

🧰 5. Використання з класами <br>
І interface, і type можуть описувати форму класу, але зазвичай використовують interface:
```
interface Flyable {
  fly(): void;
}

class Bird implements Flyable {
  fly() {
    console.log("Flying...");
  }
}
```
✅ interface більш природно підходить під об’єктно-орієнтований стиль.

🔹 4. Коли що використовувати 🧭

| Ситуація                                                          | Рекомендація                                        |
| ----------------------------------------------------------------- | --------------------------------------------------- |
| Описуєш **об’єкт або клас**                                       | ✅ Використовуй `interface`                          |
| Описуєш **складний тип**, union, intersection, tuple              | ✅ Використовуй `type`                               |
| Пишеш **публічний API/SDK**, який можна буде **розширювати**      | ✅ Використовуй `interface` (через можливість merge) |
| Потрібно створювати **utility типи або складні обчислювані типи** | ✅ Використовуй `type`                               |
| Потрібно просто описати форму даних без розширення                | Будь-що — `interface` або `type`, не має різниці    |

🧩 6. Коротке резюме

| Критерій                             | `interface`        | `type`                        |
| ------------------------------------ | ------------------ | ----------------------------- |
| Може бути об’єднаний (merged)        | ✅ Так              | ❌ Ні                          |
| Може бути union або tuple            | ❌ Ні               | ✅ Так                         |
| Розширення інших типів               | ✅ Через `extends`  | ✅ Через `&`                   |
| ООП стиль (implements)               | ✅ Краще підходить  | ⚙️ Можна, але не типово       |
| Utility типи (Partial, Pick...)      | ✅ Так              | ✅ Так                         |
| Зрозумілість у великих кодових базах | ✅ Зручніше для API | ✅ Зручніше для складних типів |

### Extending Interfaces

In TypeScript, you can extend an interface by creating a new interface that inherits from the original interface using the "extends" keyword.

---

## Classes

### Constructor Params

In TypeScript, constructor parameters can be declared with access modifiers (e.g. public, private, protected) and/or type annotations. The parameters are then automatically assigned to properties of the same name within the constructor, and can be accessed within the class. For example:
```
class Example {
  constructor(private name: string, public age: number) {}
}
```

### Access Modifiers
`private`, `public`, `protected` are access modifiers in TypeScript that determine the visibility of class members (properties and methods).

### Abstract Classes

### Inheritance vs Polymorphism

Inheritance and polymorphism are two fundamental concepts in object-oriented programming, and they are supported in TypeScript as well.

Inheritance refers to a mechanism where a subclass inherits properties and methods from its parent class. This allows a subclass to reuse the code and behavior of its parent class while also adding or modifying its own behavior. In TypeScript, inheritance is achieved using the extends keyword.

Polymorphism refers to the ability of an object to take on many forms. This allows objects of different classes to be treated as objects of a common class, as long as they share a common interface or inheritance hierarchy. In TypeScript, polymorphism is achieved through method overriding and method overloading.
```
class Animal {
  makeSound(): void {
    console.log('Making animal sound');
  }
}

class Dog extends Animal {
  makeSound(): void {
    console.log('Bark');
  }
}

class Cat extends Animal {
  makeSound(): void {
    console.log('Meow');
  }
}

let animal: Animal;

animal = new Dog();
animal.makeSound(); // Output: Bark

animal = new Cat();
animal.makeSound(); // Output: Meow
```

### Method Overriding

In TypeScript, method overriding is a mechanism where a subclass provides a new implementation for a method that is already defined in its parent class. This allows the subclass to inherit the behavior of the parent class, but change its behavior to fit its own needs.

### Constructor Overloading

In TypeScript, you can achieve constructor overloading by using multiple constructor definitions with different parameter lists in a single class. Given below is the example where we have multiple definitions for the constructor:

```
class Point {
  // Overloads
  constructor(x: number, y: string);
  constructor(s: string);
  constructor(xs: any, y?: any) {
    // TBD
  }
}
```

Note that, similar to function overloading, we only have one implementation of the constructor and it's the only the signature that is overloaded.

---

## Generics

Generics in TypeScript are a way to write code that can work with multiple data types, instead of being limited to a single data type. Generics allow you to write functions, classes, and interfaces that take one or more type parameters, which act as placeholders for the actual data types that will be used when the function, class, or interface is used.

For example, the following is a generic function that takes a single argument of any data type and returns the same data type:

```
function identity<T>(arg: T): T {
  return arg;
}

let output = identity<string>('Hello'); // type of output will be 'string'
```
In this example, the identity function takes a single argument of any data type and returns the same data type. The actual data type is specified when the function is called by using <string> before the argument "Hello".

Generics can also be used with classes, interfaces, and object types, allowing them to work with multiple data types as well.  For example:
```
class GenericNumber<T> {
  zeroValue: T;
  add: (x: T, y: T) => T;
}

let myGenericNumber = new GenericNumber<number>();
myGenericNumber.zeroValue = 0;
myGenericNumber.add = function (x, y) {
  return x + y;
};
```

### Generic Constraints

Generic constraints in TypeScript allow you to specify the requirements for the type parameters used in a generic type. These constraints ensure that the type parameter used in a generic type meets certain requirements.

Constraints are specified using the `extends` keyword, followed by the type that the type parameter must extend or implement.

```
interface Lengthwise {
  length: number;
}

function loggingIdentity<T extends Lengthwise>(arg: T): T {
  // Now we know it has a .length property, so no more error
  console.log(arg.length);

  return arg;
}

loggingIdentity(3); // Error, number doesn't have a .length property
loggingIdentity({ length: 10, value: 3 }); // OK
```

In this example, the `Lengthwise` interface defines a `length` property. The `loggingIdentity` function uses a generic type parameter `T` that is constrained by the `Lengthwise` interface, meaning that the type parameter must extend or implement the `Lengthwise` interface. This constraint ensures that the length property is available on the argument passed to the `loggingIdentity` function.

--

## Decorators

Decorators are a feature of TypeScript that allow you to modify the behavior of a class, property, method, or parameter. They are a way to add additional functionality to existing code, and they can be used for a wide range of tasks, including logging, performance optimization, and validation.

Here's an example of how you might use a decorator in TypeScript:

```
function log(
  target: Object,
  propertyKey: string | symbol,
  descriptor: PropertyDescriptor
) {
  const originalMethod = descriptor.value;

  descriptor.value = function (...args: any[]) {
    console.log(`Calling ${propertyKey} with arguments: ${args}`);
    return originalMethod.apply(this, args);
  };

  return descriptor;
}

class Calculator {
  @log
  add(a: number, b: number): number {
    return a + b;
  }
}

const calculator = new Calculator();
calculator.add(1, 2);
// Output: Calling add with arguments: 1,2
// Output: 3
```

In this example, we use the `@log` decorator to modify the behavior of the `add` method in the `Calculator` class. The `log` decorator logs the arguments passed to the method before calling the original method. This allows us to see what arguments are being passed to the method, without having to modify the method's code.

🔹 Типи декораторів

| Тип декоратора          | Використовується для | Приклад синтаксису            |
| ----------------------- | -------------------- | ----------------------------- |
| **Class decorator**     | Класів               | `@Logger`                     |
| **Property decorator**  | Властивостей класу   | `@Readonly` name: string;     |
| **Method decorator**    | Методів класу        | `@LogExecution` run() {}      |
| **Accessor decorator**  | get/set методів      | `@CheckAccess` get value() {} |
| **Parameter decorator** | Параметрів методу    | `method(@Inject service) {}`  |

🔹 Приклад декоратора методу

```
function Log(
  target: any,
  propertyName: string,
  descriptor: PropertyDescriptor
) {
  const original = descriptor.value;
  descriptor.value = function (...args: any[]) {
    console.log(`Calling ${propertyName} with`, args);
    const result = original.apply(this, args);
    console.log(`Result:`, result);
    return result;
  };
}

class MathService {
  @Log
  add(a: number, b: number) {
    return a + b;
  }
}

const m = new MathService();
m.add(2, 3);
```

🔹 Декоратори властивостей

```
function Readonly(target: any, propertyName: string) {
  Object.defineProperty(target, propertyName, {
    writable: false,
  });
}

class Example {
  @Readonly
  title = 'Hello';
}

const e = new Example();
e.title = 'Changed'; // ❌ Помилка в runtime: не можна змінити властивість
```

🔹 Увімкнення декораторів у TypeScript<br>
Щоб їх можна було використовувати, треба включити їх у `tsconfig.json`:
```
{
  "compilerOptions": {
    "experimentalDecorators": true
  }
}
```

🔹 Використання в реальних проєктах

Декоратори активно використовуються у фреймворках:

🏗 NestJS — для визначення контролерів, сервісів і залежностей:

```
@Controller('users')
export class UserController {
  constructor(private readonly service: UserService) {}

  @Get()
  findAll() {}
}
```

🧩 TypeORM — для опису сутностей бази даних:

```
@Entity()
class User {
  @PrimaryGeneratedColumn()
  id: number;

  @Column()
  name: string;
}
```

---

## Utility Types

TypeScript provides several utility types that can be used to manipulate and transform existing types. Here are some of the most common ones:

`Partial`: makes all properties of a type optional.
`Readonly`: makes all properties of a type read-only.
`Pick`: allows you to pick specific properties from a type.
`Omit`: allows you to omit specific properties from a type.
`Exclude`: creates a type that is the set difference of A and B.
..and more.

### Partial

The Partial type in TypeScript allows you to make all properties of a type optional. This is useful when you need to create an object with only a subset of the properties of an existing type.

Here's an example of using the Partial type in TypeScript:

```
interface User {
  name: string;
  age: number;
  email: string;
}

function createUser(user: Partial<User>): User {
  return {
    name: 'John Doe',
    age: 30,
    email: 'john.doe@example.com',
    ...user,
  };
}

const newUser = createUser({ name: 'Jane Doe' });

console.log(newUser);
// Output: { name: 'Jane Doe', age: 30, email: 'john.doe@example.com' }
```

### Pick

Pick constructs a type by picking the set of properties Keys (string literal or union of string literals) from Type.

```
interface Todo {
  title: string;
  description: string;
  completed: boolean;
}

type TodoPreview = Pick<Todo, 'title' | 'completed'>;

const todo: TodoPreview = {
  title: 'Clean room',
  completed: false,
};
```

### Omit

Omit constructs a type by picking all properties from Type and then removing Keys (string literal or union of string literals).

```
interface Todo {
  title: string;
  description: string;
  completed: boolean;
  createdAt: number;
}

type TodoPreview = Omit<Todo, 'description'>;

const todo: TodoPreview = {
  title: 'Clean room',
  completed: false,
  createdAt: 1615544252770,
};

type TodoInfo = Omit<Todo, 'completed' | 'createdAt'>;

const todoInfo: TodoInfo = {
  title: 'Pick up kids',
  description: 'Kindergarten closes at 5pm',
};
```

### Readonly

Readonly constructs a type with all properties of Type set to readonly, meaning the properties of the constructed type cannot be reassigned.

```
interface Todo {
  title: string;
}

const todo: Readonly<Todo> = {
  title: 'Delete inactive users',
};

// Cannot assign to 'title' because it is a read-only property.
todo.title = 'Hello';
```

### Record

Record constructs an object type whose property keys are Keys and whose property values are Type. This utility can be used to map the properties of a type to another type.

```
interface CatInfo {
  age: number;
  breed: string;
}

type CatName = 'miffy' | 'boris' | 'mordred';

const cats: Record<CatName, CatInfo> = {
  miffy: { age: 10, breed: 'Persian' },
  boris: { age: 5, breed: 'Maine Coon' },
  mordred: { age: 16, breed: 'British Shorthair' },
};
```

У TypeScript утилітний тип Record — це вбудований generic-тип, який дозволяє створювати об’єкт із певним набором ключів і типом значень.

```
Record<Keys, Type>
```

- `Keys` — це тип ключів (наприклад, `'a' | 'b' | 'c'` або `string`).
- `Typ`e — це тип значення для кожного ключа.

🔹 Простий приклад

```
type UserRoles = Record<'admin' | 'user' | 'guest', string>;
```

Еквівалентно такому:
```
type UserRoles = {
  admin: string;
  user: string;
  guest: string;
};
```

Тобто:
```
const roles: UserRoles = {
  admin: 'Can manage users',
  user: 'Can view listings',
  guest: 'Can only browse'
};
```

🔹 Приклад з типом ключа string

```
const phoneBook: Record<string, number> = {
  Artur: 123456789,
  Alice: 987654321
};
```
👉 Це означає, що ключі можуть бути будь-якими рядками, а значення — числами.

🔹 Приклад з enum

```
enum Status {
  Active = 'active',
  Inactive = 'inactive',
  Pending = 'pending'
}

type StatusColors = Record<Status, string>;

const colors: StatusColors = {
  [Status.Active]: 'green',
  [Status.Inactive]: 'gray',
  [Status.Pending]: 'yellow'
};
```

🔹 Порівняння з індексними типами

Ось таке:

```
type UserAges = { [key: string]: number };
```

еквівалентне:

```
type UserAges = Record<string, number>;
```

✅ Але Record більш зручний і читається краще, особливо коли Keys — не просто string, а конкретні значення або enum.

🔹 Підсумок

| Параметр                  | Опис                                                |
| ------------------------- | --------------------------------------------------- |
| **Призначення**           | Створює об’єктний тип із фіксованими ключами        |
| **Синтаксис**             | `Record<Keys, Type>`                                |
| **Корисно для**           | Мапінгу ключів на тип значень                       |
| **Приклади застосування** | Enum → value, Role → permissions, Route → component |

### Exclude

Exclude constructs a type by excluding from UnionType all union members that are assignable to ExcludedMembers.

```
type T0 = Exclude<'a' | 'b' | 'c', 'a'>; // "b" | "c"
type T1 = Exclude<'a' | 'b' | 'c', 'a' | 'b'>; // "c"
type T2 = Exclude<string | number | (() => void), Function>; // string | number
```

Exclude — це ще один вбудований утилітний тип у TypeScript, який дозволяє виключити певні типи з об’єднання (union type).

```
Exclude<UnionType, ExcludedMembers>
```

- `UnionType` — це об’єднання типів (наприклад, `'a' | 'b' | 'c'`).
- `ExcludedMembers` — тип або типи, які треба виключити з цього об’єднання.

```
type Letters = 'a' | 'b' | 'c';
type WithoutB = Exclude<Letters, 'b'>;
```

✅ Тепер WithoutB → `'a' | 'c'`

🔹 Часте використання з keyof

```
interface User {
  id: number;
  name: string;
  password: string;
}

type PublicFields = Exclude<keyof User, 'password'>;
// 'id' | 'name'
```

Тобто `PublicFields` — це всі ключі `User`, крім `password`.

### Extract

Extract constructs a type by extracting from Type all union members that are assignable to Union.

Є утилітний тип Extract, який є протилежністю Exclude. Він дозволяє витягти з об’єднання (union type) лише ті типи, які сумісні з вказаним типом.

### Awaited

???

### Parameters

???

### NonNullable

Видаляє null і undefined з типу.

✅ Приклад:

```
type User = string | null | undefined;
type CleanUser = NonNullable<User>;
// CleanUser = string
```

🧠 Корисно коли:

Ти хочеш “очистити” тип, щоб він гарантовано не був null або undefined.

```
function printName(name: NonNullable<string | undefined>) {
  console.log(name.toUpperCase());
}
```

### ReturnType

Витягує тип, який повертає функція.

```
function getUser() {
  return { name: 'Artur', age: 25 };
}

type User = ReturnType<typeof getUser>;
// User = { name: string; age: number }
```

🧠 Це дуже зручно, якщо у тебе є функція, і ти хочеш повторно використати її результат як тип (наприклад, у тестах, DTO або сервісах).

### InstanceType

???

---

## Advanced Types

https://roadmap.sh/typescript

### Mapped Types

Mapped types in TypeScript are a way to create a new type based on an existing type, where each property of the existing type is transformed in some way. Mapped types are declared using a combination of the keyof operator and a type that maps each property of the existing type to a new property type.

For example, the following is a mapped type that takes an object type and creates a new type with all properties of the original type but with their type changed to `readonly`:

```
type Readonly<T> = {
  readonly [P in keyof T]: T[P];
};

let obj = { x: 10, y: 20 };
let readonlyObj: Readonly<typeof obj> = obj;
```

### Conditional Types

Conditional types in TypeScript are a way to select a type based on a condition. They allow you to write a type that dynamically chooses a type based on the types of its inputs. Conditional types are declared using a combination of the infer keyword and a type that tests a condition and selects a type based on the result of the test.

For example, the following is a conditional type that takes two types and returns the type of the first argument if it extends the second argument, and the type of the second argument otherwise:

```
type Extends<T, U> = T extends U ? T : U;

type A = Extends<string, any>; // type A is 'string'
type B = Extends<any, string>; // type B is 'string'
```

📌 Простий приклад

```
type IsString<T> = T extends string ? true : false;

type A = IsString<string>; // true
type B = IsString<number>; // false
```

### Literal Types

Literal types in TypeScript are a way to specify a value exactly, rather than just a type. Literal types can be used to enforce that a value must be of a specific type and a specific value. Literal types are created by using a literal value, such as a string, number, or boolean, as a type.

For example, the following is a literal type that represents a value of 42:

```
type Age = 42;

let age: Age = 42; // ok
let age: Age = 43; // error
``` 

### Template Literal Types

Template literal types in TypeScript are a way to manipulate string values as types. They allow you to create a type based on the result of string manipulation or concatenation. Template literal types are created using the backtick (``) character and string manipulation expressions within the type.

For example, the following is a template literal type that concatenates two strings:

```
type Name = `Mr. ${string}`;

let name: Name = `Mr. Smith`;  // ok
let name: Name = `Mrs. Smith`;  // error
```

In this example, the Name template literal type is created by concatenating the string "Mr. " with the type string. This type can then be used to enforce that a value must be a string that starts with "Mr. ".

### Recursive Types

Recursive types in TypeScript are a way to define a type that references itself. Recursive types are used to define complex data structures, such as trees or linked lists, where a value can contain one or more values of the same type.

For example, the following is a recursive type that represents a linked list:

```
type LinkedList<T> = {
  value: T;
  next: LinkedList<T> | null;
};

let list: LinkedList<number> = {
  value: 1,
  next: { value: 2, next: { value: 3, next: null } },
};
```

In this example, the LinkedList type is defined as a type that extends T and contains a property next of the same type LinkedList<T>. This allows us to create a linked list where each node contains a value of type T and a reference to the next node in the list.

---

## Modules

In TypeScript, modules are used to organize and reuse code. There are two types of modules in TypeScript:
- Internal
- External

Internal modules are used to organize code within a file and are also referred to as namespaces. They are defined using the "namespace" keyword.

External modules are used to organize code across multiple files. They are defined using the "export" keyword in one file and the "import" keyword in another file. External modules in TypeScript follow the CommonJS or ES modules standards.

Here is an example of how you can use internal modules in TypeScript:

```
// myModule.ts
namespace MyModule {
  export function doSomething() {
    console.log('Doing something...');
  }
}

// main.ts
/// <reference path="myModule.ts" />
MyModule.doSomething(); // Output: "Doing something..."
```

Modules в TypeScript — це спосіб організувати код у окремі файли та робити змінні/функції/класи доступними в інших файлах через export / import.

Типово кожен файл у TypeScript (або JavaScript), який містить хоч один import або export, автоматично стає модулем.

🎯 Для чого потрібні модулі?
- Розділити код на логічні частини
- Повторно використовувати функції/класи в різних файлах
- Уникати конфліктів глобальних змінних
- Робити код чистішим, структурованим і масштабованим

📦 Як працюють модулі в TS
1. Експорт
Експортували — значить дали можливість іншим файлам це використовувати.

👉 Іменований експорт:
```
// math.ts
export const sum = (a: number, b: number) => a + b;
export const multiply = (a: number, b: number) => a * b;
```
👉 Експорт за замовчуванням:
```
// utils.ts
export default function log(message: string) {
  console.log(message);
}
```

2) Імпорт
Іменований імпорт:
```
import { sum, multiply } from './math';
```

Імпорт всього модуля:
```
import * as MathUtils from './math';

// MathUtils.sum()
```

Імпорт за замовчуванням:
```
import log from './utils';
```

🔗 Види модулів у TypeScript

TypeScript підтримує два основних модульних формати:

1) ES Modules (ESM) — сучасний стандарт

Працює з ключовими словами import / export.

TS компілює в залежності від module у tsconfig:

```
{
    "compilerOptions": {
        "module": "ESNext"  // або "ES2020", "CommonJS", "AMD" тощо
    }
}
```

2) CommonJS (CJS) — формат Node.js до ESM
```
   // CommonJS
   const fs = require('fs');
   module.exports = { ... };
```

TypeScript може компілювати код у CommonJS.

🌐 Глобальний скрипт vs Модуль

Файл без імпортів/експортів — вважається глобальним скриптом:

```
// global.ts
let a = 10;  // стає глобальною змінною
```

Файл з імпортом або експортом — це модуль, і всі змінні всередині доступні тільки у цьому файлі.

```
// module.ts
export const a = 10;  // не глобальна
```

🧩 Як TS обробляє модулі?
1. Ти пишеш TypeScript-код з import та export
2. TypeScript компілює його в JS, використовуючи формат модулів зазначений у tsconfig
3. Під час виконання Node.js або браузер використовує модульну систему:
- Node.js використовує ES Modules або CommonJS
- Браузер — тільки ES Modules

🔥 Приклад складніший: Експорт типів

TypeScript дозволяє експортувати і типи:

```
// types.ts
export type User = {
    id: number;
    name: string;
};
```

І імпортувати так:

```
import { User } from "./types";

const u: User = { id: 1, name: "Artur" };
```

Або тільки як тип:

```
import type { User } from "./types";
```

Це не потрапить у згенерований JS — лише для компілятора.

🧠 Підсумок

| Що?                   | Пояснення                                    |
| --------------------- | -------------------------------------------- |
| **Модуль**            | Файл з import/export                         |
| **Експорт**           | Робить частину коду доступною зовні          |
| **Імпорт**            | Дозволяє використовувати код з іншого модуля |
| **ESM**               | Сучасний формат модулів                      |
| **CommonJS**          | Старий формат Node.js                        |
| **Глобальний скрипт** | Файл без модулів, всі змінні глобальні       |

### Namespaces

NAMESPACES ARE DEPRECATED APPROACH — use ES Modules instead.

In TypeScript, namespaces are used to organize and share code across multiple files. Namespaces allow you to group related functionality into a single unit and prevent naming conflicts.

Here's an example of how you can use namespaces in TypeScript:

```
// myNamespace.ts
namespace MyNamespace {
  export function doSomething() {
    console.log('Doing something...');
  }
}

// main.ts
/// <reference path="myNamespace.ts" />
MyNamespace.doSomething(); // Output: "Doing something..."
```

### Ambient Modules

???

### External Modules

In TypeScript, external modules allow you to organize and share code across multiple files. External modules in TypeScript follow the CommonJS or ES modules standards.

Here's an example of how you can use external modules in TypeScript:

```
// myModule.ts
export function doSomething() {
  console.log('Doing something...');
}

// main.ts
import { doSomething } from './myModule';
doSomething(); // Output: "Doing something..."
```

### Namespace Augmentation

In TypeScript, namespace augmentation is a way to extend or modify existing namespaces. This is useful when you want to add new functionality to existing namespaces or to fix missing or incorrect declarations in third-party libraries.

Here's an example of how you can use namespace augmentation in TypeScript:

```
// myModule.d.ts
declare namespace MyModule {
  export interface MyModule {
    newFunction(): void;
  }
}

// main.ts
/// <reference path="myModule.d.ts" />
namespace MyModule {
  export class MyModule {
    public newFunction() {
      console.log('I am a new function in MyModule!');
    }
  }
}

const obj = new MyModule.MyModule();
obj.newFunction(); // Output: "I am a new function in MyModule!"
```

### Global Augmentation

In TypeScript, global augmentation is a way to add declarations to the global scope. This is useful when you want to add new functionality to existing libraries or to augment the built-in types in TypeScript.

Here's an example of how you can use global augmentation in TypeScript:

```
// myModule.d.ts
declare namespace NodeJS {
  interface Global {
    myGlobalFunction(): void;
  }
}

// main.ts
global.myGlobalFunction = function () {
  console.log('I am a global function!');
};

myGlobalFunction(); // Output: "I am a global function!"
```

In this example, we declare a new namespace "NodeJS" and add an interface "Global" to it. Within the "Global" interface, we declare a new function "myGlobalFunction".

---

## Ecosystem

### Formatting

Prettier is an opinionated code formatter with support for JavaScript, HTML, CSS, YAML, Markdown, GraphQL Schemas. By far the biggest reason for adopting Prettier is to stop all the on-going debates over styles. Biome is a faster alternative to Prettier! (It also does linting!)

### Linting

With ESLint you can impose the coding standard using a certain set of standalone rules.

???

### Useful Packages

???

### Build Tools

???

---