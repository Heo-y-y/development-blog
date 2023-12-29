# 상위 n개 레코드

## 문제

![스크린샷 2023-12-29 오후 12 37 07](https://github.com/Heo-y-y/development-blog/assets/112863029/7549bfcf-fa61-4ae9-acf4-bcb3aff71648)

## 풀이

가장 먼저 들어오는 건 `DATETIME`를 앞순으로 정렬해서 맨위에서 하나만 뽑게 `LIMIT`을 1로 설정하면 된다.

```sql
SELECT NAME
FROM ANIMAL_INS
ORDER BY DATETIME
LIMIT 1;
```

---

**참고 자료**

- <https://school.programmers.co.kr/learn/courses/30/lessons/59405>
