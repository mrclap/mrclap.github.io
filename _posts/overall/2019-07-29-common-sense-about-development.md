---
layout: post
title: 객체지향, RESTful API, TDD, 함수형프로그래밍, MVC pattern, Git(Github)
categories:
  - overall 
# feature_image: ""
tags: 컴퓨터과학
---
> 개발 기본 상식, [한재엽님의 Part 1-1 Development common sense](https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/master/Development_common_sense)를 기반으로 여러분들의 블로그를 참조!


# 01_Development common sense
- 객체지향?
- RESTful API
- TDD
- 함수형 프로그래밍
- MVC Pattern
- Git, GitHub

## 객체지향
- 현실 세계를 인간의 관점에서 ~~그대로~~ 프로그래밍화 하는 것

- 코드가 잘게 잘게 쪼개짐
- 재사용성이 높아지고, 디버깅이나 유지보수에 용이해짐

- 객체 간 정보교환이 모두 메시지 교환을 통해 일어나기에 많은 overhead가 발생
- 객체가 상태를 갖는 것에 의해 객체가 에측할 수 없는 상태를 갖는 경우가 있음
- 함수형 패러다임이 주목받음

### 설계 원칙(SOLID)
1. SRP(Single Responsibility Principle): 단일 책임 원칙
클래스는 하나의 책임만을 가져야 하며, 클래스의 변경이유 역시 하나의 이유이어야 한다.
1. OCP(Open-closed Principle) : 개방-폐쇄 원칙
확장에는 열려 있되, 변경에는 닫혀있어야 한다.
1. LSP(Liskov Substitution Principle) : 리스코프 치환 원칙
상위 타입의 객체를 하위 타입의 객체로 치환해도 상위 타입을 사용하는 프로그램은 정상 동작해야 한다.
1. ISP(Interface Segregation Principle) : 인터페이스 분리 원칙
인터페이스는 그 인터페이스를 사용하는 클라이언트를 기준으로 분리해야 한다.
클라이언트가 사용하지 않는 메서드에 의존하지 않아야 한다.


## RESTful API
<code>RE</code>presentational <code>S</code>tate <code>T</code>ransfer
Resource Oriented Architecture로 볼 수 있으며, API설계 중심에 자원이 있고, HTTP Method를 통해 자원을 처리함.

### 구성
리소스, 메서드, 메세지

리소스
- 리소스 지향 아키텍쳐 스타일임을 상기하여 명사로 표현하자


### 6개 원칙
- Uniform Interface
  - URI로 지정되어있는 리소스로 부터의 조작을 정해진 인터페이스로 수행
- Stateless
  - 세션정보, 쿠키 등의 정보를 별도로 저장, 관리 하지 않으며 들어오는 요청만 단순하게 처리함.
  - 주로 api key방식을 이용해서 사용자를 인증한다(카카오 api사용할때를 생각해보자!)
- Caching
  - E-Tag : 메시지에 담겨있는 엔터티를 위한 엔터티 태그를 제공한다. 이를 활용하여 리소스를 식별할 수 있다. 
  - Last-Modified : 엔터티가 마지막으로 변경된 시각에 대한 정보를 제공한다. (파일인 경우 파일 시스템이 제공해 준 최근 변경 시각, 동적으로 생성된 리소스라면 응답이 만들어진 시간)
    - Client가 'Last-Modified'값을 함께 보냈을때, '304 Not Modified'를 리턴받으면 client는 캐시에 있는값을 그대로 사용
- Client-Server 구조
  - 클라이언트는 사용자 인증, 컨텍스트(세션, 로그인 정보 등)를 관리, 책임지며,
    서버는 비지니스 로직 수행
- Hierarchical system
  - 클라이언트는 REST API서버만 호출하며, 서버는 내부적으로 다중 계층으로 구성되어 질 수 있음
- Code on demand
  - Server에서 실행가능한 코드를 Client에게 전송함으로써, Client의 기능들을 일시적으로 확장/커스터마이징 할 수 있음.
