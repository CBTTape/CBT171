# CBT171
Converted to GitHub via [cbt2git](https://github.com/wizardofzos/cbt2git)

This is still a work in progress. 
Due to amazing work by Alison Zhang and Jake Choi repos are no longer deleted.

```
//***FILE 171 is a collection of several important utilities,       *   FILE 171
//*           contributed by Richard Rice.                          *   FILE 171
//*                                                                 *   FILE 171
//*           email:  rcmodeller1955@yahoo.com                      *   FILE 171
//*                                                                 *   FILE 171
//*      These utilities are:                                       *   FILE 171
//*                                                                 *   FILE 171
//* (Removed) DITTO   -  Removed because it doesn't work anymore    *   FILE 171
//*                      with the latest z/OS operating systems.    *   FILE 171
//*                                                                 *   FILE 171
//*     (New) DLIUTILS - New. Utilities to unload and reload an     *   FILE 171
//*                      IMS DL/I database.                         *   FILE 171
//*                                                                 *   FILE 171
//*           TAPEMAP -  A REWRITE OF THE PROGRAM THAT IS ON        *   FILE 171
//*                      FILE 299, BUT BROKEN INTO SEPARATE         *   FILE 171
//*                      CSECTS AND SUBROUTINE CALLS.  The reports  *   FILE 171
//*                      look different from the ones produced by   *   FILE 171
//*                      File 299's TAPEMAP.                        *   FILE 171
//*                                                                 *   FILE 171
//*           DISASM  -  A REDESIGN OF THE DISASSEMBLER ON          *   FILE 171
//*                      FILE 217, BUT BROKEN INTO CSECTS.  THIS    *   FILE 171
//*                      DISASSEMBLER CALLS THE ASSEMBLER AND       *   FILE 171
//*                      ALLOWS YOU TO USE REAL MACROS AND THEIR    *   FILE 171
//*                      DSECTS FOR LABEL MAPPING.                  *   FILE 171
//*                                                                 *   FILE 171
//*           SMFSPLIT - SMFSPLIT IS AN ASSEMBLER PROGRAM WHICH     *   FILE 171
//*                      ALLOWS YOU TO BREAK UP SMF RECORDS BY      *   FILE 171
//*                      TYPE, TO SEPARATE DATASETS.  THIS DEALS    *   FILE 171
//*                      WITH THE RAW SMF RECORDS.  YOU CAN POST    *   FILE 171
//*                      PROCESS THEM LATER.  I THINK THAT THIS     *   FILE 171
//*                      UTILITY IS QUITE A RARE DEAL.  MOST SMF    *   FILE 171
//*                      PROCESSING PROGRAMS PICK A TYPE, AND       *   FILE 171
//*                      FORMAT A REPORT.  THIS PROGRAM ISOLATES    *   FILE 171
//*                      ALL RECORDS OF A GIVEN TYPE TO AN          *   FILE 171
//*                      EXTRACTION FILE.                           *   FILE 171
//*                                                                 *   FILE 171
//*           TPX     -  IF THE NETWORK PACKAGE, TPX, IS RUNNING    *   FILE 171
//*                      ON AN ISOLATED MACHINE, AND USERS FROM     *   FILE 171
//*                      THE PRODUCTION MACHINES ARE TRYING TO      *   FILE 171
//*                      LOG ONTO TPX, THERE IS A PROBLEM IN        *   FILE 171
//*                      SYNCHRONIZING RACF DATABASES FROM THE      *   FILE 171
//*                      SEPARATE SYSTEMS TO PROPERLY VERIFY THE    *   FILE 171
//*                      LOGON.  THIS IS A TPX EXIT AND AN STC,     *   FILE 171
//*                      WHICH SOLVES THE PROBLEM VERY INGENIOUSLY. *   FILE 171
//*                                                                 *   FILE 171
//*           FX      -  THIS PACKAGE IS A VTAM APPLICATION THAT    *   FILE 171
//*                      RUNS ON MULTIPLE SYSTEMS, AND ALLOWS       *   FILE 171
//*                      YOU TO SEND DATA FILES IN BULK, FROM       *   FILE 171
//*                      ONE SYSTEM TO ALL OF THEM.                 *   FILE 171
//*                                                                 *   FILE 171
//*           SYSTEM UTILITY (SUTL)  -  A VTAM LU 6.2 APPLICATION   *   FILE 171
//*                      THAT ALLOWS A TSO USER TO OBTAIN INFORMA-  *   FILE 171
//*                      TION ABOUT EXECUTING JOBS, THE APF LIST,   *   FILE 171
//*                      IPL DATE/TIME/SYSRES, LINK LIST, ETC.      *   FILE 171
//*                      MOST OF THIS INFORMATION IS USUALLY        *   FILE 171
//*                      AVAILABLE VIA OTHER UTILITIES ALREADY IN   *   FILE 171
//*                      USE, BUT THIS UTILITY ALLOWS THE TSO USER  *   FILE 171
//*                      TO GET INFO FROM A SYSTEM THAT HE IS NOT   *   FILE 171
//*                      LOGGED ON TO.                              *   FILE 171
//*                                                                 *   FILE 171
//*        (NOTE.  DAVE CARTWRIGHT, WHO CONTRIBUTED FILE 172 TO     *   FILE 171
//*                THIS TAPE, HAS MADE SOME UPDATES TO A FEW OF     *   FILE 171
//*                THE DITTO FILES.  THIS WAS FOR AN MVS/ESA 3.1    *   FILE 171
//*                SYSTEM.  IF YOU FEEL YOU NEED THESE UPDATES,     *   FILE 171
//*                THEY ARE INCLUDED HERE AS MEMBER $DITCRTW.)      *   FILE 171
//*                                                                 *   FILE 171
//*           - - - - - - - - - - - - - - - - - - - - -             *   FILE 171
//*                                                                 *   FILE 171
//*                    SYSTEM UTILITY (SUTL)                        *   FILE 171
//*                                                                 *   FILE 171
//*        SUTL IS A VTAM LU 6.2 APPLICATION THAT ALLOWS A TSO      *   FILE 171
//*        USER TO OBTAIN INFORMATION ABOUT EXECUTING JOBS, THE     *   FILE 171
//*        APF LIST, IPL DATE/TIME/SYSRES, LINK LIST, ETC.  MOST    *   FILE 171
//*        OF THIS INFORMATION IS USUALLY AVAILABLE VIA OTHER       *   FILE 171
//*        UTILITIES ALREADY IN USE, SO WHY BOTHER GOING TO THE     *   FILE 171
//*        TROUBLE OF 'RE-INVENTING' THIS WHEEL AND ADDING VTAM     *   FILE 171
//*        OVER-HEAD IN THE PROCESS?  BEING A VTAM APPLICATION      *   FILE 171
//*        MEANS THAT A TSO USER CAN GET INFO FROM A SYSTEM         *   FILE 171
//*        THAT HE IS NOT LOGGED ON TO.  IF YOU HAVE MULTIPLE       *   FILE 171
//*        PROCESSORS OR LPARS, YOU CAN "WATCH" EXECUTING JOBS ON   *   FILE 171
//*        ANY OF THE SYSTEMS NO MATTER WHICH SYSTEM YOU ARE        *   FILE 171
//*        LOGGED ON TO.  BESIDES IT WAS A GOOD WAY TO LEARN        *   FILE 171
//*        SOMETHING AND HAVE A USEFUL UTILITY WHEN IT WAS          *   FILE 171
//*        WORKING.                                                 *   FILE 171
//*                                                                 *   FILE 171
//*        SUTL CONSISTS OF TWO BASIC COMPONENTS, (1) A DATA        *   FILE 171
//*        COLLECTOR THAT WOULD PROBABLY BE BEST TO RUN AS A        *   FILE 171
//*        STARTED TASK (STC) AND (2) THE TSO/SPF CODE THAT SENDS   *   FILE 171
//*        REQUESTS TO THE DATA COLLECTOR AND DISPLAYS THE DATA.    *   FILE 171
//*                                                                 *   FILE 171
//*        THE DATA COLLECTOR (STC) SHOULD BE RUN ON EACH SYSTEM.   *   FILE 171
//*        THE STC DOES REQUIRE APF AUTHORIZATION FOR THE UCB       *   FILE 171
//*        FUNCTION.  IF YOU REMOVE THE UCB FUNCTION, SUTL WILL     *   FILE 171
//*        NOT REQUIRE ANY SPECIAL PRIVILEGES.                      *   FILE 171
//*                                                                 *   FILE 171
//*        THE TSO/SPF PART REQUIRES ONE VTAM APPL ID PER ACTIVE    *   FILE 171
//*        TSO USER.  THESE APPL IDS ARE ASSEMBLED AND LINK         *   FILE 171
//*        EDITED INTO A LOAD MODULE AS PART OF THE INSTALLATION    *   FILE 171
//*        STEPS.  I FELT THAT IT WOULD BE LESS OVERHEAD PER        *   FILE 171
//*        INVOCATION TO SEARCH A PRE-ASSEMBLED/LINK EDITED LOAD    *   FILE 171
//*        MODULE THAN TO READ A PARAMETER DATA SET (THIS WOULD     *   FILE 171
//*        MEAN ALLOCATING THE DATA SET, OPENING IT, READING AND    *   FILE 171
//*        SCANNING EACH STATEMENT, CLOSING, AND THEN               *   FILE 171
//*        DE-ALLOCATING).                                          *   FILE 171
//*                                                                 *   FILE 171
//*   IEFUTL  -  A sample IEFUTL SMF exit which does the following  *   FILE 171
//*              things:                                            *   FILE 171
//*                                                                 *   FILE 171
//*        If this is for a batch job or started                    *   FILE 171
//*        task, allow to abend.                                    *   FILE 171
//*                                                                 *   FILE 171
//*        For TSO users:                                           *   FILE 171
//*        Check user's access to a RACF resource.                  *   FILE 171
//*        As is, this exit checks for the user's access to         *   FILE 171
//*        class 'TIMEOUT', entity 'TSOUSER'.                       *   FILE 171
//*                                                                 *   FILE 171
//*        If permitted to resource                                 *   FILE 171
//*          If wait time exceeded                                  *   FILE 171
//*             extend time 5 minutes                               *   FILE 171
//*                                                                 *   FILE 171
//*        If CPU time exceeded                                     *   FILE 171
//*          cancel                                                 *   FILE 171
//*                                                                 *   FILE 171
//*        If not permitted to resource                             *   FILE 171
//*          cancel                                                 *   FILE 171
//*                                                                 *   FILE 171
```
