# 💡 1장 자바스크립트에서 타입스크립트로

## 자바스크립트의 탄생..?

- 자바스크립트는 호환성 유지, 유연한 언어..

## 바닐라 자바스크립트의 함정

### 1. 값 비싼 자유

- 대부분의 불만은 핵심 기능 이 자유에 있다..! 제한없음이 파일의 크기가 커질수록 훼손된다.
- 자바스크립트는 동적타입 언어.. 이로인한 문제 발생!

### 2. 부족한 문서

- JSDoc 의 방대한 양과 한계

### 3. 부족한 개발자 도구

- 코드베이스에 대한 대규모 변경을 자동화 하기 어렵다.

## 타입스크립트

- 프로그래밍 언어 : 자바스크립트의 모든 구문과, 타입 정의 및 새로운 타입스크립트 구문 포함
- 타입 검사기 : 파일의 모든 구성 요소를 이해, 잘못된 부분을 말해줌
- 컴파일러 : 타입 검사기를 실행, 문제보고 이에 대응하는 JS 코드 작성
- 언어 서비스 : 타입 검사기를 통해 편집기에 유틸리티 제공법을 알려줌

## 타입스크립트 시작하기

- 스니펫 사용

### 1. 제한을 통한 자유

- 잘못된 수의 인수를 사용해서 함수를 호출하는 것 = TS가 제한하는 JS의 근시안적인 자유
  - Error: Expected 1 argument, but got 2.

### 2. 정확한 문서화, 더 강력한 개발자 도구

- 타입 선언
- 문자열 자동 완성 등 지원

## 자바스크립트로의 확장

- 현재의 미래의 ECMA스크립트 제안에 맞춤
- 모든 자바스크립트 코드의 런타임 동작 유지

---

# 📝 2장 타입 시스템

## 타입의 종류

### 1. 원시타입

- null
- undefined
- boolean
- string
- number
- bigint
- symbol

### 2. 타입시스템

- 코드를 읽고 존재하는 모든 타입과 값을 이해
- 각 값이 초기 선언에서 가질 수 있는 타입 확인
- 각 값이 추후 코드에서 어떻게 사용될 수 있는지 모든 방법을 확인
- 값의 사용법이 타입과 일치하지 않으면 사용자에게 오류 표시

```jsx
let firstName = 'sara'; // firstName의 타입 스트링으로 선언
firstName.length(); // length멤버는 함수가 아닌 숫자라는 오류 표시, 호출불가
```

### 3. 오류 종류

- 구문 오류 : 자바스크립트로 변환되는 것을 차단한 경우
  - ex) let let wat;
- 타입 오류
  - ex) console.blub(”blah”);

### 4. 할당 가능성

```jsx
let lastName = 'Ko';
lastName = true; // Error: Type 'boolean' is not assignable to type 'string'.
```

- 함수 호출이나 변수의 값을 제공할 수 있는지 여부를 확인하는 것

### 5. 타입 annotation

- 진화하는 any

```jsx
// annotation 미제공
let rocker; // 타입: any
rocker = 'ggombee'; // 타입: string
rocker.toUpperCase(); // OK

rocker = 19; // 타입: number;
rocker.toUpperCase(); // Error: 'toUpperCase' does not exist on type 'number'.

// annotation 제공
let rocker: string; // 타입: string
rocker = 'ggombee'; // 타입: string
rocker.toUpperCase(); // OK

rocker = 19; // Error: Type 'number' is not asssignable to 'string'.

// annotation 중복
let rocker: string = 'ggombee'; // 타입 시스템 변경 되지 않음
```

- 스크립트가 사용가능한 형태에서만의 작업을 허용
  - string.length, .push() …

### 6. 모듈

- 모듈 : export 또는 import 가 있는 파일
- 스크립트 : 모듈이 아닌 모든 파일

```jsx
// a.ts
export const shared = 'Cher'; // 모듈로 인식

// b.ts
const shared = 'Cher'; // import, export 없어서 스크립트로 전역 스코프로 선언

// c.ts
import { shared } from './a';

export const shared = 'Cher'; // 중복변수 선언
```

---

# 🌍 **3장 유니언과 리터럴**

- 유니언 : 값에 허용된 타입을 두 개 이상의 가능한 타입으로 확장
- 내로잉 : 값에 허용된 타입이 하나 이상의 가능한 타입이 되지 않도록 좁히는 것

## 유니언 타입

```jsx
// 타입: undefined 혹은 string
let mathmatician = Math.random() > 0.5 ? undefined : 'GGombee';
```

### 1. 유니언 타입 선언

- `let thinker: string | null = null;`
- 유니언 타입 사용할 경우 특정 타입에만 사용할 수 있는 함수 사용 불가
  - ex) toUpperCase(), toFixed() …

## 내로잉

- 값이 정의, 선언 유추 된 것보다 더 구체적인 타입임을 코드에서 유추

### 1. 값 할당 통한 내로잉

```jsx
let inventor: number | string = 'GGombee'; // 타입: string
inventor.toUpperCase(); // OK
inventor.toFixed(); // Error: Property 'toFixed' does not exist on type 'string'
```

### 2. 조건 검사를 통한 내로잉

```jsx
let mathmatician = Math.random() > 0.5 ? 10 : 'GGombee';

if (mathmatician === 'GGombee') {
  mathmatician.toUppperCase(); // OK -> if 문을 통해 내로잉 함
}

mathmatician.toUppperCase(); // Error: Property 'toFixed' does not exist on type 'string'
```