- (특징으로 분류시)Self-descriptiveness(자체 표현 구조)
  - REST API 메시지 만으로 쉽게 그 동작을 이해할 수 있음
  
### RESTful한 API디자인이란?
1. 리소스와 행위를 명시적이고 직관적으로 분리
- 행위는 HTTP Method로 / 목적은 GET, POST, PUT(PATCH), DELETE로!
1. Message의 Header와 Body를 분명하게 분리
- Body : Entity 내용은
- Header : API 버전 정보 / MIME 타입(text/plain, text/html, image/jpeg 등)
1. API 버전을 관리
- 환경 변화 등에 의한 API 시그니쳐 변경을 유의
- 하위 호환성 보장
1. 서버-클라이언트 간 같은 방식을 사용
- 브라우져 : json - 서버 : json

### 단점?
1. 사용가능한 Method가 4가지 뿐이다.
1. 분산환경에 적합하지 못하다
  - 더 찾아봐야 할 듯
1. HTTP 통신 모델에서만 지원


## TDD
### TDD란?
Test-Driven Developement는 **매우 짧은 개발 사이클의 반복에 의존하는 소프트웨어 개발 프로세스** 임
요구되는 새로운 기능에 대한 자동화된 테스트케이스를 작성한 후에 테스트를 통과하는 가장 간단한 코드를 작성함
테스트 통과하는 코드 작성 후 상황에 맞게 리팩토링 과정을 거침!!!

### Add a test 
테스트를 작성하기 위해서는 해당 기능에 대한 요구사항과 명세를 분명히 이해해야 함.
이는 사용자 케이스와 사용자 스토리 등으로 이해가 가능하며,
개발자가 코드를 **작성하기 전에** 요구사항에 더 집중할 수 있도록 도와준다.

### Run all tests and see if new one fails
새로운 기능 추가에 따른 기존 기능들의 오류 발생 위험 방지를 위함.
새로운 기능 추가시마다 테스트 코드를 작성하여 기존 코드 + 추가 코드를 모두 테스트

### Refactor code
코드량이 방대해지면 리팩토링을 하게 됨.
이때에 테스트 주도 개발을 통해 개발한 경우라면, 테스트들을 중심으로 개발이 가능

> *convention : 일종의 관습, 많은 개발자들이 약속처럼 지키는 그런 것, Class는 대문자로 시작하고, 상수는 대문자이며 등
-
Java
http://www.oracle.com/technetwork/java/javase/documentation/codeconvtoc-136057.html
http://www.oracle.com/technetwork/java/codeconventions-150003.pdf
디자인 패턴 https://www.tutorialspoint.com/design_pattern/
C
https://users.ece.cmu.edu/~eno/coding/CCodingStandard.html
C++
https://users.ece.cmu.edu/~eno/coding/CppCodingStandard.html
Google C++ Style Guide
Javascript
https://www.w3schools.com/js/js_conventions.asp
https://github.com/airbnb/javascript
https://google.github.io/styleguide/jsguide.html
MySQL
https://www.toadworld.com/platforms/mysql/w/wiki/6103.naming-conventions
https://www.toadworld.com/platforms/mysql/w/wiki/6108.naming-indexes
http://www.sqlstyle.guide/
Oracle
https://oracle-base.com/articles/misc/naming-conventions
Linux kernel
https://www.kernel.org/doc/html/v4.10/process/coding-style.html
Shell
https://google.github.io/styleguide/shell.xml
MongoDB
https://docs.mongodb.com/manual/meta/style-guide/
HTML
https://www.w3schools.com/html/html5_syntax.asp


### 의문점
코드 생산성
- 보드량이 분명 늘어남, <code>코드퀄리티 < 빠른 </code> 생산성이 요구된다면 문제가 될지도 ?


테스트 코드 작성은 쉬운가?
- 어떠한 부분을 테스트해야 하는지
- 어떻게 테스트 할지
- 여러 테스트 프레임워크 중 어떤 것을 사용해야 할지 등에 대한 학습 및 공감대가 필요.



