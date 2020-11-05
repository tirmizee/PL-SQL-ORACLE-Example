
## Examples

- #### [Split large file into multiple file](#section-1)
- #### [Read a text file into  table](#section-2)

#### <a name="section-1"></a>Split large file into multiple file

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

...

<b>

    BEGIN
       SPLIT_FILE (
            file_name   => 'transaction.csv', 
            source_dir  => 'TEMP_DIR', 
            target_dir  => 'TEMP_DIR'
        );
    END;

</b>

#### <a name="section-2"></a>Read a text file into  table

- STRING_TO_TABLE
- UTL_FILE
