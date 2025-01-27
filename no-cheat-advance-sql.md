-- Step 1: Retrieve the crime scene report for the murder on Jan.15, 2018 in SQL City
SELECT *
FROM crime_scene_report
WHERE city = 'SQL City'
  AND date = 20180115
  AND type = 'murder';

-- Step 2: Find the two witnesses
-- First witness: Morty Schapiro (last house on Northwestern Dr)
SELECT *
FROM person
WHERE address_street_name = 'Northwestern Dr'
ORDER BY address_number DESC
LIMIT 1;

-- Second witness: Annabel Miller (lives on Franklin Ave)
SELECT *
FROM person
WHERE address_street_name = 'Franklin Ave'
  AND name LIKE '%Annabel%';

-- Step 3: Retrieve witness interview transcripts
-- Morty Schapiro's interview
SELECT *
FROM interview
WHERE person_id = (
    SELECT id
    FROM person
    WHERE address_street_name = 'Northwestern Dr'
    ORDER BY address_number DESC
    LIMIT 1
);

-- Annabel Miller's interview
SELECT *
FROM interview
WHERE person_id = (
    SELECT id
    FROM person
    WHERE address_street_name = 'Franklin Ave'
      AND name LIKE '%Annabel%'
);

-- Step 4: Analyze Morty's statement to find gym members with "48Z" membership
SELECT *
FROM get_fit_now_member
WHERE id LIKE '48Z%'
  AND membership_status = 'gold';

-- Step 5: Analyze Annabel's statement to find gym check-ins on Jan.9, 2018
SELECT *
FROM get_fit_now_check_in
WHERE check_in_date = 20180109
  AND membership_id IN (
      SELECT id
      FROM get_fit_now_member
      WHERE id LIKE '48Z%'
        AND membership_status = 'gold'
  );

-- Step 6: Find car owners with license plates containing "H42W"
SELECT *
FROM drivers_license
WHERE plate_number LIKE '%H42W%';

-- Step 7: Cross-reference gym members with car owners
SELECT p.id, p.name, p.license_id
FROM person p
JOIN get_fit_now_member gm ON p.id = gm.person_id
JOIN drivers_license dl ON p.license_id = dl.id
WHERE gm.id LIKE '48Z%'
  AND dl.plate_number LIKE '%H42W%';

-- Step 8: Retrieve Jeremy Bowers' interview transcript
SELECT *
FROM interview
WHERE person_id = (
    SELECT p.id
    FROM person p
    JOIN get_fit_now_member gm ON p.id = gm.person_id
    JOIN drivers_license dl ON p.license_id = dl.id
    WHERE gm.id LIKE '48Z%'
      AND dl.plate_number LIKE '%H42W%'
      AND p.name = 'Jeremy Bowers'
);

-- Step 9: Find people who attended the SQL Symphony Concert 3 times in December 2017
SELECT person_id, COUNT(*) AS attendance_count
FROM facebook_event_checkin
WHERE event_name LIKE '%SQL Symphony Concert%'
  AND date BETWEEN 20171201 AND 20171231
GROUP BY person_id
HAVING COUNT(*) = 3;

-- Step 10: Find women with red hair who drive a Tesla Model S
SELECT *
FROM drivers_license
WHERE hair_color = 'red'
  AND car_make = 'Tesla'
  AND car_model = 'Model S'
  AND gender = 'female';

-- Step 11: Cross-reference concert attendees with Tesla drivers
SELECT p.id, p.name, p.license_id
FROM person p
JOIN drivers_license dl ON p.license_id = dl.id
JOIN (
    SELECT person_id
    FROM facebook_event_checkin
    WHERE event_name LIKE '%SQL Symphony Concert%'
      AND date BETWEEN 20171201 AND 20171231
    GROUP BY person_id
    HAVING COUNT(*) = 3
) ca ON p.id = ca.person_id
WHERE dl.hair_color = 'red'
  AND dl.car_make = 'Tesla'
  AND dl.car_model = 'Model S'
  AND dl.gender = 'female';

-- Step 12: Final output - Identify the mastermind
SELECT *
FROM person
WHERE id = (
    SELECT p.id
    FROM person p
    JOIN drivers_license dl ON p.license_id = dl.id
    JOIN (
        SELECT person_id
        FROM facebook_event_checkin
        WHERE event_name LIKE '%SQL Symphony Concert%'
          AND date BETWEEN 20171201 AND 20171231
        GROUP BY person_id
        HAVING COUNT(*) = 3
    ) ca ON p.id = ca.person_id
    WHERE dl.hair_color = 'red'
      AND dl.car_make = 'Tesla'
      AND dl.car_model = 'Model S'
      AND dl.gender = 'female'
);
