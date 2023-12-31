# [러닝 타입스크립트] Chapter.8 ~ Chapter.10 정리

## 정리

### 클래스 메서드 (p.152)

```markdown
타입스크립트는 독립 함수를 이해하는 것과 동일한 방식으로 메서드를 이해합니다. 매개변수 타입에 타입이나
기본값을 지정하지 않으면 any 타입을 기본으로 갖습니다.

클래스 생성자는 매개변수와 관련하여 전형적인 클래스 메서드처럼 취급됩니다.
```

### 클래스 속성 (p.153)

```markdown
타입스크립트에서 클래스의 속성을 읽거나 쓰려면 클래스에 명시적으로 선언해야 합니다. 클래스 속성은 인터페이스와
동일한 구문을 사용해 선언합니다. 클래스 속성 이름 뒤에는 선택적으로 타입 애너테이션이 붙습니다.

타입스크립트는 생성자 내의 할당에 대해서 그 멤버가 클래스에 존재하는 멤버인지 추론하려고 시도하지 않습니다.
```

### any 다시 보기 (p.180)

```markdown
any 타입은 top 타입처럼 작동할 수 있습니다. any는 일반적으로 console.log의 매개변수와 같이 모든
타입의 데이터를 받아들이는 위치에서 사용합니다.

any는 타입스크립트가 해당 값에 대한 할당 가능성 또는 멤버에 대해 타입 검사를 수행하지 않도록 명시적으로
지시한다는 문제점을 갖는다.

어떤 값이든 될 수 있음을 나타내려면 unknown 타입이 훨씬 안전합니다.
```

## 새롭게 알게된 부분

### 함수 속성 (p.154)

```markdown
메서드 접근 방식 (`myFunction(){}`)
- 함수를 클래스 프로토타입에 할당하므로 모든 클래스 인스턴스는 동일한 함수 정의를 사용

값이 함수인 속성을 선언하는 방식 (`myFunction:() => {}`)
- 클래스 인스턴스당 새로운 함수가 생성되며, 항상 클래스 인스턴스를 가리켜야 하는 화살표 함수에서 this 스코프를
사용하면 클래스 인스턴스당 새로운 함수를 생성하는 시간과 메모리 비용 측면에서 유용할 수 있다.

함수 속성은 클래스 멤버로 할당된 값이고 그 값은 함수이다.
```

## 다시 찾아보면 좋을 것 같은 부분

### 엄격한 초기화 검사 (p.155 ~ p.156)

```tsx
class MissingInitializer {
  property: string
}

new MissingInitializer().property.length
// TypeError: Cannot read property 'length' of undefined
```

- 엄격한 초기화 검사가 없다면 비록 타입 시스템이 undefined 값에 접근할 수 없다고 말할지라도, 클래스 인스턴스는 undefined 값에 접근할 수 있습니다.
- 십억 달러의 실수가 다시 발생

### 타입으로서의 클래스 (p.160)

```markdown
타입스크립트는 클래스의 동일한 멤버를 모두 포함하는 모든 객체 타입을 클래스에 할당할 수 있는 것으로 간주합니다.
타입스크립트의 구조적 타이핑이 선언되는 방식이 아니라 객체의 형태만 고려하기 때문입니다.
```

### 재정의된 속성 (p.171)

```markdown
속성이 유니언 타입의 허용된 값 집합을 확장할 수는 없습니다.
```

```tsx
class NumericGrade {
  value = 0
}

class VagueGrade extends NumericGrade {
  value = Math.random() > 0.5 ? 1 : '...' // Error: 'number'
}
```

- extends 했을 때, 타입 좁히기만 가능

### top 타입과 bottom 타입 (p.179)

```markdown
top 타입
- 시스템에서 가능한 모든 값을 타타내는 타입

bottom 타입
- 값을 가질 수 없고 참조할 수 없는 타입
```

### unknown 타입(p.180 ~ p.181)

```markdown
unknown 타입은 진정한 top 타입입니다. 모든 객체를 unknown 타입의 위치로 전달할 수 있다는 점에서 any 타입과 유사합니다.
unknown 타입과 any 타입의 주요 차이점으로는 타입스크립트는 unknown 타입의 값을 훨씬 더 제한적으로
취급한다는 점입니다.
```

- 타입스크립트는 unknown 타입 값의 속성에 직접 접근할 수 없다
- unknown 타입은 top 타입이 아닌 타입에는 할당할 수 없다.

### unknown 타입의 추론

```markdown
unknown 타입에 접근할 수 있는 유일한 방법은 instanceof나 typeof 또는 타입 어설션을 사용하는 것처럼
값의 타입이 제한된 경우이다.
```

## 궁금한 부분

### Q. 저자는 왜 이렇게 생각하고 이 부분을 넣었을까? (p.151)

```markdown
저는 일부 기능적 장치인 클래스를 절대 사용하지 않으려고 합니다. 저에게는 클래스가 너무 강렬해요!
```

### Q. 존재하는 멤버인지 추론하지 않는데 왜 할당시, 에러가 발생하는가? (p.153)

```markdown
타입스크립트는 생성자 내의 할당에 대해서 그 멤버가 클래스에 존재하는 멤버인지 추론하려고 시도하지 않습니다.
```