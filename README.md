# Базові типи

- Базові та складні типи;
- Типи для змінних та аргументів;
- Типи для методів та функцій;
- Custom Types;
- Advanced Types;
- Generics.

## [Прості (скалярні) типи](./Prosti_typy.md)


## Складні типи

### Object

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

```ts
let user: { name: string; age: number } = {
  age: 30, // відсутня властивість `name`
};

let userNameLikeNumber: { name: string; age: number } = {
  name: 10, // неправильний тип даних
  age: 30,
};

export {};
```

Не зручно дублювати так типи та описувати їх перед присвоєнням - можемо винести тип окремо за допомогою ключового слова **type**:

```ts
type User = {
  name: string;
  age: number;
};

let user: User = {
  name: 'Tom',
  age: 30,
};

let userJack: User = {
  name: 'Jack',
  age: 25,
};
```

**User** — це наш власний тип, який ми створили для представлення користувача. Використали його для двох змінних: **user** та **userJack**.

Крім того, ми можемо використати **interface** для визначення об'єкта:

```ts
interface User {
  name: string;
  age: number;
}

let user: User = {
  name: 'Tom',
  age: 30,
};
```

Загалом, типи та інтерфейси дозволяють покращити структуру та повторне використання коду, та допомагають уникнути помилок за рахунок суворої типізації.

В цьому разі немає суттєвої різниці між **type** та **interface**.

### Array

Масиви в **TypeScript** — це структури, які є впорядкованим набором елементів. 
Для оголошення масиву використовується конструкція з квадратними дужками **[]** або загальний тип **Array**.

```ts
// Вказуємо масив рядків:
let arrString: string[];

// Cтворимо масив чисел:
let arrNumber: number[];
```

Масиви можуть бути багатовимірними:

```ts
let matrix: number[][] = [[1, 2], [3, 4]];
```

Масиви також можуть містити елементи різних типів:

```ts
let mixed: (number | string)[] = [1, 'two'];
```

Можемо вказати тип масиву через узагальнення (**generic**):

```ts
let numbers: Array<number> = [1, 2, 3, 4, 5];
```

Можна визначити масив об'єктів:

```ts
let users: {
name: string;
age: number;
}[] = [
{ name: 'Tom', age: 30 },
{ name: 'Jack', age: 25 },
{ name: 'Alice', age: 32 },
];
```

Або з використанням більш зручного запису:

```ts
type User = {
name: string;
age: number;
};

let users: User[] = [
{ name: 'Tom', age: 30 },
{ name: 'Jack', age: 25 },
{ name: 'Alice', age: 32 },
];
```

Тепер якщо якесь значення об'єкта буде не того типу, ми отримаємо помилку.

Це демонструє всю силу суворої типізації. Але іноді нам це не потрібно, і тоді ми можемо скористатися типом даних **any**:

```ts
let arrAny: any[];
```

У такому масиві можна зберігати будь-що.

---

# Типи для змінних та аргументів

## Any

**Any** — тип даних, який використовується, коли не знаєте, який тип даних може міститися у змінній. 
Змінні з типом **any** дозволяють викликати будь-які властивості та методи без перевірок типів. 
Цей тип даних робить змінну аналогічною змінною в **JavaScript**, що дозволяє передавати в неї будь-які значення. 

```ts
let notSure: any = 4;
notSure = 'maybe a string instead';
notSure = false;
notSure = {};

let num: number;

num = notSure;
```

Основною проблемою використання типу **any** в **TypeScript** є відсутність суворої типізації. Зберігай його у змінну, де вказано тип.

```ts
let num: number;

num = notSure;
```

У цьому випадку **TypeScript** не викличе помилку на етапі компіляції, адже **any** потенційно може являти собою будь-який тип даних. Навіть якщо з коду зрозуміло, що `notSure` — це об'єкт, а не число.


**В яких випадках він корисний?**

