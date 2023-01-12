# Проект №6 “Базовый SQL”

### Описание проекта

В самостоятельном проекте этого курса вы будете работать с базой данных, которая хранит информацию о венчурных фондах и инвестициях в компании-стартапы. Эта база данных основана на датасете [Startup Investments](https://www.kaggle.com/justinas/startup-investments), опубликованном на популярной платформе для соревнований по исследованию данных Kaggle.

Анализировать рынок инвестиций без подготовки может быть непросто. Поэтому сначала познакомьтесь с важными понятиями, которые вам встретятся в работе с базой данных.

*Венчурные фонды* — это финансовые организации, которые могут позволить себе высокий риск и инвестировать в компании с инновационной бизнес-идеей или разработанной новой технологией, то есть в *стартапы*. Цель венчурных фондов — в будущем получить значительную прибыль, которая в разы превысит размер их трат на инвестиции в компанию. Если стартап подорожает, венчурный фонд может получить долю в компании или фиксированный процент от её выручки.

Чтобы процесс финансирования стал менее рискованным, его делят на стадии — *раунды*. Тот или иной раунд зависит от того, какого уровня развития достигла компания.

Первые этапы — предпосевной и посевной раунды. Предпосевной раунд предполагает, что компания как таковая ещё не создана и находится в стадии замысла. Следующий — посевной — раунд знаменует рост проекта: создатели компании разрабатывают бизнес-модель и привлекают инвесторов.

Если компании требуется ментор или наставник — она привлекает бизнес-ангела. Бизнес-ангелы — инвесторы, которые помимо финансовой поддержки предлагают экспертную помощь. Такой раунд называют *ангельским*.

Когда стартап становится компанией с проверенной бизнес-моделью и начинает зарабатывать самостоятельно, предложений инвесторов становится больше. Это раунд A, а за ним следуют и другие: B, C, D — на этих этапах компания активно развивается и готовится к IPO.

Иногда выделяют венчурный раунд — финансирование, которое могло поступить от венчурного фонда на любом этапе: начальном или более позднем.

В данных об инвестициях вам встретятся упоминания раундов, но самостоятельный проект не предполагает, что вы должны разбираться в их специфике лучше любого инвестора. Главное — понимать, как устроены данные.

Вы уже знаете, что такое ER-диаграмма. Работу с новой базой данных лучше начать с изучения схемы.

---

### Схема базы данных

![Untitled](%D0%9F%D1%80%D0%BE%D0%B5%D0%BA%D1%82%20%E2%84%966%20%E2%80%9C%D0%91%D0%B0%D0%B7%D0%BE%D0%B2%D1%8B%D0%B8%CC%86%20SQL%E2%80%9D%2057041afbce644f1a85c527dc4ebdb2d2/Untitled.png)

### Описание базы данных

`acquisition`

Содержит информацию о покупках одних компаний другими.

Таблица включает такие поля:

- первичный ключ `id` — идентификатор или уникальный номер покупки;
- внешний ключ `acquiring_company_id` — ссылается на таблицу `company` — идентификатор компании-покупателя, то есть той, что покупает другую компанию;
- внешний ключ `acquired_company_id` — ссылается на таблицу `company` — идентификатор компании, которую покупают;
- `term_code` — способ оплаты сделки:
    - `cash` — наличными;
    - `stock` — акциями компании;
    - `cash_and_stock` — смешанный тип оплаты: наличные и акции.
- `price_amount` — сумма покупки в долларах;
- `acquired_at` — дата совершения сделки;
- `created_at` — дата и время создания записи в таблице;
- `updated_at` — дата и время обновления записи в таблице.

---

`company`

Содержит информацию о компаниях-стартапах.

- первичный ключ `id` — идентификатор, или уникальный номер компании;
- `name` — название компании;
- `category_code` — категория деятельности компании, например:
    - `news` — специализируется на работе с новостями;
    - `social` — специализируется на социальной работе.
- `status` — статус компании:
    - `acquired` — приобретена;
    - `operating` — действует;
    - `ipo` — вышла на IPO;
    - `closed` — перестала существовать.
- `founded_at` — дата основания компании;
- `closed_at` — дата закрытия компании, которую указывают в том случае, если компании больше не существует;
- `domain` — домен сайта компании;
- `twitter_username` — название профиля компании в твиттере;
- `country_code` — код страны, например, `USA` для США, `GBR` для Великобритании;
- `investment_rounds` — число раундов, в которых компания участвовала как инвестор;
- `funding_rounds` — число раундов, в которых компания привлекала инвестиции;
- `funding_total` — сумма привлечённых инвестиций в долларах;
- `milestones` — количество важных этапов в истории компании;
- `created_at` — дата и время создания записи в таблице;
- `updated_at` — дата и время обновления записи в таблице.

---

`education`

Хранит информацию об уровне образования сотрудников компаний.

- первичный ключ `id` — уникальный номер записи с информацией об образовании;
- внешний ключ `person_id` — ссылается на таблицу `people` — идентификатор человека, информация о котором представлена в записи;
- `degree_type` — учебная степень, например:
    - `BA` — Bachelor of Arts — бакалавр гуманитарных наук;
    - `MS` — Master of Science — магистр естественных наук.
- `instituition` — учебное заведение, название университета;
- `graduated_at` — дата завершения обучения, выпуска;
- `created_at` — дата и время создания записи в таблице;
- `updated_at` — дата и время обновления записи в таблице.

---

`fund`

Хранит информацию о венчурных фондах.

- первичный ключ `id` — уникальный номер венчурного фонда;
- `name` — название венчурного фонда;
- `founded_at` — дата основания фонда;
- `domain` — домен сайта фонда;
- `twitter_username` — профиль фонда в твиттере;
- `country_code` — код страны фонда;
- `investment_rounds` — число инвестиционных раундов, в которых фонд принимал участие;
- `invested_companies` — число компаний, в которые инвестировал фонд;
- `milestones` — количество важных этапов в истории фонда;
- `created_at` — дата и время создания записи в таблице;
- `updated_at` — дата и время обновления записи в таблице.

---

`funding_round`

Содержит информацию о раундах инвестиций.

- первичный ключ `id` — уникальный номер инвестиционного раунда;
- внешний ключ `company_id` — ссылается на таблицу `company` — уникальный номер компании, участвовавшей в инвестиционном раунде;
- `funded_at` — дата проведения раунда;
- `funding_round_type` — тип инвестиционного раунда, например:
    - `venture` — венчурный раунд;
    - `angel` — ангельский раунд;
    - `series_a` — раунд А.
- `raised_amount` — сумма инвестиций, которую привлекла компания в этом раунде в долларах;
- `pre_money_valuation` — предварительная, проведённая до инвестиций оценка стоимости компании в долларах;
- `participants` — количество участников инвестиционного раунда;
- `is_first_round` — является ли этот раунд первым для компании;
- `is_last_round` — является ли этот раунд последним для компании;
- `created_at` — дата и время создания записи в таблице;
- `updated_at` — дата и время обновления записи в таблице.

---

`investment`

Содержит информацию об инвестициях венчурных фондов в компании-стартапы.

- первичный ключ `id` — уникальный номер инвестиции;
- внешний ключ `funding_round_id` — ссылается на таблицу `funding_round` — уникальный номер раунда инвестиции;
- внешний ключ `company_id` — ссылается на таблицу `company` — уникальный номер компании-стартапа, в которую инвестируют;
- внешний ключ `fund_id` — ссылается на таблицу `fund` — уникальный номер фонда, инвестирующего в компанию-стартап;
- `created_at` — дата и время создания записи в таблице;
- `updated_at` — дата и время обновления записи в таблице.

---

`people`

Содержит информацию о сотрудниках компаний-стартапов.

- первичный ключ `id` — уникальный номер сотрудника;
- `first_name` — имя сотрудника;
- `last_name` — фамилия сотрудника;
- внешний ключ `company_id` — ссылается на таблицу `company` — уникальный номер компании-стартапа;
- `twitter_username` — профиль сотрудника в твиттере;
- `created_at` — дата и время создания записи в таблице;
- `updated_at` — дата и время обновления записи в таблице.

---

Note: 

первая ячейка с кодом - это непосредственно код который был введен

вторая ячейка - результат выполнения кода

---

- **Оглавление:**

---

### 1. Посчитайте, сколько компаний закрылось.

```sql
SELECT COUNT(id)
FROM company
WHERE status = 'closed'
```

| count |
| --- |
| 2584 |

### 2. Отобразите количество привлечённых средств для новостных компаний США.

Используйте данные из таблицы `company`. Отсортируйте таблицу по убыванию значений в поле `funding_total` .

```sql
SELECT funding_total
FROM company
WHERE category_code = 'news'
    AND country_code = 'USA'
GROUP BY name, funding_total
ORDER BY funding_total DESC;
```

| funding_total |
| --- |
| 6.22553e+08 |
| 2.5e+08 |
| 1.605e+08 |
| 1.28e+08 |
| … |

### 3. Найдите общую сумму сделок по покупке одних компаний другими в долларах.

Отберите сделки, которые осуществлялись только за наличные с 2011 по 2013 год включительно.

```sql
SELECT SUM(price_amount)
FROM acquisition
WHERE EXTRACT(YEAR FROM CAST(acquired_at  AS DATE)) BETWEEN 2011 AND 2013
    AND term_code = 'cash'
```

| sum |
| --- |
| 1.37762e+11 |

### 4. Отобразите имя, фамилию и названия аккаунтов людей в твиттере, у которых названия аккаунтов начинаются на `'Silver'`.

```sql
SELECT first_name,
    last_name,
    twitter_username
FROM people
WHERE twitter_username LIKE 'Silver%'
```

| first_name | last_name |
| --- | --- |
| Rebecca | Silver |
| Silver | Teede |
| Mattias | Guilotte |

### 5. Выведите на экран всю информацию о людях, у которых названия аккаунтов в твиттере содержат подстроку `'money'`, а фамилия начинается на `'K'`.

```sql
SELECT *
FROM people
WHERE twitter_username LIKE '%money%'
    AND last_name LIKE 'K%'
```

| id | first_name | last_name | company_id | twitter_username | created_at | updated_at |
| --- | --- | --- | --- | --- | --- | --- |
| 63081 | Gregory | Kim |  | gmoney75 | 2010-07-13 03:46:28 | 2011-12-12 22:01:34 |

### 6. Для каждой страны отобразите общую сумму привлечённых инвестиций, которые получили компании, зарегистрированные в этой стране.

Страну, в которой зарегистрирована компания, можно определить по коду страны. Отсортируйте данные по убыванию суммы.

```sql
SELECT SUM(funding_total) AS sum_tot,
    country_code
FROM company
GROUP BY country_code
ORDER BY sum_tot DESC
```

| sum_tot | country_code |
| --- | --- |
| 3.10588e+11 | USA |
| 1.77056e+10 | GBR |
| 1.08559e+10 |  |
| 1.06897e+10 | CHN |
| … |  |

### 7. Составьте таблицу, в которую войдёт дата проведения раунда, а также минимальное и максимальное значения суммы инвестиций, привлечённых в эту дату.

Оставьте в итоговой таблице только те записи, в которых минимальное значение суммы инвестиций не равно нулю и не равно максимальному значению.

```sql
SELECT CAST (funded_at AS date),
       MIN(raised_amount),
       MAX(raised_amount)
FROM funding_round
GROUP BY CAST (funded_at AS date)
HAVING MIN(raised_amount) != 0
  AND MIN(raised_amount) != MAX(raised_amount);
```

| funded_at | min | max |
| --- | --- | --- |
| 2012-08-22 | 40000 | 7.5e+07 |
| 2010-07-25 | 3.27825e+06 | 9e+06 |
| 2002-03-01 | 2.84418e+06 | 8.95915e+06 |
| 2010-10-11 | 28000 | 2e+08 |

### 8. Создайте поле с категориями:

- Для фондов, которые инвестируют в 100 и более компаний, назначьте категорию `high_activity`.
- Для фондов, которые инвестируют в 20 и более компаний до 100, назначьте категорию `middle_activity`.
- Если количество инвестируемых компаний фонда не достигает 20, назначьте категорию `low_activity`.

Отобразите все поля таблицы `fund` и новое поле с категориями.

|  | name | founded_at | domain | twitter_username | country_code | investment_rounds | invested_companies | milestones | created_at | updated_at | category |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 13180 | University Incubator | 2013-01-01 | universityincubator.com | UniversIncubat | USA | 0 | 0 | 2 | 2013-08-21 22:03:36 | 2013-08-21 22:32:30 | low_activity |
| 6444 | Riverlake Partners | 2003-01-01 | riverlakepartners.com |  | USA | 2 | 2 | 0 | 2010-10-26 13:26:14 | 2013-05-09 18:58:51 | low_activity |
| 3498 | Östersjöstiftelsen | 1994-01-01 | ostersjostiftelsen.se |  |  |  |  |  |  |  |  |

### 9. Для каждой из категорий, назначенных в предыдущем задании, посчитайте округлённое до ближайшего целого числа среднее количество инвестиционных раундов, в которых фонд принимал участие.

Выведите на экран категории и среднее число инвестиционных раундов. 

Отсортируйте таблицу по возрастанию среднего.

```sql
SELECT i.activity,
ROUND(AVG(i.investment_rounds)) AS avg_rd

FROM(SELECT *,
       CASE
           WHEN invested_companies>=100 THEN 'high_activity'
           WHEN invested_companies>=20 THEN 'middle_activity'
           ELSE 'low_activity'
       END AS activity
FROM fund) AS i

GROUP BY i.activity
ORDER BY avg_rd ASC;
```

| activity | avg_rd |
| --- | --- |
| low_activity | 2 |
| middle_activity | 51 |
| high_activity | 252 |

### 10. Проанализируйте, в каких странах находятся фонды, которые чаще всего инвестируют в стартапы.

Для каждой страны посчитайте минимальное, максимальное и среднее число компаний, в которые инвестировали фонды этой страны, основанные с 2010 по 2012 год включительно. 

Исключите страны с фондами, у которых минимальное число компаний, получивших инвестиции, равно нулю. Выгрузите десять самых активных стран-инвесторов.

Отсортируйте таблицу по среднему количеству компаний от большего к меньшему, а затем по коду страны в лексикографическом порядке.

```sql
SELECT MIN(invested_companies) AS min_num_of_inv,
    MAX(invested_companies) AS max_num_of_inv,
    AVG(invested_companies) AS avg_num_of_inv,
    country_code
    
FROM fund

WHERE EXTRACT(YEAR FROM CAST(founded_at AS DATE)) BETWEEN 2010 AND 2012

GROUP BY country_code
HAVING  MIN(invested_companies) <> 0
ORDER BY avg_num_of_inv DESC, country_code
LIMIT 10;
```

| min_num_of_inv | max_num_of_inv | avg_num_of_inv | country_code |
| --- | --- | --- | --- |
| 25 | 35 | 30 | BGR |
| 29 | 29 | 29 | CHL |
| 8 | 10 | 9 | UKR |
| 5 | 5 | 5 | LTU |

### 11. Отобразите имя и фамилию всех сотрудников стартапов.

Добавьте поле с названием учебного заведения, которое окончил сотрудник, если эта информация известна.

```sql
SELECT first_name,
       last_name,
       instituition
FROM people AS p
LEFT JOIN education AS ed ON p.id = ed.person_id
```

| first_name | last_name | instituition |
| --- | --- | --- |
| John | Green | Washington University, St. Louis |
| John | Green | Boston University |
| David | Peters | Rice University |
| Dan | Birdwhistell | University of Cambridge |
| … |  |  |

### 12. Для каждой компании найдите количество учебных заведений, которые окончили её сотрудники.

Выведите название компании и число уникальных названий учебных заведений. 

Составьте топ-5 компаний по количеству университетов.

```sql
SELECT     comp.name,
    COUNT(DISTINCT(ed.instituition))
    
FROM company AS comp
    INNER JOIN people AS ppl ON ppl.company_id = comp.id
    INNER JOIN education AS ed ON ppl.id = ed.person_id
    
WHERE ed.instituition IS NOT NULL
    
GROUP BY comp.name

ORDER BY COUNT(DISTINCT(ed.instituition)) DESC
LIMIT 5;
```

| name | count |
| --- | --- |
| Google | 167 |
| Yahoo! | 115 |
| Microsoft | 111 |
| Knight Foundation | 74 |
| … |  |

### 13. Составьте список с уникальными названиями закрытых компаний, для которых первый раунд финансирования оказался последним.

```sql
SELECT DISTINCT name
    FROM company AS comp
    LEFT OUTER JOIN funding_round AS fd ON comp.id = fd.company_id
        WHERE 
            status = 'closed'
            AND is_first_round = 1
            AND is_last_round = 1
            
ORDER BY name ASC
```

| name |
| --- |
| 10BestThings |
| 11i Solutions |
| 169 ST. |
| … |

### 14. Составьте список уникальных номеров сотрудников, которые работают в компаниях, отобранных в предыдущем задании.

```sql
WITH
first_is_last AS (SELECT company_id AS id
                  FROM funding_round
                  WHERE is_first_round=1
                        AND is_last_round=1),
companies AS (SELECT c.id
              FROM company AS c
              WHERE c.status='closed')
SELECT DISTINCT p.id
      -- e.instituition
FROM people AS p
INNER JOIN companies AS c ON p.company_id=c.id
INNER JOIN first_is_last As fl ON p.company_id=fl.id
```

| id |
| --- |
| 62 |
| 97 |
| 98 |
| … |

### 15. Составьте таблицу, куда войдут уникальные пары с номерами сотрудников из предыдущей задачи и учебным заведением, которое окончил сотрудник.

```sql
WITH
first_is_last AS (SELECT company_id AS id
                  FROM funding_round
                  WHERE is_first_round=1
                        AND is_last_round=1),
companies AS (SELECT c.id
              FROM company AS c
              WHERE c.status='closed')
SELECT DISTINCT
        p.id,
       e.instituition
FROM people AS p
INNER JOIN companies AS c ON p.company_id=c.id
INNER JOIN first_is_last As fl ON p.company_id=fl.id
INNER JOIN education AS e ON p.id=e.person_id
```

| id | instituition |
| --- | --- |
| 349 | AKI |
| 349 | ArtEZ Hogeschool voor de Kunsten |
| 349 | Rijks Akademie |
| 699 | Imperial College |
| … |  |

### 16. Посчитайте количество учебных заведений для каждого сотрудника из предыдущего задания.

При подсчёте учитывайте, что некоторые сотрудники могли окончить одно и то же заведение дважды.

```sql
WITH
first_is_last AS (SELECT DISTINCT company_id AS id
                  FROM funding_round
                  WHERE is_first_round=1
                        AND is_last_round=1),
companies AS (SELECT DISTINCT c.id
              FROM company AS c
              WHERE c.status='closed')
SELECT p.id,
      --e.instituition
       COUNT(e.instituition)
       
FROM people AS p
INNER JOIN companies AS c ON p.company_id=c.id
INNER JOIN first_is_last As fl ON p.company_id=fl.id
INNER JOIN education AS e ON p.id=e.person_id
GROUP BY  p.id
ORDER BY p.id ASC
```

| id | count |
| --- | --- |
| 349 | 3 |
| 699 | 1 |
| 779 | 2 |
| 968 | 1 |
| … |  |

### 17. Дополните предыдущий запрос и выведите среднее число учебных заведений (всех, не только уникальных), которые окончили сотрудники разных компаний.

Нужно вывести только одну запись, группировка здесь не понадобится.

```sql
SELECT SUM(new_tab.cnt)/COUNT(DISTINCT new_tab.id)

FROM(WITH
first_is_last AS (SELECT DISTINCT company_id AS id
                  FROM funding_round
                  WHERE is_first_round=1
                        AND is_last_round=1),
companies AS (SELECT DISTINCT c.id
              FROM company AS c
              WHERE c.status='closed')
SELECT p.id,
      --e.instituition
       --COUNT(e.instituition)/COUNT(p.id)
       COUNT(e.instituition) AS cnt
       
FROM people AS p
INNER JOIN companies AS c ON p.company_id=c.id
INNER JOIN first_is_last As fl ON p.company_id=fl.id
INNER JOIN education AS e ON p.id=e.person_id
GROUP BY p.id) AS new_tab
```

| ?column? |
| --- |
| 1.41509 |

### 18. Напишите похожий запрос: выведите среднее число учебных заведений (всех, не только уникальных), которые окончили сотрудники Facebook.

```sql
SELECT SUM(new_tab.cnt)/COUNT(DISTINCT new_tab.id)

FROM(WITH
first_is_last AS (SELECT DISTINCT company_id AS id
                  FROM funding_round
                  ),
companies AS (SELECT DISTINCT c.id
              FROM company AS c
              WHERE c.name='Facebook')
SELECT p.id,
      --e.instituition
       --COUNT(e.instituition)/COUNT(p.id)
       COUNT(e.instituition) AS cnt
       
FROM people AS p
INNER JOIN companies AS c ON p.company_id=c.id
INNER JOIN first_is_last As fl ON p.company_id=fl.id
INNER JOIN education AS e ON p.id=e.person_id
GROUP BY p.id) AS new_tab
```

| ?column? |
| --- |
| 1.51111 |

### 19. Составьте таблицу из полей:

- `name_of_fund` — название фонда;
- `name_of_company` — название компании;
- `amount` — сумма инвестиций, которую привлекла компания в раунде.

В таблицу войдут данные о компаниях, в истории которых было больше шести важных этапов, а раунды финансирования проходили с 2012 по 2013 год включительно.

```sql
SELECT 
    --comp.funding_rounds,
    comp.name AS name_of_company ,
    f.name AS name_of_fund,
    f_r.raised_amount AS amount

        
FROM investment AS inv

INNER JOIN company AS comp ON inv.company_id = comp.id
INNER JOIN fund AS f ON inv.fund_id = f.id
INNER JOIN funding_round AS f_r ON inv.funding_round_id = f_r.id

WHERE EXTRACT(YEAR FROM CAST(f_r.funded_at  AS DATE)) BETWEEN 2012 AND 2013
AND comp.milestones  > 6
GROUP BY comp.name, f.name, f_r.raised_amount
ORDER BY amount DESC
```

| name_of_company | name_of_fund | amount |
| --- | --- | --- |
| OpenX | Accel Partners | 2.50112e+07 |
| OpenX | Index Ventures | 2.50112e+07 |
| OpenX | Mitsui Global Investment | 2.50112e+07 |
| … |  |  |

### 20. Выгрузите таблицу, в которой будут такие поля:

- название компании-покупателя;
- сумма сделки;
- название компании, которую купили;
- сумма инвестиций, вложенных в купленную компанию;
- доля, которая отображает, во сколько раз сумма покупки превысила сумму вложенных в компанию инвестиций, округлённая до ближайшего целого числа.

Не учитывайте те сделки, в которых сумма покупки равна нулю. Если сумма инвестиций в компанию равна нулю, исключите такую компанию из таблицы.

Отсортируйте таблицу по сумме сделки от большей к меньшей, а затем по названию купленной компании в лексикографическом порядке. 

Ограничьте таблицу первыми десятью записями.

```sql
SELECT tabl_1.name,
        price,
        comp_2.name,
        comp_2.funding_total AS funding_tot,
        ROUND(price/funding_total) AS round

FROM(SELECT acquiring_company_id,
         comp_1.name,
         price_amount AS price,
         acquired_company_id
    FROM acquisition AS ac 
    JOIN company AS comp_1 ON ac.acquiring_company_id = comp_1.id
    ) AS tabl_1
        
JOIN company AS comp_2 ON tabl_1.acquired_company_id = comp_2.id
WHERE price > 0
  AND funding_total > 0
  
ORDER BY price DESC, comp_2.name
LIMIT 10
```

| name | price | name | funding_tot | round |
| --- | --- | --- | --- | --- |
| Microsoft | 8.5e+09 | Skype | 7.6805e+07 | 111 |
| Scout Labs | 4.9e+09 | Varian Semiconductor Equipment Associates | 4.8e+06 | 1021 |
| Broadcom | 3.7e+09 | Aeluros | 7.97e+06 | 464 |
| … |  |  |  |  |

### 21. Выгрузите таблицу, в которую войдут названия компаний из категории `social`, получившие финансирование с 2010 по 2013 год включительно.

Проверьте, что сумма инвестиций не равна нулю. 

Выведите также номер месяца, в котором проходил раунд финансирования.

```sql
SELECT name,
    EXTRACT(MONTH FROM CAST(funded_at AS DATE))
FROM company AS comp
JOIN funding_round AS f_r ON comp.id = f_r.company_id

WHERE category_code LIKE 'social'
    AND EXTRACT(YEAR FROM CAST(funded_at AS DATE)) BETWEEN 2010 AND 2013 
    AND raised_amount > 0
```

| name | date_part |
| --- | --- |
| Klout | 1 |
| WorkSimple | 3 |
| HengZhi | 1 |
| … |  |

### 22. Отберите данные по месяцам с 2010 по 2013 год, когда проходили инвестиционные раунды.

Сгруппируйте данные по номеру месяца и получите таблицу, в которой будут поля:

- номер месяца, в котором проходили раунды;
- количество уникальных названий фондов из США, которые инвестировали в этом месяце;
- количество компаний, купленных за этот месяц;
- общая сумма сделок по покупкам в этом месяце.

```sql
WITH a AS
    (SELECT EXTRACT(MONTH FROM CAST(fr.funded_at AS timestamp)) AS month,
        COUNT(DISTINCT f.name) AS funds_total
    FROM funding_round AS fr
    INNER JOIN investment AS i ON fr.id=i.funding_round_id
    INNER JOIN fund AS f ON i.fund_id=f.id
        WHERE EXTRACT(YEAR FROM CAST(fr.funded_at AS timestamp)) BETWEEN 2010 AND 2013
            AND f.country_code = 'USA'
    GROUP BY month),
        
b AS 
    (SELECT EXTRACT(MONTH FROM CAST(acquired_at AS timestamp)) AS month,
        COUNT(acquired_company_id) AS companies_total,
    SUM(price_amount) AS total
    FROM acquisition
        WHERE EXTRACT(YEAR FROM CAST(acquired_at AS timestamp)) BETWEEN 2010 AND 2013
    GROUP BY month)
        
SELECT a.month,
    a.funds_total,
    b.companies_total,
    b.total
    
FROM a JOIN b ON a.month=b.month;
```

| month | funds_total | companies_total | total |
| --- | --- | --- | --- |
| 1 | 815 | 600 | 2.71083e+10 |
| 2 | 637 | 418 | 4.13903e+10 |
| 3 | 695 | 458 | 5.95016e+10 |
| 4 | 718 | 411 | 3.03837e+10 |
| … |  |  |  |

### 23. Составьте сводную таблицу и выведите среднюю сумму инвестиций для стран, в которых есть стартапы, зарегистрированные в 2011, 2012 и 2013 годах.

Данные за каждый год должны быть в отдельном поле. 

Отсортируйте таблицу по среднему значению инвестиций за 2011 год от большего к меньшему.

```sql
WITH 
a AS (SELECT country_code,
    AVG(funding_total) AS total_funding_2011
FROM company
WHERE EXTRACT(YEAR FROM CAST(founded_at AS timestamp)) = 2011
GROUP BY country_code),

b AS (SELECT country_code,
    AVG(funding_total) AS total_funding_2012
FROM company
WHERE EXTRACT(YEAR FROM CAST(founded_at AS timestamp)) = 2012
GROUP BY country_code),

c AS (SELECT country_code,
    AVG(funding_total) AS total_funding_2013
FROM company
WHERE EXTRACT(YEAR FROM CAST(founded_at AS timestamp)) = 2013
GROUP BY country_code)

SELECT a.country_code,
    a.total_funding_2011 AS year_2011,
    b.total_funding_2012 AS year_2012,
    c.total_funding_2013 AS year_2013
    
FROM a 
JOIN b ON a.country_code=b.country_code
JOIN c ON b.country_code=c.country_code
ORDER BY year_2011 DESC;
```

| country_code | year_2011 | year_2012 | year_2013 |
| --- | --- | --- | --- |
| PER | 4e+06 | 41000 | 25000 |
| USA | 2.24396e+06 | 1.20671e+06 | 1.09336e+06 |
| HKG | 2.18078e+06 | 226227 | 0 |
| PHL | 1.75e+06 | 4218.75 | 2500 |
| … |  |  |  |
