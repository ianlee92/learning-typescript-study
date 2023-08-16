### 📅 2023년 8월 16일

# 📚 13장 구성 옵션
### 파일 포함
- tsc는 현 디렉터리와 하위 디렉터리에 있는 숨겨지지 않은 모든 .ts 파일에서 실행되고, 숨겨진 디렉터리와 node_modules 디렉터리는 무시합니다.
- 파일을 포함하는 가장 흔한 방법으로 tsconfig.json의 최상위 "include" 속성을 사용합니다. include 속성에 타입스크립트 컴파일에 포함한 디렉터리와 파일을 설명하는 문자열 배열을 명시합니다.
- 포함할 파일을 더 세밀하게 제어하기 위해 include 문자열에 글로브 와일드카드가 허용됩니다. * : 0개 이상 일치, ? : 하나 일치, **/ : 모든 레벨에 중첩된 모든 디렉터리와 일치
- 프로젝트의 include 파일 목록에 타입스크립트로 컴파일할 수 없는 파일이 포함되는 경우 exclude를 사용한다.
    ```jsx
    {
        "exclude": ["**/external", "node_modules"],
        "include": ["src"]
    }
    ```
- 기본적으로 exclude에는 컴파일된 외부 라이브러리 파일에 대해 타입스크립트 컴파일러가 실행되지 않도록 ["node_modules", "bower_components", "jspm_packages"]가 포함된다.

### JSX
- JSX 구문은 리액트 같은 UI 라이브러리에서 자주 사용한다. 기술적으로 자바스크립트가 아니다. 자바스크립트로 컴파일되는 자바스크립트 구문의 확장이다.
- 타입스크립트가 .tsx 파일에 대한 자바스크립트 코드를 내보내는 방법은 "jsx" 컴파일러 옵션에 사용되는 값으로 결정된다. "preserve", "react", "react-native" 세 가지 값 중 하나를 사용한다.
    ```jsx
    {
        "compilerOptions": {
            "jsx": "preserve"
        }
    }
    ```

### resolveJsonModule
- true로 설정하면 .json 파일을 읽을 수 있다. 이렇게 하면 .json 파일을 마치 객체를 내보내는 .ts 파일을 읽을 수 있다.

### target
- 자바스크립트 코드 구문을 지원하기 위해 어느 버전까지 변환해야 하는지를 지정하는 target 컴파일러 옵션을 제공한다. target을 지정하지 않으면 이전 버전과의 호환성을 위해 기본적으로 "es3"이 지정된다.

### 소스 맵(source map)
- 출력 파일의 내용이 원본 소스 파일과 어떻게 일치하는지에 대한 설명이다. 소스 맵은 출력 파일을 탐색할 때 디버거 같은 개발자 도구에서 원본 소스 코드를 표시하도록 설정한다. 브라우저 개발자 도구와 IDE에서 디버깅하는 동안 원본 소스 파일 내용을 볼 수 있도록 하는 시각적인 디버거에 특히 소스 맵이 유용하다.

### noEmit
- 다른 도구를 이용해 소스 파일을 컴파일하고, 자바스크립트를 출력하는 프로젝트에서 타입스크립트는 파일 생성을 모두 건너뛰도록 지시할 수 있다. noEmit 컴파일러 옵션을 활성화하면 타입스크립트가 온전히 타입 검사기로만 작동한다.
- tsc --noEmit을 실행하면 어떠한 파일도 생성하지 않고 타입스크립트는 발견한 구문 또는 타입 오류만 보고한다.

### lib
- 타입스크립트가 런타임 환경에 있다고 가정하는 전역 API는 lib 컴파일러 옵션으로 구성할 수 있다.
- lib 설정을 변경하는 유일한 이유는 브라우저에서 실행되지 않는 프로젝트에서 기본으로 포함된 dom을 제거하기 위함이다.
    ```jsx
    {
        "compilerOptions": {
            "lib": ["dom", "es2021"]
        }
    }
    ```
- 최신 자바스크립트 API를 지원하기 위해 폴리필(polyfill)을 사용하는 프로젝트에서 lib 컴파일러 옵션을 사용해 dom과 ECMA스크립트 특정 버전을 포함할 수 있다.

### skipLibCheck
- 소스 코드에 명시적으로 포함되지 않은 선언 파일에서 타입 검사를 건너뛰도록 하는 컴파일러 옵션. 공유된 라이브러리의 정의가 서로 다르고 충돌할 수 있는 패키지 의존성을 많이 사용하는 애플리케이션에 유용하다.
- 타입 검사 일부를 건너뛰는 작업으로 타입스크립트 성능을 개선한다.

### strict
- 모든 엄격 모드 검사가 활성화된다. 특정 검사를 명시적으로 비활성화할 수 있다.
    ```jsx
    {
        "compilerOptions": {
            "noImplicitAny": false,
            "strict": true
        }
    }
    ```
- noImplicitAny: 암시적 any로 대체될 때 타입 검사 오류가 발생하도록 지시.
- strictNullChecks: 코드의 모든 타입에 null | undefined가 추가되고 모든 변수가 null 또는 undefined를 받을 수 있도록 허용. (충돌을 방지하고 십억 달러의 실수를 제거하는데 매우 유용)

