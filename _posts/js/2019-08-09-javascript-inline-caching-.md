---
layout: post
title: JS - 인라인캐싱을 고려한 최적화(Chrome only)
categories:
  - js
# feature_image: ""
tags: javascript, inline caching
---
> 크롬의 V8 자바스크립트 엔진에는 최적화를 위해 'inline caching'라는 기법을 사용한다.  
> [JavaScript essentials: why you should know how the engine works](https://www.freecodecamp.org/news/javascript-essentials-why-you-should-know-how-the-engine-works-c2cc0d321553/)와 [자바스크립트 엔진의 최적화 기법 (2) - Hidden class, Inline Caching](https://meetup.toast.com/posts/78)의 내용을 참고

## Inline caching
Inline caching은 반복문 내의 객체 접근 시 '조회'작업을 생략하게 함으로써 성능향상을 도모하는 기법이다

### 비슷한 두 코드의 엄청난 속도차이
```javascript
(() => {
     const han = {firstname: "Han", lastname: "Solo"};
     const luke = {firstname: "Luke", lastname: "Skywalker"};  
     const leia = {firstname: "Leia", lastname: "Organa"};  
     const obi = {firstname: "Obi", lastname: "Wan"};  
     const yoda = {firstname: "", lastname: "Yoda"};  
     const people = [
      han, luke, leia, obi, 
      yoda, luke, leia, obi];
     const getName = (person) => person.lastname;
  console.time("engine");
  for(var i = 0; i < 1000 * 1000 * 1000; i++) {
    getName(people[i & 7]);
  }
  console.timeEnd("engine");     
```

```javascript
(() => {
  const han = {firstname: "Han", lastname: "Solo", spacecraft: "Falcon"};
  const luke = {firstname: "Luke", lastname: "Skywalker", job: "Jedi"};
  const leia = {firstname: "Leia", lastname: "Organa", gender: "female"};
  const obi = {firstname: "Obi", lastname: "Wan", retired: true};
  const yoda = {lastname: "Yoda"};
  const people = [
    han, luke, leia, obi, 
    yoda, luke, leia, obi];
  const getName = (person) => person.lastname;
  console.time("engine");
  for(var i = 0; i < 1000 * 1000 * 1000; i++) {
    getName(people[i & 7]);
  }
  console.timeEnd("engine");
})();
```

### 왜?
- V8엔진의 인라인캐시는 최적화 과정에서 object의 attribute에 접근하는 부분에 실제 메모리 주소를 할당하여 lookup과정을 생략
- 즉 `person.lastname`의 attribute접근과정에서 해당 object에서 `lastname`이라는 attribute의 위치를 offset으로 저장하여, static하게 최적화 코드에 넣어버림
- 따라서 object의 구조가 동일해야 object를 구분하는 과정을 생략하고 offset을 적용하여 최적화에 용이하다.
