# PL-SQL-ORACLE-Example

## Block

    DECLARE
      Declartion statement
    BEGIN 
      Executable statement
    Exceptions
      Exception handling statement
    END;

- A PL/SQL block without a name is Anonymous block. 
- A block that has a name is called a Stored Procedure or Function.

## Variable

#### Declaring variables

    variablename datatype;

...

    SET SERVEROUTPUT ON;

    DECLARE 
        vString VARCHAR2(20);
        vNumber NUMBER(20);
    BEGIN
       NULL;
    END;

####  Variable checking

    SET SERVEROUTPUT ON;

    DECLARE 
        vString VARCHAR2(20) NOT NULL;
        vNumber NUMBER(20) NOT NULL;
    BEGIN
       NULL;
    END;

#### Variables with initial or default value

    SET SERVEROUTPUT ON;

    DECLARE 
        vString VARCHAR2(20) := 'Hello world';
        vNumber NUMBER(20) := 2020;
    BEGIN
        dbms_output.put_line(vString);
        dbms_output.put_line(vNumber);
    END;
    /
     DECLARE 
        vString VARCHAR2(20) DEFAULT 'Hello world';
        vNumber NUMBER(20) DEFAULT 2020;
    BEGIN
        dbms_output.put_line(vString);
        dbms_output.put_line(vNumber);
    END;

#### Variables constants

you must initialize a constants at its declaration 

    SET SERVEROUTPUT ON;

    DECLARE 
        vPI CONSTANT NUMBER(7,6) := 3.141592;
        vMsg CONSTANT NVARCHAR2(20) := 'This is constants';
    BEGIN
        dbms_output.put_line(vPI);
        dbms_output.put_line(vMsg);
    END;
    /
    DECLARE 
        vPI CONSTANT NUMBER(7,6) DEFAULT 3.141592;
        vMsg CONSTANT NVARCHAR2(20) DEFAULT 'This is constants';
    BEGIN
        dbms_output.put_line(vPI);
        dbms_output.put_line(vMsg);
    END;
  

## Data type

#### %TYPE

    SET SERVEROUTPUT ON;

    DECLARE 
        vUserId NUMBER(20,0);
        vUsername VARCHAR2(100);
        vPassword VARCHAR2(200);
        vEnabled NUMBER(1,0);
        vCreateDate TIMESTAMP(6);
        vAcountExpiredDate DATE;
    BEGIN
        SELECT 
            USER_ID, 
            username,
            password, 
            ENABLED, 
            CREATE_DATE, 
            ACCOUNT_EXPIRED_DATE
            into 
            vUserId, 
            vUsername, 
            vPassword, 
            vEnabled, 
            vCreateDate, 
            vAcountExpiredDate
        FROM USERS WHERE username = 'tirmizee';
        dbms_output.put_line(vUserId);
        dbms_output.put_line(vUsername);
        dbms_output.put_line(vPassword);
        dbms_output.put_line(vEnabled);
        dbms_output.put_line(vCreateDate);
        dbms_output.put_line(vAcountExpiredDate);
    END;
    /
    DECLARE 
        vUserId USERS.USER_ID%TYPE;
        vUsername USERS.username%TYPE;
        vPassword USERS.password%TYPE;
        vEnabled USERS.ENABLED%TYPE;
        vCreateDate USERS.CREATE_DATE%TYPE;
        vAcountExpiredDate USERS.ACCOUNT_EXPIRED_DATE%TYPE;
    BEGIN
        SELECT 
            USER_ID, 
            username,
            password, 
            ENABLED, 
            CREATE_DATE, 
            ACCOUNT_EXPIRED_DATE
            into 
            vUserId, 
            vUsername, 
            vPassword, 
            vEnabled, 
            vCreateDate, 
            vAcountExpiredDate
        FROM USERS WHERE username = 'tirmizee';
        dbms_output.put_line(vUserId);
        dbms_output.put_line(vUsername);
        dbms_output.put_line(vPassword);
        dbms_output.put_line(vEnabled);
        dbms_output.put_line(vCreateDate);
        dbms_output.put_line(vAcountExpiredDate);
    END;

#### %ROWTYPE

    SET SERVEROUTPUT ON;

    DECLARE 
        vEmpId NUMBER(19,0);
        vEmpCode VARCHAR2(20 BYTE);
        vEmpFirstName VARCHAR2(100 BYTE);
        vEmpLastName VARCHAR2(100 BYTE);
        vManagerId NUMBER(19,0);
    BEGIN
        SELECT * INTO
           vEmpId,
           vEmpCode,
           vEmpFirstName,
           vEmpLastName,
           vManagerId
        FROM employee WHERE EMP_ID = 1;
        dbms_output.put_line(vEmpId);
        dbms_output.put_line(vEmpCode);
        dbms_output.put_line(vEmpFirstName);
        dbms_output.put_line(vEmpLastName);
        dbms_output.put_line(vManagerId);
    END;
    /
    DECLARE 
        tempEmp employee%ROWTYPE;
    BEGIN
        SELECT * INTO tempEmp FROM employee WHERE EMP_ID = 1;
        dbms_output.put_line(tempEmp.EMP_ID);
        dbms_output.put_line(tempEmp.EMP_CODE);
        dbms_output.put_line(tempEmp.EMP_FIRST_NAME);
        dbms_output.put_line(tempEmp.EMP_LAST_NAME);
        dbms_output.put_line(tempEmp.MANAGER_ID);
    END;

#### Record type

    TYPE type_name IS RECORD (
       field_name data_type,
       field_name data_type,
       ...N
    );
.

    SET SERVEROUTPU ON;

    DECLARE
        TYPE customType IS RECORD (
            username users.username%TYPE,
            ENABLED users.enabled%TYPE,
            FIRST_NAME profile.first_name%TYPE,
            LAST_NAME profile.last_name%TYPE,
            CITIZEN_ID profile.citizen_id%TYPE,
            TEL profile.tel%TYPE,
            EMAIL profile.email %TYPE
        );
        vDetail customType;
        CURSOR CURSOR_DETAIL(pLimit number := 1) IS 
            SELECT
                u.username,
                u.ENABLED,
                p.FIRST_NAME,
                p.LAST_NAME,
                p.CITIZEN_ID,
                p.TEL,
                p.EMAIL
            FROM users u
            INNER JOIN profile p ON u.profile_id = p.profile_id
            WHERE ROWNUM <= pLimit;
    BEGIN
        OPEN CURSOR_DETAIL(10);
            LOOP 
                FETCH CURSOR_DETAIL INTO vDetail;
                EXIT WHEN CURSOR_DETAIL%NOTFOUND;
                dbms_output.put_line(vdetail.username);
                dbms_output.put_line(vdetail.ENABLED);
                dbms_output.put_line(vdetail.FIRST_NAME);
                dbms_output.put_line(vdetail.LAST_NAME);
                dbms_output.put_line(vdetail.CITIZEN_ID);
                dbms_output.put_line(vdetail.TEL);
                dbms_output.put_line(vdetail.EMAIL);
                dbms_output.put_line('END ROW');
            END LOOP;
        CLOSE CURSOR_DETAIL;
        FOR temp IN CURSOR_DETAIL(2) LOOP
            vDetail := temp;
            dbms_output.put_line(vdetail.username);
            dbms_output.put_line(vdetail.ENABLED);
            dbms_output.put_line(vdetail.FIRST_NAME);
            dbms_output.put_line(vdetail.LAST_NAME);
            dbms_output.put_line(vdetail.CITIZEN_ID);
            dbms_output.put_line(vdetail.TEL);
            dbms_output.put_line(vdetail.EMAIL);
            dbms_output.put_line('END ROW2');
        END LOOP;
    END;

#### Collection Data Types

A homogeneous single dimension data structure which is made up of elements of same datatype is called collection in Oracle Database. In simple language we can say that, an array in Oracle Database is called Collection.

<p align="center">
  <img src="https://user-images.githubusercontent.com/15135199/97087455-93f06000-1654-11eb-97ed-0bfb2b2367d7.png" width="650">
</p>

- <b>Nested Tables</b> : List
- <b>Variable Sized Arrays or VARRAYs</b> : Array
- <b>Associative arrays</b> : Map

#### Collection Methods

