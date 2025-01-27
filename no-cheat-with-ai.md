-- Step 1: Find the murderer and the mastermind in a single query
SELECT 
    killer.name AS killer_name,
    mastermind.name AS mastermind_name
FROM person killer
-- Join to find the killer's gym membership and car details
JOIN get_fit_now_member gym ON killer.id = gym.person_id
JOIN drivers_license car ON killer.license_id = car.id
-- Join to find the mastermind based on Jeremy Bowers' confession
JOIN (
    SELECT p.id, p.name
    FROM person p
    JOIN drivers_license dl ON p.license_id = dl.id
    JOIN (
        SELECT person_id
        FROM facebook_event_checkin
        WHERE event_name LIKE '%SQL Symphony Concert%'
          AND date BETWEEN 20171201 AND 20171231
        GROUP BY person_id
        HAVING COUNT(*) = 3
    ) concert ON p.id = concert.person_id
    WHERE dl.hair_color = 'red'
      AND dl.car_make = 'Tesla'
      AND dl.car_model = 'Model S'
      AND dl.gender = 'female'
) mastermind ON 1=1
-- Filter based on witness statements and Jeremy's confession
WHERE gym.id LIKE '48Z%' -- Morty's statement: gym bag with "48Z"
  AND gym.membership_status = 'gold' -- Morty's statement: gold member
  AND car.plate_number LIKE '%H42W%' -- Morty's statement: car plate with "H42W"
  AND killer.name = 'Jeremy Bowers'; -- Confirmed killer from earlier steps