### 3. typeof 검사를 통한 내로잉

```jsx
let mathmatician = Math.random() > 0.5 ? 10 : "GGombee";

if (typeof mathmatician === "string") {
		mathmatician.toUppperCase(); // OK -> if 문을 통해 내로잉 함
}

typeof mathmatician === "string"
		? mathmatician.toUpperCase();      // OK
		: mathmatician.toFixed();          // OK
```

## 리터럴 타입

```jsx
let lifespan: number | 'ongoing' | 'uncertain';

lifespan = 89; // OK
lifespan = 'ongoing'; // OK

lifespan = true;
// Error: ~ not assignable to type 'number | "ongoing" | "uncertain"';
```

### 1. 리터럴 할당 가능성

```jsx
let specificallyType: 'Special';

specificallyType = 'Special'; // OK
specificallyType = 'Common';
// Error: Type '"Common"' is not assignable to type '"Special"'.
```

단순 변수 할당으로 할당 가능성이 사라짐.

### 2. 엄격한 null 검사

- strictNullChecks를 통해 잠재적 충돌 확인

### 3. 참 검사를 통한 내로잉

- falsy 한 값을 제외 하면 모두 참
  - ex) false, 0, -0, 0n, “”, null, undefined, NaN

### 4. 초깃값이 없는 변수

- 초기값을 넣어주지 않으면 선언오류가 발생

```jsx
let mathmatician: string;

mathmatician?.length; // Error: Variable 'math' is used before being assigned.

let mathmatician: string | undefined;

mathmatician?.length; // OK
```

### 5. 타입 별칭

```jsx
type RawData = boolean | number;
type RawDataCombine = string | RawData; // 타입 별칭 결합

let rawDataFirst: RawData;

console.log(RawData);
// Error: 'RawData' only refers to a type, but is being used as a value here.
```

- `타입 별칭은 자바스크립트로 컴파일 되지 않는다.`
  - 런타임 코드에 참조할 수 없고, 존재하지 않는 타입에 접근하려고 하면 타입오류

---

# 🍀 4장 객체

## 객체 타입

- 객체 접급을 위해 value.멤버 혹은 value[’멤버’] 사용

### 1. 별칭 객체 타입

```jsx
type Poet = {
  born: number,
  name: string,
};

let poetLater: Poet;
```

## 구조적 타이핑

- 타입을 충족하는 모든 값을 해당 타입의 값으로 사용가능

```jsx
type First {
		firstName: string;
}

type Last {
		lastName: string;
}

const hasBoth = {
		firstName : "ggom";
		lastName : "bee";
}

let first: First = hasBoth; // OK: 'hasBoth'는 'string'타입의 'firstName'을 포함함
let last: Last = hasBoth;
```

- 구조적 타이핑은 정적 시스템이 타입 검사하는 경우
- `덕 타이핑`은 런타임에서 사용될 때까지 객체 타입을 검사하지 않는 것

### 1. 사용검사

```jsx
type Names {
		firstName: string;
		lastName: string;
}

const hasBoth = {           // OK
		firstName : "ggom";
		lastName : "bee";
}

const hasOne = {
		firstName : "ggom";       // Error: lastName required..
}

const hasMany = {
		firstName : "ggom";
		lastName : "bee";
		activity : "golf";    // Error: activity does not exist on type names...
}

const extraNames: Names = hasMany;  // OK 타입오류 발생 X , 초과 속성 검사 우회
```

## 객체 타입 유니언

```jsx
// 유추된 유니언 타입 { name: string; pages?: number; rhymes?: boolean;}
const poem = Math.random > 0.5
		? { name: "ggom", pages: 7}
		: { name: "bee", rhymes: true}

// 명시된 객체 유니언
type PoemPages{
		name: string;
		pages: number;
}

type PoemRhymes{
		name: string;
		rhymes: boolean;
}

type Poem = PoemPages | PoemRhymes;

poem.name;    // OK
poem.pages;   // Error
```

### 1. 객체 타입 내로잉

```jsx
if ('pages' in poem) {
  poem.pages; // poem은 PoemPages로 내로잉
} else {
  poem.rhymes; // poem은 PoemRhymes로 내로잉
}
```

### 2. 판별된 내로잉

```jsx
type PoemPages{
		name: string;
		pages: number;
		type: 'pages';
}

type PoemRhymes{
		name: string;
		rhymes: boolean;
		type: 'rhymes';
}

if(poem.types === "pages") {
		console.log(poem.pages);    // OK
} else {
		console.log(poem.rhymes);    // OK
}

poem.pages   // Error
```

### 3. 교차타입

```jsx
type PoemPages{
		name: string;
		pages: number;
}

type PoemRhymes{
		name: string;
		rhymes: boolean;
}

type Poem = PoemPages & PoemRhymes;
// 다음과 같음
// {
//		name: string;
//		pages: number;
//		rhymes: boolean;
//}

type ShortPoem = { author: string } & ( PoemPages | PoemRhymes)

const newPoem: ShortPoem = {
		// author 필수
		// name + pages 혹은 name + rhymes 무조건 들어가야함
}
```

- 교차타입은 잘못 사용하기 쉽고 불가능한 타입을 생성하기도 한다.
  - 원시타입은 교차타입 사용할 수 없다. `never`
