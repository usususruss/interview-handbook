[< Назад](./index.md)

```ts
// Реализовать алгоритм сжатия строки "AAADDDGFFFAAR" → "3A3DG3F2AR"

function zipString(str) {
    // ...
}
```

<details>
  <summary>Answer</summary>

```ts
const zipString = (str) => {
    if (!str) return ''

    let zippingLetter = ''
    let zippingCount = 0
    let zipped = ''
    let index = 0

    function getStackContentZipped(letter, count) {
        return `${count === 1 ? '' : count}${letter}`
    }

    while(index < str.length) {
        const currentLetter = str[index]

        if (!zippingLetter) {
            zippingLetter = currentLetter
            zippingCount += 1
            continue
        }

        if (zippingLetter === currentLetter) {
            zippingCount += 1
        } else {
            zipped += getStackContentZipped(zippingLetter, zippingCount)
            zippingLetter = currentLetter
            zippingCount = 1
        }

        index++
    }

    zipped += getStackContentZipped(zippingLetter, zippingCount)

    return zipped
}
```

</details>