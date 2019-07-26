---
layout: post
title: Django documentation
categories:
  - django
# feature_image: ""
tags:
  - 
---
> Django documentation Intro


[Django2.2 Document](https://docs.djangoproject.com/en/2.2/)

# Django documentation
## Intro
### 도큐멘테이션의 구성
- [Tutorials](https://docs.djangoproject.com/en/2.2/intro/)
  - 웹 어플리케이션 만들기를 위한 단계별 시리즈 제공
- [Topic guides](https://docs.djangoproject.com/en/2.2/topics/)
  - 중요 토픽 및 개념에 대한 이야기들
  - 유용한 배경지식과 설명
- [Reference guides](https://docs.djangoproject.com/en/2.2/ref/)
  - API들에 대한 기술적 참조사항
  - 장고 시스템의 또 다른 측면에 대한 내용
  - 어떻게 작동하고, 어떻게 사용할 것인지에 대한 내용
  - 장고에 대한 기본적 이해 필요
- [How-to guides](https://docs.djangoproject.com/en/2.2/howto/)
  - 사용법
  - 주요한 문제와 사용예에 대한 안내
  - tutorial보다 고급단게
  - 장고에 대한 중간이상의 이해 필요

<br />
### Model layer
장고는 데이터의 구조화 및 조작을 위한 Model layer를 가짐

- 관련 개념(추후 link 필요)
  - Models : [models](./2019-07-26-django-model-layer-models)
  - QuerySets
  - Model instances
  - Migrations
  - Advanced
  - Other

<br />
### View layer
장고는 user의 request를 받아, response하는 프로세스 로직을 책임지는 캡슐화된 View layer를 가짐

- 관련 개념(추후 link 필요)
  - The Basics
  - References
  - File uploads
  - Class-based View
  - Advanced
  - Middleware

<br />
### Template layer
장고는 페이지 렌더링을 위한 디자이너 친화적인 문법을 제공한다.

- 관련 개념(추후 link 필요)
  - The basics
  - For desiners
  - For programmers

<br />
### Forms
장고는 form의 생성과 form data 조작을 위한 기능을 제공한다.

- 관련 개념(추후 link 필요)
  - The basics
  - ADvanced

<br />
### Development process
개발과 테스트를 위한 다양한 componenets와 tools

- 관련 개념(추후 link 필요)
  - Settings
  - Applications
  - Exceptions
  - django-admin and manage.py
  - Testing
  - Deployment

<br />
### Admin
장고의 가장 유용한 기능 중 하나인 자동화된 어드민 인터페이스

- 관련 개념(추후 link 필요)
  - Admin site
  - Admin actions
  - Admin documentation generator

<br />
### Security
장고는 가장 중요한(paramount importance) 주제인 보안을 위해 다양한 보안툴과 매카니즘을 제공

- 관련 개념(추후 link 필요)
  - Security overview
  - Disclosed security issues in Django
  - Clickjacking protection
  - Cross Site Request Forgery protection
  - Cryptographic signing
  - Security Middleware

<br />
### Internationaliztion and localzation
장고는 국제화(Internationalization) 및 지역화(localzation)을 위한 프레임워크를 제공. 이는 언어 및 지역을 고려한 개발에 도움

- 관련 개념(추후 link 필요)
  - Overview
  - Time zones

<br />
### Performance and optimization
더 효율적(fewer resources)이고 빠른 코드 동작을 위해 다양한 테크닉과 툴

- 관련 개념(추후 link 필요)
  - Performances and optimization overview

<br />
### Geographic framework
[GeoDjango](https://docs.djangoproject.com/en/2.2/ref/contrib/gis/)는 세계 최고 수준의 geographic Web framework. 


<br />
### Common Web Apllication tools
보편적인 다양한 웹 어플리케이션 개발 툴

- 관련 개념(추후 link 필요)
  - Authentication
  - Caching
  - Logging
  - Sending emails
  - Syndication feeds(RSS / Atom)
  - Pagination
  - Messages framework
  - Serialzation
  - Sessions
  - Sitemaps
  - Static files management
  - Data validation

<br />
### Other core functionalities
- Conditional content processing
- content types and generic relations
- Flatpages
- Redirects
- Signals
- System check framework
- The sites framework
- Unicode in Django

<br />
### Django open-source project
...