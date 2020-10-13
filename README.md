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
