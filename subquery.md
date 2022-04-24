# 서브쿼리

## 단일 행 서브쿼리

단일 행 서브 쿼리는 서브쿼리 SELECT문에서 얻은 한 개 행의 결괏값을 메인 쿼리로 전달하는 서브쿼리. 

조건식인 WERE절에 기술되는 열의 개수와 데이터 타입은 메인쿼리와 서브쿼리가 서로 같아야 함.

```SQL
SELECT *
FROM employees A

    SELECT salary
    FROM emplyess
    WHERE last_name='De Haan'
);
```

만약  last_name이 De Haan이 두명 이상이면 다중 행 서브쿼리를 적용해야함

 &nbsp;

## 다중 행 서브쿼리
사용법은 단일 행 버스쿼리와 같음

다중행연산자 | 설명 | 예
-----------|-----------|-----------|
IN | 같은 값 아님 | IN(10,20) $\rightarrow$ 10이나 20이 포함
NOT IN | 같은 값이 아님 | NOT IN(10,20) $\rightarrow$ 10이나 20이 포함되지 않음
EXISTS | 값이 있으면 반환 | EXISTS(10) $\rightarrow$ 10이 존재하면 참
ANY | OR 개념. <,= 등과 같이 사용 | ANY(10,20) $\rightarrow$ 10이나 20이 포함
ALL | AND 개념. <,= 등과 같이 사용 | ALL(10,20) $\rightarrow$ 10과 20이 포함

```SQL
SELECT *
FROM employees A
WHERE A.salary IN (
    SELECT MIN(salary) 최저급여
    FROM employees
    GROUP BY department_id
);
```
 &nbsp;
## 다중 열 서브쿼리
메인 쿼리와 서브쿼리를 비교하는 WHERE 조건식에서 비교되는 열이 여러 개일 때 사용.

```SQL
SELECT *
FROM employees A
WHERE (A.job_id, A.salary) IN (
    SELECT job_id, MIN(salary) 그룹별급여
    FROM employees
    GROUP BY job_id
);
```
 &nbsp;

## FROM 절 서브쿼리: 인라인 뷰
FROM 절에서 테이블을 기술해서 해당 테이블의 데이터 값을 불러올 수 있음. FROM 절에 서브쿼리를 사용하면 특정 조건식을 갖는 SELECT 문을 테이블처럼 사용할 수 있음.

```SQL
SELECT *
FROM employees AS A, (
    SELECT department_id
    FROM departments
    WHERE department_name='IT' AS B
    )
WHERE A.deaprtment_id = B.deaprtment_id;
```
 &nbsp;
# DML
 &nbsp;
## DML
DML(Data Manipulation Language)은 말 그대로 데이터를 조작하는 명령어. INSERT, DELETE, UPDATE는 데이터를 직접 삽입, 삭제, 갱신하는 명령어. 데이터를 조작하여 저장하는 이령읜 과정을 Transaction이라고 부름.
 &nbsp;
## INSERT
테이블에 새로운 행 삽입.

```SQL
INSERT INTO departments (depart,emt_id, department_name, manager_id, location_id)
VALUES (271,'Sample_Dept', 200, 1700);
```

```SQL
INSERT INTO departments
VALUES (272, 'Sample_Dept', 200, 1700);
```

이러한 DML명령어를 최종적으로 데이터베이스에 반영하려면 commit해야함.

```SQL
commit;
```
 &nbsp;
## UPDATE
UPDATE는 기존의 데이터 값을 다른 데이터 값으로 변경할 때 사용.

```SQL
UPDATE departments
SET manager_id=201,
    location_id=1800
WHERE department_name='Sample_Dept';
```

```SQL
UPDATE departments
SET (manager_id,location_id)=(SELECT manager_id, location_id
                                FROM departments
                                WHERE department_id=40)
WHERE department_name='Sample_Dept';
```
 &nbsp;
 
 ## DELETE
 DELETE는 테이블의 데이터를 삭제할 때 사용.

```SQL
DELETE FROM departments
WHERE department_name='Sample_Dept';
```
혹은
```SQL
DELETE FROM departments
WHERE department_name IN (SELECT department_id
                            FROM departments
                            WHERE dpartment_name='Sample_Dept');
```
 &nbsp;

# DDL

테이블과 관련 열을 생성하고 변경하고 삭제하는 명령어를 Data Definition Language라고 함.
 &nbsp;
## CREATE
새로운 태이븡르 생성

```SQL
CREATE TABLE sample_product
        (
            product_id number,
            product_name varchar2(30),
            manu_date date
        );
```
값 INSERT
```SQL
INSERT INTO sample_product VALUES (1, 'television', to_date('140101', 'YYMMDD'))
INSERT INTO sample_product VALUES (2, 'washer', to_date('150101', 'YYMMDD'))
INSERT INTO sample_product VALUES (3, 'cleaner', to_date('160101', 'YYMMDD'))
```
&nbsp;
## ALTER

이미 생성한 테이블에 열을 추가, 변경, 삭제.
&nbsp;
### 열 추가
```SQL
ALTER TABLE sample_product ADD (factory varchar(10));
```
&nbsp;
### 열 수정
```SQL
ALTER TABLE sample_product MODIFY (factory char(10));
```
&nbsp;
### 열 이름 바꾸기

```SQL
ALTER TABLE sample_product RENAME COLUMN factory to factory_name;
```
&nbsp;
### 열 삭제하기

```SQL
ALTER TABLE sample_product DROP COUMN factory_name; 
```
&nbsp;
## TRUNCATE

테이블의 데이터를 모두 삭제하고 사용하던 기억 공간도 해제. 테이블에 생성된 인덱스와 같은 객체도 같이 삭제.

```SQL
TRUNCATE TABLE sample_product;
```
&nbsp;
## DROP

테이블을 완전히 삭제.

```SQL
DROP TABLE sample_product;
```

