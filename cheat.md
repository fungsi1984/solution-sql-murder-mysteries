Certainly! Let's simplify the solution using JOINs to make it more concise and easier to follow. The goal is to find the murderer and the person who hired them.

### Step 1: Find the Witnesses
We know there are two witnesses:
1. The first witness lives at the last house on "Northwestern Dr".
2. The second witness is named Annabel and lives on "Franklin Ave".

```sql
-- Find the first witness
SELECT * 
FROM person 
WHERE address_street_name = 'Northwestern Dr' 
ORDER BY address_number DESC 
LIMIT 1;

-- Find the second witness
SELECT * 
FROM person 
WHERE name LIKE 'Annabel%' 
AND address_street_name = 'Franklin Ave';
```

### Step 2: Get Witness Statements
We need to find the interviews of these witnesses.

```sql
-- Get Morty's statement
SELECT * 
FROM interview 
WHERE person_id = (
    SELECT id 
    FROM person 
    WHERE address_street_name = 'Northwestern Dr' 
    ORDER BY address_number DESC 
    LIMIT 1
);

-- Get Annabel's statement
SELECT * 
FROM interview 
WHERE person_id = (
    SELECT id 
    FROM person 
    WHERE name LIKE 'Annabel%' 
    AND address_street_name = 'Franklin Ave'
);
```

### Step 3: Identify the Suspect
From the witness statements, we know:
- The suspect has a "Get Fit Now Gym" bag with a membership number starting with "48Z".
- The suspect is a gold member.
- The suspect's car plate includes "H42W".

```sql
-- Find the suspect
SELECT p.name, gf.id, gf.membership_status, dl.plate_number
FROM person p
JOIN drivers_license dl ON p.license_id = dl.id
JOIN get_fit_now_member gf ON p.id = gf.person_id
WHERE gf.id LIKE '48Z%'
AND gf.membership_status = 'gold'
AND dl.plate_number LIKE '%H42W%';
```

### Step 4: Confirm the Suspect's Alibi
Check the suspect's gym check-in on the day of the murder.

```sql
-- Check gym check-in
SELECT * 
FROM get_fit_now_check_in 
WHERE membership_id = '48Z55' 
AND check_in_date = 20180109;
```

### Step 5: Find the Mastermind
From the suspect's interview, we know:
- The mastermind is a woman with red hair, around 5'5" to 5'7", drives a Tesla Model S, and attended the SQL Symphony Concert 3 times in December 2017.

```sql
-- Find the mastermind
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
```

### Step 6: Verify the Mastermind's Income
Check the income of the mastermind to confirm she has a lot of money.

```sql
-- Verify income
SELECT * 
FROM income 
WHERE ssn = (
    SELECT ssn 
    FROM person 
    WHERE name = 'Miranda Priestly'
);
```

### Final Answer
The murderer is **Jeremy Bowers**, and the mastermind who hired him is **Miranda Priestly**.

### Simplified Query with JOINs
Here’s a more streamlined version using JOINs:

```sql
-- Find the murderer
SELECT p.name, gf.id, gf.membership_status, dl.plate_number
FROM person p
JOIN drivers_license dl ON p.license_id = dl.id
JOIN get_fit_now_member gf ON p.id = gf.person_id
WHERE gf.id LIKE '48Z%'
AND gf.membership_status = 'gold'
AND dl.plate_number LIKE '%H42W%';

-- Find the mastermind
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
```

This approach simplifies the process by using JOINs to combine related tables, making the query more readable and efficient.
