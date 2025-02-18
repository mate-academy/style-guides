# JS Style Guide <!-- omit in toc -->

[go/js-style-guide](http://go/js-style-guide)

- [1. Objects](#1-objects)
    - [1.1. Prefer the object spread operator over `Object.assign` to shallow-copy objects. Use the object rest operator to get a new object with certain properties omitted.](#11-prefer-the-object-spread-operator-overobjectassignto-shallow-copy-objects-use-the-object-rest-operator-to-get-a-new-object-with-certain-properties-omitted)
    - [1.2. Check if property exists in object before accessing it. In Mate we prefer "noUncheckedIndexedAccess" option to be enabled in tsconfig.json for preventing runtime errors.](#12-check-if-property-exists-in-object-before-accessing-it-in-mate-we-prefer-nouncheckedindexedaccess-option-to-be-enabled-in-tsconfigjson-for-preventing-runtime-errors)
- [2. Destructuring](#2-destructuring)
    - [2.1. Use object destructuring for multiple return values, not array destructuring.](#21use-object-destructuring-for-multiple-return-values-not-array-destructuring)
    - [2.2. Check if property exists in object after destructuring it.](#22check-if-property-exists-in-object-after-destructuring-it)
    - [2.3. Check if property exists in array after destructuring it.](#23check-if-property-exists-in-array-after-destructuring-it)
- [3. Functions](#3-functions)
    - [3.1. Avoid side effects with default parameters.](#31-avoid-side-effects-with-default-parameters)
    - [3.2. Always put default parameters last.](#32-always-put-default-parameters-last)
    - [3.3. If parameters number is greater than 2, put them into `options` parameter](#33-if-parameters-number-is-greater-than-2-put-them-into-options-parameter)
    - [3.4 Do not use optional fields that change method behavior](#34-do-not-use-optional-fields-that-change-method-behavior)
- [4. Arrow Functions](#4-arrow-functions)
    - [4.1. In case the expression spans over multiple lines, wrap it in parentheses for better readability.](#41-in-case-the-expression-spans-over-multiple-lines-wrap-it-in-parentheses-for-better-readability)
- [5. Modules](#5-modules)
    - [5.1. Prefer named export over default export.](#51-prefer-named-export-over-default-export)
- [6. Variables](#6-variables)
    - [6.1. Avoid using unary increments and decrements (`++`, `--`).](#61avoid-using-unary-increments-and-decrements---)
    - [6.2. Avoid linebreaks before or after `=` in an assignment. If your assignment violates `max-len`, surround the value in parens.](#62-avoid-linebreaks-before-or-afterin-an-assignment-if-your-assignment-violatesmax-len-surround-the-value-in-parens)
- [7. Comparison Operators \& Equality](#7-comparison-operators--equality)
    - [7.1. Use shortcuts for booleans, but explicit comparisons for strings and numbers.](#71use-shortcuts-for-booleans-but-explicit-comparisons-for-strings-and-numbers)
- [8. Comments](#8-comments)
    - [8.1 Use `@deprecated` JSDOC tag in comments to mark deprecated parts of code](#81usedeprecated-jsdoc-tag-in-comments-to-mark-deprecated-parts-of-code)
    - [8.2. `FIXME` and `TODO` comments should have a link to the task](#82fixme-and-todo-comments-should-have-a-link-to-the-task)
    - [8.3. Use `// FIXME:` to annotate problems.](#83use-fixmeto-annotate-problems)
    - [8.4. Use `// TODO:` to annotate solutions to problems.](#84use-todoto-annotate-solutions-to-problems)
    - [8.5. Fields in Sequelize models and business logic methods should have a `@description` annnotation with JSDoc comment](#85-fields-in-sequelize-models-and-business-logic-methods-should-have-a-description-annnotation-with-jsdoc-comment)
- [9. Type Casting \& Coercion](#9-type-casting--coercion)
    - [9.1 Strings:](#91strings)
    - [9.2. Numbers: Use `Number` for type casting and `parseInt` always with a radix for parsing strings.](#92-numbers-usenumberfor-type-casting-andparseintalways-with-a-radix-for-parsing-strings)
    - [9.3 Booleans:](#93booleans)
- [10. Naming Conventions](#10-naming-conventions)
    - [10.1 Avoid single letter names. Be descriptive with your naming.](#101avoid-single-letter-names-be-descriptive-with-your-naming)
    - [10.2. Use camelCase when naming objects, functions, and instances. Function names are typically verbs or verb phrases.](#102-use-camelcase-when-naming-objects-functions-and-instances-function-names-are-typically-verbs-or-verb-phrases)
    - [10.3. Use PascalCase only when naming constructors or classes.](#103-use-pascalcase-only-when-naming-constructors-or-classes)
    - [10.4 Do not use trailing or leading underscores.](#104do-not-use-trailing-or-leading-underscores)
    - [10.5 A base filename should exactly match the name of its primary export.](#105a-base-filename-should-exactly-match-the-name-of-its-primary-export)
    - [10.6 Acronyms and initialisms should always be all capitalized, or all lowercased.](#106acronyms-and-initialisms-should-always-be-all-capitalized-or-all-lowercased)
    - [10.7 You may optionally uppercase a constant only if it (1) is exported, (2) is a `const` (it can not be reassigned), and (3) the programmer can trust it (and its nested properties) to never change.](#107you-may-optionally-uppercase-a-constant-only-if-it-1-is-exported-2-is-aconstit-can-not-be-reassigned-and-3-the-programmer-can-trust-it-and-its-nested-properties-to-never-change)
    - [10.8 Don't shorten variables or functions names](#108-dont-shorten-variables-or-functions-names)
    - [10.9 If the variable/property/method is a `boolean`, use `isVal()` or `hasVal()`.](#109if-the-variablepropertymethod-is-aboolean-useisvalorhasval)
- [11. Standard Library](#11-standard-library)
    - [11.1. Use `Number.isNaN` instead of global `isNaN`.](#111usenumberisnaninstead-of-globalisnan)
    - [11.2. Use `Number.isFinite` instead of global `isFinite`.](#112usenumberisfiniteinstead-of-globalisfinite)
- [12. Testing](#12-testing)
    - [12.1. **Yup.**](#121yup)
    - [12.2. **No, but seriously**:](#122no-but-seriously)



1\. Objects
-----------


#### 1.1. Prefer the object spread operator over [`Object.assign`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Object/assign) to shallow-copy objects. Use the object rest operator to get a new object with certain properties omitted.



```javascript
// very bad
const original = {
  a: 1,
  b: 2
};

const copy = Object.assign(original, { c: 3 }); // this mutates `original` ಠ_ಠ
delete copy.a; // so does this

// ❌ bad
const original = {
  a: 1,
  b: 2
};
const copy = Object.assign({}, original, { c: 3 });
// copy => { a: 1, b: 2, c: 3 }

// ✅ good
const original = { a: 1, b: 2 };
const copy = { ...original, c: 3 };
// copy => { a: 1, b: 2, c: 3 }

const { a, ...noA } = copy;
// noA => { b: 2, c: 3 }
```

#### 1.2. Check if property exists in object before accessing it. In Mate we prefer ["noUncheckedIndexedAccess" option](https://www.typescriptlang.org/tsconfig#noUncheckedIndexedAccess) to be enabled in tsconfig.json for preventing runtime errors.

```js
// ❌ bad
const user: User = {
  name: 'John',
  age: '25',
};

// some code that can change user object

return user.city.length; // runtime error

// ✅ good
const user = {
  name: 'John',
  age: '25',
};

// some code that can change user object

if ('city' in user) {
  // city is defined
  return user.city.length;
}
```

For TypeScript projects (API, Frontend) we recommend to use [optional chaining](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-3-7.html#optional-chaining) to prevent runtime errors. Also, it's a good practice to use [nullish coalescing](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-3-7.html#nullish-coalescing) to provide default values for undefined or null properties.

```ts
// ❌ bad
type User = {
  name: string;
  age: string;
  city?: string;
};

const user: User = {
  name: 'John',
  age: '25',
};

// some code that can change user object

return user.city.length; // runtime error

// ✅ good
type User = {
  name: string;
  age: string;
  city?: string;
};

const user: User = {
  name: 'John',
  age: '25',
};

// some code that can change user object

return user.city?.length ?? 0; // no runtime error
```

To prevent runtime errors with accessing non-existing properties in objects, we recommend to enable ["noUncheckedIndexedAccess" option](https://www.typescriptlang.org/tsconfig#noUncheckedIndexedAccess) in tsconfig.json. This option will force TypeScript to check if property exists in object before accessing it.

```json
{
  "compilerOptions": {
    "noUncheckedIndexedAccess": true
  }
}
```

2\. Destructuring
-----------------

#### 2.1. Use object destructuring for multiple return values, not array destructuring.


>❓Why? You can add new properties over time or change the order of things without breaking call sites.

```javascript
// ❌ bad
function processInput(input) {
  // then a miracle occurs
  return [left, right, top, bottom];
}

// the caller needs to think about the order of return data
const [left, __, top] = processInput(input);

// ✅ good
function processInput(input) {
  // then a miracle occurs
  return { left, right, top, bottom };
}

// the caller selects only the data they need
const { left, top } = processInput(input);
```

#### 2.2. Check if property exists in object after destructuring it.

When using TypeScript, you may face a situation when you need to destructure an object with optional properties. In this case, you need to check if the property exists before using it in the code below to prevent runtime errors.

```ts
// ❌ bad
type User = {
  name: string;
  age: string;
  city?: string;
};

const user: User = {
  name: 'John',
  age: '25',
};

// some code that can change user object

const { city } = user;

return city.length; // runtime error

// ✅ good
type User = {
  name: string;
  age: string;
  city?: string;
};

const user: User = {
  name: 'John',
  age: '25',
};

// some code that can change user object

const { city } = user;

if (city) {
  return city.length;
}

// --------------------------------
// or with setting default value

const { city = 'Fallback city' } = user;

return city.length;
```

#### 2.3. Check if property exists in array after destructuring it.

When using TypeScript, you may face a situation when you need to destructure an array with unknown length. In this case, you need to check if the property exists before using it in the code below to prevent runtime errors.

```ts
// ❌ bad
type Cities = string[];

const cities: Cities = ['Kyiv', 'London', 'New York'];

cities.push('Paris');

// some other code that can change cities array

const oslo = cities[4];
// undefined, but no runtime error as type Cities is string[]

return oslo.length; // runtime error

// ✅ good
type Cities = string[];

const cities: Cities = ['Kyiv', 'London', 'New York'];

cities.push('Paris');

// some other code that can change cities array

const oslo = cities[4];

if (oslo) {
  return oslo.length;
}
// --------------------------------
// or with setting default value
const oslo = cities[4] ?? 'Fallback city';

return oslo.length;
```

In cases, when you know the exact length of the array, you can use array tuple types to prevent runtime errors.

```ts
// ✅ good
type Cities = [string, string, string];
//or alternative way
const CITIES = ['Kyiv', 'London', 'New York'] as const;
```

3\. Functions
-------------

#### 3.1. Avoid side effects with default parameters.


>❓Why? They are confusing to reason about.


```javascript
var b = 1;

// ❌ bad
function count(a = b++) {
  console.log(a);
}

count();  // 1
count();  // 2
count(3); // 3
count();  // 3
```



#### 3.2. Always put default parameters last.

```javascript
// ❌ bad
function handleThings(opts = {}, name) {
  // ...
}

// ✅ good
function handleThings(name, opts = {}) {
  // ...
}
```



#### 3.3. If parameters number is greater than 2, put them into `options` parameter



>❓Why? It improves readability and helps to avoid mess with parameters order



```javascript
// ❌ bad
function createUser(
  firstName,
  middleName,
  lastName,
  userName,
  nickName,
  city
) {
  // ...
}

createUser('Bob', null, 'Doe', 'bob_doe', 'bobbie', 'New York'); // need to check parameters list everytime


// ✅ good
function createUser({
  firstName,
  middleName,
  lastName,
  userName,
  nickName,
  city
} = {}) {
  // ...
}

createUser({
  firstName: 'Bob',
  middleName: null,
  lastName: 'Doe',
  userName: 'bob_doe',
  nickName: 'bobbie',
  city: 'New York'
});
```

#### 3.4 Do not use optional fields that change method behavior

💡 Note: it is related only to fields that impact behavior inside methods. In other words - if there is `if` in the code, related to the field, the field should be required

>❓Why? While it is generally recommended to avoid params that change method behavior, sometimes it's not possible to avoid. For such cases, having required fields helps to address all places where the method is used and update them accordingly. If the field is optional, it's easy to forget about it, leading to bugs

```typescript
// ❌ bad
const getNextClosestWeekday = (options: {
  // optional variable
  keepReferenceTime?: boolean
}): Date {
  let closestWeekdayDate = // logic here

  // changed behavior. easy to forget about if keepReferenceTime is not passed
  if (!keepReferenceTime) {
    closestWeekdayDate = closestWeekdayDate.set({
      hour: 0,
      minute: 0,
      second: 0,
      millisecond: 0,
    });
  }
}

// ✅ good

const getNextClosestWeekday = (options: {
  // requierd variable
  keepReferenceTime: boolean
}): Date {
  let closestWeekdayDate = // logic here

  // changed behavior. As variable is always passed, it's obvious what's changed inside
  if (!keepReferenceTime) {
    closestWeekdayDate = closestWeekdayDate.set({
      hour: 0,
      minute: 0,
      second: 0,
      millisecond: 0,
    });
  }
}

```

4\. Arrow Functions
-------------------


#### 4.1. In case the expression spans over multiple lines, wrap it in parentheses for better readability.


>❓Why? It shows clearly where the function starts and ends.


```javascript
// ❌ bad
['get', 'post', 'put'].map(httpMethod => Object.prototype.hasOwnProperty.call(
    httpMagicObjectWithAVeryLongName,
    httpMethod,
  )
);

// ✅ good
['get', 'post', 'put'].map(httpMethod => (
  Object.prototype.hasOwnProperty.call(
    httpMagicObjectWithAVeryLongName,
    httpMethod,
  )
));
```

5\. Modules
------------

#### 5.1. Prefer named export over default export.



>❓Why? Easier to maintain and refactor. Same module name is enforced across all project imports



```javascript
// ❌ bad
export default function foo() {}

import foo from './foo';
import myFoo from './foo';

// ✅ good
export function foo() {}

import { foo } from './foo' // import name is stable
```


6\. Variables
------------


#### 6.1. Avoid using unary increments and decrements (`++`, `--`).

eslint [`no-plusplus`](https://eslint.org/docs/rules/no-plusplus)


>❓Why? Per the eslint documentation, unary increment and decrement statements are subject to automatic semicolon insertion and can cause silent errors with incrementing or decrementing values within an application. It is also more expressive to mutate your values with statements like `num += 1` instead of `num++` or `num ++`. Disallowing unary increment and decrement statements also prevents you from pre-incrementing/pre-decrementing values unintentionally which can also cause unexpected behavior in your programs.



```javascript
// ❌ bad

const array = [1, 2, 3];
let num = 1;
num++;
--num;

let sum = 0;
let truthyCount = 0;
for (let i = 0; i < array.length; i++) {
  let value = array[i];
  sum += value;
  if (value) {
    truthyCount++;
  }
}

// ✅ good

const array = [1, 2, 3];
let num = 1;
num += 1;
num -= 1;

const sum = array.reduce((a, b) => a + b, 0);
const truthyCount = array.filter(Boolean).length;
```


#### 6.2. Avoid linebreaks before or after `=` in an assignment. If your assignment violates [`max-len`](https://eslint.org/docs/rules/max-len.html), surround the value in parens.

eslint [`operator-linebreak`](https://eslint.org/docs/rules/operator-linebreak.html)



>❓Why? Linebreaks surrounding `=` can obfuscate the value of an assignment.



```javascript
// ❌ bad
const foo =
  superLongLongLongLongLongLongLongLongFunctionName();

// ❌ bad
const foo
  = 'superLongLongLongLongLongLongLongLongString';

// ✅ good
const foo = (
  superLongLongLongLongLongLongLongLongFunctionName()
);

// ✅ good
const foo = 'superLongLongLongLongLongLongLongLongString';


```


7\. Comparison Operators & Equality
------------------------------------


#### 7.1. Use shortcuts for booleans, but explicit comparisons for strings and numbers.

```javascript
// ❌ bad
if (isValid === true) {
  // ...
}

// ✅ good
if (isValid) {
  // ...
}

// ❌ bad
if (name) {
  // ...
}

// ✅ good
if (name !== '') {
  // ...
}

// ❌ bad
if (collection.length) {
  // ...
}

// ✅ good
if (collection.length > 0) {
  // ...
}
```


8\. Comments
-------------

#### 8.1 Use [`@deprecated` JSDOC tag](https://jsdoc.app/tags-deprecated) in comments to mark deprecated parts of code

```javascript
// ❌ bad
// deprecated, use makeElement
function make(tag) {

  // ...

  return element;
}

// ✅ good
/**
 * @deprecated use makeElement
 */
function make(tag) {

  // ...

  return element;
}
```


#### 8.2. `FIXME` and `TODO` comments should have a link to the task

Prefixing your comments with `FIXME` or `TODO` helps other developers quickly understand if you’re pointing out a problem that needs to be revisited, or if you’re suggesting a solution to the problem that needs to be implemented. These are different than regular comments because they are actionable. The actions are `FIXME: -- need to figure this out` or `TODO: -- need to implement`.



#### 8.3. Use `// FIXME:` to annotate problems.

```javascript
class Calculator extends Abacus {
  constructor() {
    super();

    // FIXME: shouldn’t use a global here (https://task.manager/t/12345)
    total = 0;
  }
}
```



#### 8.4. Use `// TODO:` to annotate solutions to problems.

```javascript
class Calculator extends Abacus {
  constructor() {
    super();

    // TODO: total should be configurable by an options param (https://task.manager/t/12345)
    this.total = 0;
  }
}
```

#### 8.5. Fields in Sequelize models and business logic methods should have a `@description` annnotation with JSDoc comment

>❓Why? It gives more context right at the moment of reading code. Especially useful for people working with other teams' code. Extremely useful for QAs, Data analytics, new Mates and others who don't work with particular module day to day but needs context.

>❓What is "Business logic method"? In short, if method requires domain context, it is considered as the business logic one. There is no need to write description to "sum" helper

Example of business logic method:
**/modules/subscription/subscription.service.ts**
```typescript
  /**
   * @description Payment is "intro" if it is a FIRST payment and
   * payment_type is TRIAL or INTRO_OFFER. Usually it means this payment is processed
   * with a discount.
   */
  async isIntroPayment(
    options: {
      // list of options
    },
  ): Promise<boolean> {
    // rest of code here
  }
```

**models/Subscription.ts**
```typescript
// ❌ bad
export class Subscription extends ModelBase<User> {
  @Column
  started: boolean;

  @Column({
    type: DataType.DOUBLE,
  })
  price: number;

  @ForeignKey(() => Currency)
  @Column({
    field: 'currency_id',
  })
  currencyId: number;
}
// ----

// ✅ good
export class Subscription extends ModelBase<User> {
  /**
   * @description Is subscription started. If true, user has access to the subscription benefits.
   * Is set up after the first payment.
   */
  @Column
  started: boolean;

  /**
   * @description Subscription price. Copied on creation from the SubscriptionPlanPricingOption or from the SubscriptionPlan
   * if the pricing option is not set.
   * It allows to store the price at the moment of the subscription creation and not to change it on the plan changes.
   */
  @Column({
    type: DataType.DOUBLE,
  })
  price: number;

  /**
   * @description Subscription currency id. Related to the Currency model.
   * Copied on creation from the SubscriptionPlanPricingOption or from the SubscriptionPlan
   * if the pricing option is not set.
   * It allows to store the currency at the moment of the subscription creation and not to change it on the plan changes.
   */
  @ForeignKey(() => Currency)
  @Column({
    field: 'currency_id',
  })
  currencyId: number;
}
```


9\. Type Casting & Coercion
----------------------------



#### 9.1 Strings:

eslint: [`no-new-wrappers`](https://eslint.org/docs/rules/no-new-wrappers)



```javascript
// => this.reviewScore = 9;

// ❌ bad
const totalScore = new String(this.reviewScore); // typeof totalScore is "object" not "string"

// ❌ bad
const totalScore = this.reviewScore + ''; // invokes this.reviewScore.valueOf()

// ❌ bad
const totalScore = this.reviewScore.toString(); // isn’t guaranteed to return a string

// ✅ good
const totalScore = String(this.reviewScore);
```



#### 9.2. Numbers: Use `Number` for type casting and `parseInt` always with a radix for parsing strings.

eslint: [`radix`](https://eslint.org/docs/rules/radix) [`no-new-wrappers`](https://eslint.org/docs/rules/no-new-wrappers)



```javascript
const inputValue = '4';

// ❌ bad
const val = new Number(inputValue);

// ❌ bad
const val = +inputValue;

// ❌ bad
const val = inputValue >> 0;

// ❌ bad
const val = parseInt(inputValue);

// ✅ good
const val = Number(inputValue);

// ✅ good
const val = parseInt(inputValue, 10);
```


#### 9.3 Booleans:

eslint: [`no-new-wrappers`](https://eslint.org/docs/rules/no-new-wrappers)



```javascript
const age = 0;

// ❌ bad
const hasAge = new Boolean(age);

// ✅ good, but ! and !! may be confused
const hasAge = !!age;

// best
const hasAge = Boolean(age);
```



10\. Naming Conventions
-----------------------



#### [10.1](https://mate-academy.github.io/style-guides/javascript.html#naming--descriptive) Avoid single letter names. Be descriptive with your naming.

eslint: [`id-length`](https://eslint.org/docs/rules/id-length)



**💡 Note:** it's allowed to use single letter name for iterator variable



```javascript
// ❌ bad
function q() {
  // ...
}

// ✅ good
function query() {
  // ...
}

// ✅ good
for (let i = 0; i < array.length; i+=1) {
  // ...
}
```



#### 10.2. Use camelCase when naming objects, functions, and instances. Function names are typically verbs or verb phrases.

eslint: [`camelcase`](https://eslint.org/docs/rules/camelcase.html)



```javascript
// ❌ bad
const OBJEcttsssss = {};
const this_is_my_object = {};
function c() {}

// ✅ good
const thisIsMyObject = {};
function calculatePrice() {}
```



#### 10.3. Use PascalCase only when naming constructors or classes.

eslint: [`new-cap`](https://eslint.org/docs/rules/new-cap.html)



```javascript
// ❌ bad
function user(options) {
  this.name = options.name;
}

const bad = new user({
  name: 'nope',
});

// ✅ good
class User {
  constructor(options) {
    this.name = options.name;
  }
}

const good = new User({
  name: 'yup',
});
```



#### 10.4 Do not use trailing or leading underscores.

eslint: [`no-underscore-dangle`](https://eslint.org/docs/rules/no-underscore-dangle.html)



>❓Why? JavaScript does not have the concept of privacy in terms of properties or methods. Although a leading underscore is a common convention to mean “private”, in fact, these properties are fully public, and as such, are part of your public API contract. This convention might lead developers to wrongly think that a change won’t count as breaking, or that tests aren’t needed. tl;dr: if you want something to be “private”, it must not be observably present.



```javascript
// ❌ bad
this.__firstName__ = 'Panda';
this.firstName_ = 'Panda';
this._firstName = 'Panda';

// ✅ good
this.firstName = 'Panda';
```


#### 10.5 A base filename should exactly match the name of its primary export.

```javascript
// file 1 contents
class CheckBox {
  // ...
}
export default CheckBox;

// file 2 contents
export default function fortyTwo() { return 42; }

// file 3 contents
export default function insideDirectory() {}

// file 4 contents
export class User {}

// in some other file
// ❌ bad
import CheckBox from './checkBox'; // PascalCase import/export, camelCase filename
import FortyTwo from './FortyTwo'; // PascalCase import/filename, camelCase export
import InsideDirectory from './InsideDirectory'; // PascalCase import/filename, camelCase export
import { User } from './user'; // PascalCase import/filename, camelCase export

// ❌ bad
import CheckBox from './check_box'; // PascalCase import/export, snake_case filename
import forty_two from './forty_two'; // snake_case import/filename, camelCase export
import inside_directory from './inside_directory'; // snake_case import, camelCase export
import index from './inside_directory/index'; // requiring the index file explicitly
import insideDirectory from './insideDirectory/index'; // requiring the index file explicitly

// ✅ good
import CheckBox from './CheckBox'; // PascalCase export/import/filename
import { User } from './User' // PascalCase export/import/filename
import fortyTwo from './fortyTwo'; // camelCase export/import/filename
import insideDirectory from './insideDirectory'; // camelCase export/import/directory name/implicit "index"
// ^ supports both insideDirectory.js and insideDirectory/index.js
```



#### 10.6 Acronyms and initialisms should always be all capitalized, or all lowercased.



>❓Why? Names are for readability, not to appease a computer algorithm.



```javascript
// ❌ bad
import SmsContainer from './containers/SmsContainer';

// ❌ bad
const HttpRequests = [
  // ...
];

// ✅ good
import SMSContainer from './containers/SMSContainer';

// ✅ good
const HTTPRequests = [
  // ...
];

// also good
const httpRequests = [
  // ...
];

// best
import TextMessageContainer from './containers/TextMessageContainer';

// best
const requests = [
  // ...
];
```



#### 10.7 You may optionally uppercase a constant only if it (1) is exported, (2) is a `const` (it can not be reassigned), and (3) the programmer can trust it (and its nested properties) to never change.



>❓Why? This is an additional tool to assist in situations where the programmer would be unsure if a variable might ever change. UPPERCASE\_VARIABLES are letting the programmer know that they can trust the variable (and its properties) not to change.



*   What about all `const` variables? - This is unnecessary, so uppercasing should not be used for constants within a file. It should be used for exported constants however.
*   What about exported objects? - Uppercase at the top level of export (e.g. `EXPORTED_OBJECT.key`) and maintain that all nested properties do not change.



```javascript
// ❌ bad
const PRIVATE_VARIABLE = 'should not be unnecessarily uppercased within a file';

// ❌ bad
export const THING_TO_BE_CHANGED = 'should obviously not be uppercased';

// ❌ bad
export let REASSIGNABLE_VARIABLE = 'do not use let with uppercase variables';

// ---

// allowed but does not supply semantic value
export const apiKey = 'SOMEKEY';

// better in most cases
export const API_KEY = 'SOMEKEY';

// ---

// ❌ bad - unnecessarily uppercases key while adding no semantic value
export const MAPPING = {
  KEY: 'value'
};

// ✅ good
export const MAPPING = {
  key: 'value'
};
```



#### 10.8 Don't shorten variables or functions names



>❓Why? For better readability and maintainance



```javascript
// ❌ bad
const usr = new User();
this.repo = new UserRepository();
this.userRepo = new UserRepoistory();
const findVac = () => this.vacRep.find();

// ✅ good
const user = new User();
this.userRepository = new UserRepository();
const findVacancy = () => this.vacancyRepository.find();
```


#### 10.9 If the variable/property/method is a `boolean`, use `isVal()` or `hasVal()`.

```javascript
// ❌ bad
if (!dragon.age()) {
  return false;
}

// ❌ bad
if (married) {
  // ...
}

// ✅ good
if (!dragon.hasAge()) {
  return false;
}

// ✅ good
if (isMarried) {
  // ...
}
```


11\. Standard Library
---------------------

The [Standard Library](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects) contains utilities that are functionally broken but remain for legacy reasons.



#### 11.1. Use `Number.isNaN` instead of global `isNaN`.

eslint: [`no-restricted-globals`](https://eslint.org/docs/rules/no-restricted-globals)



>❓Why? The global `isNaN` coerces non-numbers to numbers, returning true for anything that coerces to NaN. If this behavior is desired, make it explicit.



```javascript
// ❌ bad
isNaN('1.2'); // false
isNaN('1.2.3'); // true

// ✅ good
Number.isNaN('1.2.3'); // false
Number.isNaN(Number('1.2.3')); // true
```



#### 11.2. Use `Number.isFinite` instead of global `isFinite`.

eslint: [`no-restricted-globals`](https://eslint.org/docs/rules/no-restricted-globals)



>❓Why? The global `isFinite` coerces non-numbers to numbers, returning true for anything that coerces to a finite number. If this behavior is desired, make it explicit.



```javascript
// ❌ bad
isFinite('2e3'); // true

// ✅ good
Number.isFinite('2e3'); // false
Number.isFinite(parseInt('2e3', 10)); // true
```



12\. Testing
------------

#### 12.1. **Yup.**


#### 12.2. **No, but seriously**:

*   Whichever testing framework you use, you should be writing tests!
*   Strive to write many small pure functions, and minimize where mutations occur.
*   Be cautious about stubs and mocks - they can make your tests more brittle.
*   We primarily use [`mocha`](https://www.npmjs.com/package/mocha) and [`jest`](https://www.npmjs.com/package/jest) at Mate academy
*   100% test coverage is a good goal to strive for, even if it’s not always practical to reach it.
*   Whenever you fix a bug, _write a test_. A bug fixed without a test is almost certainly going to break again in the future.
