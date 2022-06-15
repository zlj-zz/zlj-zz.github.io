---
title: pigit 快速使用 git
date: 2022-04-24
category:
---

- `?`:表示属性或参数为可选项

  ```ts
  interface Person {
    name: string;
    age: number;
    weight?: number;
  }

  const person = new Person((name = "Bob"), (age = 18));
  ```

- `??`:空值合并运算符。当左侧操作数为 null 或 undefined 时，其返回右侧的操作数，否则返回左侧的操作数。
  ```ts
  console(person.weight ?? 0);
  ```
- `!`:表示类型推断排除 `null`、`undefined`
  ```ts
  function myFunc(maybeString: string | undefined | null) {
    const onlyString: string = maybeString; // Error
    const ignoreUndefinedAndNull: string = maybeString!; // Ok
  }
  ```
- `!!`:用于将类型转换为布尔值。其中 `null`、`undefined`、`0`、`''` 转换后为 `false`，其他都为 `true`。

  ```ts
  let a = null;
  console.log(!!a); // false

  let b = "hello world";
  console.log(!!b); // true

  let bob = { name: "Bob", age: 18 };
  console.log(!!bob); // true
  ```
