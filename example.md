
#### [Split large file into multiple file](#section-1)

#### <a name="section-1"></a>Split large file into multiple file

<b>
  
    DECLARE 
        v_file         UTL_FILE.FILE_TYPE;
        v_file_name    NVARCHAR2(100) := 'transaction.csv';
        v_dir          NVARCHAR2(100) := 'TEMP_DIR';
    BEGIN
        v_file := UTL_FILE.FOPEN(v_dir, v_file_name, 'W', 1000);
        FOR i IN 1..10500 LOOP
            UTL_FILE.PUT_LINE(v_file,'Transaction_' || i ||',' || 'WAITING');
        END LOOP;
        UTL_FILE.FCLOSE(v_file);
    END;

</b>