## 함수형 프로그래밍
### 함수형 프로그래밍 언어란?
부작용에 적대적인, 입출력이 표면에 모두 나타난 순수 함수를 구현하는 것


### 함수형 프로그래밍이란?
.. 아직 이해하기 어렵지만
[한주영님의 (번역)어떤 프로그래밍 언어들이 함수형인가?](https://medium.com/@jooyunghan/어떤-프로그래밍-언어들이-함수형인가-fec1e941c47f)를 참고하면 좋을듯!!


### immutable vs mutable
immutable
- 변경 불가능, 객체가 가지고 있는 값을 변경할 수 없는 객체
- 값이 변경될 경우 새로운 객체를 생성, 변경된 값을 주입하여 반환

mutable
- 반대 ㅎㅎㅎ

### first-citizen
함수형 프로그래밍 패러다임을 따르는 언어에서 함수 = 일급 객체(first class citizen)

### 일급객체
- 변수나 데이터 구조안에 함수를 담을 수 있어서 함수의 파라미터로 전달이 가능하며, 함수의 반환값으로 사용할 수 있음
- 할당에 사용된 이름과 관게없이 고유한 구별이 가능
- 함수를 리터럴로 바로 정의 할 수 있음

### Reactive Programming
반응형 프로그래밍은 기본적으로 모든 것을 stream으로 보며,
스트림이란 값들의 집합으로, 제공되는 함수형 메소드를 통해 데이터를 imuutable하게 관리가 가능


## MVC패턴
### Controller
요청에 대한 실제 업무를 수행하는 Model 호출
클라이언트가 보낸 데이터를 가공, 모델의 결과를 뷰와함께 클라이언트에 전달 등

### Model
비지니스 로직 구현
DB연결, 저장 삭제 등

### View
컨트롤러로부터 건대 받은 결과값을 가지고 사용자에게 출력할 화면을 만듦.


## Git/ GitHub
### Git
깃 연습하기 딱 좋은 사이트!!!![HERE](https://learngitbranching.js.org)

#### reset
git reset --mixed => default, HEAD옮김, Staging Area비움, Working Directory 유지
git reset --soft => HEAD옮김, Staging Area/Working Directory 유지
git reset --hard => HEAD옮김, Staging Area 비움 / Working Directory 되돌림

#### stash
작업 중 잠시 브랜치를 변경하거나 커밋을 변경해야 한다면!?
stash를 통해 modified 파일과 Staging Area의 변경사항을 스택형식으로 저장할 수 있음
git stash -> 현재 상황 저장
git stash save {name} -> {name}으로 저장
git stash list -> 스택안의 스냅샷 리스트 확인
git stash pop -> 후입 선출로 스냅샷 뽑아냄
git stash stash@{number} => list로 확인한 stash중 필요한것 찝어서 뽑아냄
git stash drop => 마지막 stash 스택 지움
git stash clear => 스택 날림

#### pull
pull은 fetch + merge


### Reference 
- [Interview_Question_for_Beginner](https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/master/Development_common_sense)
- RESTful API [조대협의 블로그 - api 개념 소개](https://bcho.tistory.com/953)
- RESTful API [조대협의 블로그 - api 디자인 가이드 ](https://bcho.tistory.com/954?category=252770)
- RESTful API [조대협님의 대용량 분산 아키텍쳐 설계 #5.rest](https://www.slideshare.net/Byungwook/5-rest-34910637)
- [Code convetion과 개발자가 지켜야할 수칙에 관하여](https://novemberde.github.io/2017/05/21/Javascript_policy.html)
- [(번역) 함수형 프로그래밍이란 무엇인가?](https://medium.com/@jooyunghan/함수형-프로그래밍이란-무엇인가-fab4e960d263)
- MVC pattern[MVC 아키텍쳐에 대한 이해](https://asfirstalways.tistory.com/180)
- [ [Web] Servlet과 JSP의 차이와 관계](https://gmlwjd9405.github.io/2018/11/04/servlet-vs-jsp.html)