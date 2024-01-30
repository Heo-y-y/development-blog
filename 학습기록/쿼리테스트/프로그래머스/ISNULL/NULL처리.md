# NULL 처리하기

## 문제

![스크린샷 2024-01-30 오후 1 43 16](https://github.com/Heo-y-y/development-blog/assets/112863029/5911798f-4d66-4ba4-b3a2-5407aac734f4)

## 풀이

```sql
SELECT ANIMAL_TYPE, IFNULL(NAME, 'No name') AS NAME, SEX_UPON_INTAKE 
FROM ANIMAL_INS
ORDER BY ANIMAL_ID
```

---

**참고 자료**

- <https://school.programmers.co.kr/learn/courses/30/lessons/59410>
