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
- A block that has a name is called a Stored Procedure.

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


## Select into

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

## CURSOR
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

#### Explicit Cursor FOR Loop

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

#### Explicit Cursor with parameterized

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
   
#### Explicit Cursor with parameterized and default value   
   
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

#### SYS_REFCURSOR

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

## Datatype

#### Record Datatype

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
    
## Functions 

#### Syntax

    CREATE [OR REPLACE] FUNCTION function_name(Parameter 1, Parameter 2,...) RETURN data_type
    IS
        Declare variable...;
    BEGIN
        executable statement;
        RETURN (value);
    END;

#### Creating functions

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

### Calling Notation 

Calling notation is a way of providing values to the parameters of a subroutine such as PL/SQL function or a stored procedure.

## Package 

- Package header
- Package body

#### Package header

    CREATE OR REPLACE PACKAGE PKG_SIMPLE AS
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
    BEGIN
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

- <b>A DML Statement</b> INSERT, UPDATE, and DELETE
- <b>A DDL Statement</b>
- <b>A system event.</b>
- <b>A User Events</b> LOGON LOGOFF 

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
    
### Reference

- https://itsourteamwork.wordpress.com/2009/12/29/anchor-data-types-using-rowtype-in-oracle/

- https://www.databasestar.com/oracle-sqlcode-sqlerrm/
