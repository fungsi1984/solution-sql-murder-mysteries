-- Step 1: Find witnesses and their statements
SELECT p.name, i.transcript
FROM person p
JOIN interview i ON p.id = i.person_id
WHERE (p.address_street_name = 'Northwestern Dr'
       AND p.address_number = (
           SELECT MAX(address_number)
           FROM person
           WHERE address_street_name = 'Northwestern Dr'
       ))
OR (p.name LIKE 'Annabel%'
    AND p.address_street_name = 'Franklin Ave');

-- Step 2: Find the suspect
SELECT p.name, gf.id AS membership_id, dl.plate_number
FROM person p
JOIN drivers_license dl ON p.license_id = dl.id
JOIN get_fit_now_member gf ON p.id = gf.person_id
WHERE gf.id LIKE '48Z%'
AND gf.membership_status = 'gold'
AND dl.plate_number LIKE '%H42W%';

-- Step 3: Confirm suspect's alibi (with correct date handling)
SELECT *
FROM get_fit_now_check_in
WHERE membership_id IN (
    SELECT gf.id
    FROM person p
    JOIN drivers_license dl ON p.license_id = dl.id
    JOIN get_fit_now_member gf ON p.id = gf.person_id
    WHERE gf.id LIKE '48Z%'
    AND gf.membership_status = 'gold'
    AND dl.plate_number LIKE '%H42W%'
)
AND check_in_date = 20180109;


-- Step 4: Find the mastermind (corrected)
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
AND substr(fb.date, 1, 4) || '-' || substr(fb.date, 5, 2) = '2017-12'
GROUP BY p.id
HAVING COUNT(fb.date) >= 3;

-- Step 5: Verify mastermind's income (corrected)
SELECT i.ssn, i.annual_income
FROM income i
JOIN person p ON i.ssn = p.ssn
JOIN drivers_license dl ON p.license_id = dl.id
JOIN facebook_event_checkin fb ON p.id = fb.person_id
WHERE dl.gender = 'female'
AND dl.hair_color = 'red'
AND dl.height BETWEEN 65 AND 67
AND dl.car_make = 'Tesla'
AND dl.car_model = 'Model S'
AND fb.event_name = 'SQL Symphony Concert'
AND substr(fb.date, 1, 4) || '-' || substr(fb.date, 5, 2) = '2017-12'
GROUP BY p.id
HAVING COUNT(fb.date) >= 3;
