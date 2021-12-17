---
title: Javascript Arrow Function
date: 2021-12-13 17:10:00 +0900
categories: [강의정리, Javascript ES6+ 제대로 알아보기 - 초급]
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
- 이 내용은 Javascript ES6+ 제대로 알아보기 - 초급 강의 중 Arrow Function 내용을 정리하였습니다.
- [강의링크](https://www.inflearn.com/course/ecmascript-6-flow/dashboard)


## Arrow function 표기법
```js
const 함수명 = (인자) => {
  //함수 구현부
}
```

### Arrow function 축약

1. 구현부가 1줄이며, 리턴을 해야되는 값이라면, `{}` 및 `return`를 생략할 수 있다.
    ```js
    // var fa = function() {
    //     return new Date();
    // }
    const fa = () => new Date();
    ```

2. 다만 리턴해야되는 값이 객체 선언일때, `{}`는 함수의 구현부로 인식하므로 `()`로 객체를 감싸야 된다.
    ```js
      const fo = () => ({a:10, b:20});
    ```

3. 인자가 1개면 `()`를 생략할 수 있다.
    
    ```js
    // var fb = function(a) {
    //     return a * a;
    // }
    const fb = a => a * a;

    ```

## 기존 함수 선언과 차이점
- this를 바인딩 하지 않는다. 즉 this의 값은 `scope chain`에 의해서 결정된다. 반대로 말하면 this를 `call`이나 `apply`를 이용해서 설정 이나 변경을 할 수 없다.
- prototype이 존재하지 않는다. 즉 arrow function는 `함수로써만` 사용하도록 설계되었다.