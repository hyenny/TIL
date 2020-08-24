# MSSQL, ORACLE에서 날짜 예외처리하기

CHAR 타입인 날짜를 TO_DATE() 함수를 사용해 DATE 타입으로 변환하던 도중 `ORA-01843: 지정한 월이 부적합합니다.` 에러가 발생했다.
원인은 '20200631'(예시)이었다. 2020년 6월은 30일까지 밖에 없는데 31일을 DATE 타입으로 변환하려니 문제가 발생한 것이었다.

어떻게 잘못된 데이터를 바로 잡을 수 있을까? 검색을 해보니, MSSQL의 경우 `ISDATE()`라는 함수가 있었다. DATE가 유효한 경우 1, 유효하지 않은 경우 0을 리턴해서 예외처리를 하는 것이다. 하지만 ~~ORACLE은 `ISDATE()`함수가 없다고 한다.~~ 그래서 PL/SQL로 FUNCTION을 만들어줘야한다.

#  

## MSSQL : ISDATE()
### 구문
```SQL
ISDATE(expression)
```
expression이 유효한 날짜인지 확인한다.      
  
### 예제
```SQL
SELECT ISDATE('2020-08-31'); -- 1
SELECT ISDATE('2020-06-31'); -- 0
SELECT ISDATE('HELLO'); -- 0
```
expression이 유효한 날짜인 경우 1을 리턴, 아닌 경우 0을 리턴한다.  


#       


## ORACLE : PL/SQL로 FUCTION 생성

### 예제
```SQL
CREATE OR REPLACE FUNCTION FN_IS_DATE(V_STR_DATE IN VARCHAR2, V_DATE_FORMAT IN VARCHAR2 DEFAULT 'YYYYMMDD')
RETURN NUMBER
IS
    V_DATE DATE;
BEGIN
    V_DATE := TO_DATE(V_STR_DATE, V_DATE_FORMAT);
    RETURN 1;
EXCEPTION
    WHEN OTHERS THEN
    RETURN 0;
END;
```
V_STR_DATE를 V_DATE_FORMAT형식으로 변환 성공시 1을 리턴, 에러시 0을 리턴한다.

```SQL
CREATE OR REPLACE FUNCTION FN_IS_DATE(V_STR_DATE IN VARCHAR2, V_DATE_FORMAT IN VARCHAR2 DEFAULT 'YYYYMMDD')
RETURN DATE
IS
    V_DATE DATE;
BEGIN
    V_DATE := TO_DATE(V_STR_DATE, V_DATE_FORMAT);
    RETURN V_DATE;
EXCEPTION
    WHEN OTHERS THEN
    V_DATE := TO_DATE('9999-12-31', 'YYYYMMDD');
    RETURN V_DATE;
END;
```
V_STR_DATE를 V_DATE_FORMAT 형식으로 변환 성공시 변환된 값(V_DATE)을 리턴, 에러시 임의의 DATE '9999/12/31'을 리턴한다.  


# 참고자료
- https://gimmotti.tistory.com/2