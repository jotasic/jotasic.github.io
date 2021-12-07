---
title: Typescript 연산자를 이용한 타입정의
date: 2021-12-04 23:25:00 +0900
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

# 연산자를 이용한 타입정의

## Union Tpye
유니온 타입(Union Type)이란 자바스크립트의 OR 연산자와 같이 A이거나 B이다 라는 의미의 타입이다.

이처럼 any를 사용하는 경우 마치 자바스크립트로 작성하는 것처럼 동작을 하고 유니온 타입을 사용하면 타입스크립트의 이점을 살리면서 코딩할 수 있다.

```ts
function logText(age: string | number) {
    if(typeof age === 'number'){
        age.toFixed();
        return age;
    }
    else if (typeof age === 'string'){
        return age;
    }
    return new TypeError('age must be number or string');
}

```

### Union Type을 쓸 때 주의할 점

아래와 같이 interface를 이용해서 union type을 사용할때 타입스크립트 관점에서는 함수를 호출하는 시점에 Person도 되고, Developer도 될 수 있으니 이 둘다 포함되는 `age`만 접근 가능하게 된다.

즉  어느 타입이 들어오든 간에 오류가 안 나는 방향으로 타입을 추론하게 됩니다.
```ts
interface Person {
  name: string;
  age: number;
}
interface Developer {
  name: string;
  skill: string;
}
function introduce(someone: Person | Developer) {
  someone.name; // O 정상 동작
  someone.age; // X 타입 오류
  someone.skill; // X 타입 오류
}
```

## Intersection Type
인터섹션 타입(Intersection Type)은 여러 타입을 모두 만족하는 하나의 타입을 의미한다.

```ts
interface Person{
    name: string;
    age: number;
}

interface Developer{
    name: string;
    skill: number;
}

//type Capt = Person & Developer;
//or
const test :Person & Developer = {
    name : 'Tom',
    age: 20,
    skill: 100
}

```

# 참고
- [타입스크립 핸드북](https://joshua1988.github.io/ts/intro.html)