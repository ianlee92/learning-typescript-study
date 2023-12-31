## ch. 13 구성 옵션

-> 이 챕터는 가볍게 후루룩 읽었습니다.

**13.4.2 resolveJsonModule**
<br/>
resolveJsonModule 컴파일러 옵션을 true로 설정하면 .json 파일을 읽을 수 있다. 이렇게 하면 .json 파일을 마치 객체를 내보내는 .ts 파일인 것처럼 가져오고 해당 객체의 타입을 const 변수인 것처럼 유추한다.

```
//activist.json
{
"activist": "Mary Astell"
}

//useActiist.ts
import {activist} from "./activist.json";
```

## ch. 14 구문 확장

오늘날 대부분의 타입스크립트 개발자들은 타입스크립트의 구문 확장 사용을 지양합니다.

- 런타임 구문 확장이 최신 버전 자바스크립트의 새로운 구문과 충돌할 수 있음<br/>
- 상위 집합 언어 코드를 사용하고 자바스크립트를 내보내는 트랜스파일러의 복잡성을 증가시킴<br/>

#### 14.2 실험적인 데코레이터

ECMA 스크립트에서는 아직 데코레이터를 승인하지 않았으므로 타입스크립트 버전 4.7.2에서는 기본적으로 데코레이터를 지원하지 않습니다. <br/>

-> 저자가 데코레이터를 사용하지 말라고 강권했지만 그냥 궁금해서 찾아봤습니다. 기존에는 --experimentalDecorators 플래그 없이는 데코레이터를 사용할 수 없었지만 올해 5월 16일 타입스크립트 5.0 소개를 보면 5.0부터는 데코레이터를 지원한다고 합니다. 실험적인 데코레이터 (experimental decorators)도 쭉 유지되지만 그 플래그가 있는 코드와 없는 코드는 타입검사와 출력이 다르게 작동한다고 하네요.

