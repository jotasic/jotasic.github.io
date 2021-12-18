---
title: Javascript Iterable, iterator, generator
date: 2021-12-18 21:00:00 +0900
categories: [강의정리, Javascript ES6+ 제대로 알아보기 - 중급]
tags: [javascript]
math: true
toc: true
mermaid: true
comments: true
image:
  width: 800
  height: 500
pin: true
---

# 들어가며
- 이 내용은 Javascript ES6+ 제대로 알아보기 - 중급 강의 중 Iterable, iterator, generator를 정리한 내용 입니다.
- [강의링크](https://www.inflearn.com/course/es6-2/dashboard)



# Iterable
내부 요소들을 공개적으로 탐색(반복) 할 수 있는 데이터 구조로 `[Symbol.iterator]`메소드를 가지고 있다.

`[Symbol.iterator]`를 실행한 결과를 가지고, `next()`를 반복 호출하는 로직을 기반으로 `Array.from`, `spread operator`, `for .. of` 등의 기능들을 실행한다.

기본적으로 내장하고 있는 객체는 array, map, set, string 등이 있다.

사용자가 규칙에 맞게 `[Symbol.iterator]`를 구현하면 `Iterable`한 객체가 될 수 있다.

이 외에 Generator도 다른 방식으로 구현되어 있지만 `Iterable`한 객체이다.

# Iterator
반복을 위해 설계뙨 특별한 인터페이스를 가진 객체
- 객체 내부에는 `next()` 메소드가 있는데, 이 메소드는 `value`와 `done` 프로퍼티를 지닌 객체를 반환한다.
- done 프러퍼티는 boolean 값 이다.
- 이러한 조건을 가진 객체를 반환하도록 객체내 `[Symbol.iterator]`메소드를 정의해주면 해당 객체는 `Iterable`한 객체가 된다.
- done 프러퍼티가 true가 될 때까지 `next()`메소드를 호출한다. 따라서 done 프로퍼티가 true가 되지 않는 조건으로 Iterator를 만들면 안된다.

# Generator
- 중간에서 멈췄다가 이어서 실행할 수 있는 함수.
- function 키워드 뒤에  `*`를 붙여 표현하며, 함수 내부에는 `yield` 키워드를 활용한다.
- 함수 실행 결과에 대해 `next()` 메소드를 호출할 때마다 순차적으로 제너레이터 함수 내부의 `yield` 키워드를 만나기 전까지 실행하고, yield 키워드에서 일시정지한다.

```js
const generator = function* () {
    yield 1;
    yield 2;
    yield 3;

}

const gen = generator();
console.log(`gen()`, gen.next()); // {value:1, done: false}
console.log(`gen()`, gen.next()); // {value:2, done: false}
console.log(`gen()`, gen.next()); // {value:3, done: false}
console.log(`gen()`, gen.next()); // {value:undefine, done: true}
```

## generator를 이용한 iterator 구현
 `[Symbol.iterator]`를 generator로 만들면 yield만 신경쓰면 된다.
 iterator의 조건은 next() 메소드를 가지고, 그 메소드는 done과 value를 반환하는 객체이기 때문에 가능하다.
 
 ```js
 // 1. generator로 구현
 const obj = {
    *[Symbol.iterator]() {
        for(let count=1 ; count <= 3 ; count++) {
            yield count;
        }
    }
}

// 2. iterator객체를 직접 구현
const iter = {
    count: 1,
    next() {
        const done = this.count > 3;
        return {
            done,
            value: !done? this.count++ : undefined
        }
    }
}

const obj = {
    [Symbol.iterator](){
        return iter;
    }
}
console.log(`obj`, [...obj]);
 ```

## yield*
Generator내에서 사용되며, Iterable한 객체의 값들을 yield를 차례대로 호출한 형태의 의미이다.

```js
function* gen() {
    const arr = [1,2,3,4,5];
    yield* arr;
    /*
    아래와 같다
    yield 1
    yield 2
    yield 3
    yield 4
    yield 5
    */
}
```