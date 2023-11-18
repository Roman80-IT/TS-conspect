# Базові типи

- Базові та складні типи;
- Типи для змінних та аргументів;
- Типи для методів та функцій;
- Custom Types;
- Advanced Types;
- Generics.

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

### Складні типи

#### Object

У **JavaScript**, а отже, і у **TypeScript** тип `Object` використовується для зберігання колекції даних або більш складних структур. 
Він є структурою даних, яка може містити дані різних типів.

Тип даних `object`:

```ts
const obj: object = {};
```

Це по факту порожній об'єкт. Аналогічно можна написати так:

```ts
const obj: {} = {};
```

Як і зі скалярними типами даних, ми можемо не уточнювати, що це `Object`:

```ts
let user = {
name: "Tom",
age: 30
};
```

Однак, використання типу `object` не дає нам особливого контролю над формою цього об'єкта.

Ми можемо використовувати більш точну анотацію, з допомогою типу об'єкта:

```ts
let user: { name: string; age: number } = {
name: "Tom",
age: 30
};
```

Вказали, що **user** - це об'єкт, який має властивість **name** типу `string` і властивість **age** типу `number`.

Тепер, якщо не вкажемо якусь властивість, - то отримаємо помилку.

https://stackblitz.com/edit/typescript-p76oyj?file=complexType%2F1_object.1.ts

https://typescript-p76oyj.stackblitz.io

complexType/1_object.1.ts

https://stackblitz.com/edit/typescript-p76oyj?embed=1&file=complexType%2F1_object.1.ts

import sdk from '@stackblitz/sdk'

sdk.embedProjectId(
  'elementOrId',
  'typescript-p76oyj',
  {
    forceEmbedLayout: true,
    openFile: 'complexType/1_object.1.ts',
  }
);


У першому випадку ми отримали помилку через те, що відсутня властивість name, а в другому, що там використовується неправильний тип даних.

Але погодьтеся, що не дуже зручно дублювати так типи та описувати їх перед присвоєнням. Ми можемо винести тип окремо за допомогою ключового слова type:

Тут User — це наш власний тип, який ми створили для представлення користувача. Ми можемо використовувати цей тип скрізь, і ми використали його для двох змінних: user та userJack.

Крім того, ми можемо використати interface для визначення об'єкта:

Загалом, типи та інтерфейси дозволяють покращити структуру та повторне використання коду, а також допомагають уникнути помилок за рахунок суворої типізації.

В цьому разі немає суттєвої різниці між type та interface, у майбутніх блоках ми розберемо їх докладніше.

Array

Масиви в TypeScript — це структури, які є впорядкованим набором елементів. Для оголошення масиву в TypeScript використовується конструкція з квадратними дужками [] або загальний тип Array.

Якщо ми хочемо вказати масив рядків, ми робимо це так:

let arrString: string[];

Якщо ми спробуємо передати в нього не рядковий тип даних, ми отримаємо помилку.

Як бачимо, число підсвітилося як помилка.

Давайте створимо масив чисел:

let arrNumber: number[];

Тепер він може містити лише числа, і будь-який інший тип даних буде викликати помилку.

Крім того, масиви в TypeScript можуть бути багатовимірними. Наприклад:

let matrix: number[][] = [[1, 2], [3, 4]];

Масиви також можуть містити елементи різних типів. Наприклад:

let mixed: (number | string)[] = [1, 'two'];

Ми також можемо вказати тип масиву через узагальнення (generic):

let numbers: Array<number> = [1, 2, 3, 4, 5];

Можна визначити масив об'єктів у TypeScript. Наприклад:

let users: {
name: string;
age: number;
}[] = [
{ name: 'Tom', age: 30 },
{ name: 'Jack', age: 25 },
{ name: 'Alice', age: 32 },
];

Або з використанням більш зручного запису:

type User = {
name: string;
age: number;
};

let users: User[] = [
{ name: 'Tom', age: 30 },
{ name: 'Jack', age: 25 },
{ name: 'Alice', age: 32 },
];

Тепер якщо якесь значення об'єкта буде не того типу, ми отримаємо помилку.

Це демонструє всю силу суворої типізації. Але іноді нам це не потрібно, і тоді ми можемо скористатися типом даних any:

let arrAny: any[];

У такому масиві можна зберігати будь-що.

Але не варто зловживати any, інакше TypeScript швидко перетвориться на JavaScript :)

# Базові типи

Метою цього домашнього завдання є закріплення ваших навичок роботи з базовими типами TypeScript. Ви будете працювати з типами, такими як number, string, boolean, null, undefined, unknown, any, а також кортежами, переліками (enum) та об'єднаннями типів.

В кінці домашнього завдання ви також попрактикуєтеся у створенні свого типу на основі наявних об'єктів. Це допоможе вам краще зрозуміти, як TypeScript може бути використаний для забезпечення типової безпеки ваших даних та підвищення якості вашого коду.

### Завдання 1

Є наступний JavaScript код:

```ts
let age = 50;
let name = "Max";
let toggle = true;
let empty = null;
let notInitialize;
let callback = (a) => {
  return 100 + a;
};
```

Перетворіть цей код на TypeScript, вказавши відповідні типи для всіх змінних.

### Завдання 2

JavaScript змінна може зберігати значення будь-якого типу:

```ts
let anything = -20;
anything = "Text";
anything = {};
```

Який тип ви надаєте змінній anything в TypeScript, щоб зберегти її гнучкість?

### Завдання 3

