# PL-SQL-Example

## Block

    DECLARE
      Declartion statement
    BEGIN 
      Executable statement
    Exceptions
      Exception handling statement
    END;

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

### Reference

- https://itsourteamwork.wordpress.com/2009/12/29/anchor-data-types-using-rowtype-in-oracle/
