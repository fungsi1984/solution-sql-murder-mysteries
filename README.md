- check how much person we need to identified, we use person cause it is the central of this table

SELECT count(*)
FROM person;

- it output 10011

- check table value using distinct

SELECT DISTINCT type FROM crime_scene_report;

- output, we gonna use this 

robbery
murder
theft
fraud
arson
bribery
assault
smuggling
blackmail


### unordered notes
You vaguely remember that the crime was a ​murder​ that occurred sometime on ​Jan.15, 2018​ and that it took place in ​SQL City​. Start by retrieving the corresponding crime scene report from the police department’s database.


SELECT name 
  FROM sqlite_master
 where type = 'table'

SELECT sql 
  FROM sqlite_master
 where name = 'crime_scene_report'

select * from crime_scene_report

select *  from crime_scene_report
where date = '20180115'


select *  from crime_scene_report
where date = '20180115'
and type = 'murder'

select *  from crime_scene_report
where date = '20180115'
and type = 'murder'
and city = 'SQL City'


select * from person
where (address_street_name = 'Northwestern Dr'
or address_street_name = 'Franklin Ave')

select * from person
where (address_street_name = 'Northwestern Dr'
or address_street_name = 'Franklin Ave')
and name like '%Annabel%'

select MAX(address_number) from person 
where address_street_name = 'Northwestern Dr'

select *from person 
where address_street_name = 'Northwestern Dr'
and address_number = 4919

select *from person 
where address_street_name = 'Northwestern Dr'
order by address_number desc

select *from person 
where address_street_name = 'Northwestern Dr'
order by address_number desc
limit 1

select * from interview 
where person_id = 16371
or person_id = 14887

select * from get_fit_now_member
where id like '48Z%'

select * from drivers_license 
WHERE plate_number like '%h42w%'

select * from person 
where license_id in (423327, 664760 )

select * from get_fit_now_member
WHERE id like '%48z%'
and membership_status ='gold'
and person_id = 67318

-- I was hired by a woman with a lot of money. I don't know her name but I know she's around 5'5" (65") or 5'7" (67"). 
-- She has red hair and she drives a Tesla Model S. I know that she attended the SQL Symphony Concert 3 times in December 2017.


select * from drivers_license
where gender = 'female'
and car_model = 'Model S'
and hair_color = 'red'

SELECT * from person where license_id = 291182

--202298 Miranda Priestly	202298	1883	Golden Ave	987756388
--291182 90700	Regina George	291182	332	Maple Ave	337169072
--918773 78881	Red Korb	918773	107	Camerata Dr	961388910


cheat
SELECT p.name, gf.id, gf.membership_status, dl.plate_number
FROM person p
JOIN drivers_license dl ON p.license_id = dl.id
JOIN get_fit_now_member gf ON p.id = gf.person_id
WHERE gf.id LIKE '48Z%'
AND gf.membership_status = 'gold'
AND dl.plate_number LIKE '%H42W%';

SELECT p.name, dl.height, dl.hair_color, dl.car_make, dl.car_model
FROM person p
JOIN drivers_license dl ON p.license_id = dl.id
JOIN facebook_event_checkin fb ON p.id = fb.person_id
WHERE dl.gender = 'female'
AND dl.hair_color = 'red'
AND dl.height BETWEEN 65 AND 67
AND dl.car_make = 'Tesla'
AND dl.car_model = 'Model S'
AND fb.event_name = 'SQL Symphony Concert'
GROUP BY p.id
HAVING COUNT(*) >= 3;
