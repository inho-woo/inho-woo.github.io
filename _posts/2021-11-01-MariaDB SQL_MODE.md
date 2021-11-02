---
layout: post
title: "MariaDB SQL_MODE"
date: 2021-11-01 09:21:35
image: '/assets/img/'
description: 'SQL_MODE 설명 및 사용방법'
tags:
- MariaDB
- RDBMS
categories:
- SQL_MODE
twitter_text: ''
---

Mairadb 10.1.6 버전 포함 이하버전은 sql_mode 가 없다.

하지만, 그 이후 버전부터는 sql_mode가  
default setting 되어있기때문에 간혹가다가 수정이 필요할수있다.  
{% highlight sql %}  

변경 쿼리  

SET  @@sql_mode = '변경할 모드';  

{% endhighlight %}  

root 계정으로 SET을 실행하면 서버를 껐다 켜도 sql_mode는 변경한 값이 유지가 되지만

그 외 계정으로 실행시 그 세션에서만 적용이 되기때문에 이부분은 참고해야한다.

또한 하나의 DB에만 설정이 되는것이 아닌,  
서버 안에 있는 __DB모두 적용__ 되기 때문에 주의해서 사용해야한다.