- <b>EXISTS</b> :
- <b>COUNT</b> :
- <b>LIMIT</b> :
- <b>FIRST</b> :
- <b>LAST</b> :
- <b>EXTEND</b> :
- <b>DELETE</b> :
- <b>PRIOR</b> :
- <b>NEXT</b> :
- <b>TRIM</b> : 
- <b>DELETE</b> :

#### Nested Tables

A Nested table is a collection in which the size of the array is not fixed. It has the numeric subscript type. 

#### Nested Table syntax

    TYPE nested_table_name IS TABLE OF element_type [NOT NULL];

#### Nested table as Database Object 

    CREATE OR REPLACE TYPE NUMBERS_TYPE IS TABLE OF NUMBER;

#### Nested table with simple

    SET SERVEROUTPUT ON;
    DECLARE
        TYPE NUMBERS_TYPE IS TABLE OF NUMBER;
        v_numbers  NUMBERS_TYPE DEFAULT NUMBERS_TYPE (9,18,27,36,45,54,63,72,81,90);
    BEGIN
        DBMS_OUTPUT.PUT_LINE ('Value Stored at index 1 in NT is ' ||v_numbers (1)); 
        DBMS_OUTPUT.PUT_LINE ('Value Stored at index 2 in NT is ' ||v_numbers (2));
        DBMS_OUTPUT.PUT_LINE ('Value Stored at index 3 in NT is ' ||v_numbers (3));
        DBMS_OUTPUT.PUT_LINE ('Value Stored at index 4 in NT is ' ||v_numbers (5));
        DBMS_OUTPUT.PUT_LINE ('Value Stored at index 5 in NT is ' ||v_numbers (6));

        FOR i IN 1..v_numbers.COUNT LOOP
            DBMS_OUTPUT.PUT_LINE ('Value Stored at index 1 in NT is ' ||v_numbers(i)); 
        END LOOP;

    END;

#### Nested table (delete and exist)

    SET SERVEROUTPUT ON;
    DECLARE
        TYPE LIST_TYPE IS TABLE OF VARCHAR2(10);
        V_LIST LIST_TYPE DEFAULT LIST_TYPE('A',NULL,'C',NULL,'E');
    BEGIN
        FOR i IN V_LIST.FIRST..V_LIST.LAST LOOP
            DBMS_OUTPUT.PUT_LINE('Value First '||V_LIST.FIRST||', Last '||V_LIST.LAST||', Count '|| V_LIST.COUNT || ', Limit ' || V_LIST.LIMIT ); 
            IF V_LIST(i) IS NOT NULL THEN
                DBMS_OUTPUT.PUT_LINE('Stored at index ' || i || ' in NT is ' ||V_LIST (i)); 
            ELSE 
                DBMS_OUTPUT.PUT_LINE('Stored at index ' || i || ' in NT is NULL'); 
            END IF;
        END LOOP;
    END;
    /
    DECLARE
        TYPE LIST_TYPE IS TABLE OF VARCHAR2(10);
        V_LIST LIST_TYPE DEFAULT LIST_TYPE('A','B','C','D','E');
    BEGIN
        V_LIST.DELETE(2,3);
        FOR i IN 1..V_LIST.LAST LOOP
            DBMS_OUTPUT.PUT_LINE('Value First '||V_LIST.FIRST||', Last '||V_LIST.LAST||', Count '|| V_LIST.COUNT || ', Limit ' || V_LIST.LIMIT );
            IF V_LIST.EXISTS(i)THEN
                DBMS_OUTPUT.PUT_LINE('Stored at index ' || i || ' in NT is ' ||V_LIST (i)); 
            ELSE 
                DBMS_OUTPUT.PUT_LINE('Stored at index ' || i || ' in NT is NULL'); 
            END IF;
        END LOOP;
    END;

#### VARRAYs Type

Varray is a collection method in which the size of the array is fixed. The array size cannot be exceeded than its fixed value.     
    
    TYPE type_name IS [VARRAY | VARYING ARRAY] (size_limit) OF element_type;
.

    SET SERVEROUTPUT ON;
    DECLARE
        TYPE ARRAYS_TYPE IS VARRAY(5) OF NVARCHAR2(5);
        V_ARRAYS ARRAYS_TYPE DEFAULT ARRAYS_TYPE('A','B','C','D',NULL);
    BEGIN
        FOR i IN 1..V_ARRAYS.COUNT LOOP
            DBMS_OUTPUT.PUT_LINE ('Value Stored at index ' || i || ' in NT is ' ||V_ARRAYS (i)); 
        END LOOP;
        FOR i IN 1..V_ARRAYS.LIMIT LOOP
            DBMS_OUTPUT.PUT_LINE ('Value Stored at index ' || i || ' in NT is ' ||V_ARRAYS (i)); 
        END LOOP;
    END;

####  Associative array Type

    TYPE aArray_name IS TABLE OF element_datatype [Not Null] INDEX BY index_elements_datatype;

####  Associative array with simple 

    SET SERVEROUTPUT ON;
    DECLARE
        TYPE books IS TABLE OF NUMBER INDEX BY VARCHAR2 (20);
        isbn books;
    BEGIN
    -- How to insert data into the associative array 
        isbn('Oracle Database') := -999;
        isbn('MySQL') := 999; 
        DBMS_OUTPUT.PUT_LINE ('Value '||isbn ('Oracle Database'));
        DBMS_OUTPUT.PUT_LINE ('Value '||isbn ('MySQL'));
    END;
    /
    DECLARE
        TYPE books IS TABLE OF VARCHAR2 (20) INDEX BY PLS_INTEGER;
        isbn books;
    BEGIN
    -- How to insert data into the associative array 
        isbn(1) := 'Oracle Database';
        isbn(2) := 'MySQL'; 
        DBMS_OUTPUT.PUT_LINE ('Value '||isbn (1));
        DBMS_OUTPUT.PUT_LINE ('Value '||isbn (2));
    END;

## Conditional Statements

#### IF - THEN

    IF condition THEN
        statement;
        ...
    END IF;

...

    SET SERVEROUTPU ON;

    DECLARE
        vAge number(2,0) := 26;
    BEGIN
        IF vAge = 26 THEN
            dbms_output.put_line('Age is ' || vAge);
            dbms_output.put_line('Age is ' || vAge);
        END IF;
        dbms_output.put_line('Finish');
    END;
    /
    DECLARE
        vFirstName VARCHAR2(100) := 'Pratya';
        vLastName VARCHAR2(100) := 'Yeekhaday';
    BEGIN
        IF vFirstName = 'Pratya' AND vLastName = 'Yeekhaday' THEN
            dbms_output.put_line('Fullname is ' || vFirstName || ' ' ||vlastname);
        END IF;
        dbms_output.put_line('Finish');
    END;

#### IF-THEN-ELSE

    IF condition THEN
        statement;
    ELSE
        statement;
    END IF;
...

    SET SERVEROUTPU ON;

    DECLARE
        vNumber NUMBER(3) := 2;
    BEGIN
        IF MOD(vNumber, 2) = 0 THEN
            dbms_output.put_line('vNumber is event');
        ELSE 
            dbms_output.put_line('vNumber is odd');
        END IF;
        dbms_output.put_line('Finish');
    END;
    
#### IF-THEN-ELSIF    

    IF condition1 THEN
        statement;
    ELSIF condition2 THEN
        statement;
    ELSIF condition3 THEN
        statement;
    ELSE
        statement;
    END IF;

...

    SET SERVEROUTPU ON;

    DECLARE
        vScore NUMBER(3) := 90;
    BEGIN
        IF vScore >= 80 THEN
            dbms_output.put_line('Grade A');
        ELSIF vScore >= 70 THEN
            dbms_output.put_line('Grade B');
        ELSIF vScore >= 60 THEN
            dbms_output.put_line('Grade C');
        ELSIF vScore >= 50 THEN
            dbms_output.put_line('Grade D');
        ELSE
            dbms_output.put_line('Grade F');
        END IF;
        dbms_output.put_line('Finish');
    END;
    
#### Case Expression

    CASE search_expression
        WHEN input_expression THEN statement;
        ....
        ELSE statement;
    END CASE;

