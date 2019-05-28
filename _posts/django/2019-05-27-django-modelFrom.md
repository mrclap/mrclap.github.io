---
layout: post
title: Django, ModelFrom 사용해보기
categories:
  - django
# feature_image: ""
---
> django로 간단한 웹은 몇 번 만들어본 적이 있으나(혹은 만드는 것에 참여한) modelForm을 사용해본 적이 없었다. 이미 구현해놓은 Model객체로 간단하게 form을 만들 수 있고, html태그로 view에 간단히 전달 할 수 도 있다.
하지만 가장 편리한 건 **request로 받아낸 form의 각각의 값(input태그의 value들)을 다시 model객체에 맵핑해주지 않아도 된다는 것**이 가장 장점이 아닐까?

#### Django ModelForm
django 프레임워크에서 Model클래스를 상속받아 작성한 model 클래스를 이용하여 손쉽게 view에 html태그 형태로 전달할 수 있다.

1. ModelForm을 상속받아 000Form을 작성하고
2. Meta클래스에 대상 Model을 넣은 뒤
3. form에 필요한 field를 list로 선정하면 손쉽게 modelForm을 만들 수 있다.

```python
from django.forms import ModelForm  # 1.
from app.models import OOO  # 2.

class OOOForm(ModelForm):   # 1.
  class Meta:
    model = OOO   # 2.
    fields = ['field1, field2, field3']   # 3.
```
##### form 만들기
- 완전히 새로운 데이터로 create을 위한 form이라면  
```python
form = OOOForm()
```

- 기존의 model instance를 활용한 update를 위한 form이라면  
```python
OOO = OOO.objects.get(pk=1)
form = OOOForm(instance=OOO)
```

##### form을 template에 전달하기
- form을 생성해서 template에 싣어보내고
```python
form = UserForm()
data_dict['form'] = form
return render(request, 'user/signup.html', data_dict)
```

- django templatetag를 이용해서 form을 사용할 수 있다.  
```html
  <form method="post">{% raw %}
     {% csrf_token %}  
     {{ form.as_p }}
     <input type="submit" value="login" />{% endraw %}
  </form>
```
참고: [as_p이외에도 다양한 형태로 html에 추가할 수 있다](https://docs.djangoproject.com/en/2.2/ref/forms/api/#as-p)


##### form으로 model save()하기
- POST로 받은 request를 이용하여 곧 바로 객체를 DB에 저장할 수 있다.
```python
OOO = OOOForm(request.POST)
OOO.save()
```

- 물론 기존 객체를 이용하여 Update도 가능하다
```python
old = OOO.objects.get(pk=1)
new = OOOForm(request.POST, instance=a)
new.save()
```
