### 📅 2023년 7월 5일

# 📚 8장 클래스

내용없음.

# 📚 9장 타입 제한자

- top 타입: 시스템에서 가능한 모든 값을 나타내는 타입. 모든 타입은 top 타입에 할당할 수 있다.
- any 타입은 모든 타입의 위치에 제공될 수 있다는 점에서 top 타입처럼 작동할 수 있다.
- 어떤 값이든 될 수 있음을 나타내려면 any보다 unknown 타입이 훨씬 안전하다.
## unknown
- 타입스크립트에서 unknown 타입은 진정한 top 타입.
- any와의 차이점은 타입스크립트는 unknown 타입의 값을 훨씬 더 제한적으로 취급한다.
- 타입스크립트가 unknown 타입에 접근할 수 있는 유일한 방법은 instanceof나 typeof 또는 타입 어서션을 사용하는 것처럼 값의 타입이 제한된 경우.
###

## keyof
- 타입스크립트에서는 기존에 존재하는 타입을 사용하고, 해당 타입에 허용되는 모든 키의 조합을 반환하는 keyof 연산자를 제공한다.
- keyof Ratings는 'audience' | 'critic'과 동일하지만, 작성하는 것이 훨씬 빠르고 Ratings 인터페이스가 변경되더라도 수동으로 업데이트할 필요가 없다.
- keyof는 존재하는 타입의 키를 바탕으로 유니언 타입을 생성하는 훌륭한 기능이다.

## typeof
- typeof는 제공되는 값의 타입을 반환한다. 값의 타입을 수동으로 작성하는 것이 짜증날 정도로 복잡한 경우에 사용하면 매우 유용하다.
- 자바스크립트의 typeof 연산자는 타입에 대한 문자열 이름을 반환하는 런타임 연산.
- 타입스크립트의 typeof 연산자는 타입스크립트에서만 사용할 수 있으며 컴파일된 자바스크립트 코드에 나타나지 않는다.

## keyof typeof
- typeof는 값의 타입을 검색하고, keyof는 타입에 허용된 키를 검색한다.
```jsx
const ratings = {
    imdb: 8.4,
    metacritic: 82,
}

function logRating(key: keyof typeof ratings) {
    console.log(ratings[key]);
}

logRating("imdb"); // OK

logRating("invalid");
// Error: Argument of type '"missing"' is not assignable
// to parameter of type '"imdb" | "metacritic"'.
```

## 타입 어서션

- 타입스크립트는 값의 타입에 대한 타입 시스템의 이해를 재정의하기 위한 구문으로 타입 어서션(타입 캐스트라고도 부름)을 제공한다.
- 다른 타입을 의미하는 값의 타입 다음에 as 키워드를 배치한다.
- 타입스크립트 모범 사례는 가능한 한 타입 어서션을 사용하지 않는 것이다.
- 그러나 타입 어서션이 유용하고 심지어 필요한 경우가 종종 있다.
    - 오류를 처리할 때 매우 유용할 수 있다. 코드 영역이 Error 클래스의 인스턴스를 발생시킬 거라 틀림없이 확신한다면 타입 어서션을 사용해 포착된 어서션을 오류로 처리할 수 있다.
    - 발생된 오류가 예상된 오류 타입인지를 확인하기 위해 instanceof 검사와 같은 타입 내로잉을 사용하는 것이 더 안전하다.
    - null 또는 undefined를 포함할 수 있는 변수에서 null과 undefined를 제거할 때 타입 어서션을 주로 사용한다.
    - non-null 어서션 !를 사용한다. null 또는 undefined가 아니라고 간주한다.
    ```jsx
    // 타입이 Date라고 간주됨
    maybeData as Data;
    maybeData!;
    ```
- any 타입과 마찬가지로 타입 어서션은 타입스크립트의 타입 시스템에 필요한 하나의 도피 수단이다. 사용하는 것이 안전하다고 확실히 확신할 때만 사용해야 한다.

## const 어서션
- 배열, 원시 타입, 값, 별칭 등 모든 값을 상수로 취급해야 함을 나타내는 데 사용한다.
- 원시 타입 대신 특정 리터럴을 사용하는 것이 더 구체적으로 값을 반환 가능.
```jsx
// 타입: () => string
const getName = () => "Maria Bamford";

// 타입: () => "Maria Bamford"
const getNameConst = () => "Maria Bamford" as const;
```
- 모든 멤버 속성은 readonly가 되고, 리터럴은 일반적인 원시 타입 대신 고유한 리터럴 타입으로 간주되며, 배열은 읽기 전용 튜플이 된다.