##### Simple Case Expression

    SET SERVEROUTPU ON;

    DECLARE
        vScore NUMBER(3) := 2;
        vResult NVARCHAR2(10);
    BEGIN
        CASE vscore
            WHEN 1 THEN vResult := 'ONE';
            WHEN 2 THEN vResult := 'TWO';
            WHEN 3 THEN vResult := 'THREE';
            WHEN 4 THEN vResult := 'FOUR';
            WHEN 5 THEN vResult := 'FIVE';
            WHEN 6 THEN vResult := 'SIX';
            WHEN 7 THEN vResult := 'SEVEN';
            WHEN 8 THEN vResult := 'EIGHT';
            ELSE vResult := 'ERROR';
        END CASE;
        dbms_output.put_line(vResult);
    END;
    /
    DECLARE
        vScore NVARCHAR2(20) := 'THREE';
        vResult NUMBER(2);
    BEGIN
        CASE vscore
            WHEN 'ONE'    THEN vResult := 1;
            WHEN 'TWO'    THEN vResult := 2;
            WHEN 'THREE'  THEN vResult := 3;
            WHEN 'FOUR'   THEN vResult := 4;
            WHEN 'FIVE'   THEN vResult := 5;
            WHEN 'SIX'    THEN vResult := 6;
            WHEN 'SEVEN'  THEN vResult := 7;
            WHEN 'EIGHT'  THEN vResult := 8;
            ELSE vResult := 'ERROR';
        END CASE;
        dbms_output.put_line(vResult);
    END;

##### Searched Case Expression

    SET SERVEROUTPU ON;

    DECLARE
        vFirstname NVARCHAR2(50) := 'Pratya';
        vLastname NVARCHAR2(50) := 'Yeekhaday';
        vResult NVARCHAR2(50);
    BEGIN
        CASE 
            WHEN vFirstname = 'Pratya' AND vlastname = 'Yeekhaday' THEN vResult := 'Tirmizee';
            WHEN vFirstname = 'Pratya1' AND vlastname = 'Yeekhaday1' THEN vResult := 'Tirmizee1';
            WHEN vFirstname = 'Pratya2' AND vlastname = 'Yeekhaday2' THEN vResult := 'Tirmizee2';
            ELSE vResult := 'ERROR';
        END CASE;
        dbms_output.put_line(vResult);
    END;
    /
    DECLARE
        vText NVARCHAR2(20) := 'THREE';
        vNumber NUMBER(2) := 3;
        vResult NUMBER(2);
    BEGIN
        CASE 
            WHEN vText= 'ONE'   AND vNumber = 1 THEN vResult := 1;
            WHEN vText= 'TWO'   AND vNumber = 2 THEN vResult := 2;
            WHEN vText= 'THREE' AND vNumber = 3 THEN vResult := 3;
            WHEN vText= 'FOUR'  AND vNumber = 4 THEN vResult := 4;
            WHEN vText= 'FIVE'  AND vNumber = 5 THEN vResult := 5;
            WHEN vText= 'SIX'   AND vNumber = 6 THEN vResult := 6;
            WHEN vText= 'SEVEN' AND vNumber = 7 THEN vResult := 7;
            WHEN vText= 'EIGHT' AND vNumber = 8 THEN vResult := 8;
            ELSE vResult := 'ERROR';
        END CASE;
        dbms_output.put_line(vResult);
    END;

## LOOP statement

#### Simple loop

    LOOP
        statement1;
        statement2;
        EXIT statement;
    END LOOP;

##### Simple loop with exit

    SET SERVEROUTPU ON;

    DECLARE
        vCount NUMBER(3) DEFAULT 0;
        vResult NUMBER;
    BEGIN
        LOOP
            vCount := vCount + 1;
            vResult := 19 * vCount;
            dbms_output.put_line('count '|| vCount || ' result ' || vResult);
            IF vCount >= 10 THEN
                EXIT;
            END IF;
        END LOOP;
        dbms_output.put_line('Finish');
    END;
    
##### Simple loop with exit when

    DECLARE
        vCount NUMBER(3) DEFAULT 0;
        vResult NUMBER;
    BEGIN
        LOOP
            vCount := vCount + 1;
            vResult := 19 * vCount;
            dbms_output.put_line('count '|| vCount || ' result ' || vResult);
            EXIT WHEN vCount >= 10;
        END LOOP;
        dbms_output.put_line('Finish');
    END;

#### While loop

    WHILE diffcondition LOOP
        statement1;
        statement1;
    END LOOP;

...

    SET SERVEROUTPU ON;

    DECLARE
        vCount NUMBER(3) DEFAULT 0;
        vResult NUMBER;
    BEGIN
        WHILE vCount <= 10 LOOP
            vResult := 19 * vCount;
            dbms_output.put_line('count '|| vCount || ' result ' || vResult);
            vCount := vCount + 1;
        END LOOP;
        dbms_output.put_line('Finish');
    END;
    /
    DECLARE
        vTest BOOLEAN DEFAULT TRUE;
        vCount NUMBER(3) DEFAULT 0;
        vResult NUMBER;
    BEGIN
        WHILE vTest LOOP
            vResult := 19 * vCount;
            dbms_output.put_line('count '|| vCount || ' result ' || vResult);
            vCount := vCount + 1;
            IF vCount >= 10 THEN
                vTest := FALSE;
            END IF;
        END LOOP;
        dbms_output.put_line('Finish');
    END;

#### FOR Loop

    FOR count IN [REVERSE] lower .. upper LOOP
        statement;
        statement;
    END LOOP;

...

    SET SERVEROUTPU ON;

    BEGIN
        FOR counter IN 1..10 LOOP
            dbms_output.put_line('counter ' || counter);
        END LOOP;
        dbms_output.put_line('Finish');
    END;
    /
    BEGIN
        FOR counter IN REVERSE 1..10 LOOP
            dbms_output.put_line('counter ' || counter);
        END LOOP;
        dbms_output.put_line('Finish');
    END;

#### FORALL

FORALL statement helps to process bulk data in an optimized manner by sending DML statements or a MERGE statement (if you are using 11g or above) in batches from PL/SQL engine to SQL engine. <b>FORALL is not FOR LOOP</b>

#### FORALL syntax 

    FORALL index IN bound_clauses 
        [SAVE EXCEPTION]
        DML statement;
- <b>SAVE EXCEPTION</b> is an optional choice which keeps the FORALL statement running even when DML statement causes an exception. These exceptions are saved in a cursor attribute called SQL%Bulk_Exceptions.
- <b>DML Statement</b> DML statement could be any DML statement like INSERT, UPDATE or DELETE

#### FORALL simple with insert   

    SET SERVEROUTPUT ON;
    DECLARE
        TYPE TYPE_EMPLOYEE IS TABLE OF employee%ROWTYPE ;

        V_LIST_EMPLOYEE   TYPE_EMPLOYEE  DEFAULT TYPE_EMPLOYEE();
        V_SIZE            NUMBER         DEFAULT 100000;
    BEGIN
        V_LIST_EMPLOYEE.EXTEND(V_SIZE); 
        FOR i IN 1..V_SIZE LOOP
            V_LIST_EMPLOYEE(i).EMP_CODE        := 'EM999';
            V_LIST_EMPLOYEE(i).EMP_FIRST_NAME  := 'TEMP';
            V_LIST_EMPLOYEE(i).EMP_LAST_NAME   := 'TEMP';
        END LOOP;

        FORALL i IN 1..V_SIZE
            INSERT INTO employee VALUES V_LIST_EMPLOYEE(i);
        ROLLBACK;
    END;

#### FORALL simple with update   

    SET SERVEROUTPUT ON;
    DECLARE
        TYPE TYPE_EMPLOYEE IS TABLE OF employee%ROWTYPE ;

        V_LIST_EMPLOYEE   TYPE_EMPLOYEE;
        V_SIZE            NUMBER DEFAULT 100000;
    BEGIN
        SELECT * BULK COLLECT INTO V_LIST_EMPLOYEE FROM employee;
        FOR i IN 1..V_LIST_EMPLOYEE.COUNT LOOP
            V_LIST_EMPLOYEE(i).MANAGER_ID := 1;
        END LOOP;

        FORALL i IN 1..V_LIST_EMPLOYEE.COUNT
            UPDATE employee SET 
                EMP_CODE = V_LIST_EMPLOYEE(i).EMP_CODE,
                EMP_FIRST_NAME = V_LIST_EMPLOYEE(i).EMP_FIRST_NAME,
                EMP_LAST_NAME = V_LIST_EMPLOYEE(i).EMP_LAST_NAME,
                MANAGER_ID = V_LIST_EMPLOYEE(i).MANAGER_ID
            WHERE EMP_ID = V_LIST_EMPLOYEE(i).EMP_ID;
        COMMIT;
    END;

