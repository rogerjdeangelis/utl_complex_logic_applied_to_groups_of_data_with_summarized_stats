# utl_complex_logic_applied_to_groups_of_data_with_summarized_stats
Complex logic applied to groups of data with summarized stats.  Keywords: sas sql join merge big data analytics macros oracle teradata mysql sas communities stackoverflow statistics artificial inteligence AI Python R Java Javascript WPS Matlab SPSS Scala Perl C C# Excel MS Access JSON graphics maps NLP natural language processing machine learning igraph DOSUBL DOW loop stackoverflow SAS community.
    SAS L: Complex logic applied to groups of data with summarized stats

      Not sure you wanted 4 obs out or 14. I chose 4.
      Add a DOW loop or select v1-v3 in the sql clause. if you want 14.


      github
      https://tinyurl.com/yc8esplb
      https://github.com/rogerjdeangelis/utl_complex_logic_applied_to_groups_of_data_with_summarized_stats

      see
      https://listserv.uga.edu/cgi-bin/wa?A2=SAS-L;5b941b2b.1803c

      Same results in WPS and SAS.

      Two Solutions

         1. SQL
         2. DOW loop

    INPUT
    =====

     RULES

       VAR1n2 should equal 1 if VAR1 equals 1 and VAR2 equals 1.
       VAR1n3 should equal 1 if VAR1 equals 1 and VAR3 equals 1.


     WORK.HAVE total obs=14                   |  RULE (4 obs out)
                                              |
       ID     TIME     VAR1    VAR2    VAR3   |   VAR1N2    VAR1N3
                                              |
       101    12:55      1       0       0    |
       101    12:55      0       1       0    |
       101    12:55      0       0       0    |      1         0     Because var1 has a 1 and var2 has a 1
                                              |
       101    13:06      1       0       1    |
       101    13:06      0       1       0    |
       101    13:06      0       0       0    |
       101    13:06      0       0       0    |      1         1     Because var1, var2 and var3 each have a 1
                                              |
       102    13:15      0       1       1    |
       102    13:15      0       0       0    |
       102    13:15      0       0       0    |      0         0
       102    13:15      0       0       0    |
                                              |
       102    13:26      0       0       0    |
       102    13:26      0       0       1    |
       102    13:26      0       0       0    |      0         0

     EXAMPLE OUTPUT

       VAR1N2    VAR1N3

          1         0
          1         1
          0         0
          0         0

    PROCESS
    =======

       1. SQL

          proc sql;
            create
               table wantsql as
            select
               id
              ,time
              ,max(var1=1) * max(var2=1)               as var1n2
              ,max(var1=1) * max(var2=1) * max(var3=1) as var1n3
            from
               have
            group
               by id, time
          ;quit;

       2. DOW

          data wantdow;

           retain v1 v2 v3 .; * not needed but I like to declare;

           keep id time var1n:;

           do until (last.time);

             set have;
             by id time notsorted;

             /* rules
             VAR1n2 should equal 1 if VAR1 equals 1 and VAR2 equals 1.
             VAR1n3 should equal 1 if VAR1 equals 1 and VAR3 equals 1.
             */

            v1 = sum(v1=1,var1);
            v2 = sum(v2=1,var2);
            v3 = sum(v3=1,var3);

           end;

           var1n2 = (v1>0 and v2>0);
           var1n3 = (v1>0 and v2>0 and v3>0);

           call missing(v1,v2,v3);
          run;quit;

    *                _               _       _
     _ __ ___   __ _| | _____     __| | __ _| |_ __ _
    | '_ ` _ \ / _` | |/ / _ \   / _` |/ _` | __/ _` |
    | | | | | | (_| |   <  __/  | (_| | (_| | || (_| |
    |_| |_| |_|\__,_|_|\_\___|   \__,_|\__,_|\__\__,_|

    ;

    data have;
    input id$ time$ var1-var3;
    cards4;
    101 12:55 1 0 0
    101 12:55 0 1 0
    101 12:55 0 0 0
    101 13:06 1 0 1
    101 13:06 0 1 0
    101 13:06 0 0 0
    101 13:06 0 0 0
    102 13:15 0 1 1
    102 13:15 0 0 0
    102 13:15 0 0 0
    102 13:15 0 0 0
    102 13:26 0 0 0
    102 13:26 0 0 1
    102 13:26 0 0 0
    ;;;;
    run;quit;

    *    _
      __| | _____      __
     / _` |/ _ \ \ /\ / /
    | (_| | (_) \ V  V /
     \__,_|\___/ \_/\_/

    ;

    * SAS;
    data want;

     retain v1 v2 v3 .; * not needed but I like to declare;

     keep id time var1n:;

     do until (last.time);

       set have;
       by id time notsorted;

       /* rules
       VAR1n2 should equal 1 if VAR1 equals 1 and VAR2 equals 1.
       VAR1n3 should equal 1 if VAR1 equals 1 and VAR3 equals 1.
       */

      v1 = sum(v1=1,var1);
      v2 = sum(v2=1,var2);
      v3 = sum(v3=1,var3);

     end;

     var1n2 = (v1>0 and v2>0);
     var1n3 = (v1>0 and v2>0 and v3>0);

     call missing(v1,v2,v3);
    run;quit;

    *wps;
    %utl_submit_wps64('
    libname wrk sas7bdat "%sysfunc(pathname(work))";
    data wrk.wantDowWps;

     retain v1 v2 v3 .; * not needed but I like to declare;

     keep id time var1n:;

     do until (last.time);

       set wrk.have;
       by id time notsorted;

       /* rules
       VAR1n2 should equal 1 if VAR1 equals 1 and VAR2 equals 1.
       VAR1n3 should equal 1 if VAR1 equals 1 and VAR3 equals 1.
       */

      v1 = sum(v1=1,var1);
      v2 = sum(v2=1,var2);
      v3 = sum(v3=1,var3);

     end;

     var1n2 = (v1>0 and v2>0);
     var1n3 = (v1>0 and v2>0 and v3>0);

     call missing(v1,v2,v3);
    run;quit;
    ');

    *          _
     ___  __ _| |
    / __|/ _` | |
    \__ \ (_| | |
    |___/\__, |_|
            |_|
    ;


    * wps
    %utl_submit_wps64('
    libname wrk sas7bdat "%sysfunc(pathname(work))";
    proc sql;
      create
         table wrk.wantSqlWps as
      select
         id
        ,time
        ,max(var1=1) * max(var2=1)               as var1n2
        ,max(var1=1) * max(var2=1) * max(var3=1) as var1n3
      from
         wrk.have
      group
         by id, time
    ;quit;
    run;quit;
    ');


