
## Examples

 #### [1. Split large file into multiple file](#section-1)
 #### [2. Creating StringUtils package.](#section-2)
 #### [3. Read a text file into table](#section-3)
 #### [4. Export single table to a Text file](#section-4)
 #### [5. Export multiple table to a Text file](#section-5)
 #### [6. Update data from Text file.](#section-5)


### <a name="section-1"></a> 1. Split large file into multiple file 

<b>

    SET SERVEROUTPUT ON;

    CREATE OR REPLACE PROCEDURE SPRIT_FILE(
        file_name NVARCHAR2, 
        source_dir NVARCHAR2, 
        target_dir NVARCHAR2, 
        sprit_length NUMBER DEFAULT 1000
    ) AS 
        v_file_write         UTL_FILE.FILE_TYPE;
        v_file_read          UTL_FILE.FILE_TYPE;
        v_read_line          NVARCHAR2(1000);
        v_file_write_name    NVARCHAR2(100);
        v_file_count         NUMBER DEFAULT 0;
    BEGIN
        v_file_read := UTL_FILE.FOPEN(source_dir, file_name, 'R', 1000);

        LOOP
            BEGIN
                v_file_count := v_file_count + 1;
                v_file_write_name := 'transaction_' || v_file_count || '.csv';
                v_file_write := UTL_FILE.FOPEN(target_dir, v_file_write_name, 'W', 1000);

                FOR i IN 1..sprit_length LOOP 
                    UTL_FILE.GET_LINE(v_file_read, v_read_line);
                    UTL_FILE.PUT_LINE(v_file_write, v_read_line);
                END LOOP;

                UTL_FILE.FCLOSE(v_file_write);   

                EXCEPTION  
                    WHEN NO_DATA_FOUND THEN
                        UTL_FILE.FCLOSE(v_file_write);  
                        UTL_FILE.FCLOSE(v_file_write);  
                        EXIT;
            END;
        END LOOP;
        DBMS_OUTPUT.PUT_LINE ('Finish.'); 
    END;

</b>


##### Using

<b>

    BEGIN
       SPLIT_FILE (
            file_name   => 'transaction.csv', 
            source_dir  => 'TEMP_DIR', 
            target_dir  => 'TEMP_DIR'
        );
    END;

</b>


### <a name="section-2"></a> 2. Creating StringUtils package. 

##### Create package header

<b>
    
    CREATE OR REPLACE PACKAGE STRING_UTILS AS
    
        TYPE LIST_CAHR IS TABLE OF CHAR(1);
        TYPE LIST_STRING IS TABLE OF NVARCHAR2(500);

        FUNCTION SPLIT_TEXT( text NVARCHAR2, separator CHAR DEFAULT ':') RETURN LIST_STRING;

    END STRING_UTILS;

</b>

##### Create package body

<b>
    
    CREATE OR REPLACE PACKAGE BODY STRING_UTILS AS

        FUNCTION SPLIT_TEXT( text NVARCHAR2, separator CHAR DEFAULT ':') RETURN LIST_STRING
        AS
            v_chars             LIST_CAHR       DEFAULT LIST_CAHR();
            v_strings           LIST_STRING     DEFAULT LIST_STRING();
            v_length            NUMBER          DEFAULT v_strings.COUNT;
            v_start             NUMBER          DEFAULT 0;
            v_split_start       NUMBER          DEFAULT 0;
            v_split_length      NUMBER          DEFAULT 0;
            v_first             BOOLEAN         DEFAULT TRUE;
        BEGIN
            FOR i IN 1..LENGTH(text) LOOP

                v_chars.EXTEND(1);
                v_chars(i) := SUBSTR( text, i, 1 );

                IF v_first AND v_chars(i) = separator THEN
                    v_length := 1;
                    v_split_start := v_start;
                    v_split_length :=  i - 1;
                    v_strings.EXTEND(1);
                    v_strings(v_length) := SUBSTR(text, v_split_start, v_split_length);
                    v_start := i;
                    v_first := FALSE;
                    CONTINUE;
                END IF;

                IF v_chars(i) = separator THEN
                    v_length := v_length + 1;
                    v_split_start :=  v_start + 1;
                    v_split_length :=  i - (v_start + 1);
                    v_strings.EXTEND(1);
                    v_strings(v_length) := SUBSTR(text, v_split_start, v_split_length);
                    v_start := i;
                    CONTINUE;
                END IF;

                IF i = LENGTH(text) THEN
                    v_strings.EXTEND(1);
                    v_split_start :=  v_start + 1;
                    v_split_length := i + 1;
                    v_length := v_length + 1;
                    v_strings(v_length) := SUBSTR(text, v_split_start, v_split_length);
                END IF;

            END LOOP;
            RETURN v_strings;
        END SPLIT_TEXT;

    END STRING_UTILS;