### Select into

    SET SERVEROUTPUT ON;

    DECLARE 
        vString VARCHAR2(200);
    BEGIN
       SELECT password into vString from USERS WHERE username = 'tirmizee';
       dbms_output.put_line(vString);
    END;
    /
    DECLARE 
        vString VARCHAR2(200);
        vNumber NUMBER(1,0);
    BEGIN
        SELECT 
            password, 
            enabled 
            into 
            vString, 
            vNumber  
        from USERS WHERE username = 'tirmizee';
        dbms_output.put_line(vString);
        dbms_output.put_line(vNumber);
    END;

### Execute Immediate

    SET SERVEROUTPUT ON;
    DECLARE
        temp employee%ROWTYPE;
        vDynamicQuery NVARCHAR2(200);
    BEGIN
        vDynamicQuery := 'SELECT * FROM EMPLOYEE  WHERE EMP_CODE   = :EMP_CODE AND EMP_FIRST_NAME = :EMP_FIRST_NAME';
        EXECUTE IMMEDIATE vDynamicQuery INTO temp USING 'EM001', 'Pratyaya' ;
        DBMS_OUTPUT.PUT_LINE (temp.EMP_FIRST_NAME);
    EXCEPTION
        WHEN NO_DATA_FOUND THEN
             DBMS_OUTPUT.PUT_LINE ('NO_DATA_FOUND');
    END;

#### Immediate Syntax

    EXECUTE IMMEDIATE <dynamic_query>
    [INTO <variable>]
    [USING <bind_variable_value>]
    [RETURNING|RETURN-INTO <clause>];  


#### Immediate with INTO

    SET SERVEROUTPUT ON;
    DECLARE
        temp employee%ROWTYPE;
        vDynamicQuery NVARCHAR2(200);
    BEGIN
        vDynamicQuery := 'SELECT * FROM EMPLOYEE  WHERE EMP_CODE = ' || q'['EM001']';
        EXECUTE IMMEDIATE vDynamicQuery INTO temp;
        DBMS_OUTPUT.PUT_LINE (temp.EMP_FIRST_NAME);
    END;
    /
    DECLARE
        temp NUMBER(3);
        vDynamicQuery NVARCHAR2(200);
    BEGIN
        vDynamicQuery := 'SELECT COUNT(*) FROM EMPLOYEE';
        EXECUTE IMMEDIATE vDynamicQuery INTO temp;
        DBMS_OUTPUT.PUT_LINE (temp);
    END;

#### Immediate with USING 

    SET SERVEROUTPUT ON;
    DECLARE
        temp employee%ROWTYPE;
        vDynamicQuery NVARCHAR2(200);
    BEGIN
        vDynamicQuery := 'SELECT * FROM EMPLOYEE  WHERE EMP_CODE   = :EMP_CODE AND EMP_FIRST_NAME = :EMP_FIRST_NAME';
        EXECUTE IMMEDIATE vDynamicQuery INTO temp USING 'EM001', 'Pratyaya' ;
        DBMS_OUTPUT.PUT_LINE (temp.EMP_FIRST_NAME);
    EXCEPTION
        WHEN NO_DATA_FOUND THEN
             DBMS_OUTPUT.PUT_LINE ('NO_DATA_FOUND');
    END;

#### Immediate with BULK COLLECT INTO

    SET SERVEROUTPUT ON;
    DECLARE
        TYPE LIST_TYPE IS TABLE OF NVARCHAR2(100);
        V_LIST_OF_CODE LIST_TYPE;
        V_QUERY NVARCHAR2(200);
    BEGIN
        V_QUERY := 'SELECT EMP_CODE FROM EMPLOYEE WHERE EMP_CODE LIKE :EMP_CODE';
        EXECUTE IMMEDIATE V_QUERY BULK COLLECT INTO V_LIST_OF_CODE USING '%0%';
        FOR I IN 1..V_LIST_OF_CODE.COUNT LOOP
            DBMS_OUTPUT.PUT_LINE (I || ' : ' || V_LIST_OF_CODE(I));
        END LOOP;
    END;
    /
    DECLARE
        TYPE LIST_TYPE IS TABLE OF EMPLOYEE%ROWTYPE;
        V_LIST_OF_EMPLOYEE LIST_TYPE;
        V_QUERY NVARCHAR2(200);
    BEGIN
        V_QUERY := 'SELECT * FROM EMPLOYEE WHERE EMP_CODE LIKE :EMP_CODE';
        EXECUTE IMMEDIATE V_QUERY BULK COLLECT INTO V_LIST_OF_EMPLOYEE USING '%0%';
        FOR I IN 1..V_LIST_OF_EMPLOYEE.COUNT LOOP
            DBMS_OUTPUT.PUT_LINE (I || ' : ' || V_LIST_OF_EMPLOYEE(I).EMP_CODE || ', ' || V_LIST_OF_EMPLOYEE(I).EMP_FIRST_NAME);
        END LOOP;
    END;

## CURSOR


<p align="center">
  <img src="https://user-images.githubusercontent.com/15135199/96413139-6733e700-1215-11eb-821d-00313cb60c8e.png" width="650">
</p>

A Cursor is a pointer to this context area
- <b>Implicit Cursor</b>
- <b>Explicit Cursor</b>

### Cursor Action

- <b>DECLARE:</b> the programmer actually creates a named context area for the Select statement. The select statement is declared in this section of the PL/SQL program.
- <b>OPEN:</b> In this section oracle actually allocates memory for the cursor.
- <b>FETCH:</b> In this section actual execution starts. The select statement fetches the records from the database and stores it in the allocated memory. The data is fetched record by record way. It is a record level activity. So the set of records is also called Active Set.
- <b>CLOSE:</b> This statement is used to close the cursor and the memory allocated to the cursor will be released.

### Cursor attributes 

- <b>%ROWCOUNT:</b> It tells how many rows are returned by the cursor.
- <b>%ISOPEN:</b> This attribute returns Boolean which means TRUE if the cursor is open or else FALSE.
- <b>%FOUND:</b> This attribute also returns Boolean. TRUE if successful fetch has been executed and FALSE if there is unsuccessful fetch (no row returned).
- <b>%NOTFOUND:</b> This attribute returns opposite of the %FOUND attribute. FALSE if the row is fetched and TRUE if no row is fetched.

#### Syntax

    CURSOR cursor_name IS select_statement;

#### Explicit Cursor fetch single row

    SET SERVEROUTPU ON;

    DECLARE
        vTemp employee%ROWTYPE;
        CURSOR CURSOR_EMPLOYEE IS 
        SELECT
            *
        FROM employee WHERE emp_id = 1; 
    BEGIN
        OPEN CURSOR_EMPLOYEE;
            FETCH CURSOR_EMPLOYEE INTO vTemp;
            dbms_output.put_line(vTemp.emp_code);
        CLOSE CURSOR_EMPLOYEE;
    END;

#### Explicit Cursor fetch multiple row

    SET SERVEROUTPU ON;

    DECLARE
        vFirstName NVARCHAR2(100);
        vLastName NVARCHAR2(100);
        CURSOR CURSOR_EMPLOYEE IS 
        SELECT
            emp_first_name,
            emp_last_name
        FROM employee;
    BEGIN
        OPEN CURSOR_EMPLOYEE;
            LOOP
                FETCH CURSOR_EMPLOYEE INTO vFirstName, vLastName; 
                EXIT WHEN CURSOR_EMPLOYEE%NOTFOUND;
                dbms_output.put_line(vFirstName || ' ' ||vLastName);
            END LOOP;
        CLOSE CURSOR_EMPLOYEE;
    END;

#### Static Cursor FOR Loop

    SET SERVEROUTPU ON;

    DECLARE
        CURSOR CURSOR_EMPLOYEE IS 
        SELECT
            *
        FROM employee WHERE emp_id > 1; 
    BEGIN
        FOR row_cursor IN CURSOR_EMPLOYEE LOOP
            dbms_output.put_line(row_cursor.emp_code || ' ' || row_cursor.EMP_FIRST_NAME);
        END LOOP;
    END;

#### Static Cursor FOR LOOP with a SELECT statement

    SET SERVEROUTPU ON;
    DECLARE
    BEGIN
        FOR row_cursor IN (
            SELECT
                *
            FROM employee WHERE emp_id > 150000
        ) LOOP
            dbms_output.put_line(row_cursor.emp_code || ' ' || row_cursor.EMP_FIRST_NAME);
        END LOOP;
    END;

