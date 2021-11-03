---
layout: post
title: "jekyll에 disqus 추가하기"
date: 2021-11-03 13:00:35
image: '/assets/img/'
description: '간단하게 댓글기능을 만들자'
tags:
- jekyll
- disqus
categories:
- jekyll
twitter_text: ''
comments: true
---

## 댓글기능을 만들면 좋겠는데 무얼쓰지?

블로그를 운영하는데 있어 댓글기능은 방문자들과의 소통 창구 역할을 하게 되는데  
Github Pages에선 댓글기능을 지원하지 않는다.  
또한, 내가 작성하고 있는 블로그는 __jekyll__ 기반으로 __정적__ 블로그이기 때문에  
__동적__ 기능을 사용하는 댓글 서비스를 운영하는데 있어 기능을 구현하기 막막했다.  
열심히 찾아본 결과 __disqus__ 라는 플러그인을 통해 기능을 구현할수 있다 그래서   
내 Gitblog에 적용시키기로 하였다.


### step 1 ###
__disqus__ 를 사용하기 위해선, [__disqus__]를 접속해 회원가입부터 진횅해야한다.


---
#### User-agent: * #### 
#### Allow: / ####

---

② 모든 로봇에게 모든 문서 접근 차단  

---

#### User-agent: *  ####
#### Disallow: / ####

---

③ 모든 로봇에게 디렉터리 및 특정파일 접근을 차단  

---

#### User-agent: *  ####
#### Disallow: /directory/  ####
#### Disallow: /directory/file.html ####

---  

④ 특정 로봇에 모든 문서 접근 차단  

---

#### User-agent: Bot_name #### 
#### Disallow: / ####

---  

등등 여러 크롤링 봇을 허용 및 차단 시켜주는 기능이다.

이번에 Gitblog를 작성하면서, 우연히 알게된 정보인데  
무단 크롤링을 방지하기위해 나중에 사용해야될거같다.

[__disqus__] : https://blog.disqus.com

