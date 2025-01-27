-- problem
-- A crime has taken place and the detective needs your help. 
-- The detective gave you the crime scene report, but you somehow lost it. 
-- You vaguely remember that the crime was a ​murder​ that occurred sometime on ​Jan.15, 2018​ and that it took place in ​SQL City​. 
-- Start by retrieving the corresponding crime scene report from the police department’s database.


--SELECT DISTINCT type FROM crime_scene_report

--SELECT *
--FROM crime_scene_report
--where city = 'SQL City'
--and date = 20180115
--and type = 'murder'

-- Security footage shows that there were 2 witnesses. 
-- The first witness lives at the last house on "Northwestern Dr". 
-- The second witness, named Annabel, lives somewhere on "Franklin Ave".

--SELECT *
--FROM person
--WHERE address_street_name = 'Northwestern Dr'
--order by address_number desc

-- The first witness lives at the last house on "Northwestern Dr". 
-- 14887	Morty Schapiro	118009	4919	Northwestern Dr	111564949

-- The second witness, named Annabel, lives somewhere on "Franklin Ave".

--SELECT *
--FROM person
--WHERE address_street_name = 'Franklin Ave'
--and name like '%Annabel%'

-- The second witness, named Annabel, lives somewhere on "Franklin Ave".
-- 16371	Annabel Miller	490173	103	Franklin Ave	318771143


--select * 
--from interview 
--where person_id = '14887'

-- 14887	Morty Schapiro	118009	4919	Northwestern Dr	111564949
-- I heard a gunshot and then saw a man run out. 
-- He had a "Get Fit Now Gym" bag. 
-- The membership number on the bag started with "48Z". 
-- Only gold members have those bags. The man got into a car with a plate that included "H42W".

--select * 
--from interview 
--where person_id = '16371'

-- 16371	Annabel Miller	490173	103	Franklin Ave	318771143
-- I saw the murder happen, 
-- and I recognized the killer from my gym when I was working out last week on January the 9th.

--SELECT *
--from get_fit_now_check_in 
--where check_in_date = '20180109'

-- The membership number on the bag started with "48Z". 

--SELECT *
--from get_fit_now_check_in 
--where check_in_date = '20180109'
--and membership_id like '%48z%'

-- 48Z7A Joe Germuska
-- 48Z55 Jeremy Bowers

-- Only gold members have those bags. The man got into a car with a plate that included "H42W".

--SELECT *
--from drivers_license 
--where plate_number like '%h42w%'

-- 423327	30	70	brown	brown	male	0H42W2	Chevrolet	Spark LS
-- 664760	21	71	black	black	male	4H42WR	Nissan	Altima

--select *
--from person 
--WHERE license_id = 423327

-- 67318	Jeremy Bowers	423327	530	Washington Pl, Apt 3A	871539279

--select *
--from person 
--WHERE license_id = 664760

-- 51739	Tushar Chandra	664760	312	Phi St	137882671

--select *
--from person 
--where name = 'Jeremy Bowers'


-- -- 67318	Jeremy Bowers	423327	530	Washington Pl, Apt 3A	871539279
--select *
--from interview 
--where person_id = '67318'

-- 67318	"I was hired by a woman with a lot of money. 
-- I don't know her name but I know she's around 5'5" (65") or 5'7" (67"). 
-- She has red hair and she drives a Tesla Model S. 
-- I know that she attended the SQL Symphony Concert 3 times in December 2017.

--
-- I know that she attended the SQL Symphony Concert 3 times in December 2017.

--select * 
--from facebook_event_checkin 
--where event_name like '%sql%'

--SELECT *
--FROM facebook_event_checkin
--WHERE event_name LIKE '%SQL Symphony Concert%'
--  AND date BETWEEN 20171201 AND 20171231
--GROUP BY person_id
--HAVING COUNT(*) = 3;

-- machine Ln
--24556	1143	SQL Symphony Concert	20171207

-- golden ave
--99716	1143	SQL Symphony Concert	20171206

-- She has red hair and she drives a Tesla Model S. 
--select * 
--from drivers_license 
--WHERE hair_color = 'red'
--and car_make like '%tesla%' 
--and gender = 'female'

--202298	68	66	green	red	female	500123	Tesla	Model S
--291182	65	66	blue	red	female	08CM64	Tesla	Model S
--918773	48	65	black	red	female	917UU3	Tesla	Model S

--select * 
--from person  
--where license_id = 202298

-- 202298	68	66	green	red	female	500123	Tesla	Model S
-- address_street_name = Golden Ave
-- 99716	Miranda Priestly	202298	1883	Golden Ave	987756388

-- 291182	65	66	blue	red	female	08CM64	Tesla	Model S
-- 90700	Regina George	291182	332	Maple Ave	337169072

-- 918773	48	65	black	red	female	917UU3	Tesla	Model S
-- 78881	Red Korb	918773	107	Camerata Dr	961388910

--select * 
--from facebook_event_checkin 
--WHERE event_name LIKE '%SQL Symphony Concert%'
--AND date BETWEEN 20171201 AND 20171231
--GROUP BY person_id
--HAVING COUNT(*) = 3;

--24556	1143	SQL Symphony Concert	20171207
--24556	Bryan Pardo	101191	703	Machine Ln	816663882 


--99716	1143	SQL Symphony Concert	20171206
--99716	Miranda Priestly	202298	1883	Golden Ave	987756388 = 310000

--select * 
--from interview 
--where person_id = 99716