Наприклад, коли ми працюємо з бібліотекою на **JavaScript**, що може повертати різні типи даних, або в ситуаціях, коли конкретний тип даних не має значення в контексті нашого коду.

```ts
let data: any = fetchData();
```

Інший приклад, коли функція приймає аргумент, тип якого нам не відомий, і який загалом не важливий у цьому контексті. Це може бути, наприклад, певна колбек-функція.

```ts
function fetchUserData(id: string, callback: (data: any) => void): void {
  // Тут може бути якийсь запит, але ми його заповнимо самі
  const responseData = { name: 'Tom' };

  callback(responseData);
}

// Використання функції:
fetchUserData('123', (data) => {
  console.log(data.name); // TypeScript не викличе помилку, навіть якщо поле name не існує
});
```

Це дозволяє працювати з даними, не знаючи їхньої точної структури. Однак це також означає, що **TypeScript** не зможе надати нам підказки про те, які властивості та методи доступні для об'єкта **data**.

## Unknown

Тип **unknown** схожий на **any**, але забезпечує більше безпеки під час роботи зі змінними. 
Присвоїти значення змінної типу **unknown** іншій змінній з конкретним типом можна після явного приведення типів. Це запобігає випадковому присвоєнню значень неправильного типу.

```ts
let unknownVariable: unknown = "Hello, TypeScript!";

// Явне приведення типу змінної типу 'unknown' до типу string
let stringVariable: string;

if (typeof unknownVariable === 'string') {
    stringVariable = unknownVariable; // Явне приведення типу
    console.log(stringVariable.toUpperCase()); // Використання змінної типу string
} else {
    console.log("Змінна не є типу string.");
}
```

Тип **unknown** підходить для сценаріїв, коли не знаєте точного типу даних, але хочете підтримувати сувору перевірку типів. Змінні цього типу слід перевіряти перед їх використанням.

Візьмемо для прикладу таку ситуацію: Ви отримуєте дані з API та не знаєте їхнього точного формату. У цьому випадку вам потрібно буде провести уточнення типів.

```ts
function fetchUserData() {
  return 'Tom';
}

let userData: unknown = fetchUserData(); // fetchUserData повертає невідомі дані
if (typeof userData === 'string') {
  console.log(userData.toUpperCase()); // OK, тепер ми знаємо, що це рядок
}
```

Отже, ми можемо бути впевнені, що обробляємо дані правильного типу.

## Tuple

**Кортеж**, особливо популярний в Python, - це незмінний масив. 
У **TypeScript** це тип даних, що дозволяє визначити масив з фіксованою кількістю елементів, типи яких відомі, але не обов'язково повинні бути однаковими. 

Він створюється як масив, але замість значень ми передаємо типи даних - наприклад **[string, number]**.

```ts
let tupleType: [string, boolean];
tupleType = ['hello', true]; // OK
tupleType = [true, 'hello']; // Error. Неправильні типи
tupleType = ['hello', true, true]; // Error. Більше значень ніж у tuple
```

**Кортежі** зручні, коли нам потрібно зберегти в масив фіксовані значення, наприклад, день, місяць та рік.

```ts
let date: [number, number, number];
date = [7, 11, 2023]; // OK
```

Але є нюанс: якщо ми додамо елемент у **кортеж** через метод **push**, то **TypeScript** не заперечуватиме, він не відстежує реальний вміст масиву.

```ts
let fixed: [string, number];

fixed = ['Text', 10];

fixed.push('Add this text');
```

Тут компілятор не зміг розібратися і видати помилку.

Використання **оператора розширення (...)** для створення кортежів змінної довжини:


```ts
let tuple: [string, ...number[]];

tuple = ['hello', 42, 100, 200]; // OK
```

У цьому випадку перший елемент **кортежу** має бути рядком, проте всі наступні — числами.

## Enum

Ця структура настільки широко використовується, що в **TypeScript** додали її як тип даних. Згідно з хорошими практиками програмування, імена змінних цього типу мають починатися з великої літери:

```ts
enum Role {ADMIN, USER};
```

