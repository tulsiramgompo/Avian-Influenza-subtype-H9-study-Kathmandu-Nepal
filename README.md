import delimited "/Users/Tulsigompo/Desktop/poultry_n_pigs/AIV_study_kathmandu/Data_AIV/case_control_data_for_multivariable_1.csv", encoding(ISO-8859-1)

** Descriptive study codes 

*To calulcate mean and medain flock size of study population
summarize flock_size, detail

*Testing normality of "age","flock size", "farm distance from the nearest commercial farms" and "farm distance from the main road"

tabulate age_days
qnorm age_days
qnorm flock_size
qnorm distance_from_main_road_meter
qnorm  near_by_farm_distance_meter


** Since age and flock size are not normal we use median age and medain flock as cut off for dividing the 

_pctile age_days, nquantiles(6)
return list
_pctile flock_size, nquantiles(6)
return list


** Testing correlations between the continuous variables age, flock size , nearby commercail farm distance and distance of farm from the main road (meter)

cor age_days flock_size   near_by_farm_distance_meter distance_from_main_road_meter

** generating the category for the age (days) and 
gen age_cat_1=.
replace  age_cat_1=1 if age_days<= 50
replace  age_cat_1=2 if age_days > 50
** generating the category for the flock size

gen flock_size_cat_1 =.
replace flock_size_cat_1=1 if flock_size<=2000
replace flock_size_cat_1=2 if flock_size >2000

** generating the category for distance from the main road
gen distance_from_main_road_km_cat_1=.
replace distance_from_main_road_km_cat_1=1 if near_by_farm_distance_meter <=500
replace distance_from_main_road_km_cat_1=2 if near_by_farm_distance_meter >500

** generating the category for distance from nearby commercail farm distance
gen nearby_farm_distance_cat_1=.
replace nearby_farm_distance_cat_1=1 if near_by_farm_distance_meter <=1000
replace nearby_farm_distance_cat_1=2 if near_by_farm_distance_meter >1000

** Univariable logsitic regression codes
 * Age of birds
xi:logistic outcome ib2.age_cat_1 
 
**  Mean Flock size
xi:logistic  outcome i.flock_size_cat_1 

**Types of birds kept on farms
xi:logistic outcome i.farm_type 

** Farm distance from the main road
xi:logistic outcome ib2.distance_from_main_road_km_cat_1  
 
** Distance from the nearest commercial farm
xi:logistic outcome ib2.nearby_farm_distance_cat_1 

** Culling of sick birds
xi:logistic outcome i.culling_during_sick 

 ** Flooring type of farm 
xi:logistic outcome i.flooring 

** Previous history of AI (H9) outbreak on farm
xi:logistic outcome ib2.previous_outbreak_of_ai 

** Apron used during farm operation
xi:logistic outcome i.apron 

** Boot applied during farm operation
xi:logistic outcome ib2.boot 

** Visitors allowed at farm
xi:logistic outcome ib2.visitors 

** testing correlations between the categorical variables (Sperman's Rank Correlation)

spearman age_cat_1  flock_size_cat_1  farm_type water_bath distance_from_main_road_km_cat_1 nearby_farm_distance_cat_1 culling_during_sick  flooring stats previous_outbreak_of_ai apron boot visitors, stats (rho obs p)

 ** Multivariable logistic regression and Model selections

* Model selection or model building for final model

* forward_selection
 stepwise, pe(.15): logistic outcome flock_size_cat_1  nearby_farm_distance_cat_1  previous_outbreak_of_ai boot  visitors

* backward selection
stepwise, pr(.15): logistic outcome flock_size_cat_1  nearby_farm_distance_cat_1  previous_outbreak_of_ai  boot  visitors

* Cheking the fitness of model by "Hosmer–Lemeshow goodness-of-fit test"
estat gof
