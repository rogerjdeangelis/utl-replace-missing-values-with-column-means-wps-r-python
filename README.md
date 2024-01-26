# utl-replace-missing-values-with-column-means-wps-r-python
Replace missing values with column means wps r python 
    %let pgm=utl-replace-missing-values-with-column-means-wps-r-python;

     Replace missing values with column means wps r python

     github
     http://tinyurl.com/2xy73buu
     https://github.com/rogerjdeangelis/utl-replace-missing-values-with-column-means-wps-r-python

      SOLUTIONS

          1 wps sql
          2 wps r sql
          3 wps python sql
          4 wps r dplyr
            https://stackoverflow.com/questions/76318378/replace-nas-by-the-mean-of-the-column-using-replace-na
            https://stackoverflow.com/users/8015585/blitz1259
          5 wps r zoo
          6 wps python no sql
          7 wps r python sql (very odd worls without on clause)
          8 wps stdize
    /*               _     _
     _ __  _ __ ___ | |__ | | ___ _ __ ___
    | `_ \| `__/ _ \| `_ \| |/ _ \ `_ ` _ \
    | |_) | | | (_) | |_) | |  __/ | | | | |
    | .__/|_|  \___/|_.__/|_|\___|_| |_| |_|
    |_|
    */

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*        INPUT                             PROCESS                                           OUTPUT                      */
    /*                                                                                                                        */
    /*  SD1.HAVE total obs=19           WPS SQL                                             SD1.WANT total obs=19             */
    /*                                  =======                                                                               */
    /*   NAME     AGE HEIGHT WEIGHT     select                                           NAME      AGE    HEIGHT   WEIGHT     */
    /*                                     l.name                                                                             */
    /*   Alfred    14  69.0   112.5        ,coalesce(l.age,r.avg_age)       as age       Alfred   14.00  69.0000  112.500     */
    /*   Alice     13  56.5    84.0        ,coalesce(l.height,r.avg_height) as height    Alice    13.00  56.5000   84.000     */
    /*   Barbara   13  65.3    98.0        ,coalesce(l.weight,r.avg_weight) as weight    Barbara  13.00  65.3000   98.000     */
    /*   Carol     14  62.8   102.5     from                                             Carol    14.00  62.8000  102.500     */
    /*   Henry      .  63.5   102.5         sd1.have as l left join                      Henry    13.25  63.5000  102.500     */
    /*   James      .  57.3    83.0           (                                          James    13.25  57.3000   83.000     */
    /*   Jane      12  59.8      .             select                                    Jane     12.00  59.8000   97.933     */
    /*   Janet     15  62.5   112.5               name                                   Janet    15.00  62.5000  112.500     */
    /*   Jeffrey   13    .     84.0              ,avg(age)    as avg_age                 Jeffrey  13.00  62.9765   84.000     */
    /*   John      12  59.0    99.5              ,avg(height) as avg_height              John     12.00  59.0000   99.500     */
    /*   Joyce     11    .     50.5              ,avg(weight) as avg_weight              Joyce    11.00  62.9765   50.500     */
    /*   Judy      14  64.3    90.0             from                                     Judy     14.00  64.3000   90.000     */
    /*   Louise    12  56.3    77.0               sd1.have                               Louise   12.00  56.3000   77.000     */
    /*   Mary       .  66.5      .            ) as r                                     Mary     13.25  66.5000   97.933     */
    /*   Philip    16  72.0      .       on                                              Philip   16.00  72.0000   97.933     */
    /*   Robert    12  64.8   128.0         l.name = r.name                              Robert   12.00  64.8000  128.000     */
    /*   Ronald    15  67.0   133.0                                                      Ronald   15.00  67.0000  133.000     */
    /*   Thomas    11  57.5      .                                                       Thomas   11.00  57.5000   97.933     */
    /*   William   15  66.5   112.0      WPS R and PYTHON (INTERESTING)                  William  15.00  66.5000  112.000     */
    /*                                   ================                                                                     */
    /*                                   select  name                                                                         */
    /*                                          ,coalesce (age,avg(age)       over ())  as age                                */
    /*                                          ,coalesce (height,avg(height) over ())  as height                             */
    /*                                          ,coalesce (weight,avg(weight) over ())  as weight                             */
    /*                                   from    have                                                                         */
    /*                                                                                                                        */
    /*                                   WPS R dplyr                                                                          */
    /*                                   ===========                                                                          */
    /*                                   savnam <- have[,1];                                                                  */
    /*                                   have<-have[,-1];                                                                     */
    /*                                   have[is.na(have)] <- sapply(have, function(x) mean(x, na.rm = TRUE));                */
    /*                                   want<-cbind(NAME=savnam,have);                                                       */
    /*                                   want;                                                                                */                                                 */
    /*                                                                                                                        */
    /*                                   WPS R zoo                                                                            */
    /*                                   ==========                                                                           */
    /*                                   want<- na.aggregate(have[,-1]);                                                      */
    /*                                   want<-cbind(NAME=have[,1],want);                                                     */
    /*                                                                                                                        */
    /*                                   WPS Python                                                                           */
    /*                                   ==========                                                                           */
    /*                                   have=have.fillna(have.mean(numeric_only=True));                                      */
    /*                                                                                                                        */
    /*                                                                                                                        */
    /*                                   WPS R Python SQL (very odd does not require on clause)                               */
    /*                                   =======================================================                              */
    /*                                   select                                                                               */
    /*                                      l.name                                                                            */
    /*                                      ,coalesce(l.age,r.avg_age)       as age                                           */
    /*                                      ,coalesce(l.height,r.avg_height) as height                                        */
    /*                                      ,coalesce(l.weight,r.avg_weight) as weight                                        */
    /*                                   from                                                                                 */
    /*                                       sd1.have as l left join                                                          */
    /*                                         (                                                                              */
    /*                                          select                                                                        */
    /*                                             name                                                                       */
    /*                                            ,avg(age)    as avg_age                                                     */
    /*                                            ,avg(height) as avg_height                                                  */
    /*                                            ,avg(weight) as avg_weight                                                  */
    /*                                           from                                                                         */
    /*                                             sd1.have                                                                   */
    /*                                         ) as r                                                                         */
    /*                                                                                                                        */
    /*                                    WORKS WITHOUT ON CLAUSE                                                             */
    /*                                    FAILS WITH ON CLAUSE                                                                */
    /*                                                                                                                        */
    /*                                    WPS PYTHON STDIZE                                                                   */
    /*                                    =================                                                                   */
    /*                                    proc stdize data=sd1.have out=sd1.want method=mean reponly;                         */
    /*                                         var _numeric_;                                                                 */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*                   _
    (_)_ __  _ __  _   _| |_
    | | `_ \| `_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    */


    options validvarname=upcase;
    libname sd1 "d:/sd1";
    data sd1.have;
      input NAME$ AGE HEIGHT WEIGHT;
    cards4;
    Alfred  14  69.0   112.5
    Alice   13  56.5    84.0
    Barbara 13  65.3    98.0
    Carol   14  62.8   102.5
    Henry    .  63.5   102.5
    James    .  57.3    83.0
    Jane    12  59.8      .
    Janet   15  62.5   112.5
    Jeffrey 13    .     84.0
    John    12  59.0    99.5
    Joyce   11    .     50.5
    Judy    14  64.3    90.0
    Louise  12  56.3    77.0
    Mary     .  66.5      .
    Philip  16  72.0      .
    Robert  12  64.8   128.0
    Ronald  15  67.0   133.0
    Thomas  11  57.5      .
    William 15  66.5   112.0
    ;;;;
    run;quit;

    /*                                  _
    / | __      ___ __  ___   ___  __ _| |
    | | \ \ /\ / / `_ \/ __| / __|/ _` | |
    | |  \ V  V /| |_) \__ \ \__ \ (_| | |
    |_|   \_/\_/ | .__/|___/ |___/\__, |_|
                 |_|                 |_|
    */

    proc datasets lib=sd1 nolist mt=data mt=view nodetails;delete want; run;quit;

    %utl_submit_wps64x('
    options validvarname=any;
    libname sd1 "d:/sd1";
    proc sql;
     create
        table want as
     select
        l.name
        ,coalesce(l.age,r.avg_age)       as age
        ,coalesce(l.height,r.avg_height) as height
        ,coalesce(l.weight,r.avg_weight) as weight
     from
         sd1.have as l left join
           (
            select
               name
              ,avg(age)    as avg_age
              ,avg(height) as avg_height
              ,avg(weight) as avg_weight
             from
               sd1.have
           ) as r
      on
         l.name = r.name
    ;quit;
    proc print data=want split="_";
    run;quit;
    ');

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*  The WPS System                                                                                                        */
    /*                                                                                                                        */
    /*  Obs     NAME       age     height    weight    * mean substituted                                                     */
    /*                                                                                                                        */
    /*    1    Alfred     14.00     69.0     112.500                                                                          */
    /*    2    Alice      13.00     56.5      84.000                                                                          */
    /*    3    Barbara    13.00     65.3      98.000                                                                          */
    /*    4    Carol      14.00     62.8     102.500                                                                          */
    /*    5    Henry      13.25 *   63.5     102.500                                                                          */
    /*    6    James      13.25     57.3      83.000                                                                          */
    /*    7    Jane       12.00     59.8      97.933 *                                                                        */
    /*    8    Janet      15.00     62.5     112.500                                                                          */
    /*    9    Jeffrey    13.00     63.0      84.000                                                                          */
    /*    ,,,,                                                                                                                */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*___                                          _
    |___ \  __      ___ __  ___   _ __   ___  __ _| |
      __) | \ \ /\ / / `_ \/ __| | `__| / __|/ _` | |
     / __/   \ V  V /| |_) \__ \ | |    \__ \ (_| | |
    |_____|   \_/\_/ | .__/|___/ |_|    |___/\__, |_|
                     |_|                        |_|
    */

    proc datasets lib=sd1 nolist mt=data mt=view nodetails;delete want; run;quit;

    %utl_submit_wps64x("
    libname sd1 'd:/sd1';
    proc r;
    export data=sd1.have r=have;
    submit;
    library(sqldf);

    want<-sqldf('
     select
         name
        ,coalesce (age,avg(age)       over ())  as age
        ,coalesce (height,avg(height) over ())  as height
        ,coalesce (weight,avg(height) over ())  as weight
     from
            have
    ');
    ');
    want;
    endsubmit;
    import data=sd1.want r=want;
    run;quit;
    proc print data=sd1.want split='_';
    run;quit;
    ");

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*  The WPS r System                                                                                                      */
    /*                                                                                                                        */
    /*  Obs     NAME       AGE     HEIGHT    WEIGHT                                                                           */
    /*                                                                                                                        */
    /*    1    Alfred     14.00     69.0     112.500                                                                          */
    /*    2    Alice      13.00     56.5      84.000                                                                          */
    /*    3    Barbara    13.00     65.3      98.000                                                                          */
    /*    4    Carol      14.00     62.8     102.500                                                                          */
    /*    5    Henry      13.25     63.5     102.500                                                                          */
    /*    6    James      13.25     57.3      83.000                                                                          */
    /*    7    Jane       12.00     59.8      62.976                                                                          */
    /*    8    Janet      15.00     62.5     112.500                                                                          */
    /*    9    Jeffrey    13.00     63.0      84.000                                                                          */
    /*   10    John       12.00     59.0      99.500                                                                          */
    /*   11    Joyce      11.00     63.0      50.500                                                                          */
    /*   12    Judy       14.00     64.3      90.000                                                                          */
    /*   13    Louise     12.00     56.3      77.000                                                                          */
    /*   14    Mary       13.25     66.5      62.976                                                                          */
    /*   15    Philip     16.00     72.0      62.976                                                                          */
    /*   16    Robert     12.00     64.8     128.000                                                                          */
    /*   17    Ronald     15.00     67.0     133.000                                                                          */
    /*   18    Thomas     11.00     57.5      62.976                                                                          */
    /*   19    William    15.00     66.5     112.000                                                                          */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*____                                    _   _                             _
    |___ /  __      ___ __  ___   _ __  _   _| |_| |__   ___  _ __    ___  __ _| |
      |_ \  \ \ /\ / / `_ \/ __| | `_ \| | | | __| `_ \ / _ \| `_ \  / __|/ _` | |
     ___) |  \ V  V /| |_) \__ \ | |_) | |_| | |_| | | | (_) | | | | \__ \ (_| | |
    |____/    \_/\_/ | .__/|___/ | .__/ \__, |\__|_| |_|\___/|_| |_| |___/\__, |_|
                     |_|         |_|    |___/                                |_|
    */

    proc datasets lib=sd1 nolist mt=data mt=view nodetails;delete want; run;quit;
    proc datasets lib=work mt=data mt=view; delete want want_r_long_names; run;quit;

    %utlfkil(d:/xpt/want.xpt);

    %utl_submit_wps64x("
    options validvarname=any lrecl=32756;
    libname sd1 'd:/sd1';
    proc python;
    export data=sd1.have python=have;
    submit;
    import pyreadstat as ps;
    from os import path;
    import pandas as pd;
    import numpy as np;
    from pandasql import sqldf;
    mysql = lambda q: sqldf(q, globals());
    from pandasql import PandaSQL;
    pdsql = PandaSQL(persist=True);
    sqlite3conn = next(pdsql.conn.gen).connection.connection;
    sqlite3conn.enable_load_extension(True);
    sqlite3conn.load_extension('c:/temp/libsqlitefunctions.dll');
    mysql = lambda q: sqldf(q, globals());
    want = pdsql('''
     select
         name
        ,coalesce (age,avg(age)       over ())  as age
        ,coalesce (height,avg(height) over ())  as height
        ,coalesce (weight,avg(height) over ())  as weight
     from
            have
    ''');
    print(want);
    ps.write_xport(want,'d:\\xpt\\want.xpt',table_name='want',file_format_version=5
    ,column_labels=['NAME','AGE' ,'HEIGHT' ,'WEIGHT']);
    endsubmit;
    ");

    /*--- handles long variable names by using the label to rename the variables  ----*/

    libname xpt xport "d:/xpt/want.xpt";
    proc contents data=xpt._all_;
    run;quit;

    data want_py_long_names;
      %utl_rens(xpt.want) ;
      set want;
    run;quit;
    libname xpt clear;

    proc print data=want_py_long_names;
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*  The WPS PYTHON Procedure                                                                                              */
    /*                                                                                                                        */
    /*          NAME    age     height      weight                                                                            */
    /*  0   Alfred    14.00  69.000000  112.500000                                                                            */
    /*  1   Alice     13.00  56.500000   84.000000                                                                            */
    /*  2   Barbara   13.00  65.300000   98.000000                                                                            */
    /*  3   Carol     14.00  62.800000  102.500000                                                                            */
    /*  4   Henry     13.25  63.500000  102.500000                                                                            */
    /*  5   James     13.25  57.300000   83.000000                                                                            */
    /*  6   Jane      12.00  59.800000   62.976471                                                                            */
    /*  7   Janet     15.00  62.500000  112.500000                                                                            */
    /*  8   Jeffrey   13.00  62.976471   84.000000                                                                            */
    /*  9   John      12.00  59.000000   99.500000                                                                            */
    /*  10  Joyce     11.00  62.976471   50.500000                                                                            */
    /*  11  Judy      14.00  64.300000   90.000000                                                                            */
    /*  12  Louise    12.00  56.300000   77.000000                                                                            */
    /*  13  Mary      13.25  66.500000   62.976471                                                                            */
    /*  14  Philip    16.00  72.000000   62.976471                                                                            */
    /*  15  Robert    12.00  64.800000  128.000000                                                                            */
    /*  16  Ronald    15.00  67.000000  133.000000                                                                            */
    /*  17  Thomas    11.00  57.500000   62.976471                                                                            */
    /*  18  William   15.00  66.500000  112.000000                                                                            */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*  _                                     _       _
    | || |   __      ___ __  ___   _ __    __| |_ __ | |_   _ _ __
    | || |_  \ \ /\ / / `_ \/ __| | `__|  / _` | `_ \| | | | | `__|
    |__   _|  \ V  V /| |_) \__ \ | |    | (_| | |_) | | |_| | |
       |_|     \_/\_/ | .__/|___/ |_|     \__,_| .__/|_|\__, |_|
                      |_|                      |_|      |___/
    */

    proc datasets lib=sd1 nolist mt=data mt=view nodetails;delete want; run;quit;

    %utl_submit_wps64x("
    libname sd1 'd:/sd1';
    proc r;
    export data=sd1.have r=have;
    submit;
     library(dplyr);
     savnam <- have[,1];
     have<-have[,-1];
     have[is.na(have)] <- sapply(have, function(x) mean(x, na.rm = TRUE));
     want<-cbind(NAME=savnam,have);
     want;
    endsubmit;
    ");

    /*___
    | ___|  __      ___ __  ___   _ __   _______   ___
    |___ \  \ \ /\ / / `_ \/ __| | `__| |_  / _ \ / _ \
     ___) |  \ V  V /| |_) \__ \ | |     / / (_) | (_) |
    |____/    \_/\_/ | .__/|___/ |_|    /___\___/ \___/
                     |_|
    */

    %utl_submit_wps64x("
    libname sd1 'd:/sd1';
    proc r;
    export data=sd1.have r=have;
    submit;
    library(zoo);
    want<- na.aggregate(have[,-1]);
    want<-cbind(NAME=have[,1],want);
    want;
    endsubmit;
    import data=sd1.want r=want;
    run;quit;
    proc print data=sd1.want;
    run;quit;
    ");

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* The WPS System                                                                                                         */
    /*                                                                                                                        */
    /*       NAME   AGE   HEIGHT    WEIGHT                                                                                    */
    /* 1   Alfred 14.00 69.00000 112.50000                                                                                    */
    /* 2    Alice 13.00 56.50000  84.00000                                                                                    */
    /* 3  Barbara 13.00 65.30000  98.00000                                                                                    */
    /* 4    Carol 14.00 62.80000 102.50000                                                                                    */
    /* 5    Henry 13.25 63.50000 102.50000                                                                                    */
    /* 6    James 13.25 57.30000  83.00000                                                                                    */
    /* 7     Jane 12.00 59.80000  97.93333                                                                                    */
    /* 8    Janet 15.00 62.50000 112.50000                                                                                    */
    /* 9  Jeffrey 13.00 62.97647  84.00000                                                                                    */
    /* 10    John 12.00 59.00000  99.50000                                                                                    */
    /* 11   Joyce 11.00 62.97647  50.50000                                                                                    */
    /* 12    Judy 14.00 64.30000  90.00000                                                                                    */
    /* 13  Louise 12.00 56.30000  77.00000                                                                                    */
    /* 14    Mary 13.25 66.50000  97.93333                                                                                    */
    /* 15  Philip 16.00 72.00000  97.93333                                                                                    */
    /* 16  Robert 12.00 64.80000 128.00000                                                                                    */
    /* 17  Ronald 15.00 67.00000 133.00000                                                                                    */
    /* 18  Thomas 11.00 57.50000  97.93333                                                                                    */
    /* 19 William 15.00 66.50000 112.00000                                                                                    */
    /*                                                                                                                        */
    /**************************************************************************************************************************/


    /*__                                      _   _                                           _
     / /_   __      ___ __  ___   _ __  _   _| |_| |__   ___  _ __    _ __   ___    ___  __ _| |
    | `_ \  \ \ /\ / / `_ \/ __| | `_ \| | | | __| `_ \ / _ \| `_ \  | `_ \ / _ \  / __|/ _` | |
    | (_) |  \ V  V /| |_) \__ \ | |_) | |_| | |_| | | | (_) | | | | | | | | (_) | \__ \ (_| | |
     \___/    \_/\_/ | .__/|___/ | .__/ \__, |\__|_| |_|\___/|_| |_| |_| |_|\___/  |___/\__, |_|
                     |_|         |_|    |___/                                              |_|
    */

    %utlfkil(d:/xpt/res.xpt);

    proc datasets lib=sd1 nolist mt=data mt=view nodetails;delete want; run;quit;
    proc datasets lib=work mt=data mt=view; delete want want_r_long_names; run;quit;

    %utlfkil(d:/xpt/want.xpt);

    %utl_submit_wps64x("
    options validvarname=any lrecl=32756;
    libname sd1 'd:/sd1';
    proc python;
    export data=sd1.have python=have;
    submit;

     import pyreadstat as ps;
     import pandas as pd;
     import numpy as np;
     have=have.fillna(have.mean(numeric_only=True));

     want=have;
     print(want);

    ps.write_xport(want,'d:\\xpt\\want.xpt',table_name='want',file_format_version=5
    ,column_labels=['NAME','AGE' ,'HEIGHT' ,'WEIGHT']);
    endsubmit;
    ");

    /*--- handles long variable names by using the label to rename the variables  ----*/

    libname xpt xport "d:/xpt/want.xpt";
    proc contents data=xpt._all_;
    run;quit;

    data want_py_long_names;
      %utl_rens(xpt.want) ;
      set want;
    run;quit;
    libname xpt clear;

    proc print data=want_py_long_names;
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* The WPS PYTHON Procedure                                                                                               */
    /*                                                                                                                        */
    /*         NAME    AGE     HEIGHT      WEIGHT                                                                             */
    /* 0   Alfred    14.00  69.000000  112.500000                                                                             */
    /* 1   Alice     13.00  56.500000   84.000000                                                                             */
    /* 2   Barbara   13.00  65.300000   98.000000                                                                             */
    /* 3   Carol     14.00  62.800000  102.500000                                                                             */
    /* 4   Henry     13.25  63.500000  102.500000                                                                             */
    /* 5   James     13.25  57.300000   83.000000                                                                             */
    /* 6   Jane      12.00  59.800000   97.933333                                                                             */
    /* 7   Janet     15.00  62.500000  112.500000                                                                             */
    /* 8   Jeffrey   13.00  62.976471   84.000000                                                                             */
    /* 9   John      12.00  59.000000   99.500000                                                                             */
    /* 10  Joyce     11.00  62.976471   50.500000                                                                             */
    /* 11  Judy      14.00  64.300000   90.000000                                                                             */
    /* 12  Louise    12.00  56.300000   77.000000                                                                             */
    /* 13  Mary      13.25  66.500000   97.933333                                                                             */
    /* 14  Philip    16.00  72.000000   97.933333                                                                             */
    /* 15  Robert    12.00  64.800000  128.000000                                                                             */
    /* 16  Ronald    15.00  67.000000  133.000000                                                                             */
    /* 17  Thomas    11.00  57.500000   97.933333                                                                             */
    /* 18  William   15.00  66.500000  112.000000                                                                             */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*____                                                _                                   _
    |___  | __      ___ __  ___   _ __  _   _   ___  __ _| |  _ __   ___    ___  _ __     ___| | __ _ _   _ ___  ___
       / /  \ \ /\ / / `_ \/ __| | `_ \| | | | / __|/ _` | | | `_ \ / _ \  / _ \| `_ \   / __| |/ _` | | | / __|/ _ \
      / /    \ V  V /| |_) \__ \ | |_) | |_| | \__ \ (_| | | | | | | (_) || (_) | | | | | (__| | (_| | |_| \__ \  __/
     /_/      \_/\_/ | .__/|___/ | .__/ \__, | |___/\__, |_| |_| |_|\___/  \___/|_| |_|  \___|_|\__,_|\__,_|___/\___|
                     |_|         |_|    |___/          |_|
    */

    /*----                                                                   ----*/
    /*---- VERY STRANGE - WORKS WITHOUT ON CLAUSE                            ----*/
    /*----                                                                   ----*/

    proc datasets lib=sd1 nolist mt=data mt=view nodetails;delete want; run;quit;

    %utl_submit_wps64x("
    libname sd1 'd:/sd1';
    proc r;
    export data=sd1.have r=have;
    submit;
    library(sqldf);
    have;
    want<-sqldf('
       select
          l.name
          ,coalesce(l.age,r.avg_age)       as age
          ,coalesce(l.height,r.avg_height) as height
          ,coalesce(l.weight,r.avg_weight) as weight
       from
           have as l left join
             (
              select
                 name
                ,avg(age)    as avg_age
                ,avg(height) as avg_height
                ,avg(weight) as avg_weight
               from
                 have
             ) as r
    ');
    want;
    endsubmit;
    import data=sd1.want r=want;
    run;quit;
    proc print data=sd1.want split='_';
    run;quit;
    ");

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*  Obs     NAME       AGE     HEIGHT    WEIGHT                                                                           */
    /*                                                                                                                        */
    /*    1    Alfred     14.00     69.0     112.500                                                                          */
    /*    2    Alice      13.00     56.5      84.000                                                                          */
    /*    3    Barbara    13.00     65.3      98.000                                                                          */
    /*    4    Carol      14.00     62.8     102.500                                                                          */
    /*    5    Henry      13.25     63.5     102.500                                                                          */
    /*    6    James      13.25     57.3      83.000                                                                          */
    /*    7    Jane       12.00     59.8      97.933                                                                          */
    /*    8    Janet      15.00     62.5     112.500                                                                          */
    /*    9    Jeffrey    13.00     63.0      84.000                                                                          */
    /*   10    John       12.00     59.0      99.500                                                                          */
    /*   11    Joyce      11.00     63.0      50.500                                                                          */
    /*   12    Judy       14.00     64.3      90.000                                                                          */
    /*   13    Louise     12.00     56.3      77.000                                                                          */
    /*   14    Mary       13.25     66.5      97.933                                                                          */
    /*   15    Philip     16.00     72.0      97.933                                                                          */
    /*   16    Robert     12.00     64.8     128.000                                                                          */
    /*   17    Ronald     15.00     67.0     133.000                                                                          */
    /*   18    Thomas     11.00     57.5      97.933                                                                          */
    /*   19    William    15.00     66.5     112.000                                                                          */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*___                             _      _ _
     ( _ )  __      ___ __  ___   ___| |_ __| (_)_______
     / _ \  \ \ /\ / / `_ \/ __| / __| __/ _` | |_  / _ \
    | (_) |  \ V  V /| |_) \__ \ \__ \ || (_| | |/ /  __/
     \___/    \_/\_/ | .__/|___/ |___/\__\__,_|_/___\___|
                     |_|
    */
    proc datasets lib=sd1 nolist mt=data mt=view nodetails;delete want; run;quit;

    %utl_submit_wps64x('
    libname sd1 "d:/sd1";
    proc stdize data=sd1.have out=sd1.want method=mean reponly;
         var _numeric_;
    run;quit;
    proc print data=sd1.want;
    run;quit;
    ');


    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*  Obs     NAME               AGE          HEIGHT          WEIGHT                                                        */
    /*                                                                                                                        */
    /*    1    Alfred               14              69           112.5                                                        */
    /*    2    Alice                13            56.5              84                                                        */
    /*    3    Barbara              13            65.3              98                                                        */
    /*    4    Carol                14            62.8           102.5                                                        */
    /*    5    Henry             13.25            63.5           102.5                                                        */
    /*    6    James             13.25            57.3              83                                                        */
    /*    7    Jane                 12            59.8    97.933333333                                                        */
    /*    8    Janet                15            62.5           112.5                                                        */
    /*    9    Jeffrey              13    62.976470588              84                                                        */
    /*   10    John                 12              59            99.5                                                        */
    /*   11    Joyce                11    62.976470588            50.5                                                        */
    /*   12    Judy                 14            64.3              90                                                        */
    /*   13    Louise               12            56.3              77                                                        */
    /*   14    Mary              13.25            66.5    97.933333333                                                        */
    /*   15    Philip               16              72    97.933333333                                                        */
    /*   16    Robert               12            64.8             128                                                        */
    /*   17    Ronald               15              67             133                                                        */
    /*   18    Thomas               11            57.5    97.933333333                                                        */
    /*   19    William              15            66.5             112                                                        */
    /*                                                                                                                        */
    /**************************************************************************************************************************/


    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */
