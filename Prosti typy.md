## Прості (скалярні) типи

Базові типи - розбиремо ті типи, які є в `JavaScript`, і почнемо зі скалярних типів (прості типи, що містять одне значення).

**boolean**: логічний тип даних, який може приймати значення **true** або **false**:

```ts
let isDone: boolean = false;
```

**number**: числовий тип даних для цілих та дійсних чисел.

```ts
let decimal: number = 6; // десяткові
let float: number = 3.14; // речові або число з плаваючою крапкою
let hex: number = 0xf00d; // шістнадцяткове
let binary: number = 0b1010; // двійкове
let octal: number = 0o744; // вісімкове
```

**string**: текстовий тип даних для символів та рядків

```ts
let color: string = "blue";
```

**null** та **undefined**: два спеціальні типи, що відповідають значенням `null` і `undefined` відповідно:

```ts
let empty: null = null;
let notParam: undefined = undefined;
```

Не обов'язково вказувати тип даних, якщо передати його явно:

```ts
const num = 10;
const str = 'Some string';
const bool = true;
const empty = null;
const notParam = undefined;
```

Передамо в аргумент функції тип даних:

```ts
function foo (num: number, str: string, bool: boolean, empty: null) {
// Some logic
}
```

Якщо ми задаємо значення за замовчуванням у функції, тип вказувати не потрібно:

```ts
function foo (num = 10, str = 'Some string', bool = true, empty = null){
// Some logic
}
```

---