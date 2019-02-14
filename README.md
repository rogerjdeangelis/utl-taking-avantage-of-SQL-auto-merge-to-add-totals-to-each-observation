# utl-taking-avantage-of-SQL-auto-merge-to-add-totals-to-each-observation
Taking avantage of SQL auto merge to add totals to each observation
    Taking avantage of SQL auto merge to add totals to each observation

    github
    https://tinyurl.com/yyw2hog5
    https://github.com/rogerjdeangelis/utl-taking-avantage-of-SQL-auto-merge-to-add-totals-to-each-observation

    SAS Forum
    https://tinyurl.com/y5hah4cx
    https://communities.sas.com/t5/SAS-Procedures/sum-up-values-across-observation-by-multiple-conditions/m-p/535483

    Elegant Solution by PGStats
    https://communities.sas.com/t5/user/viewprofilepage/user-id/462


    INPUT
    =====

    data have;
    input ID $ AorB $ Year Supply;
    cards4;
    15169 A 2009 30
    15169 A 2009 60
    15169 A 2009 30
    15169 A 2009 90
    15169 A 2010 30
    15169 A 2010 30
    15169 B 2010 30
    15169 A 2010 30
    30629 A 2009 90
    30629 A 2009 90
    30629 A 2009 30
    30629 B 2009 30
    30629 B 2009 30
    ;;;;
    run;

    EXAMPLE OUTPUT
    --------------

    Up to 40 obs WORK.WANT total obs=15

           Original observations        Totals for Supplier A and B added to each ob
       ==============================  ================================================

                                         YEAR_              YEAR_
       ID      AORB    YEAR    SUPPLY  SUPPLY_A           SUPPLY_B

      15169     A      2009      30       210 30+90+30+60     0  No B supplyers
      15169     A      2009      90       210                 0
      15169     A      2009      30       210                 0
      15169     A      2009      60       210                 0

      15169     B      2010      30        90 30+30+30       30 30 Only one B supplier
      15169     A      2010      30        90                30
      15169     A      2010      30        90                30
      15169     A      2010      30        90                30

      30629     B      2009      30       210 30+90+90       60 30+30 for B
      30629     B      2009      30       210                60
      30629     A      2009      30       210                60
      30629     A      2009      90       210                60
      30629     A      2009      90       210                60


    SOLUTION  NICE!!
    ================

    proc sql;
    create table want as
    select
        *,
        sum(case when AorB = "A" then Supply else 0 end) as Year_Supply_A,
        sum(case when AorB = "B" then Supply else 0 end) as Year_Supply_B
    from have
    group by ID, Year;
    quit;


