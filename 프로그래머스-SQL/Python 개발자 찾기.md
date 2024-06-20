# Python 개발자 찾기
## 문서 관리자
조승효(문서 생성자)
## 문제 풀이
WHERE OR 를 활용하자
## 정답 쿼리(MySQL)
``` sql
-- 코드를 작성해주세요
SELECT ID, EMAIL, FIRST_NAME, LAST_NAME
FROM DEVELOPER_INFOS
WHERE SKILL_1 = 'Python' OR SKILL_2 = 'Python' OR SKILL_3 = 'Python'
ORDER BY 1 ASC;
```
## 문제 링크
https://school.programmers.co.kr/learn/courses/30/lessons/276013