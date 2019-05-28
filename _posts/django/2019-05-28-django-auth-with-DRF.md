---
layout: post
title: Django, DRF의 CSRF validation
categories:
  - django
# feature_image: ""
tags:
  - DRF
  - CSRF
---
> django로 간단한 웹은 몇 번 만들어본 적이 있으나(혹은 만드는 것에 참여한) modelForm을 사용해본 적이 없었다. -----

django의 SessionAuthentication을 통해 DRF의 api를 사용하면 CSRF토큰을 반드시 포함해야 한다.(GET을 제외한 PUT, PATCH, POST, DELETE에 대해)    
그렇지 않으면 403 Forbidden response를 받게되는데.. 이게 .. 참재미있는게 오히려 AnonymousUser인 경우엔 상관이 없다. 즉, 로그인을 하면 403 error가 뜨고, 오히려 로그인하지 않으면 그냥 작동한다.


이유를 [Django REST framework document의 Authentication](https://www.django-rest-framework.org/api-guide/authentication/)에서 찾아보았다.
> RDF의 CSRF 검증은 django와 약간 다른 점이있다. 왜냐하면, session기반의 authentication과 non-session기반의 authentication을 같은 view에서 동시에 지원하기 위해서이다. 이는 오직 authenticated된 request에 대해서만 CSRF토큰을 필요로 하며, **anonymous request**에 대해서는 CSRF토큰을 검증하는 과정을 거치지 않는다는 것이다.

non-session기반의 authentication에서는 CSRF토큰의 검증과정을 왜 거치지 않는 것일까..?
- cookie에 csrftoken은 존재할텐데
- csrf토큰 검증이 session based authentication 유저가 csrf공격에 의해 의도치 않은 요청을 서버에 하게되는 것인데
- non-session기반 authentication은 ~~요청할 때 검증~~을 하므로 csrf토큰의 검증이 의미가 없는 것이 아닐까?

session based Authentication 상태에서 제대로 api가 동작하도록 하기 위해서 Ajax에 CSRF토큰을 함께 싣는 과정을 추가했다.

참고 : [djangoproject document - CSRF](https://docs.djangoproject.com/en/2.2/ref/csrf/)

- form을 사용하는 경우  
  ```html
    <form method="post">{% raw %}
    {% csrf_token %}    {% endraw %}
      ...
    </form>
  ```

- Ajax를 사용하는 경우  
```javascript
  // 1. 쿠키에서 csrftoken 가져오기
  const csrf_token = Cookies.get('csrftoken');
  // 2. request header에 'X-CSRFToken'에 넣어서 보내기
  let headers = {
          'Accept': 'application/json',
    'Content-Type': 'application/json',
    'X-CSRFToken' : csrf_token
  }
  $.ajax({
    headers: headers,
    url: '/api/todos/' + id + '/',
    type: 'GET',
    ...
```

- cf) Ajax를 사용하는 경우 + csrf토큰을 session에 저장하는 경우  
(CSRF_USE_SESSIONS = True)
```javascript
  // 0. DOM에  { % csrf_token % }을 통해 csrftoken을 hidden input으로 뿌림
  // 1. 세션을 통해 DOM에 뿌려진 csrftoken 가져오기
  const csrf_token = jQuery("[name=csrfmiddlewaretoken]").val();
  // 2. request header에 'X-CSRFToken'에 넣어서 보내기
  let headers = {
          'Accept': 'application/json',
    'Content-Type': 'application/json',
    'X-CSRFToken' : csrf_token
  }
  $.ajax({
    headers: headers,
    url: '/api/todos/' + id + '/',
    type: 'GET',
    ...
```
