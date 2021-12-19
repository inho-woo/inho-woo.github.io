---
layout: post
title: "MSSQL MERGE문"
date: 2021-12-19 13:20
image: '/assets/img/'
description: 'MSSQL'
tags:
- MSSQL
- MERGE
categories:
- MSSQL
- MERGE
twitter_text: ''
comments: true
---

고객사 CS 기반 MES 화면을 생성함에 있어서 현재 회사는 프로시저를 CRUD 를 전부 나눠서 제작이 되어있었다.
하나하나 일일이 만들지 말고 하나로 __INSERT, UPDATE, DELETE__ 를 할수있는 방법이 있을까 하고 찾아보았는데
마침 __MERGE__ 라는 문법을 찾게 되어 유용하게 쓰일꺼 같아 포스팅을 하게 되었다. 

### MERGE? ###
MERGE 문을 사용하면 변경할 테이블에 데이터가 존재하는지 체크하고, UPDATE, DELETE, INSERT를 한 번에 작업이 가능한 SQL문이다.
보통 단일(한개의) 테이블에 UPDATE 또는 INSERT를 하는 경우 많이 사용하지만, 두개의 테이블을 비교하거나 서브 쿼리의 결과에 따라서 UPDATE, INSERT 작업이 가능하다.

{% highlight sql %}
  MERGE INTO (변경할 테이블)
  USING (비교할 테이블 | 서브쿼리)
  ON (조건문)
  WHERE MATCHED THEN
  (조건을 만족할경우)
  -- UPDATE SET  ~ 
  -- DELETE ~ 
  WHEN NOT MATCHED THEN
  -- INSERT INTO ~
{% endhighlight %}

기본적인 예시를 이렇게 된다.

하나의 테이블로 MERGE문을 사용하는 방법은 __DUAL__ 이라는 dummy 테이블을 이용해 처리한다.

한글로 보다 쉽게(?) 이해 할수 있도록 작성하였다.(원래 이렇게 하면 언된다..)

{% highlight sql %}
MERGE INTO 부서 AS A
USING (SELECT 1 AS DUAL) AS B
   ON (A.부서번호 = 1)
 WHEN MATCHED THEN
   UPDATE SET A.부서명 = 'IT', A.직원명 = 'inho-woo'
 WHEN NOT MATCHED THEN
   INSERT(부서번호, 부서명, 직원명) VALUES(50, 'IT', 'inho-woo')
;
{% endhighlight %}

__(SELECT 1 AS DUAL) AS B__  이부분은 dummy 서브 쿼리이기 떄문에 그대로 사용해도 무방하다.

부서 테이블에 부서번호 = '1'에 만족하는 값이 있으면 UPDATE, 없으면 INSERT 한다.

변수를 사용해 쿼리문을 작성하면 다움과 같다.

{% highlight sql %}
DECLARE @부서번호 INT = 1
DECLARE @부서명 NVARCHAR(10) = 'IT'
DECLARE @직원명 NVARCHAR(10) = 'inho-woo'

MERGE INTO 부서 AS A
USING (SELECT 1 AS DUAL) AS B
   ON (A.부서번호 = @부서번호)
 WHEN MATCHED THEN
   UPDATE SET A.부서명 = @부서명, A.직원명 = @직원명
 WHEN NOT MATCHED THEN
   INSERT(부서번호, 부서명, 직원명) VALUES(@부서번호, @부서명, @직원명)
;
{% endhighlight %}

변수를 사용해 쿼리문을 작성하면 보다 이해하기가 더 쉽다.

다음은 __서브쿼리__ 를 이용해 작성을 해보았다.

{% highlight sql %}
DECLARE @부서번호 INT = 1

MERGE INTO 부서 AS A
USING (SELECT DISTINCT 
              D.부서번호   AS 부서번호
            , D.부서명    AS 부서명
            , D.직원명    AS 직원명
         FROM 직원 AS E
        INNER JOIN 부서이력 AS D
           ON E.부서번호 = D.부서번호
        WHERE E.부서번호 = @부서번호) AS B
   ON (A.부서번호 = B.부서번호)
 WHEN MATCHED THEN
   UPDATE SET A.부서명 = B.부서명
            , A.직원명 = B.직원명
 WHEN NOT MATCHED THEN
   INSERT(부서번호, 부서명, 직원명) 
   VALUES(B.부서번호, B.부서명, B.직원명)
;
{% endhighlight %}

USING 절에 서브 쿼리를 사용한 쿼리로, 직원 테이블에 부서번호 = '1'이 존재하고, 서브 쿼리 결과와 부서 테이블을 비교하여 존재 여부에 따라서 UPDATE, INSERT 한다.

다음은 테이블 을 조인했을떄의 쿼리이다.

{% highlight sql %}
MERGE INTO 부서 AS A
USING 부서이력 AS B
   ON (A.부서번호 = B.부서번호)
 WHEN MATCHED THEN
   UPDATE SET A.부서명 = B.부서명
            , A.직원명   = B.직원명
 WHEN NOT MATCHED THEN
   INSERT(부서번호, 부서명, 직원명) 
   VALUES(B.부서번호, B.부서명, B.직원명)
;
{% endhighlight %}

부서이력 테이블의 값이 dept 테이블에 존재하는 경우, 부서이력 테이블의 값으로 UPDATE, 없으면 INSERT 한다.

MERGE 문을 사용할때의 주의사항이 있다.

__1. USING 절에는 AS = 별칭 이 필수이다.__

__2. 쿼리문 끝에는 항상 ; = 세미콜론 을 넣어줘야 한다.__

__3. USING 절의 데이터에 변경할 테이블과 비교할 테이블의 KEY 컬럼 값이 중복으로 선언항 수 었다.__


이렇게 MERGE문을 알아보았다. 보다 유용하게 사용할수 있을꺼 같아 나름대로 정리를 해보았다.
