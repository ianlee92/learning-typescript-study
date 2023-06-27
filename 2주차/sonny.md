# [러닝 타입스크립트] Chapter.5 ~ Chapter.7 정리

## 정리

### 자바스크립트의 인수 (p.100)

```markdown
자바스크립트에서는 인수의 수와 상관없이 함수를 호출할 수 있습니다. 함수가 너무 적거나 많은 인수로 호출되면
타입스크립트는 인수의 개수를 계산합니다.
```

### 타입스크립트의 함수 반환 타입 (p.105)

```markdown
함수에 다른 값을 가진 여러 개의 반환문을 포함하고 있다면, 타입스크립트는 반환 타입을 가능한 모든 반환 타입의
조합으로 유추합니다.
```

### 명시적 반환 타입 (p.105)

```markdown
변수와 마찬가지로 타입 애너테이션을 사용해 함수의 반환 타입을 명시적으로 선언하지 않는 것이 좋습니다.
그러나 특히 함수에서 반환 타입을 명시적으로 선언하는 방식이 매우 유용할 때가 종종 있습니다.
```

- 가능한 반환값이 많은 함수가 항상 동일한 타입의 값을 반환하도록 강제합니다.
- 타입스크립트는 재귀 함수의 반환 타입을 통해 타입을 유추하는 것을 거부합니다.
- 수백 개 이상의 타입스크립트 파일이 있는 매우 큰 프로젝트에서 타입스크립트 타입 검사 속도를 높일 수 있습니다.

### 타입스크립트의 함수 할당 오류 (p.108)

```markdown
두 함수를 서로 할당할 수 없다는 오류를 출력할 때 타입스크립트는 일반적으로 세 가지 상세한 단계를
제공합니다. 각 단계는 다음과 같이 점점 자세한 내용을 담고 있습니다.
```

- 첫 번째 들여쓰기 단계는 두 함수 타입을 출력합니다.
- 다음 들여쓰기 단계는 일치하지 않는 부분을 지정합니다.
- 마지막 들여쓰기 단계는 일치하지 않는 부분에 대한 정확한 할당 가능성 오류를 출력합니다.

### 함수와 void (p.112)

```markdown
특히 void를 반환하도록 선언된 타입 위치에 전달된 함수가 반환된 모든 값을 무시하도록 설정할 때 유용합니다.
```

## 새롭게 알게된 부분

### void와 never (p.113)

```markdown
일부 함수는 값을 반환하지 않을 뿐만 아니라 반환할 생각도 전혀 없습니다. never 반환 함수는 (의도적으로)
항상 오류를 발생시키거나 무한 루프를 실행하는 함수입니다.
```

never는 void와는 다릅니다. void는 아무것도 반환하지 않는 함수를 위한 것이고, never는 절대 반환하지 않는 함수를 위한 것입니다.

```tsx
// never
const neverFn = ():never => {
  throw Error('Error!')
}

// void
const voidFn = ():void => {
  return
}
```

### 타입스크립트는 어떤 언어인가 (p. 121)

```markdown
타입스크립트는 배열의 멤버를 찾아서 해당 배열의 타입 요소를 되돌려주는 전형적인 인덱스 기반 접근 방식을
이해하는 언어
```

### 불안정한 멤버 (p. 122)

```markdown
때로는 값 타입에 대한 타입 시스템의 이해가 올바르지 않을 수 있습니다. 특히 배열은 타입 시스템에서 불안정한
소스입니다.

자바스크립트에서조차도 배열의 길이보다 큰 인덱스로 배열 요소에 접근하면 undefined를 제공합니다.
```

```tsx
function withElements(elements: string[]) {
  console.log(elements[9001].length) // 타입 오류 없음
}

withElements(["It's", "over"])
```

코드 스니펫에서 elements[9001]은 undefined가 아니라 string 타입으로 간주됩니다.

### 스프레드 연산자 (p. 123)

```markdown
서로 다른 타입의 두 배열을 함께 스프레드해 새 배열을 생성하면 새 배열은 두 개의 원래 타입 중 어느 하나의
요소인 유니언 타입 배열로 이해됩니다.
```

### 읽기 전용 속성 (p.134)

```tsx
readonly 제한자는 타입 시스템에만 존재하며 인터페이스에서만 사용할 수 있습니다.
readonly 제한자는 객체의 인터페이스를 선언하는 위치에서만 사용되고 실제 객체에는 적용되지 않습니다.
```

```tsx
interface Page {
  readonly text: string
}
```

### 호출 시그니처  (p. 137)

값을 함수처럼 호출하는 방식에 대한 타입 시스템의 설명

- 사용자 정의 속성을 추가로 갖는 함수를 설명하는 데 사용할 수 있음

