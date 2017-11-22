# utl_how_to_conditionally_change_the_length_of_any_varible_in_given_sas_dataset
SAS Forum: How to conditionally change the length of any varible in given SAS dataset

    ```  SAS Forum: How to conditionally change the length of any varible in given SAS dataset                                                                        ```
    ```                                                                                                                                                               ```
    ```  I need to truncate long strings(>20 bytes) to length 20                                                                                                      ```
    ```                                                                                                                                                               ```
    ```  Note SAS does not have type varchar.                                                                                                                         ```
    ```                                                                                                                                                               ```
    ```  INPUT                                                                                                                                                        ```
    ```  =====                                                                                                                                                        ```
    ```                                                                                                                                                               ```
    ```   WORK.HAVE total obs=2                                                                                                                                       ```
    ```                                                                                                                                                               ```
    ```     Obs    NAMONE                                      NAMTWO                                                                                                 ```
    ```                                                                                                                                                               ```
    ```      1     1234567890123456789012345678901234567890    abcdefghijklmnopqrstuvwxyz                                                                             ```
    ```      2     123456789012345678901                       abcde                                                                                                  ```
    ```                                                                                                                                                               ```
    ```                                                                                                                                                               ```
    ```         Variables in Creation Order                                                                                                                           ```
    ```                                                                                                                                                               ```
    ```      #    Variable    Type    Len                                                                                                                             ```
    ```                                                                                                                                                               ```
    ```      1    NAMONE      Char     40                                                                                                                             ```
    ```      2    NAMTWO      Char     40                                                                                                                             ```
    ```                                                                                                                                                               ```
    ```                                                                                                                                                               ```
    ```  WORKING CODE                                                                                                                                                 ```
    ```  ============                                                                                                                                                 ```
    ```                                                                                                                                                               ```
    ```      PHASE 1 COMPILE TIME DOSUBL                                                                                                                              ```
    ```                                                                                                                                                               ```
    ```         set have end=dne;                                                                                                                                     ```
    ```         array chr _character_;                                                                                                                                ```
    ```         do i=1 to dim(chr);                                                                                                                                   ```
    ```            if length(chr[i]) > 20 then do;                                                                                                                    ```
    ```               nam=vname(chr[i]);                                                                                                                              ```
    ```               if index(nams,strip(nam))=0 then nams=catx(' ',nams,nam);                                                                                       ```
    ```            end;                                                                                                                                               ```
    ```         end;                                                                                                                                                  ```
    ```         if dne then call symputx('nams',nams);                                                                                                                ```
    ```                                                                                                                                                               ```
    ```      MAINLINE                                                                                                                                                 ```
    ```                                                                                                                                                               ```
    ```         length &nams $20;                                                                                                                                     ```
    ```         set have;                                                                                                                                             ```
    ```                                                                                                                                                               ```
    ```  OOTPUT                                                                                                                                                       ```
    ```  ======                                                                                                                                                       ```
    ```                                                                                                                                                               ```
    ```     WORK.WANT total obs=2                                                                                                                                     ```
    ```                                                                                                                                                               ```
    ```     Obs           NAMONE           NAMTWO                                                                                                                     ```
    ```                                                                                                                                                               ```
    ```      1     12345678901234567890    abcdefghijklmnopqrst                                                                                                       ```
    ```      2     12345678901234567890    abcde                                                                                                                      ```
    ```                                                                                                                                                               ```
    ```                                                                                                                                                               ```
    ```             Variables in Creation Order                                                                                                                       ```
    ```      #    Variable    Type    Len                                                                                                                             ```
    ```                                                                                                                                                               ```
    ```      1    NAMONE      Char     20    ** noe 20 bytes                                                                                                          ```
    ```      2    NAMTWO      Char     20                                                                                                                             ```
    ```                                                                                                                                                               ```
    ```  https://goo.gl/zG5fvr                                                                                                                                        ```
    ```  https://communities.sas.com/t5/General-SAS-Programming/How-to-conditionally-change-the-length-of-any-varible-in-given/m-p/415473                             ```
    ```                                                                                                                                                               ```
    ```  *                _               _       _                                                                                                                   ```
    ```   _ __ ___   __ _| | _____     __| | __ _| |_ __ _                                                                                                            ```
    ```  | '_ ` _ \ / _` | |/ / _ \   / _` |/ _` | __/ _` |                                                                                                           ```
    ```  | | | | | | (_| |   <  __/  | (_| | (_| | || (_| |                                                                                                           ```
    ```  |_| |_| |_|\__,_|_|\_\___|   \__,_|\__,_|\__\__,_|                                                                                                           ```
    ```                                                                                                                                                               ```
    ```  ;                                                                                                                                                            ```
    ```                                                                                                                                                               ```
    ```  options validvarname=upcase;                                                                                                                                 ```
    ```  data have;                                                                                                                                                   ```
    ```   informat namOne $40. namTwo $40.;                                                                                                                           ```
    ```   input namOne namTwo ;                                                                                                                                       ```
    ```  cards4;                                                                                                                                                      ```
    ```  1234567890123456789012345678901234567890 abcdefghijklmnopqrstuvwxyz                                                                                          ```
    ```  123456789012345678901 abcde                                                                                                                                  ```
    ```  ;;;;                                                                                                                                                         ```
    ```  run;quit;                                                                                                                                                    ```
    ```                                                                                                                                                               ```
    ```  *          _       _   _                                                                                                                                     ```
    ```   ___  ___ | |_   _| |_(_) ___  _ __                                                                                                                          ```
    ```  / __|/ _ \| | | | | __| |/ _ \| '_ \                                                                                                                         ```
    ```  \__ \ (_) | | |_| | |_| | (_) | | | |                                                                                                                        ```
    ```  |___/\___/|_|\__,_|\__|_|\___/|_| |_|                                                                                                                        ```
    ```                                                                                                                                                               ```
    ```  ;                                                                                                                                                            ```
    ```                                                                                                                                                               ```
    ```  data want;                                                                                                                                                   ```
    ```                                                                                                                                                               ```
    ```     * PHASE 1 COMPILE;                                                                                                                                        ```
    ```     * get a list of variable names with data over 20 bytes;                                                                                                   ```
    ```     if _n_=0 then do;                                                                                                                                         ```
    ```       %let rc=%sysfunc(dosubl('                                                                                                                               ```
    ```          data _null_;                                                                                                                                         ```
    ```             set have end=dne;                                                                                                                                 ```
    ```             array chr _character_;                                                                                                                            ```
    ```             length nams $4096;                                                                                                                                ```
    ```             retain  nams;                                                                                                                                     ```
    ```             do i=1 to dim(chr);                                                                                                                               ```
    ```                if length(chr[i]) > 20 then do;                                                                                                                ```
    ```                   nam=vname(chr[i]);                                                                                                                          ```
    ```                   if index(nams,strip(nam))=0 then nams=catx(' ',nams,nam);                                                                                   ```
    ```                end;                                                                                                                                           ```
    ```             end;                                                                                                                                              ```
    ```             if dne then call symputx('nams',nams);                                                                                                            ```
    ```         run;quit;                                                                                                                                             ```
    ```        '));                                                                                                                                                   ```
    ```     end;                                                                                                                                                      ```
    ```                                                                                                                                                               ```
    ```     length &nams $20;                                                                                                                                         ```
    ```     set have;                                                                                                                                                 ```
    ```                                                                                                                                                               ```
    ```  run;quit;                                                                                                                                                    ```

