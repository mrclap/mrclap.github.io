---
layout: post
title: Angular, React 그리고 Vue
categories:
  - react 
# feature_image: ""
tags:
---
>얼마 전, Draggable UI를 적용하여 Todo list를 관리하는 아주 작은 웹앱을 만들었다. 그리고 멘붕을 겪었다. HTML의 DOM과 Javascript의 데이터를 함께 관리하자니 복잡도가 업청나게 올라가는 경험을 한 것이다. 그래서 React를 한 번 적용해봤다. Javascript로 바로 DOM을 그려버리는 것이 처음에는 좀 복잡해도 HTML과 DOM의 데이터를 sync하는 것 보다 간단했다. React를 좀 더 알아보자니, Angular, Vue 등 프레임워크가 몇 개 더 있는 것 같은데, 이들의 차이를 좀 더 자세히 공부해보자.

### Angular, React, Vue는 무엇인가?
`프론트엔드 프레임워크`이다. 즉, 브라우저를 통해 보게되는 화면과 화면을 위한 비지니스 로직을 구성하는데에 사용한다.  

프레임워크라는 것이 그러하듯, 정해진 규칙에 맞춰 작성한 코드가 가성비 좋게(작성한 코드의 양에 비해 많은) 기능을 구현해낸다. 프론트엔드 프레임워크 뼈대는 무엇일까? 바로 Javascript다. 잘 생각해보면 Javascript일 수 밖에 없다. 브라우져가 담아낼 수 있는 3개의 언어 HTML, CSS, Javascript 중 다른 녀석에게 간섭을 할 수 있는 것이 Javascript뿐이다. 그렇다, 프론트엔드 프레임워크는 브라우져가 Javascript를 통해 화면을 '주도적'으로 그려나가도록 한다.

<!-- ~~만일 HTML로 프론트엔드 프레임워크를 만들어낸다면 프레임워크 사용을 위한 가장 중요한 스킬은 'cmd+c, cmd+v'가 될 것이다.~~ -->

**Angular, React, Vue**는 브라우져가 Javascript를 통해 DOM을 그려내도록하는 프론트엔드 프레임워크다. 

### Angular, React, Vue는 왜 필요한가?
예전의 웹페이지는 HTML과 CSS로 이루어진 아주 정적인 형태였으며, 정보의 변화나 기능을 수행하기 위해서는 url을 통해 다른 페이지로 **이동**하였다. 하지만 SPA로 불리우는 Single Page Application

1) SPA를 만드는데 이만 한 게 없다.
2) 상태(데이터)와 뷰(HTML) DOM의 동기화에 용이
3) component의 강력한 재사용성
  - HTML COPY가 아니다

_ _ _ 
위 3개의 라이브러리가 널리 사용되는 데에는 브라우저의 성능 향상이 지대한 영향을 미쳤다. 웹 페이지는 브라우저의 비약적인 성능 향상에 힘입어 마음껏 무겁고, 복잡해졌다.

그렇다면 왜 우리는 위와 같은 Javascript 프론트엔드 프레임워크를 필요로할까? 
- 그렇다! 아래의 모든 내용을 종합하면 물론 시간을 절약할 수 있다!
- 상태(데이터)와 뷰(HTML) DOM의 동기화에 용이하다.
- 