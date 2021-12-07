---
title: Typescript 기본타입, 함수, 인터페이스
date: 2021-12-04 12:50:00 +0900
categories: [Tip, Typescript]
tags: [typescript]
math: true
toc: true
mermaid: true
comments: true
image:
  width: 800
  height: 500
pin: true
---

# 타입스크립트 기본 타입
타입스크립트에는 12가지 기본타입이 있다.

## String
문자열을 나타냄

```ts
let str: string = 'hi';
```

## Number
숫자를 나타냄

```ts
let num: number = 10;
```

## Boolean
참/거짓을 나타냄

```ts
let isLoggedIn: boolean = false;
```

## Object
이름과 값으로 구성된 프로퍼티의 집합

```ts
let info = {name : 'typescript', age: 10};
```

### Array
데이터의 집합

```ts
let arr: number[] = [1,2,3];
// or
let arr: Array<number> = [1,2,3];
```

### Tuple
배열의 길이가 고정되고 각 요소의 타입이 지정되어 있는 배열 형식

```ts
let arr: [string, number] = ['hi', 10];
```

### Enum
특정 값(상수)들의 집합

```ts
enum Animal {Dog, Cat, Cow}
let animal: Animal = Animal.Dog;
// animal is '0'
```

## Any
모든 타입에 대해서 허용한다는 의미

```ts
let str: any = 'hi';
let num: any = 10;
let arr: any = ['a', 2, true];
```


## Void
변수에는 undefined와 null만 할당하고, 함수에는 반환 값을 설정할 수 없는 타입

```ts
let unuseful: void = undefined;

function notuse(): void {
  console.log('no return');
}

```

## Null
어떤 값이 의도적으로 비어있음을 표현하며 boolean 연산에서는 거짓으로 취급
```ts
let hobby: string = null;
```

## Undefined
정의되지 않은 값을 의미

```ts
// 정의되지 않은 human을 log로 찍고 있는 모습
console.log(human);
```

## Never
함수의 끝에 절대 도달하지 않는다는 의미를 지닌 타입
```ts
function neverEnd(): never {
  while (true) {

  }
}
```

# 타입스크립트 함수

## 함수의 기본적인 선언
기존 자바스크립트 함수의 선언 방식에서 매개변수와 함수의 반환 값에 타입이 추가됨.

**JS**
```js
function sum(a, b) {
  return a + b;
}
```

**TS**
```ts
function sum(a: number, b: number): number {
  return a + b;
}
```

`?`를 이용하면 해당 매개변수를 넘기지 않아도 된다.
만약 넘기지 않았다면 b의 값은 `undefine` 가 된다.

```ts
function sum(a: number, b?: number): number {
  return a + b;
}
```

다음과 같이 기본값을 설정할 수 있다.
매개변수를 넘기지 않았을 시, 설정된 기본값이 된다.
```ts
function sum(a: number, b=100): number {
  return a + b;
}
```

ES6 문법에서 지원하는 REST 문법은 다음과 같이 사용될 수 있다.
```ts
function sum(a: number, ...nums: number[]): number{
    let totalOfNums = 0;

    for (let key in nums) {
        totalOfNums += nums[key];
    }
    return a + totalOfNums;
}
sum(1,2,3,4,5);
```

# 인터페이스
인터페이스는 상호 간에 정의한 약속 혹은 규칙을 의미한다.

- 객체의 스펙(속성과 속성의 타입)
- 함수의 파라미터
- 함수의 스펙(파라미터, 반환 타입 등)
- 배열과 객체를 접근하는 방식
- 클래스


## 객체 선언과 관련된 타입 체킹
person interface에 맞는 object만 logName의 인자로 올 수 있다.
```ts
interface person {
    age?: number; // 생략가능
    name: string;
    readonly note: string; //읽기 전용
    [propName: string]: any; // 인터페이스에서 정의되지 않은 속성을 사용하고 싶을때
}

function logName(obj: person) {
    console.log(`obj.name`, obj.name);
    console.log(`obj.money`, obj.money);
    
}

let person = {name: 'Tom', age:33, note:'good person', money:100000};
logName(person);
```

## 함수 타입
함수의 타입을 정할 때에도 사용 가능
```ts
interface login {
    (username: string, password: string): boolean;
  }

let loginUser: login;
loginUser = function(id: string, pw: string) {
  console.log('로그인 했습니다');
  return true;
}
```

## 클래스 타입
클래스가 일정 조건을 만족하도록 규칙을 정할 수 있음
```ts
interface CraftBeer {
  beerName: string;
  nameBeer(beer: string): void;
}

class myBeer implements CraftBeer {
  beerName: string = 'Baby Guinness';
  nameBeer(b: string) {
    this.beerName = b;
  }
  constructor() {}
}
```

## 인터페이스 확장
클래스와 마찬가지로 인터페이스도 확장가능
```ts
interface Person {
  name: string;
}
interface Developer extends Person {
  skill: string;
}
let fe = {} as Developer;
fe.name = 'josh';
fe.skill = 'TypeScript';
```


# 참고
- [타입스크립 핸드북](https://joshua1988.github.io/ts/intro.html)
- [spread 와 rest](https://learnjs.vlpt.us/useful/07-spread-and-rest.html)
