### References
- https://engineering.linecorp.com/ko/blog/monorepo-with-turborepo/
- https://react.vlpt.us/using-typescript/05-ts-redux.html
- https://stackoverflow.com/questions/37233735/interfaces-vs-types-in-typescript
- https://engineering.linecorp.com/ko/blog/typescript-enum-tree-shaking
- https://www.typescriptlang.org/docs/handbook/release-notes/typescript-3-4.html#const-assertions

### 인덱스 시그니처
- 객체의 키값으로 동적인 값이 올 때, 그 동적인 키에 해당하는 값의 타입을 정의해줄 때 사용하는 타입으로 보자.
```jsx
interface Example {
  [key: string]: number;
}

const obj: Example = {
  a: 1,
  b: 2,
  c: 3,
};
```
```jsx
interface Example {
  a: number
  b: number
  c: number
}
```
이렇게 정적으로 속성명을 적지 않아도 동적인 키 값에 대한 타입을 대응할 수 있어서 사용한다.
