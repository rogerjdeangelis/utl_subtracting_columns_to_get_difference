# utl_subtracting_columns_to_get_difference
Subtracting columns to get difference. Keywords: sas sql join merge big data analytics macros oracle teradata mysql sas communities stackoverflow statistics artificial inteligence AI Python R Java Javascript WPS Matlab SPSS Scala Perl C C# Excel MS Access JSON graphics maps NLP natural language processing machine learning igraph DOSUBL DOW loop stackoverflow SAS community.
    Subtracting columns to get difference

    Same result WPS/SAS

    The key to my solution is to traverse the array backwards

    github
    https://github.com/rogerjdeangelis/utl_subtracting_columns_to_get_difference

    see
    https://tinyurl.com/yam2z8bs
    https://communities.sas.com/t5/General-SAS-Programming/Subtracting-columns-to-get-difference/m-p/462948


    INPUT
    =====

    WORK.HAVE total obs=2

    Obs     COUNTRY     SEGMENT    MAR_30    APR_2    APR_3    APR_4

     1     Singapore     Corp        126      126      126      126
     2     malaysia      Ret           3        5        9       17

    EXAMPLE OUTPUT

         COUNTRY     SEGMENT    MAR_30    APR_2    APR_3    APR_4

        malaysia      Ret           3        5        9       17
                                    ==============================

                                                   RULES

        malaysia      Ret           .      2         4         8
                                    .     5-3=2     9-5=4    17-9=8

    PROCESS
    =======

    * key is backward differences so you do not overwrite;
    data want;
      set have;
      array dtes[*] mar_30--apr_4;
      do idx=dim(dtes) to 2 by -1;
         dtes[idx]= dtes[idx]-dtes[idx-1];
      end;
      dtes[1]=.;
      drop idx;
    run;quit;


    OUTPUT
    ======

    WORK.HAVE total obs=2

       COUNTRY     SEGMENT    MAR_30    APR_2    APR_3    APR_4

      Singapore     Corp         .        0        0        0
      malaysia      Ret          .        2        4        8

    *                _              _       _
     _ __ ___   __ _| | _____    __| | __ _| |_ __ _
    | '_ ` _ \ / _` | |/ / _ \  / _` |/ _` | __/ _` |
    | | | | | | (_| |   <  __/ | (_| | (_| | || (_| |
    |_| |_| |_|\__,_|_|\_\___|  \__,_|\__,_|\__\__,_|

    ;
    data have;
    input Country :$15. Segment $ (Mar_30 Apr_2 Apr_3 Apr_4) (:comma12.);
    format Mar_30 Apr_2 Apr_3 Apr_4 comma12.;
    cards;
    Singapore Corp 126 126 126 126
    malaysia Ret 3 5 9 17
    ;
    run;

    * SOLUTION;

    %utl_submit_wps64('
    libname wrk sas7bdat "%sysfunc(pathname(work))";
    data want;
      set wrk.have;
      array dtes[*] mar_30--apr_4;
      do idx=dim(dtes) to 2 by -1;
         dtes[idx]= dtes[idx]-dtes[idx-1];
      end;
      dtes[1]=.;
      drop idx;
    run;quit;
    run;quit;
    proc print;
    run;quit;
    ');
