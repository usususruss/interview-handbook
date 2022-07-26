[< Назад](./index.md)

### Попалась на 3 этапе собесов в Яндексе.

<br/>

```ts
/*
 * Напишите функцию повторяющую запрос attempts раз при ошибке запроса.
 * Если через delay миллисекунд сервер не ответил - переходите к следующей
 * попытке.
 * 
 * Дополнительно: сохраняйте и возвращайте последний ошибочный сервера.
 * Если его нет - возвращайте строку-заглушку "Attempts number exceeded!"
 */

interface ErrorResponse {
    status: number
    message: string
}

function tryRequest<T>(
    requestFun: () => Promise<T>,
    attempts: number = 3,
    delay: number = 100
): Promise<T | ErrorResponse | string> {
    // ...
}
```