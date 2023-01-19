[< На главную](../README.md)

# Вопросы с интервью Точка банк

<br/>

## Вакансия

Специализация: Front-end developer, JavaScript <br />
Уровень: Senior <br/>
Вилка: <b>230 000 - 450 000</b>

[Скрин вакансии](./Frontend_Developer_Tochka_bank_getmatch.png)


<br />
<br />

## Вопросы

* JavaScript
    - [Ugly function](#js-ugly-function)
    - [Context check](#js-context-check)
    - [Context check on classes](#js-context-check-on-classes)
    - [Renders and tasks](#js--browser-renders-and-tasks)
* TypeScript
    - [any and unknown](#typescript-any-and-unknown)
    - [void, undefined, never](#typescript-void-undefined-never)
    - [types operations](#typescript-types-operations)
    - [as, is](#typescript-as-is)
    - [own types](#typescript-own-types)
    - [infer](#typescript-infer)
* React:
    - [Classes to FC](#react-classes-to-fc)
    - [Reconciliation](#react-reconciliation)

<br/>
<br/>
<br/>






### JS: Ugly function
<br/>

```ts
/**
 * Что делает эта функция? Нужно переписать её, чтобы она была читаема
 */ 

function wtf(s) {
    for (var i = s.length - 1, o = ''; i >= 0; o += s[i--]) {}

    return o;
}
```
<details>
  <summary>Answer</summary>

```ts
/* 
 * Complexity of 'wtf' is O(n).
 * Result example:
 * 
 * 'abc' -> 'cba'
 * [1, 2, 3, 5] -> '5321'
 */

//   O(3n)
function reverse(strOrArr) {
    let result = strOrArr
    if (typeof strOrArr === 'string') {
        result = strOrArr.split('')
    }
    
    return result.reverse().join('');
    // return [...strOrArr].reverse().join('');
}
```

</details>


<hr/>
<br/>









### JS: Context check

<br/>

```ts
/* Что выведет в консоль? */

const cat = {
    sound: 'meow',
    say() {
        console.log(this.sound)
    },
    say2: () => {
        console.log(this.sound)
    }
}

const s = cat.say.bind(cat);

cat.say();   // ?
cat.say2();  // ?
s();         // ?
```

<details>
  <summary>Answer</summary>

```ts
cat.say();  // 'meow'
cat.say2(); // undefined
s();        // 'meow'
```

</details>

<hr/>
<br/>







### JS: Context check on classes

<br/>

```ts
class Cat {
    sound = 'meow'
    say() {
        console.log(this.sound)
    }
    say2 = () => {
        console.log(this.sound)
    }
}

const cat1 = new Cat();
const cat2 = new Cat();

cat1.say(); // ?
cat1.say2(); // ?

cat1.say === cat2.say // ?
cat1.say2 === cat2.say2 // ?
```

<details>
  <summary>Answer</summary>

```ts
cat1.say(); // 'meow'
cat1.say2(); // 'meow'

cat1.say === cat2.say // true
cat1.say2 === cat2.say2 // false
```

</details>
<hr/>
<br/>









### JS & Browser: Renders and tasks

<br/>

```ts
/*
 * 1. Будет ли "подвешивать" браузер func1() ?
 * 2. Будет ли "подвешивать" браузер func2() ?
 */

function func1() {
    return setTimeout(func1, 0);
}

function func2() {
    return Promise.resolve().then(func2);
}
```

<details>
  <summary>Answer</summary>

```ts
func1()   // no lag, macro-tasks pass renders beside execution
func2()   // lag! micro-tasks do not pass renders
```

</details>
<hr/>
<br/>








### TypeScript: any and unknown

<br/>

```ts
/*
 * 1. Что такое any ?
 * 2. Что такое unknown ?
 * 3. Будут ли ошибки при вызове ?
 */

let foo: any
let bar: unknown

foo.fn() // ?
bar.fn() // ?

```

<details>
  <summary>Answer</summary>

```ts
foo.fn() // ok
bar.fn() // error
```

</details>
<hr/>
<br/>










### TypeScript: void, undefined, never

<br/>

```ts
/*
 * 1. В чём отличие fn и fn2 ?
 * 2. Что означает never?
 * 3. Привидите примеры использования never?
 */

function fn(): void {}
function fn2(): undefined {}
```

<details>
  <summary>Answer</summary>

```ts
// never examples

function fn3(): never {
    throw new Error()
}

function fn4(): never {
    while(true) {}
}

type Foo<T> = T extends Promise ? bool : never
```

</details>
<hr/>
<br/>









### TypeScript: types operations

<br/>

```ts
/*
 * 1. Какой тип будет в t1?
 * 2. Какой тип будет в t2?
 */
type Type1 = {
    foo: string | boolean | number;
    bar: string;
}

type Type2 = {
    state: string;
    bar: string;
}

const t1: Type1 | Type2;    // ?
const t2: Type1 & Type2;    // ?
```

<details>
  <summary>Answer</summary>

```ts
t1 // bar state | bar foo
t2 // foo bar state
```

</details>
<hr/>
<br/>












### TypeScript: as, is

<br/>

```ts
/* Как заставить TS понимать, что в условии коллбека он будет работать с Type1? */

type Type1 = {
    foo: string | boolean | number;
    bar: string;
}

type Type2 = {
    state: string;
    bar: string;
}

fetch('/some-api')
    .then((response: Type1 | Type2) => {
        
        if (isType1(response)) { // here is Type1
            console.log(response.foo); // ok    
        }
        
    })
```

<details>
  <summary>Answer</summary>

```ts
function isType1(data: unknown): data is Type1 {
    return !!data &&
           typeof data === 'object' &&
           Object.prototype.hasOwnProperty.call(data, 'foo')
}

function isType1(data: unknown): data is Type1 {
    return !!data &&
           typeof data === 'object' &&
           'foo' in data
}
```

</details>
<hr/>
<br/>









### TypeScript: own types

<br/>

```ts
/* Создайте generic тип Nullable. */

type Type = {
    state: string;
    bar: string;
}

type TypeNullable = Nullable<Type> 

// type TypeNullable { state: string | null, bar: string | null }

```

<details>
  <summary>Answer</summary>

```ts
type Nullable<T extends object> = {
    [P in keyof T]: T[P] | null
}
```

</details>
<hr/>
<br/>










### TypeScript: infer

<br/>

```ts
/*
 * 1. Что делает тип Foo?
 * 2. Что такое infer?
 */
type Foo<T> = T extends Array<infer U> ? U : never

```

<details>
  <summary>Answer</summary>

```ts
// Вытаскивает тип

Foo<Array<number>> // number
```

</details>
<hr/>
<br/>









### React: classes to FC

<br/>

```ts
/* Перепишите данный код на FC. */

export class Clock extends Component {
    counter = 0;
    state = {
        date: new Date()
    };

    componentDidMount() {
        setInterval(() => {
            this.counter++;
            this.setState({ date: new Date() });
        }, 1000);
    }

    componentWillUnmount() {
        sendCounterToBack(this.counter);
    }

    render() {
        return <div>{this.state.date.toString()}</div>;
    }
}
```

<details>
  <summary>Answer</summary>

```ts
function Clock(): FC {
    const counter = useRef(0)
    const [date, setDate] = useState(() => new Date)
    
    useEffect(() => {
        const timer = setInterval(() => {
            counter.current += 1;
            setDate(new Date());
        }, 1000);
        
        return () => {
            clearInterval(timer)
            sendCounterToBack(counter.current);
        }
    }, [])
    
    return <div>{date.toString()}</div>;
}
```

</details>
<hr/>
<br/>











### React: reconciliation

<br/>

```ts
/*
 * 1. Будут ли перерисовки при изменении isActive?
 * 2. Почему?
 */

const HighLoadComp = React.memo(() => {
    useEffect(() => {
        console.log('mount')
        return () => console.log('unmount')
    }, [])
    return null;
})

function Comp({ isActive }) {    
    if (isActive) {
        return <div>
            <h1>Header</h1>
            <HighLoadComp/>
        </div>;
    }

    return <div><HighLoadComp /></div>
}
```

<details>
  <summary>Answer</summary>

```ts
/* 
 * Список элементов в div у Comp меняется.
 * На первом месте оказывается то h1, то HighLoadComp.
 * Перерисовки будут.
 * Исправляет ситуацию вставка ключа в HighLoadComp для React
 */

function Comp({ isActive }) {
    const comp = <HighLoadComp key="1" />
    
    if (isActive) {
        return <div>
            <h1>Header</h1>
            {comp}
        </div>;
    }

    return <div>{comp}</div>
}
```

</details>