#### Static Cursor with parameterized

    CURSOR cursor_name(parameter_name data_type, ..N) IS select_statement;
.
    
    SET SERVEROUTPU ON;

    DECLARE
        CURSOR CURSOR_EMPLOYEE(pEmpId NUMBER) IS 
        SELECT
            *
        FROM employee WHERE emp_id > pEmpId; 
    BEGIN
        FOR row_cursor IN CURSOR_EMPLOYEE(1) LOOP
            dbms_output.put_line(row_cursor.emp_code || ' ' || row_cursor.EMP_FIRST_NAME);
        END LOOP;
    END;
    /
    DECLARE
        CURSOR CURSOR_EMPLOYEE(pEmpId NUMBER, pName NVARCHAR2) IS 
        SELECT
            *
        FROM employee WHERE emp_id > pEmpId AND emp_first_name <> pName; 
    BEGIN
        FOR row_cursor IN CURSOR_EMPLOYEE(1,'PPP') LOOP
            dbms_output.put_line(row_cursor.emp_code || ' ' || row_cursor.EMP_FIRST_NAME);
        END LOOP;
    END;
   
#### Static Cursor with parameterized and default value   
   
    SET SERVEROUTPU ON;

    DECLARE
        CURSOR CURSOR_EMPLOYEE(pEmpId NUMBER := 1, pName NVARCHAR2 := 'PPP') IS 
        SELECT
            *
        FROM employee WHERE emp_id > pEmpId AND emp_first_name <> pName; 
    BEGIN
        FOR row_cursor IN CURSOR_EMPLOYEE LOOP
            dbms_output.put_line(row_cursor.emp_code || ' ' || row_cursor.EMP_FIRST_NAME);
        END LOOP;
    END;

### Ref Cursors

It is a PL/SQL datatype using which you can declare a special type of variable called Cursor Variable.

- <b>Strong Ref Cursor</b>
    
    Any Ref Cursor which has a fixed return type is called a Strong Ref Cursor.

- <b>Weak Ref Cursor</b>

    Weak ref cursors are those which do not have any return type.
    
| Ref Cursor | Cursor |
| ------------- | ------------- |
| Ref Cursor is PL/SQL data type | Oracle creates context area for processing an SQL statement which contains all information  |
| it is dynamic | it is static |
| can be changed at runtime | can not be change at runtime |
| can be returned to client | can not be returned to client |

#### Syntax of Strong Ref Cursors

    TYPE cursor_variable_name IS REF CURSOR RETURN (return_type);

#### Syntax of Weak Ref Cursors

    TYPE ref_cursor_name IS REF CURSOR;

#### Strong Ref Cursors

    SET SERVEROUTPU ON;
    DECLARE
        TYPE ref_employee IS REF CURSOR RETURN employee%ROWTYPE;
        cursor_employee ref_employee;
        vTemp employee%ROWTYPE;
    BEGIN
        OPEN cursor_employee FOR SELECT * FROM employee WHERE emp_id = 1;
            FETCH cursor_employee INTO vTemp;
            dbms_output.put_line(vTemp.EMP_CODE);
            dbms_output.put_line(vTemp.EMP_FIRST_NAME);
            dbms_output.put_line(vTemp.EMP_LAST_NAME);
        CLOSE cursor_employee;
    END;
    /
    DECLARE
        TYPE ref_employee IS REF CURSOR RETURN employee%ROWTYPE;
        cursor_employee ref_employee;
        vTemp employee%ROWTYPE;
    BEGIN
        OPEN cursor_employee FOR SELECT * FROM employee;
            LOOP 
                FETCH cursor_employee INTO vTemp;
                EXIT WHEN cursor_employee%NOTFOUND;
                dbms_output.put_line(vTemp.EMP_CODE);
                dbms_output.put_line(vTemp.EMP_FIRST_NAME);
                dbms_output.put_line(vTemp.EMP_LAST_NAME);
            END LOOP;
        CLOSE cursor_employee;
    END;

#### Strong Ref Cursors with Defined Record Datatype

    SET SERVEROUTPU ON;
    DECLARE
        TYPE type_detail IS RECORD (
            username users.username%TYPE,
            ENABLED users.enabled%TYPE,
            FIRST_NAME profile.first_name%TYPE,
            LAST_NAME profile.last_name%TYPE,
            CITIZEN_ID profile.citizen_id%TYPE,
            TEL profile.tel%TYPE,
            EMAIL profile.email%TYPE
        );
        TYPE ref_detail IS REF CURSOR RETURN type_detail;
        cursor_detail ref_detail;
        vTemp type_detail;
    BEGIN
        OPEN cursor_detail FOR 
            SELECT
                u.username,
                u.ENABLED,
                p.FIRST_NAME,
                p.LAST_NAME,
                p.CITIZEN_ID,
                p.TEL,
                p.EMAIL
            FROM users u
            INNER JOIN profile p ON u.profile_id = p.profile_id
            WHERE p.profile_id = 1;
            FETCH cursor_detail INTO vTemp;
            dbms_output.put_line(vTemp.username);
            dbms_output.put_line(vTemp.ENABLED);
            dbms_output.put_line(vTemp.FIRST_NAME);
            dbms_output.put_line(vTemp.LAST_NAME);
            dbms_output.put_line(vTemp.CITIZEN_ID);
            dbms_output.put_line(vTemp.TEL);
            dbms_output.put_line(vTemp.EMAIL);
        CLOSE cursor_detail;
    END;

#### Weak Ref Cursors

    SET SERVEROUTPU ON;
    DECLARE
        TYPE ref_employee IS REF CURSOR;
        cursor_employee ref_employee;
        vTemp employee%ROWTYPE;
    BEGIN
        OPEN cursor_employee FOR SELECT * FROM employee WHERE emp_id = 1;
            FETCH cursor_employee INTO vTemp;
            dbms_output.put_line(vTemp.EMP_CODE);
            dbms_output.put_line(vTemp.EMP_FIRST_NAME);
            dbms_output.put_line(vTemp.EMP_LAST_NAME);
        CLOSE cursor_employee;
    END;

#### Weak Ref Cursors SYS_REFCURSOR

SYS_REFCURSOR is a predefined weak ref cursor which comes built-in with the Oracle database software.

    SET SERVEROUTPU ON;
    DECLARE
        vFirstName NVARCHAR2(100);
        vLastName NVARCHAR2(100);
        CURSOR_EMPLOYEE SYS_REFCURSOR;
    BEGIN
        OPEN CURSOR_EMPLOYEE FOR 
            SELECT EMP_FIRST_NAME, EMP_LAST_NAME FROM employee;
            LOOP
                FETCH CURSOR_EMPLOYEE INTO vFirstName, vLastName;
                EXIT WHEN CURSOR_EMPLOYEE%NOTFOUND;
                DBMS_OUTPUT.PUT_LINE(CURSOR_EMPLOYEE%ROWCOUNT ||' ' || vFirstName ||' '||vLastName);
            END LOOP;
        CLOSE CURSOR_EMPLOYEE;
    END;
    
## Functions 

    CREATE [OR REPLACE] FUNCTION function_name(Parameter 1, Parameter 2,...) RETURN data_type
    IS
        Declare variable...;
    BEGIN
        executable statement;
        RETURN (value);
    END;

#### Functions with simple

    SET SERVEROUTPU ON;

    CREATE OR REPLACE FUNCTION funcAdd(num1 NUMBER, num2 NUMBER) RETURN NUMBER
    IS
        vResult NUMBER;
    BEGIN
        vResult := num1 + num2;
        RETURN vResult;
    END;
    /
    DECLARE
        vTemp NUMBER := funcAdd(10, 20);
    BEGIN
        dbms_output.put_line(vTemp);
    END;

#### Nested function
    
    CREATE OR REPLACE FUNCTION nested_func(p_number1 NUMBER DEFAULT 0, p_number2 NUMBER DEFAULT 0) RETURN NUMBER
    AS
        FUNCTION nested_func(p_number1 NUMBER DEFAULT 0, p_number2 NUMBER DEFAULT 0) RETURN NUMBER 
        AS
            results number;
        BEGIN
            results := p_number1 + p_number2;
            RETURN results;
        END nested_func;
    BEGIN
      RETURN nested_func(p_number1, p_number2);
    END;
    /
    SET SERVEROUTPUT ON;
    BEGIN
        dbms_output.put_line(nested_func(10, 20));
    END;