[참고](https://devblogs.microsoft.com/typescript/announcing-typescript-5-0/#decorators)

#### 14.3 열거형

열거형: 각 값에 대해 친숙한 이름을 사용한 객체에 저장된 리터럴 값 집합. enum 키워드로 시작해 객체 이름(일반적으로 파스칼 케이스), 그 다음 쉼표로 구분된 키를 포함한 {} 객체. 각 키는 초깃값 앞에 선택적으로 =를 사용할 수 있다. 열거형은 자바스크립트에서 이에 상응하는 객체로 컴파일된다.

**14.3.1 자동숫자값**

열거형의 멤버는 명시적인 초깃값을 가질 필요가 없다. 값이 생략되면 타입스크립트는 첫번째 값을 0으로 시작하고 각 후속 값을 1씩 증가시킨다. 숫잣값이 있는 경우, 명시적 값을 갖지 않는 모든 멤버는 이전 값보다 1 더 큰 값을 갖는다. 그러나 순서를 수정하면 기본 번호가 변경되기 떄문에 DB 같은 곳에서 열거형 값을 저장하고 있다면 순서를 변경하거나 항목을 제거할 때 주의해야 한다. <br/>

- 숫자 대신 문자열값을 사용할 수도 있다. 이 경우 읽기 쉽고 공유 상수의 별칭을 지정하는데 유용하지만 타입스크립트가 자동으로 계산할 수 없다.

#### 14.5 타입 전용 가져오기와 내보내기

타입스크립트의 트랜스파일러는 자바스크립트 런타임에서 사용되지 않으므로 파일의 가져오기와 내보내기에서 타입 시스템에서만 사용되는 값을 제거합니다.

## ch. 15 타입 운영

타입 운영 기능은 남용해선 안되지만 무척 유용합니다.

#### 15.1 매핑된 타입

매핑된 타입: 다른 타입을 가져 와서 해당 타입의 각 속성에 대해 일부 작업을 수행하는 타입. 키 집합의 각 키에 대한 새로운 속성을 만들어 새로운 타입을 생성. 특히 다른 타입에 대해 작동하고 멤버에서 제한자를 추가하거나 제거할 수 있을 때 몹시 유용하다.<br/>

**15.1.1 타입에서 매핑된 타입** <br/>

일반적으로 매핑된 타입은 존재하는 타입에 keyof 연산자를 사용해 키를 가져오는 방식으로 작동한다. 각 매핑된 타입 멤버 값은 동일한 키 아래에서 원래 타입의 해당 멤버 값을 참조할 수 있다. 또한 각 필드를 원본 타입에서 임의의 수의 다른 타입으로 어렵게 복사하는 대신, 한 번 멤버 집합을 정의한 다음 필요한 만큼 새로운 버전을 여러 번 다시 생성할 수 있음. <br/>

**15.1.2 제한자 변경**

```
interface Catlist {
name: string;
age: number;
}

type ReadonlyCatlist = {
readonly [K in keyof Catlist]?: Catlist[K];
}

// 다음과 같음
// readonly name?: string;
// readonly age?: number;

```

**15.1.3 제네릭 매핑된 타입** <br/>

제네릭 매핑된 타입은 데이터가 애플리케이션을 통해 흐를 때 데이터가 어떻게 변형되는지 나타날 때 유용하다. 예를 들어 애플리케이션 영역이 기존 타입의 값을 가져올 수는 있지만 데이터를 수정하는 것은 허용하지 않는 것이 좋다.<br/>

-> 이 부분이 개인적으로 무척 유용하다고 생각했습니다.

```
type MakeOptional<T> = {
[K in keyof T] ?: T[K]
}
```

#### 15.2 조건부 타입<br/>

조건부 타입은 순전히 boolean 로직의 일부. 각각은 어떤 타입을 취하고, 두 가지 가능한 결과 중 하나를 얻는다.<br/>

**15.2.1 제네릭 조건부 타입**

```
type CallableSetting<T> =
	T extends () => any
	? T
	:()=> T
```

**15.2.3 유추된 타입**<br/>

🥲 이 파트의 첫 예시가 이해가 안갑니다. 새로운 Item 타입의 배열인 경우 결과 타입은 Item이 되고 그렇지 않으면 T가 된다는데.. `type StringArrayItem = ArrayItems<string[]>;`의 타입이 왜 그냥 string인가요..?!
<br/>
사실 유추된 타입 파트 전체를 잘 이해하지 못했습니다.<br/>

#### 15.3 never

never 타입 애너테이션을 적절히 추가하면 타입스크립트가 타입 시스템에 맞지 않는 코드 경로를 더 공격적으로 탐지합니다.<br/>

**15.3.1 never와 교차, 유니언 타입**<br/>

- 교차 타입(&)에 있는 never는 교차타입을 never로 만든다.<br/>
- 유니언타입(|)에 있는 never는 무시된다.<br/>

특히 유니언 타입에서 never가 무시되는 동작은 값을 필터링 하는데 유용하다.<br/>

**15.3.2 never와 조건부 타입**<br/>

제네릭 조건부 타입은 일반적으로 유니언에서 타입을 필터링하기 위해 never를 사용한다. 유니언에서 never가 무시되기 때문에 제네릭 조건부의 결과는 never가 아닌 것이 됨.

#### 15.4 템플릿 리터럴 타입

문자열이 일부 문자열 패턴과 일치하는지 나타낼 때 템플릿 리터럴 타입을 사용하면 유용하다.

```
type Brightness = "dark" | "light";
type Color = "blue" | "green";

type BrightnessAndColor = '${Brightness}- ${Color}';
```

**15.4.1 고유문자열 조작 타입**

고유문자열 조작 타입은 객체 타입의 속성 키를 조작하는데 매우 유용하다.

```
type FormalGreeting = Capitalize<"hello."> //타입: "Hello."
```

**15.4.2 템플릿 리터럴 키** <br/>

템플릿 리터럴 키는 문자열 리터럴을 사용할 수 있는 모든 위치에서 사용 가능하다. 예를 들어 매핑된 타입의 인덱스 시그니처로 사용할 수도 있음.

```
type DataKey = "location" | "name" | "year";

type ExistenceChecks = {
[K in 'check${Capitalize<DataKey>}']: ()=> boolean;
}

// checkLocation: ()=> boolean;
```

**15.4.3 매핑된 타입 키 다시 매핑하기** <br/>

템플릿 리터럴 타입을 사용해 원래 멤버를 기반으로 매핑된 타입의 멤버에 대한 새로운 키를 생성할 수 있다.

```
interface DataEntry<T> {
key: T;
value: string;
}

type DataKey = "location" | "name" | "year";

type DataEntryGetters = {
[K in DataKey as 'get${Capitalize<K>}']: ()=> DataEntry<K>;
};

// getLocation: ()=> DataEntry<"location">;
```
