
## Examples

- #### [1. Split large file into multiple file](#section-1)
- #### [2. Creating String util package.](#section-2)
- #### [3. Read a text file into  table](#section-3)


#### <a name="section-1"></a> 1. Split large file into multiple file 

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


#### <a name="section-2"></a> 2. Creating String util package. 

<b>
    
    CREATE OR REPLACE PACKAGE STRING_UTILS AS

        TYPE LIST_STRING IS TABLE OF NVARCHAR2(500);

        FUNCTION SPLIT_TEXT( text NVARCHAR2, separator CHAR DEFAULT ':') RETURN LIST_STRING;

    END STRING_UTILS;

</b>
    
#### <a name="section-3"></a> 3. Read a text file into table.

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