## Stored Procedure 

    CREATE [OR REPLACE] PROCEDURE procedure_name(Parameter 1, Parameter 2,...) 
    IS
        Declare variable...;
    BEGIN
        executable statement;
    EXCEPTION
        executable statement;
    END procedure_name;

#### Stored Procedure parameters

- <b>IN</b> type parameter sends values to a Stored Procedure.
- <b>OUT</b> type parameter gets values from the Stored Procedure.
- <b>IN OUT</b> type parameter sends and gets values from the procedure.

PL/SQL procedure has defined <b>IN</b> type as <b>default</b> parameter. 

#### Stored Procedure without parameters

    CREATE OR REPLACE PROCEDURE SIMPLE_PROCEDURE AS
        vCode NVARCHAR2(20) DEFAULT '001';
        vName NVARCHAR2(100) := 'Pratya Yeekhaday';
    BEGIN
        dbms_output.put_line(vCode);
        dbms_output.put_line(vName);
    END SIMPLE_PROCEDURE;
    /
    SET SERVEROUTPU ON;
    EXEC SIMPLE_PROCEDURE;
    
#### Stored Procedure with parameters   

PL/SQL procedure has defined IN type as default parameter.

    CREATE OR REPLACE PROCEDURE PROC_WITH_PARAMETER(pUserId NUMBER, pEnable NUMBER) 
    AS
        vUser USERS%ROWTYPE;
    BEGIN
        SELECT * INTO vUser FROM USERS WHERE user_id = pUserId AND enabled = pEnable;
        dbms_output.put_line(vUser.username);
        dbms_output.put_line(vUser.ENABLED);
    END PROC_WITH_PARAMETER;
    /
    SET SERVEROUTPU ON;
    EXEC PROC_WITH_PARAMETER(1,1);

#### Stored Procedure with parameters IN 

    CREATE OR REPLACE PROCEDURE PROC_WITH_PARAMETER(pUserId IN NUMBER , pEnable IN NUMBER) 
    AS
        vUser USERS%ROWTYPE;
    BEGIN
        SELECT * INTO vUser FROM USERS WHERE user_id = pUserId AND enabled = pEnable;
        dbms_output.put_line(vUser.username);
        dbms_output.put_line(vUser.ENABLED);
    END PROC_WITH_PARAMETER;
    /
    SET SERVEROUTPU ON;
    EXEC PROC_WITH_PARAMETER(1,1);

#### Stored Procedure with parameters OUT

    CREATE OR REPLACE PROCEDURE PROC_WITH_PARAMETER_OUT (
        inProfileId   IN  NUMBER , 
        outFirstName  OUT NVARCHAR2,
        outLastName   OUT NVARCHAR2,
        outCitizentId OUT NVARCHAR2,
        outEmail      OUT NVARCHAR2
    ) AS
        ecode   NUMBER;
        emesg   VARCHAR2(200);
    BEGIN
        SELECT 
            first_name,
            last_name,
            citizen_id,
            email
            INTO
            outFirstName,
            outLastName,
            outCitizentId,
            outEmail
        FROM profile WHERE profile_id = inProfileId;
        dbms_output.put_line('');
    EXCEPTION
        WHEN OTHERS THEN
            ecode := SQLCODE;
            emesg := SQLERRM;
            dbms_output.put_line( 'EXCEPTION '||TO_CHAR(ecode) || '-' || emesg);
    END PROC_WITH_PARAMETER_OUT;
    /
    SET SERVEROUTPU ON;

    DECLARE
        inProfileId   NUMBER(20,0) DEFAULT 1;
        outFirstName  NVARCHAR2(200);
        outLastName   NVARCHAR2(200);
        outCitizentId NVARCHAR2(200);
        outEmail      NVARCHAR2(200);
    BEGIN
        PROC_WITH_PARAMETER_OUT(inProfileId, outFirstName, outLastName, outCitizentId, outEmail);
        dbms_output.put_line(outFirstName);
        dbms_output.put_line(outLastName);
        dbms_output.put_line(outCitizentId);
        dbms_output.put_line(outEmail);
    END;

#### Stored Procedure with SYS_REFCURSOR parameters OUT

    CREATE OR REPLACE PROCEDURE PROC_REFCURSOR_OUT(pEefCur OUT SYS_REFCURSOR)
    AS
    BEGIN
        OPEN pEefCur FOR 
            SELECT EMP_FIRST_NAME,EMP_LAST_NAME FROM employee;
    END;
    /
    DECLARE
        ref_cur SYS_REFCURSOR;
        firstName employee.emp_first_name%TYPE;
        lastName employee.emp_last_name%TYPE;
    BEGIN
        PROC_REFCURSOR_OUT(ref_cur);
        LOOP
            FETCH ref_cur INTO firstName, lastName;
            EXIT WHEN ref_cur%NOTFOUND;
            dbms_output.put_line(firstName);
            dbms_output.put_line(lastName);
        END LOOP;
        CLOSE ref_cur;
    END;


#### Stored Procedure with handle exception

    CREATE OR REPLACE PROCEDURE SIMPLE_PROCEDURE AS
        vCode NVARCHAR2(20);
        vName NVARCHAR2(100);
    BEGIN
        vCode := '00100000000000000000000000000000000000000000000000000';
        vName := 'Pratya Yeekhaday';
        dbms_output.put_line(vCode);
        dbms_output.put_line(vName);
    EXCEPTION
      WHEN OTHERS THEN
        dbms_output.put_line('EXCEPTION' || SQLCODE || ' : ' || SQLERRM);
    END SIMPLE_PROCEDURE;
    /
    SET SERVEROUTPU ON;
    EXEC SIMPLE_PROCEDURE;

## Exception

### Exception type

- <b>NO_DATA_FOUND</b>
- <b>OTHERS</b>

### Syntax

    DECLARE
        declare variable statement
    BEGIN
        execute statement
    EXCEPTION
    WHEN EXCEPTION_NAME THEN
        execute statement
    END;

#### Single exception handling

    SET SERVEROUTPUT ON;
    DECLARE
        temp employee%ROWTYPE;
        vDynamicQuery NVARCHAR2(200);
    BEGIN
        vDynamicQuery := 'SELECT * FROM EMPLOYEE  WHERE EMP_CODE   = :EMP_CODE AND EMP_FIRST_NAME = :EMP_FIRST_NAME';
        EXECUTE IMMEDIATE vDynamicQuery INTO temp USING 'EM001', 'Pratyayaa' ;
        DBMS_OUTPUT.PUT_LINE (temp.EMP_FIRST_NAME);
    EXCEPTION
        WHEN NO_DATA_FOUND THEN
             DBMS_OUTPUT.PUT_LINE ('NO_DATA_FOUND');
    END;

#### Multiple exception handling

    SET SERVEROUTPUT ON;
    DECLARE
        temp employee%ROWTYPE;
        vDynamicQuery NVARCHAR2(200);
    BEGIN
        vDynamicQuery := 'SELECT * FROM EMPLOYEE  WHERE EMP_CODE   = :EMP_CODE AND EMP_FIRST_NAME = :EMP_FIRST_NAME';
        EXECUTE IMMEDIATE vDynamicQuery INTO temp USING 'EM001', 'Pratyayaa' ;
        DBMS_OUTPUT.PUT_LINE (temp.EMP_FIRST_NAME);
    EXCEPTION
        WHEN NO_DATA_FOUND THEN
             DBMS_OUTPUT.PUT_LINE ('NO_DATA_FOUND');
        WHEN OTHERS THEN
             DBMS_OUTPUT.PUT_LINE ('OTHERS');
    END;

#### Exceptions handling with Raise 

    SET SERVEROUTPUT ON
    BEGIN
        UPDATE employees
            SET salary = 10000
        WHERE employee_id = 9999;
        IF SQL%ROWCOUNT = 0 THEN
            RAISE NO_DATA_FOUND;
        END IF;
        DBMS_OUTPUT.put_line('NO_DATA_FOUND Not Raised');
    EXCEPTION
        WHEN NO_DATA_FOUND THEN
            DBMS_OUTPUT.put_line('NO_DATA_FOUND Raised');
    END;

