# Avian-Influenza-subtype-H9-study-Kathmandu-Nepal
This is a case-control study to identify the risk factors that are associated with the outbreak of avian influenza A subtype H9 in poultry farms of Kathmandu valley, Nepal.

Codes in STATA for univariable and Multivariable Anlaysis

** Use of Apron
tab   apron aiv_status

xi: logistic outcome i.apron 
** change the outcome to indicator variable
**Boots

tab   boot aiv_status
xi: logistic outcome i.boot
* visitors
tab   visitors aiv_status
xi: logistic outcome i.visitors
**Self sanitization before entering farm

tab   self_senitization aiv_status
xi: logistic outcome i.self_senitization
** foot bath
tab   water_bath aiv_status
xi: logistic outcome i.water_bath
** fencing

tab   fencing aiv_status
xi: logistic outcome i.fencing

* Multivariable analysis (selected with P value equal to or less than 0.2)
xi: logistic outcome i.boot ib2.visitors i.foot_bath i.fencing  ** base chaged for the visitors yes=1 and no=2)

** Mdified code in july 30
 *visitors yes=1 and no=2
xi:logistic outcome i.boot i.visitors  i.water_bath 