```tsx
// FunctionAlias와 CallSignature 타입은 모두 동일한 함수 매개변수와 반환 타입을 설명
type FunctionAlias = (input: string) => number

interface CallSignature {
  (input: string): number
}

interface FunctionWithCount {
  count: number
  (): void
}

let hasCallCount: FunctionWithCount

function keepsTrackOfCalls() {}
keepsTrackOfCalls.count = 0

hasCallCount = keepsTrackOfCalls // OK
```

### 함수 오버로드와 메서드 오버로드 (p. 148)

MergedMethods 인터페이스는 두 가지 오버로드가 있는 different 메서드를 생성합니다.

```tsx
// 함수 오버로드는 불가능
interface MergedProperties {
  same: (input: boolean) => string
  different: (input: string) => string
}

interface MergedProperties {
  same: (input: boolean) => string         //OK
  different: (input: number) => string     // Error!
}

// --------------------------------------------------

// 메서드 오버로드는 가능
interface MergedMethods {
  different(input: string): string
}

interface MergedMethods {
  different(input: number): string  // OK
}
```

## 다시 찾아보면 좋을 것 같은 부분

### 자바스크립트의 undefined 반환 (p. 112)

```markdown
자바스크립트 함수는 실젯값이 반환되지 않으면 기본으로 모두 undefined를 반환하지만 void는 undefined와
동일하지 않습니다. void는 함수의 반환 타입이 무시된다는 것을 의미하고 undefined는 반환되는 리터럴 값입니다.
```

### 함수 오버로드 (p. 114)

```markdown
하나의 최종 구현 시그니처와 그 함수의 본문 앞에 서로 다른 버전의 함수 이름, 매개변수, 반환 타입을 여러 번
선언합니다.

함수 오버로드는 복잡하고 설명하기 어려운 함수 타입에 사용하는 최후의 수단입니다. 함수를 단순하게 유지하고
가능하면 함수 오버로드를 사용하지 않는 것이 좋습니다.
```

**오버로드 시그니처**

- 일부 자바스크립트 함수는 선택적 매개변수와 나머지 매개변수만으로 표현할 수 없는 매우 다른 매개변수들로 호출될 수 있는 함수

**구현 시그니처**

- 실제 로직이 들어있는 최종 구현 함수

```tsx
// 오버로드 시그니처
function createDate(timestamp: number): Date
function createDate(month: number, day: number, year: number): Date

// 구현 시그니처
function createDate(monthOrTimestamp: number, day?: number, year?: number) {
  return day === undefined || year === undefined
    ? new Date(monthOrTimestamp)
    : new Date(year, monthOrTimestamp, day)
}
```

### 호출 시그니처 호환성 (p. 115)

```tsx
함수의 오버로드 시그니처에 있는 반환 타입과 각 매개변수는 구현 시그니처에 있는 동일한 인덱스의 매개변수에
할당할 수 있어야 합니다. 즉, 구현 시그니처는 모든 오버로드 시그니처와 호환되어야 합니다.
```

```tsx
// 오버로드 시그니처
function format(data: string): string // OK
function format(data: string, needle: string, haystack: string): string // OK
function format(getData: () => string): string // Error

// 구현 시그니처
function format(data: string, needle?: string, haystack?: string) {
  return needle & haystack ? data.replace(needle, haystack) : data;
}
```

### 배열과 함수 타입과 유니언 타입 배열 (p. 119)

```tsx
// 타입은 string 배열을 반환하는 함수
let createStrings: () => string[]

// 타입은 각각의 string을 반환하는 함수 배열
let stringCreators: (() => string)[]

// ------------------------------------------------

// 타입은 string 또는 number의 배열
let stringOrArrayOfNumbers: string | number[]

// 타입은 각각 number 또는 string인 요소의 배열
let arrayOfStringOrNumbers: (string | number)[]
```

### any 배열의 진화 (p. 120)

```markdown
초기에 빈 배열로 설정된 변수에서 타입 애너테이션을 포함하지 않으면 타입스크립트는 배열을 any[]로 취급하고
모든 콘텐츠를 받을 수 있습니다. any 변수가 변경되는 것처럼 any[] 배열이 변경되는 것도 좋아하지 않습니다.
```

```tsx
// 타입: any[]
let values = []

// 타입: string[]
values.push('')

// 타입: (number | string)[]
values[0] = 0
```

### 나머지 매개변수 스프레드 (p. 124)

타입스크립트는 나머지 매개변수로 배열을 스프레드하는 자바스크립트 실행을 인식하고 이에 대해 타입 검사를 수행합니다.

