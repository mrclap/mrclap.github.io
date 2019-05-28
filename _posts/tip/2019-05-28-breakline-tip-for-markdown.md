---
layout: post
title: 마크다운 줄바꿈 - atom에서 .md파일 줄바꾸기 
categories:
  - tip
# feature_image: ""
tags:
  - markdown
  - atom
  - 줄바꾸기
---
>Atom에디터에서 markdown 줄바꿈(스페이스바 2개 후 엔터)이 제대로 동작하지 않을땐 core package, 'whitespace'에서 설정이 필요하다.

Atom을 에디터로 markdown을 작성하다보면 문단을 바꾸지 않고(엔터 두 번), 줄바꿈이 필요한 경우가 있다.
인터넷에 여러 방법이 있는데 나의 경우는 줄바꿈을 하고자 할때 문장의 마지막에 스페이스를 두 번하고 엔터를 치면 효과적으로 줄바꿈이 되었다.

```
글쓰고(스페이스바 두 번 하고 엔터)
다음 줄
```
글쓰고  
다음 줄

그런데 이게 잘 안되눈 경우가 많아 살펴보니 문장의 끝에 space가 자꾸 자동으로 삭제되는 것이었다.  
> setting -> core package의 whitespace 패키지 -> 'Remove Trailing Whitespace'옵션 해제  

이제 잘 된다!
