1.  Название вида с самым маленьким id. Соответствует "м"
SELECT species.species_name
FROM species
ORDER BY species.species_id ASC
LIMIT 1;
Результат: малыш (буква «м» — совпадает)

2.  Имя вида с количеством представителей более 1800. Соответствует "б"
SELECT species.species_name
FROM species
WHERE species.species_amount > 1800;
Результат: пусто (буква «б» не получена, в данных нет таких значений)

3. Имя вида на "п", относится к типу 5, начинается на "о"
SELECT species.species_name
FROM species
WHERE species.species_name LIKE 'п%'
  AND species.type_id = 5;
Результат: пусто (буква «о» не получена, нет вида на «п» с type_id = 5)

4. Имя вида заканчивается на "са", или количество представителей равно 5. Соответствует "в"
SELECT species.species_name
FROM species
WHERE species.species_name LIKE '%са'
   OR species.species_amount = 5;
Результат: лиса (не совпадает с ожидаемой буквой «в»)

5. Имя вида, появившегося в 2023 году. Соответствует "ы"
SELECT species.species_name
FROM species
WHERE to_char(species.date_start, 'YYYY') = '2023';
Результат: обезьяна (не совпадает с ожидаемой буквой «ы»)

6. Вид, расположенный в месте с id, равным 3. Соответствует "с"
SELECT species.species_name
FROM species
JOIN species_in_places ON species.species_id = species_in_places.species_id
WHERE species.species_status = 'absent'
  AND species_in_places.place_id = 3;
Результат: пусто (буква "с" не получена, нет данных)

7. Имя вида, расположенного в доме, появившегося с мае, количество представителей. Соответствует "п" 
SELECT species.species_name, species.species_amount
FROM species
JOIN species_in_places ON species.species_id = species_in_places.species_id
JOIN places ON species_in_places.place_id = places.place_id
WHERE places.place_name = 'дом'
  AND to_char(species.date_start, 'MM') = '05';
Результат: собака 30 (не совпадает с ожидаемой буквой «п»)

8. Имя вида состоит из двух слов, содержит пробел. Соответствует "!"
SELECT species.species_name
FROM species
WHERE species.species_name LIKE '% %';
Результат: пусто (знак «!» не получен (нет данных)

9. Имя вида, появившегося с малышом в один день. Соответствует "ч"
SELECT species_name
FROM species
WHERE date_start = (
    SELECT date_start
    FROM species
    WHERE species_name = 'малыш'
	)
AND species_name != 'малыш';
Результат: кошка (не совпадает с ожидаемой буквой "ч")

10. Вид, который обитает в месте с наибольшей площадья, но только среди мест являющихся "домом" или "сараем". Соответствует "ж"
SELECT species.species_name
FROM species
JOIN species_in_places ON species.species_id = species_in_places.species_id
JOIN places ON species_in_places.place_id = places.place_id
WHERE places.place_name IN ('дом', 'сарай')
ORDER BY places.place_size DESC
LIMIT 1;
Результат: лошадь (не совпадает с ожидаемой буквой «ж»)

11. Вид, который живёт в доме и занимает пятое место по численности. Соответствует "ш"
SELECT species.species_name
FROM species
JOIN species_in_places ON species.species_id = species_in_places.species_id
JOIN places ON species_in_places.place_id = places.place_id
WHERE places.place_name = 'дом'
ORDER BY species.species_amount DESC
OFFSET 4 LIMIT 1;
Результат: попугай (не совпадает с ожидаемой буквой "ш")

12. Сказочный вид, который не живёт ни в одном месте. Соответствует "т"
SELECT species.species_name
FROM species
LEFT JOIN species_in_places ON species.species_id = species_in_places.species_id
WHERE species.species_status = 'fairy'
  AND species_in_places.species_id IS NULL;
Результат: пусто, буква «т» не получена (нет данных)

Расшифрованное послание - ТЫ ВСЁ МОЖЕШЬ!
