---
layout: post
title: Django, models, 장고 모델
categories:
  - django
# feature_image: ""
tags:
  - 
---
> Models, Django documentation

[Django2.2 Document Models](https://docs.djangoproject.com/en/2.2/topics/db/models/)

## Models
보통 하나의 모델은 하나의 데이터베이스 테이블과 연결된다(maps to)

The basics
  - 각 모델은 django.db.models.Model의 서브클래스
  - 모델의 속성읜 Database의 field를 나타낸다
  - 이를 바탕으로 자동생성된 database-access API를 사용할 수 있다.(orm을 사용할 수 있다!!!!!!)

<br/>
### Quick example
  ```python
  from django.db import models

  class Person(models.Model):
    first_name = models.CharField(max_length=30)
    last_name = models.CharField(max_length=30)
  ```

`Person`모델을 통해 아래와 같은 SQL을 낼 수 있음

  ```sql
  CREATE TABLE myapp_person (
      "id" serial NOT NULL PRIMARY KEY,
      "first_name" varchar(30) NOT NULL,
      "last_name" varchar(30) NOT NULL
  );
  ```

Technical notes
  - [테이블명에 관하여](https://docs.djangoproject.com/ko/2.2/ref/models/options/#table-names)
    - `myapp_person` 처럼 `어플리케이션이름_Model명`의 테이블이름이 기본이지만, Model에 `db_table = 원하는 DB명`을 이용해 변경 가능
  - id 필드는 자동으로 추가, override역시 가능
  - setting file을 이용해 어떤 DB(PostgreSQL, mySQL 등)를 사용하는지 명시하여 export되는 sql문을 바꿀 수 있음


<br/>
### Using models
정의한 model들을 사용하기 위해서 settings파일의 **INSTALLED_APPS**에 해당 model들이 포함된 application module name을 추가해야 함.
  ```python
  INSTALLED_APPS = [
      #...
      'myapp',
      #...
  ]
  ```
- **INSALLED_APPS**에 신규 모듈 추가시, `manage.py migrate`필요,
  - [migrate](https://docs.djangoproject.com/ko/2.2/ref/django-admin/#django-admin-migrate) : DB <-> 모델과 마이그레이션 간 sync
  - [migration](https://docs.djangoproject.com/ko/2.2/topics/migrations/) : 모델의 변경사항 등을 저장, database schema에 반영하기 위한 것

<br/>
### Fields
models API와 충돌되는 clean, save, delete는 column명으로 피할 것

Example
```python
from django.db import models

class Musician(models.Model):
    first_name = models.CharField(max_length=50)
    last_name = models.CharField(max_length=50)
    instrument = models.CharField(max_length=100)

class Album(models.Model):
    artist = models.ForeignKey(Musician, on_delete=models.CASCADE)
    name = models.CharField(max_length=100)
    release_date = models.DateField()
    num_stars = models.IntegerField()
```

#### Field types
모델의 각 필드는 Field class, Field클래스는 몇 가지를 결정한다.
  - column type(INTEGER, VARCHAR, TEXT...)
  - form field의 랜더링 시 기본 HTML tag
  - 최소한의 validation(Django admin, 자동으로 생성된 form에서)

#### Field options
각 필드는 필드 특정의 몇몇 인자를 가짐. (예, CharField의 경우 max_length)

또한 공통적인 인자들도 다수 존재
  - null : if True: NULL 허용(Default False) <-- database related
  - blank: if True: 공백 허용(Default False) <-- validation related
  - choices: 2-tuples 리스트 값, DB의 enum, 첫번째 값(FR, SO)는 DB에 저장되는 값, 두번째 값은 form widget에서 보이는 값
    ```python
    YEAR_IN_SCHOOL_CHOICES = [
    ('FR', 'Freshman'),
    ('SO', 'Sophomore'),
    ('JR', 'Junior'),
    ('SR', 'Senior'),
    ('GR', 'Graduate'),
    ]
    ```
    - get_FOO_display()를 이용하여 display value 확인 가능
      ```python
      from django.db import models

      class Person(models.Model):
          SHIRT_SIZES = (
              ('S', 'Small'),
              ('M', 'Medium'),
              ('L', 'Large'),
          )
          name = models.CharField(max_length=60)
          shirt_size = models.CharField(max_length=1, choices=SHIRT_SIZES)
      ~~~
      >>> p = Person(name="Fred Flintstone", shirt_size="L")
      >>> p.save()
      >>> p.shirt_size
      'L'
      >>> p.get_shirt_size_display()
      'Large'
      ```

  - default : 기본 값
  - help_text : form widget에 노출되는 help text
  - primary_key : 프라이머리 키 지정
    - 모델에서 primary_key=True지정하지 않으면, 자동으로 IntegerField를 추가하여 primary key를 유지
    - primary_key column은 read-only, 만약 기 존재하는 object의 primary key변경하여 .save()하면 새로운 객체가 생성
  - unique : 유니크, 
  - [더보기](https://docs.djangoproject.com/ko/2.2/ref/models/fields/#common-model-field-options)

#### Automatic primary key fields
`id = models.AutoField(primary_key=True)`

#### Verbose field name
ForeignKey, ManyToManyField, OneToOneField를 제외한 field에서 첫번째 인자로 verbose name을 사용, 주어지지않을 경우 field의 attribute이름을 그대로 사용(underscore를 spacebar로 바꿈)
  ```python
  first_name = models.CharField("person's first name", max_length=30)
  >>> Person's first name
  # 첫 글자를 대분자로 설정하지 않아도 Django에서 자동으로 대문자로 표기
  ```

<br/>
### Relations
https://docs.djangoproject.com/ko/2.2/topics/db/models/#automatic-primary-key-fields