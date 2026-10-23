1.  Название вида с самым маленьким id. 
SELECT species.species_name
FROM species
ORDER BY species.species_id ASC
LIMIT 1;

2.  Имя вида с количеством представителей более 1800. 
SELECT species.species_name
FROM species
WHERE species.species_amount > 1800;

3. Имя вида относится к типу 5
SELECT species.species_name
FROM species
WHERE species.species_name LIKE 'п%'
  AND species.type_id = 5;

4. Имя вида заканчивается на "са", или количество представителей равно 5. 
SELECT species.species_name
FROM species
WHERE species.species_name LIKE '%са'
   OR species.species_amount = 5;

5. Имя вида, появившегося в 2023 году. 
FROM species
WHERE to_char(species.date_start, 'YYYY') = '2023';

6. Вид, расположенный в месте с id, равным 3. 
SELECT species.species_name
FROM species
JOIN species_in_places ON species.species_id = species_in_places.species_id
WHERE species.species_status = 'absent'
  AND species_in_places.place_id = 3;

7. Имя вида, расположенного в доме, появившегося с мае, количество представителей.  
SELECT species.species_name, species.species_amount
FROM species
JOIN species_in_places ON species.species_id = species_in_places.species_id
JOIN places ON species_in_places.place_id = places.place_id
WHERE places.place_name = 'дом'
  AND to_char(species.date_start, 'MM') = '05';

8. Имя вида состоит из двух слов, содержит пробел. 
SELECT species.species_name
FROM species
WHERE species.species_name LIKE '% %';

9. Имя вида, появившегося с малышом в один день. 
SELECT species_name
FROM species
WHERE date_start = (
    SELECT date_start
    FROM species
    WHERE species_name = 'малыш'
	)
AND species_name != 'малыш';

10. Вид, который обитает в месте с наибольшей площадья, но только среди мест являющихся "домом" или "сараем". 
SELECT species.species_name
FROM species
JOIN species_in_places ON species.species_id = species_in_places.species_id
JOIN places ON species_in_places.place_id = places.place_id
WHERE places.place_name IN ('дом', 'сарай')
ORDER BY places.place_size DESC
LIMIT 1;

11. Вид, который живёт в доме и занимает пятое место по численности. 
SELECT species.species_name
FROM species
JOIN species_in_places ON species.species_id = species_in_places.species_id
JOIN places ON species_in_places.place_id = places.place_id
WHERE places.place_name = 'дом'
ORDER BY species.species_amount DESC
OFFSET 4 LIMIT 1;

12. Сказочный вид, который не живёт ни в одном месте. 
SELECT species.species_name
FROM species
LEFT JOIN species_in_places ON species.species_id = species_in_places.species_id
WHERE species.species_status = 'fairy'
  AND species_in_places.species_id IS NULL;

Расшифрованное послание - ТЫ ВСЁ МОЖЕШЬ!
