
## Examples

- #### [1. Split large file into multiple file](#section-1)
- #### [2. Creating StringUtils package.](#section-2)
- #### [3. Read a text file into  table](#section-3)


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
            v_strings_length    NUMBER          DEFAULT v_strings.COUNT;
            v_start             NUMBER          DEFAULT 0;
            v_first             BOOLEAN         DEFAULT TRUE;
            v_string            NVARCHAR2(200);
        BEGIN
            FOR i IN 1..LENGTH(text) LOOP

                v_chars.EXTEND(1);
                v_chars(i) := SUBSTR( text, i, 1 );

                IF v_first AND v_chars(i) = separator THEN
                    v_strings_length := 1;
                    v_strings.EXTEND(1);
                    v_strings(v_strings_length) := SUBSTR(text, v_start, i - 1);
                    v_start := i;
                    v_first := FALSE;
                    CONTINUE;
                END IF;

                IF v_chars(i) = separator THEN
                    v_strings_length := v_strings_length + 1;
                    v_strings.EXTEND(1);
                    v_strings(v_strings_length) := SUBSTR(text, v_start + 1, i - (v_start + 1));
                    v_start := i;
                    CONTINUE;
                END IF;

                IF i = LENGTH(text) THEN
                    v_strings.EXTEND(1);
                    v_strings_length := v_strings_length + 1;
                    v_strings(v_strings_length) := SUBSTR(text, v_start + 1, i + 1);
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