#### Exceptions handling with User-define

    SET SERVEROUTPUT ON;
    DECLARE
        v_dividend NUMBER := 24;
        v_divisor NUMBER := 0;
        v_result NUMBER;
      ex_arithmetic EXCEPTION;
    BEGIN
        IF v_divisor = 0 THEN
            RAISE ex_arithmetic;
        END IF;
    EXCEPTION 
        WHEN ex_arithmetic THEN
            DBMS_OUTPUT.PUT_LINE('Error Error - Your Divisor is Zero');
    END;

#### Exceptions handling with RAISE_APPLICATION_ERROR
    
    -- syntax
    raise_application_error (error_number, message , [{TRUE | FALSE}]);

    SET SERVEROUTPUT ON;
    ACCEPT var_age NUMBER PROMPT 'What is yor age';
    DECLARE
        age NUMBER := &var_age;
    BEGIN
        IF age < 18 THEN
            RAISE_APPLICATION_ERROR (-20008, 'you should be 18 or above for the DRINK!');
        END IF; 
        DBMS_OUTPUT.PUT_LINE ('Sure, What would you like to have?'); 

        EXCEPTION WHEN OTHERS THEN
            DBMS_OUTPUT.PUT_LINE (SQLERRM);
    END;
    
 #### Exceptions handling with PRAGMA EXCEPTION_INIT
 
    SET SERVEROUTPUT ON;
    DECLARE
        ex_age    EXCEPTION;
        ex_name   EXCEPTION;
        v_age     NUMBER        := 18;
        v_name    VARCHAR2(100) := 'tirmizee';

        PRAGMA    EXCEPTION_INIT(ex_age, -20008);
        PRAGMA    EXCEPTION_INIT(ex_name, -20009);
    BEGIN
        IF v_age < 18 THEN
            RAISE ex_age;
        END IF;
        IF v_name = 'tirmizee' THEN
            RAISE ex_name;
        END IF;
        DBMS_OUTPUT.PUT_LINE('Sure! What would you like to have?');

        EXCEPTION 
            WHEN ex_age THEN
                DBMS_OUTPUT.PUT_LINE(SQLERRM);  
            WHEN ex_name THEN
                DBMS_OUTPUT.PUT_LINE(SQLERRM);  
    END;
    /
    DECLARE
        ex_age    EXCEPTION;
        ex_name   EXCEPTION;
        v_age     NUMBER        := 18;
        v_name    VARCHAR2(100) := 'tirmizee';

        PRAGMA    EXCEPTION_INIT(ex_age, -20008);
        PRAGMA    EXCEPTION_INIT(ex_name, -20009);
    BEGIN
        IF v_age < 18 THEN
            RAISE_APPLICATION_ERROR(-20008, 'You should be 18 or above for the drinks!');
        END IF;
        IF v_name = 'tirmizee' THEN
            RAISE_APPLICATION_ERROR(-20009, 'can not be tirmizee');
        END IF;
        DBMS_OUTPUT.PUT_LINE('Sure! What would you like to have?');

        EXCEPTION 
            WHEN ex_age THEN
                DBMS_OUTPUT.PUT_LINE(SQLERRM);  
            WHEN ex_name THEN
                DBMS_OUTPUT.PUT_LINE(SQLERRM);  
    END;

#### Nested blocks exception handling

    SET SERVEROUTPUT ON;
    DECLARE
        V_CODE VARCHAR2(100);
        V_EMP_ID NUMBER;  
    BEGIN
        SELECT EMP_CODE INTO V_CODE FROM EMPLOYEE WHERE EMP_CODE = 'EM001'; 
        DBMS_OUTPUT.PUT_LINE('EMPLOYEE CODE IS: '||V_CODE);
        BEGIN
            SELECT EMP_ID INTO V_EMP_ID FROM EMPLOYEE WHERE EMP_ID = 100;
            DBMS_OUTPUT.PUT_LINE('EMPLOYEE ID IS : '||V_EMP_ID);
        EXCEPTION
            WHEN NO_DATA_FOUND THEN DBMS_OUTPUT.PUT_LINE('NO EMP_ID FOUND FOR '||V_CODE);
        END;    
    EXCEPTION
        WHEN NO_DATA_FOUND THEN DBMS_OUTPUT.PUT_LINE('NO EMP_CODE FOUND FOR '||V_CODE);
    END;

### Calling Notation 

Calling notation is a way of providing values to the parameters of a subroutine such as PL/SQL function or a stored procedure.

#### Types of Calling Notations

- <b>Positional Notation</b>
- <b>Named Notation</b>
- <b>Mixed calling notation</b>

#### Using Named Calling Notation

    SET SERVEROUTPUT ON;
    DECLARE

        PROCEDURE CALCULATE1(p_num1 IN NUMBER, p_num2 IN NUMBER, p_num3 IN NUMBER DEFAULT 0) AS
            BEGIN
                DBMS_OUTPUT.PUT_LINE (p_num1);
                DBMS_OUTPUT.PUT_LINE (p_num2);
                DBMS_OUTPUT.PUT_LINE (p_num3);
            END CALCULATE1;

        PROCEDURE CALCULATE2(p_num1 IN NUMBER, p_num2 IN NUMBER DEFAULT 0, p_num3 IN NUMBER ) AS
            BEGIN
                DBMS_OUTPUT.PUT_LINE (p_num1);
                DBMS_OUTPUT.PUT_LINE (p_num2);
                DBMS_OUTPUT.PUT_LINE (p_num3);
            END CALCULATE2;
            
    BEGIN
        CALCULATE1(3,4);
        CALCULATE2(p_num3 => 10,p_num1 => 4);
    END;

## Package 

- Package header
- Package body

#### Package header

    CREATE OR REPLACE PACKAGE PKG_SIMPLE AS
        MAX_NUMBER NUMBER DEFAULT 999;
        MIN_NUMBER NUMBER DEFAULT 0;
        TYPE SIMPLE_TYPE IS RECORD (
            FIELD_1   NUMBER(10,0), 
            FIELD_2   NVARCHAR2(20)
        );
        TYPE LIST_TYPE IS TABLE OF NUMBER;
        FUNCTION PRT_STRING RETURN NVARCHAR2;
        FUNCTION PRT_STRING(fname NVARCHAR2, lname NVARCHAR2) RETURN NVARCHAR2;
        PROCEDURE PROC_STRING;
        PROCEDURE PROC_STRING(fname NVARCHAR2, lname NVARCHAR2);
    END PKG_SIMPLE;

#### Package body

    CREATE OR REPLACE PACKAGE BODY PKG_SIMPLE AS
     
         FUNCTION PRT_STRING RETURN NVARCHAR2 IS
            BEGIN
                RETURN 'Hello world';
            END PRT_STRING;

        FUNCTION PRT_STRING(fname NVARCHAR2, lname NVARCHAR2) RETURN NVARCHAR2 
            IS
                results NVARCHAR2(200) DEFAULT '';
            BEGIN
                results := fname || ' ' || lname;
                RETURN results;
            END PRT_STRING;

        PROCEDURE PROC_STRING IS 
            BEGIN
                 dbms_output.put_line('Hello world');
            END PROC_STRING;

        PROCEDURE PROC_STRING(fname NVARCHAR2, lname NVARCHAR2) 
            IS
                results NVARCHAR2(200) DEFAULT '';
            BEGIN 
                results := fname || ' ' || lname;
                dbms_output.put_line(results);
            END PROC_STRING;

    END PKG_SIMPLE;

#### Using package

    SET SERVEROUTPU ON;
    DECLARE
        v_simple PKG_SIMPLE.SIMPLE_TYPE;
        v_list   PKG_SIMPLE.LIST_TYPE := PKG_SIMPLE.LIST_TYPE(1,2,3,4,5);
    BEGIN
        v_simple.FIELD_1 := 1111;
        v_simple.FIELD_2 := 'PPPPPPP';
        DBMS_OUTPUT.PUT_LINE(PKG_SIMPLE.MIN_NUMBER);
        DBMS_OUTPUT.PUT_LINE(PKG_SIMPLE.MAX_NUMBER);
        DBMS_OUTPUT.PUT_LINE(v_simple.FIELD_1 || v_simple.FIELD_2);
        DBMS_OUTPUT.PUT_LINE(v_list(1) || v_list(2) || v_list(3) || v_list(4));
        DBMS_OUTPUT.PUT_LINE(PKG_SIMPLE.PRT_STRING);
        DBMS_OUTPUT.PUT_LINE(PKG_SIMPLE.PRT_STRING('PRATYA','YEEKHADAY'));
        PKG_SIMPLE.PROC_STRING;
        PKG_SIMPLE.PROC_STRING('PRATYA','YEEKHADAY');
    END;
    /
    SELECT 
        PKG_SIMPLE.PRT_STRING,
        PKG_SIMPLE.PRT_STRING('PRATYA','YEEKHADAY')
    FROM DUAL;
    EXEC PKG_SIMPLE.PROC_STRING;
    EXEC PKG_SIMPLE.PROC_STRING('PRATYA','YEEKHADAY');