```tsx
function logWarriors(greeting: string, ...names: string[]) {}

const warriors = ['Cathay Williams', 'Lozen', 'Nzinga']
logWarriors('Hello', ...warriors) // OK

const birthYears = [1844, 1840, 1583]
logWarriors('Hello', ...birthYears) // Error! number
```

### 인터페이스에 대한 설명 (p. 131)

```markdown
일반적으로 더 읽기 쉬운 오류 메시지, 더 빠른 컴파일러 성능, 클래스와의 더 나은 상호 운용성을 위해
선호됩니다.
```

### 인터페이스 vs 타입 별칭 (p. 133)

- 속성 증가를 위해 병합할 수 있습니다.
    - 내장된 전역 인터페이스 또는 npm 패키지와 같은 외부 코드를 사용할 때 특히 유용합니다.
- 클래스가 선언된 구조의 타입을 확인하는 데 사용할 수 있지만 타입 별칭은 사용할 수 없습니다.
- 일반적으로 인터페이스에서 타입스크립트 타입 검사기가 더 빨리 작동합니다.
    - 타입 별칭이 하는 것처럼 새로운 객체 리터럴의 동적인 복사 붙여넣기보다 **내부적으로 더 쉽게 캐시할 수 있는 명명된 타입을 선언합니다.**
- 이름 없는 객체 리터럴의 별칭이 아닌 이름 있는(명명된) 객체로 간주되므로 어려운 특이 케이스에서 나타나는 오류 메시지를 좀 더 쉽게 읽을 수 있습니다.

### 함수의 메서드 (p. 136)

타입스크립트는 인터페이스 멤버를 함수로 선언하는 두 가지 방법을 제공합니다.

- **`메서드 구문`**: 인터페이스 멤버를 member(): void와 같이 **객체의 멤버로 호출되는 함수**로 선언
- **`속성 구문`**: 인터페이스의 멤버를 member: () ⇒ void와 같이 **독립 함수와 동일**하게 선언

### 메서드와 속성의 주요 차이점 (p. 137)

- 메서드는 readonly로 선언할 수 없지만 속성은 가능합니다.
- 이번 장 후반부에서 살펴볼 인터페이스 병합은 메서드와 속성을 다르게 처리합니다.
- 타입에서 수행되는 일부 작업은 메서드와 속성을 다르게 처리합니다.

### 현시점의 추천하는 메서드와 속성 관련 스타일 가이드 (p. 137)

- 기본 함수가 this를 참조할 수 있다는 것을 알고 있다면 메서드 함수를 사용하세요. 가장 일반적으로 클래스의 인스턴스에서 사용됩니다.
- 반대의 경우는 속성 함수를 사용하세요.

### 인덱스 시그니처 (p.141)

```markdown
인덱스 시그니처는 객체에 값을 할당할 때 편리하지만 타입 안정성을 완벽하게 보장하지는 않습니다.
인덱스 시그니처는 객체가 어떤 속성에 접근하든 간에 값을 반환해야 함을 나타냅니다.

정의되지 않았음에도 불구하고 정의되었다고 생각하도록 속입니다.
```

```tsx
interface DatesByName {
  [i: string]: Date
}

const publishDates: DatesByName = {
  Frankenstein: new Date('2023.06.28')
}

publishDates.Frankenstein // 타입: Date
console.log(publishDates.Frankenstein.toString()) // OK

publishDates.Beloved // 타입은 Date지만 런타임 값은 undefined
console.log(publishDates.Beloved.toString())
// 타입 시스템에서는 오류가 나지 않지만 실제 런타임에서는 오류가 발생함
```

### 숫자 인덱스 시그니처 (p. 142)

타입스크립트 인덱스 시그니처는 키로 string 대신 number 타입을 사용할 수 있지만, 명명된 속성은 그 타입을 포괄적인 용도의 string 인덱스 시그니처의 타입으로 할당할 수 있어야 합니다.

```tsx
interface MoreNarrowNumbers {
  [i: number]: string
  [i: string]: string | undefined
}

const mixesNumbersAndStrings: MoreNarrowNumbers = {
  0: '',
  key1: '',
  key2: undefined
}

inteface MoreNarrowStrings {
  [i: number]: string | undefined
  [i: string]: string
}

// key가 number인 타입은 key가 string 타입의 부분집합이어야 함
```

## 궁금한 부분

### P. 105

Q. “변수와 마찬가지로 타입 애너테이션을 사용해 함수의 반환 타입을 명시적으로 선언하지 않는 것이 좋습니다.”에서 왜 반환 타입을 명시적으로 선언하지 않는 것이 좋은지?

### P. 133

Q. “가능하다면 인터페이스 사용을 추천합니다. 즉, 타입 별칭의 유니언 타입과 같은 기능이 필요할 때까지는 인터페이스를 사용하는 것이 좋습니다.”