</b>

##### Using

<b>

    DECLARE
        v_strings STRING_UTILS.LIST_STRING;
    BEGIN 
        v_strings := STRING_UTILS.split_text('12,9993,3,3,99,11,1,333333333333,4', ',');
        FOR i IN 1..v_strings.COUNT LOOP
            DBMS_OUTPUT.PUT_LINE(v_strings(i));
        END LOOP;
    END;

</b>

### <a name="section-3"></a> 3. Read a text file into table.

##### Create a table

<b>
    
    DROP TABLE temp_file;

    CREATE TABLE temp_file (
        id            NUMBER(19,0) PRIMARY KEY NOT NULL,
        first_name    NVARCHAR2(100),
        last_name     NVARCHAR2(100),
        create_date   TIMESTAMP NOT NULL
    );

    CREATE SEQUENCE C##ORGANIZATION.seq_temp_file 
        MINVALUE 1
        START WITH 1
        INCREMENT BY 1;

</b>

##### Create Procedure

<b>

    CREATE OR REPLACE PROCEDURE READ_FILE_TO_TEMP(file_name IN NVARCHAR2, dir IN NVARCHAR2) 
    AS
        TYPE LIST_OF_TEMP IS TABLE OF temp_file%ROWTYPE;

        v_temps   LIST_OF_TEMP         DEFAULT LIST_OF_TEMP();
        v_count   NUMBER               DEFAULT v_temps.COUNT;
        v_file    UTL_FILE.FILE_TYPE;
        v_line    NVARCHAR2(1000);

        v_rows    STRING_UTILS.LIST_STRING;
    BEGIN
        v_file := UTL_FILE.FOPEN(dir, file_name,'R', 1000);
        LOOP
            BEGIN

                UTL_FILE.GET_LINE(v_file, v_line);
                v_rows := STRING_UTILS.SPLIT_TEXT(v_line, ',');

                v_temps.EXTEND(1);
                v_count := v_temps.COUNT;

                v_temps(v_count).ID          := C##ORGANIZATION.seq_temp_file.NEXTVAL;
                v_temps(v_count).FIRST_NAME  := v_rows(1);
                v_temps(v_count).LAST_NAME   := v_rows(2);
                v_temps(v_count).CREATE_DATE := CURRENT_TIMESTAMP;

                INSERT INTO temp_file VALUES v_temps(v_count);
                COMMIT;

                EXCEPTION  
                WHEN NO_DATA_FOUND THEN 
                    UTL_FILE.FCLOSE(v_file);
                    EXIT;
            END;
        END LOOP;
        UTL_FILE.FCLOSE(v_file);
        DBMS_OUTPUT.PUT_LINE ('Finish.');
    END READ_FILE_TO_TEMP;

</b>

##### Bulk Insert, allowing for speed improvements.

<b>

    FORALL i IN 1..v_count 
        INSERT INTO temp_file VALUES v_temps(i);
    COMMIT;

</b>

##### Calling Stored Procedure

<b>
 
    SET SERVEROUTPUT ON;
    BEGIN
       READ_FILE_TO_TEMP (
            file_name   => 'transaction.csv', 
            dir         => 'TEMP_DIR'
        );
    END;

</b>

### <a name="section-4"></a> 4. Export single table to a Text file.