### module
- 어떤 모듈 시스템으로 변환된 코드를 사용할지 결정하기 위해 module 컴파일러 옵션 제공.

### moduleResolution
- 모듈 해석은 import에서 가져온 경로가 module에 매핑되는 과정으로 해당 과정에 로직을 지정하는데 사용할 수 있는 옵션 제공.
- 다음 두 가지 로직 전략 중 하나를 제공하는 것을 선호한다.
    - node: 기존 Node.js와 같은 CommonJS 리졸버에서 사용하는 동작.
    - nodenext: ECMA스크립트 모듈에 대해 지정된 동작에 맞게 조정.
- 두 전략은 유사하고 대부분의 프로젝트는 둘 중 하나를 사용할 수 있으며 차이를 느끼지 못한다.

### esModuleInterop
- module이 "es2015" 또는 "esnext"와 같은 ECMA스크립트 모듈 형식이 아닌 경우 타입스크립트에서 내보낸 자바스크립트 코드에 소량의 로직을 추가한다.
- 해당 컴파일러 옵션을 활성화하는 이유 중 하나는 기본 내보내기를 제공하지 않는 "react" 같은 패키지를 위해서이다.

### isolatedModules
- 프로젝트에서 타입스크립트가 아닌 다른 도구를 사용해 자바스크립트로 변환하는 경우에 활성화는 것이 좋다.

### allowJs
- 자바스크립트 파일에 선언된 구문을 타입스크립트 파일에서 타입검사를 하도록 허용한다.
- allowJs가 활성화되지 않으면 import 문은 알려진 타입을 갖지 못한다.
- ECMA스크립트 target에 맞게 컴파일되고 자바스크립트로 내보내진 파일 목록에 자바스크립트 파일을 추가한다.
- allowJs 옵션이 활성화된 경우 소스 맵과 선언 파일도 생성된다.

# 📚 14장 구문 확장

- 내용 없음

# 📚 15장 타입 운영

### 매핑된 타입
- 매핑된 타입은 인덱스 시그니처와 유사한 구문을 사용하지만 [i: string]과 같이 :를 사용한 정적 키 타입을 사용하는 대신 [K in OriginalType]과 같이 in을 사용해 다른 타입으로부터 계산된 타입을 상요한다.
    ```jsx
    type Animals = "alligator" | "baboon" | "cat";

    tpye AnimalCounts = {
        [K in Animals]: number;
    }
    // 다음과 같음
    {
        alligator: number;
        baboon: number;
        cat: number;
    }
    ```

### 타입에서 매핑된 타입
- 존재하는 타입에 keyof 연산자를 사용해 키를 가져오는 방식으로 작동한다.
    ```jsx
    interface AnimalVariants {
        alligator: boolean;
        baboon: number;
        cat: string;
    }

    tpye AnimalCounts = {
        [K in keyof AnimalVariants]: number;
    }
    // 다음과 같음
    {
        alligator: number;
        baboon: number;
        cat: number;
    }
    ```
- 원본 객체가 SomeName이고 매핑이 [K in keyof SomeName]인 경우라면 매핑된 타입의 각 멤버는 SomeName 멤버의 값을 SomeName[K]로 참조할 수 있다.

```jsx
interface BirdVariants {
    dove: string;
    eagle: boolean;
}

tpye NullableBirdVariants = {
    [K in keyof BirdVariants]: BirdVariants[K] | null,
};

// 다음과 같음
{
    dove: string | null;
    eagle: boolean | null;
}
```

### 제네릭 매핑된 타입
```jsx
type MakeReadonly<T> {
    readonly [K in keyof T]: T[K];
}

interface Species {
    genus: string;
    name: string;
}

type ReadonlySpecies = MakeReadonly<Species>;

// 다음과 같음
{
    readonly enus: string;
    readonly name: string;
}
```

```jsx
interface GenusData {
    family: string;
    name: string;
}

type MakeOptional<T> {
    [K in keyof T]?: T[K];
}

// 다음과 같음
{
    family?: string;
    name?: string;
}
```

### never
- bottom 타입인 never는 존재할 수 없는 타입이라는 의미를 가지고 있다.
- 교차타입(&)과 함께 사용하면 교차 타입을 never로 만든다.
- 유니언 타입(|)과 함께 사용하면 never는 무시된다.
```jsx
type NeverIntersection = never & string; // 타입: never
type NeverUnion = never | string; // 타입: string
```

- 제네릭 조건부 타입은 일반적으로 유니언에서 타입을 필터링하기 위해 never를 사용한다.
```jsx
type OnlyStrings<T> = T extends string ? T: never;

type RedOrBlue = OnlyString<"red" | "blue" | 0 | false>;
// 다음과 같음: "red" | "blue"
```

- 유니언에서 never의 동작은 매핑된 타입에서 멤버를 필터링할 때도 유용하다.
```jsx
type OnlyStringProperties<T> = {
    [K in keyof T]: T[K] extends string ? K : never;
}[keyof T];

interface AllEventData {
    participants: string[];
    location: string;
    name: string;
    year: number;
}

type OnlyStringEventData = OnlyStringProperties<AllEventData>;
// 다음과 같음: "location" | "name"
```