Розглянемо застосування на прикладі користувача, який має певні права.

```ts
enum Role {
  ADMIN,
  USER,
}

const person = {
  role: Role.ADMIN,
};

if (person.role === Role.ADMIN) {
  console.log('Role: ', Role.ADMIN);
}
```

Подивимося на скомпільований код **JavaScript**:

```ts
var Role;
(function (Role) {
 Role[(Role['ADMIN'] = 0)] = 'ADMIN';
 Role[(Role['USER'] = 1)] = 'USER';
})(Role || (Role = {}));
var person = {
 role: Role.ADMIN,
};
if (person.role === Role.ADMIN) {
 console.log('Role: ', Role.ADMIN);
}

// У консолі буде:
Role:  0
```

Ми також можемо отримати значення **enum**, хоча це рідко використовується.

```ts
enum Role {
 ADMIN,
 USER,
}

console.log(Role.ADMIN); // 0
console.log(Role[Role.ADMIN]); // "ADMIN"
```

**Enum** являє собою набір констант, що робить код більш зрозумілим. 
Як ми бачили у минулому прикладі, значеннями **enum** зазвичай є числа, проте можемо задати свої значення:

```ts
enum UserStatus {
 Active = 'ACTIVE',
 Inactive = 'INACTIVE',
 Banned = 'BANNED',
}
let status: UserStatus = UserStatus.Active;
```

Можна використовувати **enum** для угруповання взаємопов'язаних значень, - корисно для спрощення коду та  читабельності:

```ts
enum HttpCodes {
  OK = 200,
  BadRequest = 400,
  Unauthorized = 401,
}

function respond(status: HttpCodes) {
  // handle response
}

respond(HttpCodes.OK);
```

Існує ще така конструкція, як **const enum**. На відміну від звичайного **`enum`**, **const enum** видаляється під час транспіляції та не створює додаткового об'єкта в JavaScript. 

Значення **const enum** вставляють у місце використання у вигляді літералів. Це може допомогти покращити продуктивність.

```ts
const enum HttpCodes {
  OK = 200,
  BadRequest = 400,
  Unauthorized = 401,
}

const status = HttpCodes.OK;


// Після компіляції у `JavaScript` отримаємо наступний код:

const status = 200;
```

Як видно з прикладу, **const enum** видаляється зі скомпільованого коду і його значення прямо вставляється в код. У цьому випадку **HttpCodes.OK** замінився на **200**. Це і є ключовою відмінністю **const enum** від звичайного **enum**.

Однак існує одне обмеження використання **const enum**: їх не можна використовувати у виразах, які вимагають виконання під час виконання. Це пов'язане з тим, що вони замінюються на їхнє значення під час компіляції.

```ts
// Подібні операції можливі лише зі звичайними enum:
const enum Test {
  A = 1,
  B = 2,
}

for (let item in Test) {
  console.log(item);
}
```

## Union Type

дозволяє вказати, що значенням може бути один із кількох типів. 
Це дуже зручно, коли хочемо визначити змінну, яка може приймати різні типи даних. 
Типи перераховуються через **вертикальну риску |**

```ts
let mixedType: string | number | boolean;

mixedType = 'string'; // OK
mixedType = 10; // OK
mixedType = true; // OK
mixedType = {}; // Error: Type '{}' is not assignable to type 'string | number | boolean'.
```

**Union Type** можна також використовувати для аргументів функцій. Давайте створимо функцію, яка об'єднує рядки або складає числа.

```ts
function combine(param1: number | string, param2: number | string) {
  return param1 + param2; // отримуємо помилку, тому що TS не знає, рядок там чи число. 
}
```

Давайте перевіримо типи у функції.

```ts
function combine(param1: number | string, param2: number | string) {
  if (typeof param1 === 'number' && typeof param2 === 'number') {
    return param1 + param2;
  } else {
    return param1.toString() + param2.toString();
  }
}
```

Тепер ми можемо передавати у функцію або числа, або рядки.

