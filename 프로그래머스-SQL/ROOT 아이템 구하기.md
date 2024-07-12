# ROOT 아이템 구하기

## 문서 관리자

조승효(문서 생성자)

## 문제 풀이

JOIN 과 IS NULL 을 활용하자

## 정답 쿼리(MySQL)

```sql
-- 코드를 작성해주세요
SELECT ITEM_ID, ITEM_NAME
FROM ITEM_INFO
JOIN ITEM_TREE USING(ITEM_ID)
WHERE PARENT_ITEM_ID IS NULL
ORDER BY 1 ASC;
```

## 문제 링크

https://school.programmers.co.kr/learn/courses/30/lessons/273710
