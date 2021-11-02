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
- MariaDB
twitter_text: ''
---

## SQL_MODE ?

__저장할 데이터에 대한 제약조건의 허용범위를 지정할수 있게 해준다.__  

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

이번 프로젝트에서 ANSI_QUOTES 가 필요해 적용을 시키려고 했지만,  
사내규정으로 인해 어쩔수없이 Python 으로 API을 만들어서 적용을 시키게 되었다.  
이번일로 인해 새로운 정보를 알게되었으니 그거로 만족해야겠다.
