[< Назад](./index.md)

### Попалась на собесе в Яндексе.

<br/>

```ts
// Реализуйте функцию sum.

const r1 = sum(1)(2)(3)() // 6
const r2 = sum(1)(2)() // 3

// sum(1)(2)...(n)()
```

<details>
  <summary>Answer</summary>

```ts
function sum(a) {
    return (b) => b === undefined
            ? a
            : sum(a + b)
}
```

</details>