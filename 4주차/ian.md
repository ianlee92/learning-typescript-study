### 📅 2023년 7월 12일

# 📚 10장 제네릭
- 매개변수 괄호 바로 앞 홑화살괄호(<,>)로 묶인 타입 매개변수에 별칭을 배치해 함수를 제네릭으로 만듭니다.
- 함수에 타입 매개변수를 추가하면 타입 안정성을 유지하고 any 타입을 피하면서 다른 입력과 함께 재사용할 수 있습니다.
    ```jsx
    function identity<T>(input: T) {
        return input;
    }
    // 화살표함수
    const indentity = <T>(input: T) => input;
    ```

- 함수가 여러 개의 타입 매개변수를 선언하면 해당 함수에 대한 호출은 명시적으로 제네릭 타입을 모두 선언하지 않거나 모두 선언해야 합니다.
    ```jsx
    function makePair<Key, Value>(key: Key, value: Value) {
        return { key, value };
    }
    makePair("abc", 123); // OK. 타입 인수가 둘 다 제공되지 않음.
    makePair<string, number>("abc", 123); // OK. 두 개의 타입 인수가 제공됨.
    makePair<string>("abc", 123); // Error.
    ```
- 함수, 인터페이스, 클래스, 타입별칭 등에서 나중에 사용할 임의의 수의 타입 매개변수를 선언할 수 있다.
    ```jsx
    interface Box<T> {
        inside: T;
    }

    type Nullish<T> = T | null | undefined;
    ```
- 타입 매개변수는 동일한 선언 안의 앞선 타입 매개변수를 기본값으로 가질 수 있다. 모든 기본 타입 매개변수는 기본 함수 매개변수처럼 선언 목록의 제일 마지막에 와야 한다.
    ```jsx
    interface Quote<T = string> {
        value: T;
    }
    // Error
    function inTheMiddle<First, Second = boolean, Third = number, Fourth>() {}
    // OK
    function intheEnd<first, Second, Thrid = number, fourth = string>() {}
    ```
- 타입스크립트의 모범 사례는 필요할 때만 제네릭을 사용하고, 제네릭을 사용할 때는 무엇을 위해 사용하는지 명확히 해야 합니다.
- 표준 명명 규칙은 기본적으로 첫 번째 타입 인수로 T를 사용하고, 후속 타입 매개변수가 존재하면 U, V 등을 사용하는 것입니다. 타입이 사용되는 용도를 가리키는 설명적인 제네릭 타입 이름을 사용하는 것이 가장 좋습니다.
    ```jsx
    // 좀 더 명확합니다.
    function labelBox<Label Value>(label: Label, value: Value) {}
    ```

# 📚 11장 선언 파일
- .d.ts 선언 파일은 런타임 코드를 포함할 수 없다는 주목할 만한 제약 사항을 제외하고는 .ts 파일과 유사하게 작동합니다.
- declare 키워드는 선언 파일이 함수, 변수 같은 런타입 값을 생성하지 않을 수 있지만 이러한 구조체가 존재한다고 선언할 수 있다.
- declare 키워드로 변수를 선언하면 초깃값이 허용되지 않는다는 점을 제외하고 일반적인 변수 선언과 동일한 구문을 사용한다.
    ```jsx
    // types.d.ts
    declare let declared: string; // Ok
    declare let initializer: string = "Wanda"; // Error
    ```
- 인터페이스와 같은 타입 형태는 .d.ts 선언 파일에서 declare 키워드 유무와는 관계없이 허용되지만, 함수나 변수 같은 런타임 구문에 declare 키워드가 없다면 타입 오류가 발생한다.

# 📚 12장 IDE 기능 사용
- IDE 선호도 궁금합니다!
- 코드 품질 검사는 어떻게 하고 계신가요?