## Triggers 

Triggers are named PL/SQL blocks which are stored in the database.  We can also say that they are specialized stored programs which execute implicitly when a triggering event occurs. This means we cannot call and execute them directly instead they only get triggered by events in the database.

### Events Which Fires the Database Triggers

- <b>A DML Statement</b> (DELETE, INSERT, UPDATE)
- <b>A DDL Statement</b> (CREATE, ALTER, DROP, TRUNCATE etc.)
- <b>A System event or database operations.</b> (SERVERERROR, LOGON, LOGOFF, STARTUP, SHUTDOWN etc)

### OLD and NEW Pseudorecords

- <b>:old</b> - refers to Old Value
- <b>:new</b> - refers to New value

#### For the row that the trigger is processing

- For an <b>INSERT</b> trigger, OLD contains no values, and NEW contains the new values.
- For an <b>UPDATE</b> trigger, OLD contains the old values, and NEW contains the new values.
- For a <b>DELETE</b> trigger, OLD contains the old values, and NEW contains no values.
    
### Avoid trigger

- <b>Do not create recursive triggers.</b>

### Disabling or Enabling a Single Trigger

    ALTER TRIGGER eval_change_trigger DISABLE;
    ALTER TRIGGER eval_change_trigger ENABLE;

#### Trigger syntax

    CREATE [OR REPLACE] TRIGGER ttrigger_name
    {BEFORE|AFTER} triggering_event ON table_name
    [FOR EACH ROW]
    [FOLLOWS another_trigger_name]
    [ENABLE/DISABLE]
    [WHEN condition]
    DECLARE
      declaration statements
    BEGIN
      executable statements
    EXCEPTION
      exception-handling statements
    END;

#### Trigger single event

    CREATE OR REPLACE TRIGGER TRG_EMPLOYEE_UPDATE
        AFTER UPDATE ON employee
        FOR EACH ROW
        ENABLE
    DECLARE
        vUser NVARCHAR2(200);
    BEGIN
        SELECT user INTO vUser FROM DUAL;
        DBMS_OUTPUT.PUT_LINE('user ' || vUser || ' update employee');
    EXCEPTION
        WHEN OTHERS THEN
            NULL;
    END;
    /
    BEGIN
        UPDATE employee set emp_code = 'EM001' WHERE emp_code = 'EM001';
    END;
    
 #### Trigger multiple event
    
    CREATE OR REPLACE TRIGGER TRG_EMPLOYEE_UPDATE
    AFTER INSERT OR UPDATE OR DELETE ON employee
    FOR EACH ROW
    ENABLE
    DECLARE
        vUser NVARCHAR2(200);
    BEGIN
        SELECT user INTO vUser FROM DUAL;
        IF INSERTING THEN
            DBMS_OUTPUT.PUT_LINE('user ' || vUser || ' insert employee');
        ELSIF UPDATING THEN
            DBMS_OUTPUT.PUT_LINE('user ' || vUser || ' update employee');
        ELSIF DELETING THEN
            DBMS_OUTPUT.PUT_LINE('user ' || vUser || ' delete employee');
        END IF;
    EXCEPTION
        WHEN OTHERS THEN
            NULL;
    END;
    /
    BEGIN
        UPDATE employee set emp_code = 'EM001' WHERE emp_code = 'EM001';
        INSERT INTO employee VALUES( 4 ,'EM004','temp','temp',null);
        DELETE FROM employee WHERE emp_id = 4;
    END;

## DIRECTORY

    CREATE [OR REPLACE] DIRECTORY directory_name AS 'path_name'

#### Creating a Directory

    CREATE OR REPLACE DIRECTORY USER_DIR AS '/usr/bin/bfile_dir';

#### Grant Permission

    GRANT READ ON DIRECTORY USER_DIR TO userName;
    GRANT READ ON DIRECTORY USER_DIR TO PUBLIC;

### UTL_FILE

UTL_FILE package, PL/SQL programs can read and write operating system text files.

- <b>FILE_TYPE</b>
- <b>FOPEN</b>

        UTL_FILE.FOPEN (
           location     IN VARCHAR2,
           filename     IN VARCHAR2,
           open_mode    IN VARCHAR2,
           max_linesize IN BINARY_INTEGER) 
        RETURN file_type;

- <b>FCLOSE</b>
- <b>GET_LINE</b>
- <b>PUT_LINE</b>
- <b>IS_OPEN</b>

Opening mode

- <b>A</b> append text.
- <b>R</b> read text.
- <b>W</b> write text.
- <b>RB/b> read byte mode.
- <b>WB</b> write byte mode.
- <b>AB</b> append byte mode.

#### Read a single line of file

    SET SERVEROUTPUT ON;
    DECLARE
        v_length NUMBER DEFAULT 200;
        v_file UTL_FILE.FILE_TYPE;
        v_line NVARCHAR2(200);
    BEGIN
        v_file := UTL_FILE.FOPEN('TEMP_DIR', 'temp.txt', 'R', 200);
        UTL_FILE.GET_LINE(v_file, v_line);
        UTL_FILE.FCLOSE(v_file);

        DBMS_OUTPUT.PUT_LINE (v_line); 

        EXCEPTION 
            WHEN NO_DATA_FOUND THEN
                DBMS_OUTPUT.PUT_LINE ('File not found.'); 
    END;

#### Read a multiple line of file

    SET SERVEROUTPUT ON;
    DECLARE
        v_line NVARCHAR2(200);
        v_file UTL_FILE.FILE_TYPE;
    BEGIN
        v_file := UTL_FILE.FOPEN('TEMP_DIR', 'temp.txt', 'R', 200);
        LOOP 
            BEGIN
                UTL_FILE.GET_LINE(v_file, v_line);
                DBMS_OUTPUT.PUT_LINE (v_line); 
                EXCEPTION WHEN NO_DATA_FOUND THEN EXIT; 
            END;
        END LOOP;
        UTL_FILE.FCLOSE(v_file);
    END;

#### Write file

    SET SERVEROUTPUT ON;
    DECLARE
        v_file         UTL_FILE.FILE_TYPE;
        v_line         NVARCHAR2(200);
        v_dir          NVARCHAR2(200) DEFAULT 'TEMP_DIR';
        v_file_name    NVARCHAR2(200) DEFAULT 'temp.txt';
    BEGIN
        v_file := UTL_FILE.FOPEN(v_dir, v_file_name, 'W', 200);
        FOR I IN 1..20 LOOP
            v_line := '0000000000' || i;
            UTL_FILE.PUT_LINE(v_file, v_line);
        END LOOP;
        UTL_FILE.FCLOSE(v_file); 
        DBMS_OUTPUT.PUT_LINE ('Finish.'); 
    END;

#### Append file

    SET SERVEROUTPUT ON;
    DECLARE
        v_file         UTL_FILE.FILE_TYPE;
        v_line         NVARCHAR2(200);
        v_dir          NVARCHAR2(200) DEFAULT 'TEMP_DIR';
        v_file_name    NVARCHAR2(200) DEFAULT 'temp.txt';
    BEGIN
        v_file := UTL_FILE.FOPEN(v_dir, v_file_name, 'A', 200);
        FOR I IN 1..20 LOOP
            v_line := '1111111111' || i;
            UTL_FILE.PUT_LINE(v_file, v_line);
        END LOOP;
        UTL_FILE.FCLOSE(v_file); 
        DBMS_OUTPUT.PUT_LINE ('Finish.'); 
    END;

## Transaction Control Language (TCL)

#### TCL Statements available in Oracle 

- <b>COMMIT</b>       : Make changes done in  transaction permanent.
- <b>ROLLBACK</b>     : Rollbacks the state of database to the last commit point.
- <b>SAVEPOINT</b>    : Use to specify a point in transaction to which later you can rollback.

### Reference

- https://itsourteamwork.wordpress.com/2009/12/29/anchor-data-types-using-rowtype-in-oracle/

- https://www.databasestar.com/oracle-sqlcode-sqlerrm/