<b>

    CREATE OR REPLACE PROCEDURE EXPORT_DATA_TO_FILE(dir IN NVARCHAR2, file_name IN NVARCHAR2) 
    AS
        v_file       UTL_FILE.FILE_TYPE;
        v_line       NVARCHAR2(1000);
        v_temp_file  TEMP_FILE%ROWTYPE;

        CURSOR C_TEMP_FILE IS
            SELECT * 
            FROM TEMP_FILE;

    BEGIN
        v_file := UTL_FILE.FOPEN(dir, file_name, 'W');
        OPEN C_TEMP_FILE;
        LOOP
            FETCH C_TEMP_FILE INTO v_temp_file;
            EXIT WHEN C_TEMP_FILE%NOTFOUND;
            v_line := v_temp_file.ID || ',' || v_temp_file.FIRST_NAME || ',' || v_temp_file.LAST_NAME|| ',' || v_temp_file.CREATE_DATE;
            UTL_FILE.PUT_LINE(v_file, v_line);     
        END LOOP;    
        CLOSE C_TEMP_FILE;
        UTL_FILE.FCLOSE(v_file);
    END EXPORT_DATA_TO_FILE;

</b>

##### Calling Stored Procedure

<b>
 
    SET SERVEROUTPUT ON;
    BEGIN
       EXPORT_DATA_TO_FILE (
            file_name   => 'export.csv', 
            dir         => 'TEMP_DIR'
        );
    END;

</b>

### <a name="section-5"></a> 5. Export multiple table to a Text file.

<b>

    CREATE OR REPLACE PROCEDURE EXPORT_DATA_TO_FILE(dir IN NVARCHAR2, file_name IN NVARCHAR2) 
    AS

        TYPE DETAIL_TYPE IS RECORD(
            USERNAME   USERS.USERNAME%TYPE,
            FIRST_NAME PROFILE.FIRST_NAME%TYPE,
            LAST_NAME  PROFILE.LAST_NAME%TYPE,
            CITIZEN_ID PROFILE.CITIZEN_ID%TYPE,
            TEL        PROFILE.TEL%TYPE,
            EMAIL      PROFILE.EMAIL%TYPE
        );

        v_file       UTL_FILE.FILE_TYPE;
        v_line       NVARCHAR2(1000);
        v_detail     DETAIL_TYPE;

        CURSOR C_DETAIL IS
            SELECT 
                U.USERNAME,
                P.FIRST_NAME,
                P.LAST_NAME,
                P.CITIZEN_ID,
                P.TEL,
                P.EMAIL
            FROM USERS U
            INNER JOIN PROFILE P ON P.PROFILE_ID = U.PROFILE_ID;

    BEGIN
        v_file := UTL_FILE.FOPEN(dir, file_name, 'W');
        OPEN C_DETAIL;
        LOOP
            FETCH C_DETAIL INTO v_detail;
            EXIT WHEN C_DETAIL%NOTFOUND;
            v_line := v_detail.USERNAME || ',' || v_detail.FIRST_NAME || ',' || v_detail.LAST_NAME|| ',' || v_detail.CITIZEN_ID || ',' || v_detail.TEL || ',' || v_detail.EMAIL;
            UTL_FILE.PUT_LINE(v_file, v_line);     
        END LOOP;    
        CLOSE C_DETAIL;
        UTL_FILE.FCLOSE(v_file);
    END EXPORT_DATA_TO_FILE;

</b>

##### Calling Stored Procedure

<b>
 
    SET SERVEROUTPUT ON;
    BEGIN
       EXPORT_DATA_TO_FILE (
            file_name   => 'export.csv', 
            dir         => 'TEMP_DIR'
        );
    END;

</b>

### <a name="section-6"></a> 6. Update data from Text file.

<b>

    CREATE TABLE TEMP_SALARY (
        ID        NUMBER(19,0),
        salary    NUMBER(8,2)
    );

    DECLARE
        v_temp TEMP_SALARY%ROWTYPE;
    BEGIN
        FOR i IN 1..100 LOOP
            v_temp.id :=  C##ORGANIZATION.seq_temp_file.NEXTVAL;
            v_temp.salary := dbms_random.value(1,10000);
            INSERT INTO TEMP_SALARY VALUES v_temp;
            COMMIT;
        END LOOP;
    END;

</b>
