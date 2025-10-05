# utl-automating-excel-data-preperation-monarch-and-altair-percsonal-slc
Monarch and altair percsonal slc automating excel data preperation
    %let pgm=utl-monarch-and-altair-percsonal-slc-automating-excel-data-preperation;

    %stop_submission;

    Monarch and altair percsonal slc automating excel data preperation

    Too long to post in a listserve, see github

    github
    https://github.com/rogerjdeangelis/utl-automating-excel-data-preperation-monarch-and-altair-percsonal-slc

    /community.altair
    https://community.altair.com/discussion/comment/36489?tab=all#Comment_36489?utm_source=community-search&utm_medium=organic-search&utm_term=monarch+e

    Most listserves mangle my posts, except SAS-L, when viewed directly.
    Most email systems also mangle my posts. Less is more, lets go back to recognizing fixed fonts?

    A window may pop up saying there is an error with the local server, just ignore it.
    See the clean log on the end.

    WHAT OP WANTS

    Example: load excel file>split the date column and filter only year value
    in that column> Split the email address and keep only domain
    address> export the file into excel sheets.

    /*--- create mondays raw excel input. You get a new excel file every day ---*/
    /*--- you need to prepare the excel for export to Monarch                ---*/
    /*--- using the ops business rules above, add YEAR * DOMAIN to excel     ---*/

    INPUT                                        OUTPUT (add year and domain)

    d:/xls/monday.xlsx                           d:/xls/want.xlsx

    -----------------------+                     -----------------+
    | A1| fx        |NAME  |                     | A1| fx  |NAME  |
    ------------------------------------------   ---------------------------------------------------------
    [_] |    A     |    B            | C | D |   [_] |  A |    B    |    C     |    C            | E | F |
    ------------------------------------------   ---------------------------------------------------------
     1  | NAME     |EMAIL            |SEX|AGE|    1  |YEAR|DOMAIN   | NAME     |EMAIL            |SEX|AGE|
     -- |----------+-----------------+---+---+    -- |----+---------+----------+-----------------+---+---+
     2  |2025-09-15|Alice.gmail.com  | F |   |    2  |2025|gmail.com|2025-09-15|Alice.gmail.com  | F |   |
     -- |--------+-------------------+---+---+    -- |--------------+--------+-------------------+---+---+
     3  |2025-09-26|Barbara.gmail.com| F | 17|    3  |2025|gmail.com|2025-09-26|Barbara.gmail.com| F | 17|
     -- |--------+-|-----------------+---+---+    -- |----|---------+--------+-|-----------------+---+---+
     4  |2025-10-14|Alfred.gmail.com | F | 11|    4  |2025|gmail.com|2025-10-14|Alfred.gmail.com | F | 11|
     -- |--------+-|-----------------+---+---+    -- |----|---------+--------+-|-----------------+---+---+
     5  |2025-10-28|Al.gmail.com     | F | 12|    5  |2025|gmail.com|2025-10-28|Al.gmail.com     | F | 12|
     -- |----------+--------------------------    -- |----+--------------------+--------------------------
    [MONDAY]                                     [WANT]

    /*                   _
    (_)_ __  _ __  _   _| |_
    | | `_ \| `_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    */

    d:/xls/monday.xlsx

    -----------------------+
    | A1| fx        |NAME  |
    ------------------------------------------
    [_] |    A     |    B            | C | D |
    ------------------------------------------
     1  | NAME     |EMAIL            |SEX|AGE|
     -- |----------+-----------------+---+---+
     2  |2025-09-15|Alice.gmail.com  | F |   |
     -- |--------+-------------------+---+---+
     3  |2025-09-26|Barbara.gmail.com| F | 17|
     -- |--------+-|-----------------+---+---+
     4  |2025-10-14|Alfred.gmail.com | F | 11|
     -- |--------+-|-----------------+---+---+
     5  |2025-10-28|Al.gmail.com     | F | 12|
     -- |----------+--------------------------
    [MONDAY]


    %utlfkil(%sysfunc(pathname(WPSWBHTM))); /*-- disable precode --*/
    &_init_;

    %utlfkil(d:/xls/monday.xlsx); /*-- incase you rerun          --*/

    libname xlsinp excel "d:/xls/monday.xlsx";

    proc datasets lib=xlsinp; /*-- incase you rerun              --*/
     delete monday;
    run;quit;

    data xlsinp.monday;
     informat date $10. email $19. sex $2.;
     input date email sex age;
    cards4;
    2025-09-15 Alice@gmail.com  F 15
    2025-09-26 Barbara@gmail.com F 17
    2025-10-14 Alfred@gmail.com F 11
    2025-10-28 Al@gmail.com F 12
    ;;;;
    run;quit;

    libname xlsinp clear;

    /*
    | | ___   __ _
    | |/ _ \ / _` |
    | | (_) | (_| |
    |_|\___/ \__, |
             |___/
    */

    862
    5863      proc datasets lib=xlsinp; /*-- incase you rerun              --*/
    NOTE: No matching members in directory
    CREATE INDENTED SEX COLUMN
    START READING AT ROW 4

    The DATASETS Procedure

       Directory

    Libref    XLSINP
    Engine    OLEDB
    5864       delete monday;
    5865      run;quit;
    NOTE: XLSINP.MONDAY (memtype="DATA") was not found, and has not been deleted
    NOTE: Procedure datasets step took :
          real time : 0.206
          cpu time  : 0.062


    5866
    5867      data xlsinp.monday;
    5868       informat date $10. email $19. sex $2.;
    5869       input date email sex age;
    5870      cards4;

    NOTE: Data set "XLSINP.monday" has an unknown number of o  ervation(s) and 4 variable(s)
    NOTE: The data step took :
          real time : 0.219
          cpu time  : 0.046


    5871      2025-09-15 Alice@gmail.com  F 15
    5872      2025-09-26 Barbara@gmail.com F 17
    5873      2025-10-14 Alfred@gmail.com F 11
    5874      2025-10-28 Al.gmail@com F 12
    5875      ;;;;
    5876      run;quit;
    NOTE: Libref XLSINP has been deassigned.
    5877
    5878      libname xlsinp clear;
    5879      quit; run;
    5880      ODS _ALL_ CLOSE;
    5881      FILENAME WPSWBHTM CLEAR;

    /*---
     _ __  _ __ ___   ___ ___  ___ ___
    | `_ \| `__/ _ \ / __/ _ \/ __/ __|
    | |_) | | | (_) | (_|  __/\__ \__ \
    | .__/|_|  \___/ \___\___||___/___/
    |_|

    We create a routine, prepxls with input and output arguments.
    Put the macro in your autocall library and call using your
    new inputs and outputs.

    %prepxls(
       inp_workbook = d:/xls/monday.xlsx
      ,inp_sheet    = monday
      ,out_workbook = d:/xls/want.xlsx
      ,out_sheet    = want
      );
    ---*/

    %utlfkil(%sysfunc(pathname(WPSWBHTM))); /*-- disable precode --*/
    &_init_;

    %macro prepxls(
       inp_workbook = d:/xls/monday.xlsx
      ,inp_sheet    = monday
      ,out_workbook = d:/xls/want.xlsx
      ,out_sheet    = want
      );

       %utlfkil(&out_workbook); /*-- incase you rerun --*/

       libname xlsinp excel "&inp_workbook";
       libname xlsout excel "&out_workbook";

       proc datasets lib=xlsout;
        delete want;
       run;quit;

       data xlsout.&outsheett;
         retain year domain;
         set xlsinp.&inp_sheet;
         domain = scan(email,2,'@');
         year   = scan(date,1,'.');
       run;quit;

       libname xlsinp clear;
       libname xlsout clear;

    %mend prepxls;

    %prepxls(
       inp_workbook = d:/xls/monday.xlsx
      ,inp_sheet    = monday
      ,out_workbook = d:/xls/want.xlsx
      ,out_sheet    = want
      );

    OUTPUT
    ======

    COLUMNS YEAR AND DOMAIN ADDED

    d:/xls/want.xlsx

    -----------------+
    | A1| fx  |NAME  |
    ---------------------------------------------------------
    [_] |  A |    B    |    C     |    C            | E | F |
    ---------------------------------------------------------
     1  |YEAR|DOMAIN   | NAME     |EMAIL            |SEX|AGE|
     -- |----+---------+----------+-----------------+---+---+
     2  |2025|gmail.com|2025-09-15|Alice.gmail.com  | F |   |
     -- |--------------+--------+-------------------+---+---+
     3  |2025|gmail.com|2025-09-26|Barbara.gmail.com| F | 17|
     -- |----|---------+--------+-|-----------------+---+---+
     4  |2025|gmail.com|2025-10-14|Alfred.gmail.com | F | 11|
     -- |----|---------+--------+-|-----------------+---+---+
     5  |2025|gmail.com|2025-10-28|Al.gmail.com     | F | 12|
     -- |----+--------------------+--------------------------
    [WANT}

    /*
    | | ___   __ _
    | |/ _ \ / _` |
    | | (_) | (_| |
    |_|\___/ \__, |
             |___/
    */

    233      ODS _ALL_ CLOSE;
    6234      FILENAME WPSWBHTM TEMP;
    NOTE: Writing HTML(WBHTML) BODY file d:\wpswrk\_TD9416\#LN00279
    6235      ODS HTML(ID=WBHTML) BODY=WPSWBHTM GPATH="d:\wpswrk\_TD9416";
    6236      %utlfkil(%sysfunc(pathname(WPSWBHTM))); /*-- disable precode --*/
    6237      &_init_;
    6238
    6239      %macro prepxls(
    6240         inp_workbook = d:/xls/monday.xlsx
    6241        ,inp_sheet    = monday
    6242        ,out_workbook = d:/xls/want.xlsx
    6243        ,out_sheet    = want
    6244        );
    6245
    6246         %utlfkil(%sysfunc(pathname(WPSWBHTM))); /*-- disable precode --*/
    6247         &_init_;
    6248
    6249         %utlfkil(d:/xls/want.xlsx); /*-- incase you rerun --*/
    6250
    6251         libname xlsinp excel "d:/xls/monday.xlsx";
    6252         libname xlsout excel "d:/xls/want.xlsx";
    6253
    6254         proc datasets lib=xlsout;
    6255          delete want;
    6256         run;quit;
    6257
    6258         data xlsout.want;
    6259           retain year domain;
    6260           set xlsinp.monday;
    6261           domain = scan(email,2,'@');
    6262           year   = scan(date,1,'.');
    6263         run;quit;
    6264
    6265         libname xlsinp clear;
    6266         libname xlsout clear;
    6267
    6268      %mend prepxls;
    6269
    6270      %prepxls(
    6271         inp_workbook = d:/xls/monday.xlsx
    6272        ,inp_sheet    = monday
    6273        ,out_workbook = d:/xls/want.xlsx
    6274        ,out_sheet    = want
    6275        );
    The file d:/xls/want.xlsx does not exist
    NOTE: Library xlsinp assigned as follows:
          Engine:        OLEDB
          Physical Name: d:/xls/monday.xlsx

    NOTE: Library xlsout assigned as follows:
          Engine:        OLEDB
          Physical Name: d:/xls/want.xlsx

    NOTE: No matching members in directory
    CREATE INDENTED SEX COLUMN
    START READING AT ROW 4

    The DATASETS Procedure

       Directory

    Libref    XLSOUT
    Engine    OLEDB
    NOTE: XLSOUT.WANT (memtype="DATA") was not found, and has not been deleted
    NOTE: Procedure datasets step took :
          real time : 0.205
          cpu time  : 0.078



    NOTE: 4 observations were read from "XLSINP.monday"
    NOTE: Data set "XLSOUT.want" has an unknown number of observation(s) and 6 variable(s)
    NOTE: The data step took :
          real time : 0.330
          cpu time  : 0.187


    NOTE: Libref XLSINP has been deassigned.
    NOTE: Libref XLSOUT has been deassigned.
    6276      quit; run;
    6277      ODS _ALL_ CLOSE;
    6278      FILENAME WPSWBHTM CLEAR;

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */
