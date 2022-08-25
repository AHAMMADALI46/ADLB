/*ADLB-----ADAM LABORATORY TEST RESUTS
source: SDTM_LB ADAM.ADSL*/


options validvarname=upcase nofmterr missing=' ' msglevel=i;

Proc sort data=sdtm.lb out=lb; by usubjid; run;
proc sort data=adam.adsl out=adsl; by usubjid; run;

data lb1 (Keep=studyid usubjid domain lbseq lbtestcd lbtest lborres lbstresc lbcat param paramcd
paramn aval lborresu lbornrlo lbornrhi anrhi lbstresc lbstresn lbstresu lbstrnrlo parcat
lbnrind lbstat lbreasnd lbnam lbspec lbspccnd lbblfl lbfast visitnum aperiodc visitdy ady
epoch lbdtc adtm adt atm adtf atmf lbdy aday aendtm aentm aendt aendtf aentmf);
set lb (drop=domain);
domain='ADLB';
param=cats(Lbtest, '(',lbstresu,')');
paramcd=lbtestcd;
paramn=lbtest;
parcat=lbcat;
if lbstresn ne .then
aval-lbstresn;
if lbstresc ne ' ' then
avalc=lbstresc;
if lblfl ne ' ' then
ablfl=lbblfl;
if lbblfl="Y" then ablfl=1;
if lbblfl="Y" and aval ne. then base=aval;
else base=.'
if ablfl ne "Y" and aval ne . then chg=aval-base;
ANrhi=put(lbstnrhi, best.);
anrlo=put(lbstnrlo, best.);
if ablfl="Y" then bnrind=lbnrind;
if ablfl ne 'Y' then anrind=lbnrind;
Aperiod=visitnum;
aperiodc=visit;
aday=visitdy;
lbendtc=lbdtc;
if lbdtc ne ' ' then lbdtc_=Cat (Lbdtc||"T"||"00:00:00");
ADTM=input(lbdtc_, anydtdtm.);
adt=input(lbdtc, yymmdd10.);
atm=input(substr(lbdtc_,12), time8.);
adtf=" ";
atmf="Y";

if lbendtc ne ' ' then lbendtc_=Cat (Lbendtc||"T"||"00:00:00");
AenDTM=input(lbendtc_, anydtdtm.);
aendt=input(lbendtc, yymmdd10.);
aentm=input(substr(lbendtc_,12), time8.);
aendtf=" ";
aentmf="Y";

format adt date9. atm time8. adtm datetime20.;
ady=lbdy;
if ady<=1 then avisit='Baseline';
else if ady>=2 and ady<=46 then avisit='Month 1';
else if ady>=47 and ady<=76 then avisit='Month 2';
else if ady>=77 and ady<=106 then avisit='Month 3';
else if ady>=107 and ady<=136 then avisit='Month 4';
else if ady >=137 and ady<=166 then avisit='Month 5';
else if ady>=167 and ady<=211 then avisit='Month 6';

if Avisit='Baseline' then avisitn=0;
else if avisit ne 'Baseline' then avisitn=input(substr(avisit,7), Best.);
run; 

data lb2;
set lb1;
if paramcd="WBC" and (AVAL>=3.6 and AVAL<7) then lbsign="low";
if paramcd="WBC" and (AVAL>=7 and AVAL<10) then Lbsign="Normal";
else if paramcd="WBC" and (AVAL>=10) then lbsign="high";

/*OR*/
if paramcd="WBC" and AVAL>=3.6 and AVAL<10 then lbsign="LOW";
else paramcd="WBC" and AVAL>=10 then lbsign="HIGH";

if lbsign="high" then pcsfl="Y";
else pcsfl=" ";
run;










