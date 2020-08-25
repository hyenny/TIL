 # VIEW(뷰)

> **오라클로 배우는 데이터베이스 개론과 실습**을 읽고 정리한 내용입니다.

#

뷰(VIEW)는 하나 이상의 테이블을 합하여 만든 가상의 테이블이다. 실제 테이블과 달리 물리적인 디스크에 실제 데이터를 저장하지 않고, 뷰를 생성할 때 사용한 SELECT문의 정의를 DBMS가 저장한다.

### 장점
- 편리성 : 미리 정의된 뷰를 일반 테이블처럼 사용할 수 있기 때문에 편리하다.또 사용자가 필요한 정보만 요구에 맞게 가공하여 뷰로 만들어 쓸 수 있다.
- 재사용성 : 자주 사용되는 질의를 뷰로 미리 정의해 놓을 수 있다.
- 보안성 : 각 사용자별로 필요한 데이터만 선별하여 보여줄 수 있다.

#

## 1. 뷰의 생성

### 문법
```SQL
CREATE VIEW 뷰이름 [(열이름 [,...n])]
AS SELECT 문
```

### 예제
1. 세 개의 테이블을 조인해 VORDERS VIEW 생성
```SQL
CREATE VIEW VORDERS
AS SELECT ORDERID, O.CUSTID, NAME, O.BOOKID, BOOKNAME, SALEPRICE, ORDERDATE
FROM CUSTOMER C, ORDERS O, BOOK B
WHERE C.CUSTID = O.CUSTID AND B.BOOKID = O.BOOKID;
```
- CUSTOMER, ORDERS, BOOK 3개의 테이블을 조인해 각 테이블에서 필요한 컬럼을 조회하여 VIEW를 생성한다.    


2. 조건에 맞는 데이터만 보여주는 VW_BOOK VIEW 생성
```SQL
CREATE VIEW VW_BOOK
AS SELECT *
    FROM BOOK
    WHERE BOOKNAME LIKE '%축구%';
```
- BOOK 테이블에 '축구'라는 문구를 포함한 도서가 새로 추가될 경우 이 데이터는 뷰에 나타난다.
- '축구'라는 문구가 포함되지 않으면 BOOK 테이블에는 존재하지만 뷰에서는 나타나지 않는다.

#

## 2. 뷰의 수정
뷰의 수정은 CREATE VIEW 문에 OR REPLACE 명령을 더해 작성한다.

### 문법
```SQL
CREATE OR REPLACE VIEW 뷰이름 [(열이름 [,...n])]
AS SELECT문
```

#

## 3. 뷰의 삭제

### 문법
```SQL
DROP VIEW 뷰이름 [,...n];
```

### 예제
```SQL
DROP VIEW VW_BOOK;
```
#

# 정리
- 뷰는 하나 이상의 테이블을 합쳐 만든 가상의 테이블이다. 
- 뷰는 실제 데이터가 저장되는 게 아니라 뷰의 정의가 DBMS에 저장된다.
- 사용자가 필요한 정보 혹은 자주 사용되는 질의를 뷰로 정의해 놓으면 편리하고 재사용할 수 있다.
- 필요한 데이터만 골라서 보여줄 수 있기 때문에 보안성이 높다.