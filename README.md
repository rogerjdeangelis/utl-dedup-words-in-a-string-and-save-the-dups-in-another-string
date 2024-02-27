# utl-dedup-words-in-a-string-and-save-the-dups-in-another-string
Dedup words in a string and save the dups in another string 
    %let pgm=utl-dedup-words-in-a-string-and-save-the-dups-in-another-string;

    Dedup words in a string and save the dups in another string

    github
    http://tinyurl.com/4hr6svs9
    https://github.com/rogerjdeangelis/utl-dedup-words-in-a-string-and-save-the-dups-in-another-string

    /*               _     _
     _ __  _ __ ___ | |__ | | ___ _ __ ___
    | `_ \| `__/ _ \| `_ \| |/ _ \ `_ ` _ \
    | |_) | | | (_) | |_) | |  __/ | | | | |
    | .__/|_|  \___/|_.__/|_|\___|_| |_| |_|
    |_|
    */

    /**************************************************************************************************************************/
    /*                            |                                                |                                          */
    /*           INPUT            |            PROCESS                             |                   OUPUT                  */
    /*                            |                                                |                                          */
    /* words="to be or not to be" | data want(keep=words deduped dups);            |       WORDS          DEDUPED      DUPS   */
    /*                            |                                                |                                          */
    /*                            |   length deduped $96 dups $96;                 | to be or not to be  to be or not  to be  */
    /*                            |                                                |                                          */
    /*                            |   words="to be or not to be";                  |                                          */
    /*                            |                                                |                                          */
    /*                            |   do _n_=1 to countw(words);                   |                                          */
    /*                            |                                                |                                          */
    /*                            |    scn=scan(words,_n_);                        |                                          */
    /*                            |                                                |                                          */
    /*                            |    deduped=ifc(findw(deduped,scan(words,_n_))  |                                          */
    /*                            |        , deduped                               |                                          */
    /*                            |        , catx(" ",deduped,scan(words,_n_)));   |                                          */
    /*                            |                                                |                                          */
    /*                            |    lagdup=lag(deduped);                        |                                          */
    /*                            |                                                |                                          */
    /*                            |    if length(lagdup)>=length(deduped) then do; |                                          */
    /*                            |       dups=catx(" ",dups,scn);                 |                                          */
    /*                            |    end;                                        |                                          */
    /*                            |                                                |                                          */
    /*                            |   end;                                         |                                          */
    /*                            |                                                |                                          */
    /*                            |   run;quit;                                    |                                          */
    /*                            |                                                |                                          */
    /**************************************************************************************************************************/

    /*                   _
    (_)_ __  _ __  _   _| |_   _ __  _ __ ___   ___ ___  ___ ___
    | | `_ \| `_ \| | | | __| | `_ \| `__/ _ \ / __/ _ \/ __/ __|
    | | | | | |_) | |_| | |_  | |_) | | | (_) | (_|  __/\__ \__ \
    |_|_| |_| .__/ \__,_|\__| | .__/|_|  \___/ \___\___||___/___/
            |_|               |_|
    */

    data want(keep=words deduped dups);

      length deduped $96 dups $96;

      words="to be or not to be";

      do _n_=1 to countw(words);

       scn=scan(words,_n_);

       deduped = ifc(findw(deduped,scan(words,_n_))
           , deduped
           , catx(" ",deduped,scan(words,_n_)));

       lagdup=lag(deduped);

       if length(lagdup)>=length(deduped) then do;
          dups=catx(" ",dups,scn);
       end;

      end;

    run;quit;

    proc print data=want;
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* WANT  String with removed dups and the duplicate words                                                                 */
    /* ======================================================                                                                 */
    /*                                                                                                                        */
    /* WORK.WANT total obs=1                                                                                                  */
    /*                                                                                                                        */
    /* Obs          WORDS              DEDUPED       DUPS                                                                     */
    /*                                                                                                                        */
    /*  1     to be or not to be     to be or not    to be                                                                    */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */
