# DML (DATA MANIPULATION LANGUAGE)

SUID|`데이터`
-|-
SELECT|조회
UPDATE|수정
INSERT|입력
DELETE|삭제

```SQL
--입력
INSERT INTO MENU(NAME) VALUES ('연어스시');

--수정
UPDATE MENU SET discount_rate = 10 (where name = '연어스시')

--삭제
DELETE FROM MENU (WHERE name = '연어스시')
```