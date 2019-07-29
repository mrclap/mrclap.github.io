---
layout: post
title: Django, view class
categories:
  - django
# feature_image: ""
tags:
  - 
---
> Django를 이용하여 MVC패턴의 웹페이지를 만들게되면 Spring의 Controller와 같은 역할을 하는 것이 바로 View이다. 보통은 FBV(Function Based View)를 이용하지만 미리 정의해둔 Model을 기반으로 List와 Detail페이지를 구현하는 경우 CBV를 이용하면 보다 편리하게 구현이 가능하다.

[Django2.2 Document](https://docs.djangoproject.com/en/2.2/ref/class-based-views/base#django.views.generic.base.View.http_method_not_allowed)

## Basic Views

### View 클래스
Django의 View클래스는 다른 모든 class-based view들의 부모클래스가 된다. 

- 아래와 같은 흐름으로 메서드들이 실행된다
  1) setup()
  2) dispatch()
  3) http_method_not_allowed()
  4) options()


- 아래와 같이 상속받아 사용한다
  ```python
  from django.http import HttpResponse
  from django.views import View

  class MyView(View): # <--- 상속

      def get(self, request, *args, **kwargs):
          return HttpResponse('Hello, World!')
  ```

- urls.py
  ```python
  from django.urls import path

  from myapp.views import MyView

  urlpatterns = [
      path('mine/', MyView.as_view(), name='my-view'),
  ]
  ```

- Methods
  - *(classmethod)* as_view(**initkwargs)
    - request를 받아 view형태의 response를 반환한다.
    - View클래스가 requets/response사이클에 들어가게되면, setup() 메서드는 받은 HttpRequest를 View의 request로 보내고, 다른 url로부터 받아내는 값들은 args와 kwargs로 보낸다.
    - 이후에 dispatch()를 호출한다.

  - setup(request, *args, **kwargs)
    - view클래스의 attribute인 self.request, self.args, self.kwargs를 초기화
    - 위 attributes의 초기화를 mixins하기 위해 override 할 수 있으며, super()의 호출이 반드시 필요하다.
  
  - dispatch(request, *args, **kwargs)
    - requets의 http method를 확인하여 알맞는 method를 매칭한다.
      - GET -> get() / POST -> post() 등
      - HEAD request의 경우 기본적으로는 get()에 할당하므로 다른 메서드로의 매칭을 원하는경우 head() 메서드를 override한다.

  - http_method_not_allowed(request, *args, **kwargs)
    - 지원하지않는 http method의 request를 받은 경우 실행된다. 
    - 기본적으로 HttpResponseNotAllowed와 함께 사용가능한 http method list를 반환한다.

  - options(request, *args, **kwargs)
    - http method의 options에 대응한다. 사용가능한 http method 리스트를 반환한다.


### TemplateView 클래스
template을 지정하여 render할 수 있다.

- `template_name`에 템플릿파일을 할당하여 지정한다
  ```python
  from django.views.generic.base import TemplateView

  from articles.models import Article

  class HomePageView(TemplateView):

      template_name = "home.html" # <--- 여기에서 지정한다.

      def get_context_data(self, **kwargs):
          context = super().get_context_data(**kwargs)
          context['latest_articles'] = Article.objects.all()[:5]
          return context
  ```


### RedirectView 클래스
주어진 url로 redirect시킨다.

- `pattern_name 혹은 url'에 redirect할 경로를 할당한다.
  - article에 접근하는 횟수를 count하는 redirectView를 구현
  ```python
  from django.shortcuts import get_object_or_404
  from django.views.generic.base import RedirectView

  from articles.models import Article

  class ArticleCounterRedirectView(RedirectView):

      permanent = False
      query_string = True
      pattern_name = 'article-detail'

      def get_redirect_url(self, *args, **kwargs):
          article = get_object_or_404(Article, pk=kwargs['pk'])
          article.update_counter()
          return super().get_redirect_url(*args, **kwargs)
  ```

- Attributes
  - url
    - redirect할 url, None일 경우 410 HTTP Error발생
  - pattern_name
    - name of URL pattern to redirect(in urls.py)
  - permanent 
    - True : http status code is **301**
    - False : http status code is **302**(default)
    - 검색엔진의 경우 301 response를 받은 경우 redirect된 url을 검색에 반영
  -  query_string
    - GET query 스트링을 그대로 전달할 것인지 결정

- method
  - get_redirect_url(*args, **kwargs)
    - 지정한 url(혹은 pattern_name)으로 redirect 수행


## generic display Views
DetailView, ListView를 통해서 정의된 Model의 데이터를 간단히 가져올 수 있음

### DetailView 클래스

- `model = 모델클래스`를 통해 모델을 설정
  ```python
  from django.utils import timezone
  from django.views.generic.detail import DetailView

  from articles.models import Article

  class ArticleDetailView(DetailView):

      model = Article

      def get_context_data(self, **kwargs):
          context = super().get_context_data(**kwargs)
          context['now'] = timezone.now()
          return context
  ```

- `object`객체를 통해 Model의 값을 template에서 사용가능
  ```python
  {% raw %}
  <h1>{{ object.headline }}</h1>
  <p>{{ object.content }}</p>
  <p>Reporter: {{ object.reporter }}</p>
  <p>Published: {{ object.pub_date|date }}</p>
  <p>Date: {{ now|date }}</p>
  {% endraw %}
  ```

- Single object mixins의 get_object를 통해 query_set에서 pk로 해당 data를 찾아옴

### ListView 클래스

- `paginate_by = 100`을 통해 페이지네이션도 가능
  ```python
  from django.utils import timezone
  from django.views.generic.list import ListView

  from articles.models import Article

  class ArticleListView(ListView):

      model = Article
      paginate_by = 100  # if pagination is desired

      def get_context_data(self, **kwargs):
          context = super().get_context_data(**kwargs)
          context['now'] = timezone.now()
          return context
  ```