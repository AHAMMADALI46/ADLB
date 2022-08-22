# ADLB
/*BDS-------BASIC DATA STRUCTURE
---------------findings Domains
StudyId
Usubjid
---Seq
Domain
--Testcd-----PARAMCD
---TEST------Param
---ORRES
---STRESC-----AVALC (Analysis Value in Character)
---STRESN-----AVAL
---STRESU----AVALU

PARAM-----TEST_NAME||"("||--stresu||")";
--BLFL------ABLFL
VISIT------AVISIT
--DY----ADY

DTYPE------LOCF, BOCF, WOCF
LOCF----Last observation carried forward
BOCF-----BEST/BASELINE Observation carried forward
WOCF-----Worst observation carried forward


IF ABLFL = 'Y' or --BLFL='Y' AND AVAL NE . Then Base=AVAL
IF ABLFL NE 'Y' AND AVAL NE . THEN CHG=AVAL-BASE;
ANRIND-------NORMAL RANGE INDICATOR FOR POST BASELINE VALUES

IF ABLFL NE 'Y' AND LBDTC>=RFSTDTC THEN  ANRIND=LBNRIND
BNRIND-------NORMAL RANGE INDICATOR FOR POST BASELINE VALUES
IF ABLFL='Y' AND THEN BNRIND=LBNRIND*/

/************Program****************/
Data Lb;
set sdtm.lb;
if lbblfl="Y" then base=lbstresn;
else BASE=.;
run;

/*ADLB-------ADAM Laboratory test results
Source: SDTM_LB, ADAM.ADSL*/

options validvarname=upcase nofmterr msglevel=1 missing=' ';

Proc sort data=sdtm.lb out=lb; By usubjid; run;
proc sort data=adma.adsl out=adsl; by usubjid; run;

data lb1 (keep=studyid domain usubjid lbseq lbtestcd lbtest param paramcd paramn lbcat lborres     aval avalc ablfl ablfn base chg lborresu lbornrlo lbornrhi anrhi lbstresc lbstresn lbstresu lbstnrlo anrlo lbstnrhi, lbspccnd lbblfl lbfast visitnum visit avisit anrhi lbnrind bnrind lbstat lbreasnd lbnam lbspec avisitn aperiod aperiodc visitdy ady epoch lbdtc adtm adt atm adtf 

set lb (drop=domain);
domain="ADLB";
param=cats(lbtest,'(',LBSTRESU,');
Paramcd=lbtestcd;
paramn=lbtest;
Parcat=lbcat;
If lbstresn ne . then 
aval=lbstresn;
If lbstresn ne ' ' then 
avalc=lbblfl;
If lbblfl ne ' ' then 
ablfl=lbblfl;
if lbblfl="Y" then ablfn=1;
if lbblfl="Y" And AVAL Ne. Then Base=Aval;
else base=.;

if ablfl ne "Y" and aval ne .  then chg=aval-base;
anrhi=put(lbornrhi, best.);
anrlo=put(LBSTNRLO, best.);

IF ABLFL="Y" Then BNRIND=LBNRIND;
IF ABLFL NE 'Y' THEN ANRIND=LBNRIND;

aperiod=visit;
aperiodc=visit;
ady=visitdy;

if LBDTC ne ' ' then LBDTC_=CAT(LBDTC||"T"||"00:00:00");
ADTM=input(LBDTC_, anydtdtm.);
ADT=input(LBDTC, YYMMDD10.);
ATM=input(substr(LBDTC_,12),time8.);
ADTF=" ";
ATMF="Y";
Format ADT date9.

lbstnrhi=lbstnrhi;