У TypeScript тип unknown дозволяє нам зберігати будь-які значення, але ми можемо привласнити unknown змінну безпосередньо інший змінної, якщо ми впевнені у її типі. У вас є наступний код:

```ts
let some: unknown;
some = "Text";
let str: string;
str = some;
```

Що потрібно виправити в цьому коді, щоб він став правильним та безпечним?

### Завдання 4

У вас є наступний JavaScript масив:

```ts
let person = ["Max", 21];
```

Як переписати його в TypeScript, використовуючи концепцію кортежів, щоб гарантувати, що перший елемент завжди буде рядком, а другий числом?

### Завдання 5

Як ви визначите змінну в TypeScript, яка може приймати рядок або число (union type)? І так само визначте змінну, яка може приймати тільки одне з двох рядкових значень: 'enable' або 'disable' (literal type)?

### Завдання 6

У вас є такі функції JavaScript:

```ts
function showMessage(message) {
  console.log(message);
}

function calc(num1, num2) {
  return num1 + num2;
}

function customError() {
  throw new Error("Error");
}
```

Як ви вкажете типи для аргументів і значень цих функцій, що повертаються?

### Завдання 7

Створіть функцію (isWeekend), яка приймає день тижня (з вашого enum) і повертає boolean значення, що вказує, чи це день робочий чи вихідний.

### Завдання 8

Створіть тип "Gender", використовуючи union type, який може містити значення "male", "female". Створіть змінну myGender цього типу.

### Завдання 9

У вас є два об'єкти:

```ts
const page1 = {
  title: "The awesome page",
  likes: 100,
  accounts: ["Max", "Anton", "Nikita"],
  status: "open",
  details: {
    createAt: new Date("2021-01-01"),
    updateAt: new Date("2021-05-01"),
  },
};

const page2 = {
  title: "Python or Js",
  likes: 5,
  accounts: ["Alex"],
  status: "close",
};
```

Створіть новий тип даних, який підходить для цих двох об'єктів.

# Generic

Мета цього завдання - допомогти вам зрозуміти та застосувати generics у TypeScript. Ви працюватимете з функціями, що повертають проміси, використовувати вбудований тип Pick, об'єднувати об'єкти за допомогою generics, а також вирішувати проблеми типів у класах.

### Завдання 1

Є функція getPromise(), яка повертає проміс, що дозволяється в масив, що містить рядки та числа. Доповніть цю функцію, використовуючи generics, щоб вона повертала правильний тип.

```ts
function getPromise() {
  return new Promise((resolve) => {
    resolve(["Text", 50]);
  });
}

getPromise().then((data) => {
  console.log(data);
});
```

### Завдання 2

У вас є тип AllType. Існує функція compare, яка приймає два об'єкти. Ці об'єкти містять поля AllType. Ваше завдання – використовувати Pick та generics для вказівки, що поля цих об'єктів належать AllType. Функція compare повинна повертати AllType.

```ts
type AllType = {
  name: string;
  position: number;
  color: string;
  weight: number;
};

function compare(top, bottom): AllType {
  return {
    name: top.name,
    color: top.color,
    position: bottom.position,
    weight: bottom.weight,
  };
}
```

### Завдання 3

У вас є функція merge, яка поєднує два об'єкти. Використовуйте generics, щоб вказати, що ці об'єкти можуть бути будь-якого типу.

```ts
function merge(objA, objB) {
  return Object.assign(objA, objB);
}
```

### Завдання 4

Використовуйте generics та інтерфейси, щоб виправити помилку в наступних класах:

```ts
class Component {
  constructor(public props: T) {}
}

class Page extends Component {
  pageInfo() {
    console.log(this.props.title);
  }
}
```

### Завдання 5

Вам потрібно реалізувати інтерфейс KeyValuePair, який описує пару ключ-значення. Використовуйте generics, щоб цей інтерфейс міг працювати з будь-якими типами ключів та значень.

```ts
interface KeyValuePair {
  key;
  value;
}
```

### Завдання 6

Ви маєте форму реєстрації користувачів. Іноді потрібно попередньо заповнити форму даними користувача для оновлення його профілю. Однак вам не завжди потрібно заповнити всі поля. Наприклад, користувач може хотіти оновити лише свій email та пароль, залишивши ім'я та прізвище без змін.

Виправте тип у аргументі функції так, щоб не було помилок типу.

```ts
type User = {
  name: string;
  surname: string;
  email: string;
  password: string;
};

function createOrUpdateUser(initialValues: User) {
  // Оновлення користувача
}

createOrUpdateUser({ email: "user@mail.com", password: "password123" });
```

### Завдання 7

У вас є перелік UserRole, який використовується для класифікації користувачів у вашому додатку. Ви хочете створити об'єкт RoleDescription, який зіставлятиме кожну роль користувача з її описом.

```ts
export enum UserRole {
  admin = "admin",
  editor = "editor",
  guest = "guest",
}

// Замініть наступний код на версію за допомогою Record
const RoleDescription = {
  admin: "Admin User",
  editor: "Editor User",
  guest: "Guest User",
};
```

### Завдання 8

У вас є тип Form, який містить інформацію про форму, включаючи поле errors. Ви хочете створити новий тип Params, який включає всі поля з Form, крім errors.

```ts
type Errors = {
  email?: string[];
  firstName?: string[];
  lastName?: string[];
  phone?: string[];
};

type Form = {
  email: string | null;
  firstName: string | null;
  lastName: string | null;
  phone: string | null;
  errors: Errors;
};

// Реалізуйте Params так, щоб унеможливити поле 'errors' з типу Form
type Params = Form;
``;
```
