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

    SET SERVEROUTPUT ON;

    DECLARE 
        vString VARCHAR2(20);
        vNumber NUMBER(20);
    BEGIN
        vString := 'Hello world';
        vNumber := 20000;
        dbms_output.put_line(vString);
        dbms_output.put_line(vNumber);
    END;


## Data type

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


### Reference

- https://itsourteamwork.wordpress.com/2009/12/29/anchor-data-types-using-rowtype-in-oracle/
