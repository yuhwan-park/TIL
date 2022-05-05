# 타입스크립트

## 타입스크립트 개요

타입스크립트를 사용하면 동적 타입을 정적으로 선언하여 컴파일 시점에서 타입 에러를 방지할 수 있다. 그리고 타입스크립트는 타입 추론(타입 유추)을 제공하기 때문에 자바스크립트로 작성된 코드도 어느 정도 타입을 유추해 타입 제어가 가능하다. 또한 자바스크립트에서 찾을 수 없는 인터페이스나 제네릭 같은 추가 기능을 제공한다.

## 타입스크립트의 타입과 타입 선언 시스템

- 기본 자료형
  - string: 문자열
  - boolean : 참 / 거짓
  - number : 숫자
  - null : 의도적으로 비어있는 값
  - undefined : 아무 값이 할당되지 않은 상태
- 참조 자료형
  - object : 기본 자료형 외의 타입
  - array : 배열
- 추가 자료형\*

  - tuple : 길이와 각 요소의 타입이 정해진 배열
  - enum : 특정 값들의 집합
  - any : 모든 타입을 저장 가능
  - void : 결과 값을 반환하지 않는 함수의 타입
  - never : 항상 오류를 발생시키거나 반환이 없는 함수의 타입

- 유니언 타입

```ts
function printId(id: number | string) {
  console.log("Your ID is: " + id);
}
```

서로 다른 두 개 이상의 타입들을 사용하여 만드는 것으로, 유니언 타입의 값은 타입 조합에 사용된 타입 중 무엇이든 하나를 타입으로 가질 수 있다.

- 타입 별칭 (Type ailas)

```ts
type Point = {
  x: number;
  y: number;
};

function printCoord(pt: Point) {
  console.log("The coordinate's x value is " + pt.x);
  console.log("The coordinate's y value is " + pt.y);
}

printCoord({ x: 100, y: 100 });
// -------------------

// 유니언 타입에도 타입 별칭을 부여할 수 있다.
type ID = number | string;
```

똑같은 타입을 한 번 이상 재사용하거나 또 다른 이름으로 부르고 싶을 때 타입에 이름을 붙일 수 있다.

- 인터페이스

```ts
interface Point {
  x: number;
  y: number;
}

function printCoord(pt: Point) {
  console.log("The coordinate's x value is " + pt.x);
  console.log("The coordinate's y value is " + pt.y);
}

printCoord({ x: 100, y: 100 });
```

인터페이스 선언은 객체 타입을 만드는 또 다른 방법이다.

[타입 별칭과 인터페이스의 차이점?](https://www.typescriptlang.org/ko/docs/handbook/2/everyday-types.html#%ED%83%80%EC%9E%85-%EB%B3%84%EC%B9%AD%EA%B3%BC-%EC%9D%B8%ED%84%B0%ED%8E%98%EC%9D%B4%EC%8A%A4%EC%9D%98-%EC%B0%A8%EC%9D%B4%EC%A0%90)

- 타입 단언

```ts
const myCanvas = document.getElementById("main_canvas") as HTMLCanvasElement;
```

어떤 값의 타입에 대한 정보가 명확할 때 as 키워드를 이용하여 타입을 좀 더 구체적으로 명시할 수 있다. 이는 사용자가 타입을 고정하는 것으로 해당 타입이 오는 것이 확실치 않다면 지양하는 것이 좋다.

```ts
function liveDangerously(x?: number | undefined) {
  // 오류 없음
  console.log(x!.toFixed());
}
```

표현식 뒤에 !를 작성하면 해당 값이 null 또는 undefined가 아니라고 타입을 단언할 수 있다. 이 또한 확실치 않다면 에러를 유발할 수 있기 때문에 확실할 때만 사용하는게 좋다.

- 리터럴 타입

```ts
function printText(s: string, alignment: "left" | "right" | "center") {
  // ...
}
printText("Hello, world", "left");
printText("G'day, mate", "centre"); // ERROR : Argument of type '"centre"' is not assignable to parameter of type '"left" | "right" | "center"'.
```

- 유틸리티 타입 (Utility Type)
  - `Partial<T>` : T의 프로퍼티를 선택적으로 구성할 수 있습니다.
  - `Readonly<T>` : T의 프로퍼티를 읽기 전용으로 설정하여, 값을 재할당하는 경우 에러가 발생합니다.
  - `Record<T, K>` : 프로퍼티 키를 K, 값을 T로 하는 타입을 만들 수 있습니다.
    - 여러 키들을 지정하기 위해 타입을 선언한 방식`(type Page = 'home' | 'about' | 'contact';)`처럼 두 개 이상의 타입을 선언하는 방식을 유니온 타입이라고 합니다.
  - `Pick<T, K>` : T 타입 중에서 K 프로퍼티만 지정하여 타입을 만들 수 있습니다.
  - `Omit<T, K>` : T 타입의 모든 프로퍼티 중 K를 제거하여 타입을 구성합니다.
  - `Exclude<T, U>` : 타입 T에서 U와 겹치는 타입을 제외한 타입을 구성합니다.
  - `Extract<T, U>` : 타입 T에서 U와 겹치는 타입만 포함하여 타입을 구성합니다.
  - `NonNllable<T>` : T 타입에서 null과 undefined를 제외한 타입을 구성합니다.
  - `Parameter<T>` : 함수 타입 T의 매개변수의 타입들의 튜플로 타입을 구성합니다.
  - `ConstructorParameters<T>` : 클래스의 생성자를 비롯한 생성자 타입의 모든 매개변수 타입을 추출합니다.
  - `ReturnType<T>` : 함수 T가 반환한 타입으로 타입을 구성합니다.
  - `Required<T>` : 타입 T의 모든 프로퍼티가 필수로 설정된 타입을 구성합니다.

## 참조

- https://www.typescriptlang.org/ko/docs/handbook/2/everyday-types.html
