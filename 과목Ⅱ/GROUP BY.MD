# GROUP BY와 HAVING

## GROUP BY는 집계함수와 함께 자주 쓰인다.

함수|설명
-|-|
COUNT(*)|NULL을 포함한 모든 행의 수
COUNT(표현식)|NULL을 제외한 행의 수
SUM|`NULL을 제외한` 합계
AVG|`NULL을 제외한` 평균
COUNT(DISTINCT)|중복은 1개로 친 행의 수
MAX|최댓값
MIN|최솟값

#### HAVING은 WHERE와 비슷한 개념으로 조건을 제한하는 것이지만, 집계함수에 대해서 조건을 제한한다. 즉, 집계함수의 제한은 WHERE절로 표현할 수 없고 HAVING을 이용해야한다.그리고, HAVING은 꼭 GROUP BY 다음에 나와야한다. 순서가 바뀌면 안된다.

### 작성 순서

SELECT ⬇️<BR>
FROM ⬇️<BR>
GROUP BY ⬇️<BR>
HAVING ⬇️<BR>
ORDER BY ⬇️<BR>

### 실행 순서

GROUP BY ⬇️<BR>
HAVING ⬇️<BR>
FROM ⬇️<BR>
SELECT ⬇️<BR>
ORDER BY ⬇️<BR>

```SQL
CASE WHEN A THEN B ELSE C END

--- A가 TRUE이면 B를 실행하고, 아니면 C를 실행해.
```
TBL
ID|
-|
100|
100|
200|
200|
200|
999|
999

```SQL
SELECT ID FROM TBL
GROUP BY ID
HAVING COUNT(*) = 2
ORDER BY (CASE WHEN ID = 999 THEN 0 ELSE ID END);
```

### 풀이

```SQL
SELECT ID,COUNT(*) AS CNT FROM TBL
GROUP BY ID
```
ID|CNT
-|-|
100|2|
200|3|
999|2

⬇️

```SQL
SELECT ID FROM TBL
GROUP BY ID
HAVING COUNT(*) = 2
```
ID|
-|
100|
999|

⬇️
```SQL
CASE WHEN ID = 999 THEN 0 ELSE ID END
```
ID|
-|
100| 
0|

```SQL
ORDER BY
```
ID|
-|
0| 
100|

답:
ID|
-|
999| 
100|

⭐표현은 다시 원래대로 바꿔놔야한다.

```SQL
-- 1번
SELECT 지역, SUM(매출금액), AS 매출금액
FROM 지역별매출
GROUP BY 지역
ORDER BY 매출금액 DESC;

-- 2번
SELECT 지역, 매출금액
FROM 지역별매출
ORDER BY 년 ASC;

-- 3번
SELECT 지역, SUM(매출금액), AS 매출금액
FROM 지역별매출
GROUP BY 지역
ORDER BY 년 DESC;

-- 4번
SELECT 지역, SUM(매출금액), AS 매출금액
FROM 지역별매출
GROUP BY 지역
HAVING SUM(매출금액) > 1000
ORDER BY COUNT(*) ASC;
```


