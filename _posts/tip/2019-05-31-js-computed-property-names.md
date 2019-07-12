---
layout: post
title: ES6, 객체 선언 시 key값에 계산식 혹은 속성접근자를 사용하는 법
categories:
  - tip
# feature_image: ""
tags:
  - computed property
---
> JS에서 Object선언시 키값은 단일 변수 혹은 리터럴 값을 사용해서만 가능했다. 하지만 ES6에서는 [] 브라켓을 이용해서 계산식 혹은 속성접근자를 자유롭게 사용할 수 있다.

```javascript
handleChange = (e) => {
  this.setState({e.target.name : e.target.value})
} // key에서 에러가 난다.

// 변수를 지정하여 해결 할 수 있지만
let name = e.target.name
handleChange = (e) => {
  this.setState({name : e.target.value})
} 

// []을 사용하면 간단한 표현식 혹은 속성 접근자의 경우 더 가독성이 좋다.
handleChange = (e) => {
  this.setState({[e.target.name] : e.target.value})
} 
```
