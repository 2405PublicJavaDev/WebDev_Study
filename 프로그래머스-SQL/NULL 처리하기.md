# NULL 처리하기

## 문서 관리자

조승효(문서 생성자)

## 문제 풀이

NVL 을 활용하자

## 정답 쿼리(Oracle)

```sql
-- 코드를 입력하세요
SELECT ANIMAL_TYPE, NVL(NAME, 'No name') "NAME", SEX_UPON_INTAKE
FROM ANIMAL_INS
ORDER BY ANIMAL_ID ASC;
```

## 문제 링크

https://school.programmers.co.kr/learn/courses/30/lessons/59410
