---
title: Javascript prototype
date: 2021-12-12 22:30:00 +0900
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
- 이 내용은 Javascript ES6+ 제대로 알아보기 - 초급 강의 중 Block scope 내용을 정리하였습니다.
- [강의링크](https://www.inflearn.com/course/ecmascript-6-flow/dashboard)

# Block scope
- Block에 의해 생기는 유효범위 (for, if, while....)
- var는 block scope에 영향을 받지 않고, let, const만 영향을 받는다.
- Block scope는 this bind를 하지 않는다. 즉 Block 밖에 있는 this와 동일하다.

## 예시
var는 Block scope의 영향을 받지 않으므로 이전에 정의된 10을 출력

```js
var  a = 10;
{
    console.log(`a`, a); // 10
    var a = 10;
}
```

const는 Block scope의 영향을 받고, 정의되기 전에 참조를 하면 reference Error가 난다. 이 구간을 TDZ라고 한다.

```js
const  a = 10;
{
    console.log(`a`, a); // reference Error
    const a = 10;
}
```

> TDZ: Temporal Dead Zone (임시사각지대) </br>
> Ecmascript에서 정의한 개념은 아님 let이나 const에서는 실제로 정의한 부분에부터 호출 이 가능
