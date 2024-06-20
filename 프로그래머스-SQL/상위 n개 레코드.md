# 상위 n개 레코드
## 문서 관리자
조승효(문서 생성자)
## 문제 풀이
상위 n개만을 하려면

``` sql
SELECT *
FROM (서브쿼리)
WHERE ROWNUM <= N;
```

과 같은 문법을 사용하자. 아예 패턴으로 기억하는게 좋을 것 같다.
## 정답 쿼리(ORACLE)
``` sql
-- 코드를 입력하세요
SELECT *
FROM (
SELECT NAME
FROM ANIMAL_INS
ORDER BY DATETIME ASC
    )
WHERE ROWNUM <= 1;
```
## 문제 링크
https://school.programmers.co.kr/learn/courses/30/lessons/59405
