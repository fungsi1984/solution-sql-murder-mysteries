-- Step 1: Retrieve the crime scene report for the murder on Jan.15, 2018 in SQL City
SELECT *
FROM crime_scene_report
WHERE city = 'SQL City'
  AND date = 20180115
  AND type = 'murder';

-- Step 2: Find the first witness (Morty Schapiro) who lives at the last house on Northwestern Dr
SELECT *
FROM person
WHERE address_street_name = 'Northwestern Dr'
ORDER BY address_number DESC
LIMIT 1;

-- Step 3: Find the second witness (Annabel Miller) who lives on Franklin Ave
SELECT *
FROM person
WHERE address_street_name = 'Franklin Ave'
  AND name LIKE '%Annabel%';

-- Step 4: Retrieve Morty Schapiro's interview transcript
SELECT *
FROM interview
WHERE person_id = 14887;

-- Step 5: Retrieve Annabel Miller's interview transcript
SELECT *
FROM interview
WHERE person_id = 16371;

-- Step 6: Check gym check-ins on Jan.9, 2018 for memberships starting with "48Z"
SELECT *
FROM get_fit_now_check_in
WHERE check_in_date = 20180109
  AND membership_id LIKE '48Z%';

-- Step 7: Find car owners with license plates containing "H42W"
SELECT *
FROM drivers_license
WHERE plate_number LIKE '%H42W%';

-- Step 8: Cross-reference car owners with the gym members
SELECT *
FROM person
WHERE license_id IN (423327, 664760);

-- Step 9: Retrieve Jeremy Bowers' interview transcript
SELECT *
FROM interview
WHERE person_id = 67318;

-- Step 10: Find people who attended the SQL Symphony Concert 3 times in December 2017
SELECT person_id, COUNT(*)
FROM facebook_event_checkin
WHERE event_name LIKE '%SQL Symphony Concert%'
  AND date BETWEEN 20171201 AND 20171231
GROUP BY person_id
HAVING COUNT(*) = 3;

-- Step 11: Find women with red hair who drive a Tesla Model S
SELECT *
FROM drivers_license
WHERE hair_color = 'red'
  AND car_make = 'Tesla'
  AND car_model = 'Model S'
  AND gender = 'female';

-- Step 12: Cross-reference the women with concert attendees
SELECT *
FROM person
WHERE license_id IN (202298, 291182, 918773);

-- Step 13: Identify the mastermind (Miranda Priestly)
SELECT *
FROM person
WHERE name = 'Miranda Priestly';

-- Step 14: Retrieve Miranda Priestly's interview transcript (if available)
SELECT *
FROM interview
WHERE person_id = 99716;