**Union Type** працює не лише з базовими типами, а й з об'єктами:

```ts
type Dog = { 
  legs: 4;
  bark: () => void;
}

type Fish = {
  fins: 2;
  swim: () => void;
}

let pet: Dog | Fish; // змінна *pet* може бути або об'єктом типу *Dog*, або об'єктом типу *Fish*
```


Існує важливе обмеження: коли ми працюємо зі змінною **Union Type**, ми можемо використовувати лише ті властивості та методи, які існують у всіх типів цього об'єднання. У прикладі вище ми не можемо викликати **pet.bark()**, якщо pet є типом **Fish**. Нам доведеться перевіряти, чи існує цей метод.

```ts
type Dog = {
  legs: 4;
  bark: () => void;
};

type Fish = {
  fins: 2;
  swim: () => void;
};

let pet: Dog | Fish;

// type guard function
function isDog(pet: Dog | Fish): pet is Dog {
  return 'bark' in pet;
}

// Перевіряємо, чи є наш вихованець собакою перед тим, як використовувати метод bark
if (isDog(pet)) {
  pet.bark(); // OK, тепер TypeScript знає, що pet - це Dog
} else {
  pet.swim(); // TypeScript знає, що якщо pet не Dog, то це має бути Fish
}
```

Дуже часто краще використовувати **Union Type** замість **enum** для перерахування всіх допустимих значень. Це особливо зручно робити у зв'язці з **Literal Type**.

## Intersection Type

є способом об'єднання декількох типів в один. Це дозволяє створювати складні типи, комбінуючи прості. У **TypeScript** можна використовувати **символ &** для створення типу **intersection**.

```ts
type Employee = {
  name: string;
  id: number;
};

type Manager = {
  employees: Employee[];
};

type CEO = Employee & Manager;

const ceo: CEO = {
  name: 'Alice',
  id: 1,
  employees: [
    {
      name: 'Bob',
      id: 2,
    },
  ],
};
```

У цьому прикладі `CEO` є **intersection** типів `Employee` і `Manager`. Це означає, що об'єкт типу CEO повинен містити всі властивості, визначені в `Employee` та `Manager`.

## Literal Type

це тип, що набуває конкретного значення. З ним ви можете визначити тип змінної так, щоб він набував лише певних значень.

```ts
// Тут OneOrTwo може набувати лише значення 1 або 2:
type OneOrTwo = 1 | 2;
let value: OneOrTwo;
value = 1; // OK
value = 2; // OK
value = 3; // Error: Type '3' is not assignable to type 'OneOrTwo'.

// YesOrNo може набувати тільки значення "yes" або "no":
type YesOrNo = 'yes' | 'no';
let answer: YesOrNo;
answer = 'yes'; // OK
answer = 'no'; // OK
answer = 'maybe'; // Error: Type '"maybe"' is not assignable to type 'YesOrNo'.
```

Розглянемо бойовий приклад. Припустимо, ми маємо функцію, що приймає рядок як аргумент і повертає стилі для кнопки в залежності від переданого значення.

```ts
type ButtonSize = 'small' | 'medium' | 'large';

function getButtonStyle(size: ButtonSize) {
  switch (size) {
    case 'small':
      return { fontSize: '10px', padding: '5px' };
    case 'medium':
      return { fontSize: '14px', padding: '10px' };
    case 'large':
      return { fontSize: '18px', padding: '15px' };
    default:
      return { fontSize: '14px', padding: '10px' };
  }
}

let myButtonStyle = getButtonStyle('medium'); // OK
myButtonStyle = getButtonStyle('extra-large'); // Error: Argument of type '"extra-large"' is not assignable to parameter of type 'ButtonSize'.
```


Тут, якщо ми спробуємо викликати функцію **getButtonStyle** з аргументом `"extra-large"`, **TypeScript** видасть помилку на етапі компіляції, оскільки "extra-large" не є допустимим значенням для **ButtonSize**.

















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
