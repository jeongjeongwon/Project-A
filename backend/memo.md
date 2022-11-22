### 좌측 표
* 표시 항목: 종목이름(name), 시초가(open), 등락폭, 추천율
    * 등락폭 = 금일 종가(close) - 전일 종가(close) (금일 5만이고, 전일 4만이면 5만-4만=+1만으로, 1만원 상승임)
    * 추천율 = {(고가(high) + 저가(low)) / 2} * 0.4

    * 조작 항목: 거래량순(volume), 상승순, 하락순, 추천율순

### 우측 상세정보
* 표시 항목: 종가(close), 고가(high), 저가(low), 거래량(volume), 시총

### 기존 코드 형식으로 임의의 종목 5개 출력
```sql
WITH temp_table AS (
SELECT * FROM kosdak_029960_m WHERE day="2022-02-03" UNION ALL
SELECT * FROM kosdak_018680_m WHERE day="2022-02-03" UNION ALL
SELECT * FROM kosdak_021880_m WHERE day="2022-02-03" UNION ALL
SELECT * FROM kosdak_025320_m WHERE day="2022-02-03" UNION ALL
SELECT * FROM kosdak_031980_m WHERE day="2022-02-03"
) SELECT * FROM temp_table order by volume desc LIMIT 28;

```

```
+-----+-------+-------+-------+-------+--------+------------+--------+
| no  | open  | high  | low   | close | volume | day        | code   |
+-----+-------+-------+-------+-------+--------+------------+--------+
| 308 |  2400 |  2495 |  2400 |  2485 | 266882 | 2022-02-03 | 025320 |
| 212 |  7980 |  8170 |  7980 |  8120 | 130791 | 2022-02-03 | 029960 |
| 301 | 15250 | 15350 | 14400 | 14600 | 112307 | 2022-02-03 | 031980 |
| 305 |   504 |   525 |   502 |   506 |  88048 | 2022-02-03 | 021880 |
| 258 |  6500 |  6730 |  6430 |  6720 |  35486 | 2022-02-03 | 018680 |
+-----+-------+-------+-------+-------+--------+------------+--------+
```

### 위에서 출력된 데이터의 종목코드를 가지고 이름을 출력
```sql
WITH temp_name AS (SELECT name, code FROM companylist WHERE code=025320 OR code=029960 OR code=031980 OR code=021880 OR code=018680
) SELECT * FROM temp_name;
```

```
+--------------------------+--------+
| name                     | code   |
+--------------------------+--------+
| 서울제약                 | 018680 |
| 메이슨캐피탈             | 021880 |
| 시노펙스                 | 025320 |
| 코엔텍                   | 029960 |
| 피에스케이홀딩스         | 031980 |
+--------------------------+--------+
```

### 종목+이름 JOIN
```sql
WITH
temp_table AS (
SELECT * FROM kosdak_029960_m WHERE day="2022-02-03" UNION ALL
SELECT * FROM kosdak_018680_m WHERE day="2022-02-03" UNION ALL
SELECT * FROM kosdak_021880_m WHERE day="2022-02-03" UNION ALL
SELECT * FROM kosdak_025320_m WHERE day="2022-02-03" UNION ALL
SELECT * FROM kosdak_031980_m WHERE day="2022-02-03"
),
temp_name AS (SELECT name, code FROM companylist WHERE code=025320 OR code=029960 OR code=031980 OR code=021880 OR code=018680
)
SELECT * FROM temp_table
INNER JOIN temp_name
ON temp_table.code = temp_name.code
GROUP BY open,high,low,close, volume, day, temp_name.code
ORDER BY volume DESC 
 LIMIT 28;
```
```
+-----+-------+-------+-------+-------+--------+------------+--------+--------------------------+--------+
| no  | open  | high  | low   | close | volume | day        | code   | name                     | code   |
+-----+-------+-------+-------+-------+--------+------------+--------+--------------------------+--------+
| 308 |  2400 |  2495 |  2400 |  2485 | 266882 | 2022-02-03 | 025320 | 시노펙스                 | 025320 |
| 212 |  7980 |  8170 |  7980 |  8120 | 130791 | 2022-02-03 | 029960 | 코엔텍                   | 029960 |
| 301 | 15250 | 15350 | 14400 | 14600 | 112307 | 2022-02-03 | 031980 | 피에스케이홀딩스         | 031980 |
| 305 |   504 |   525 |   502 |   506 |  88048 | 2022-02-03 | 021880 | 메이슨캐피탈             | 021880 |
| 258 |  6500 |  6730 |  6430 |  6720 |  35486 | 2022-02-03 | 018680 | 서울제약                 | 018680 |
+-----+-------+-------+-------+-------+--------+------------+--------+--------------------------+--------+
```