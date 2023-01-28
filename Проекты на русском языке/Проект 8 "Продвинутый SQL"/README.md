# Проект №8 “Продвинутый SQL”

### Описание данных

Проект состоит из двух частей:

- В первой части вы решите несколько задач в SQL-тренажёре, чтобы закрепить пройденный материал.
- Вторая часть проекта — аналитическая. Проверять задачи по-прежнему будет тренажёр. Однако мы рекомендуем самостоятельно анализировать полученные результаты и формулировать выводы.

Внимательно читайте условия. Формулировки задач в проекте приближены к реальной жизни. В работе часто встретятся такие формулировки: «рассчитать LTV за последние 30 дней», «отобразить самых активных покупателей» или даже «определить, почему в компании упали продажи».

В самостоятельном проекте вы будете работать с базой данных [StackOverflow](https://www.stackoverflow.com/) — сервиса вопросов и ответов о программировании. StackOverflow похож на социальную сеть — пользователи сервиса задают вопросы, отвечают на посты, оставляют комментарии и ставят оценки другим ответам.

Вы будете работать с версией базы, где хранятся данные о постах за 2008 год, но в таблицах вы найдёте информацию и о более поздних оценках, которые эти посты получили.

### Описание таблиц

### Таблица `stackoverflow.badges`

Хранит информацию о значках, которые присуждаются за разные достижения. Например, пользователь, правильно ответивший на большое количество вопросов про PostgreSQL, может получить значок postgresql.

| Поле | Описание |
| --- | --- |
| id | Идентификатор значка, первичный ключ таблицы |
| name | Название значка |
| user_id | Идентификатор пользователя, которому присвоили значок, внешний ключ, отсылающий к таблице users |
| creation_date | Дата присвоения значка |

### Таблица `stackoverflow.post_types`

Содержит информацию о типе постов. Их может быть два:

- `Question` — пост с вопросом;
- `Answer` — пост с ответом.

| Поле | Описание |
| --- | --- |
| id | Идентификатор поста, первичный ключ таблицы |
| type | Тип поста |

### Таблица `stackoverflow.posts`

Содержит информацию о постах.

| Поле | Описание |
| --- | --- |
| id | Идентификатор поста, первичный ключ таблицы |
| title | Заголовок поста |
| creation_date | Дата создания поста |
| favorites_count | Число, которое показывает, сколько раз пост добавили в «Закладки» |
| last_activity_date | Дата последнего действия в посте, например комментария |
| last_edit_date | Дата последнего изменения поста |
| user_id | Идентификатор пользователя, который создал пост, внешний ключ к таблице users |
| parent_id | Если пост написали в ответ на другую публикацию, в это поле попадёт идентификатор поста с вопросом |
| post_type_id | Идентификатор типа поста, внешний ключ к таблице post_types |
| score | Количество очков, которое набрал пост |
| views_count | Количество просмотров |

### Таблица `stackoverflow.users`

Содержит информацию о пользователях.

| id | Идентификатор пользователя, первичный ключ таблицы |
| --- | --- |
| creation_date | Дата регистрации пользователя |
| display_name | Имя пользователя |
| last_access_date | Дата последнего входа |
| location | Местоположение |
| reputation | Очки репутации, которые получают за хорошие вопросы и полезные ответы |
| views | Число просмотров профиля пользователя |

### Таблица `stackoverflow.vote_types`

Содержит информацию о типах голосов. Голос — это метка, которую пользователи ставят посту. Типов бывает несколько:

- `UpMod` — такую отметку получают посты с вопросами или ответами, которые пользователи посчитали уместными и полезными.
- `DownMod` — такую отметку получают посты, которые показались пользователям наименее полезными.
- `Close` — такую метку ставят опытные пользователи сервиса, если заданный вопрос нужно доработать или он вообще не подходит для платформы.
- `Offensive` — такую метку могут поставить, если пользователь ответил на вопрос в грубой и оскорбительной манере, например, указав на неопытность автора поста.
- `Spam` — такую метку ставят в случае, если пост пользователя выглядит откровенной рекламой.

| id | Идентификатор типа голоса, первичный ключ |
| --- | --- |
| name | Название метки |

### Таблица `stackoverflow.votes`

Содержит информацию о голосах за посты.

| id | Идентификатор голоса, первичный ключ |
| --- | --- |
| post_id | Идентификатор поста, внешний ключ к таблице posts |
| user_id | Идентификатор пользователя, который поставил посту голос, внешний ключ к таблице users |
| bounty_amount | Сумма вознаграждения, которое назначают, чтобы привлечь внимание к посту |
| vote_type_id | Идентификатор типа голоса, внешний ключ к таблице vote_types |
| creation_date | Дата назначения голоса |

### 1. Найдите количество вопросов, которые набрали больше 300 очков или как минимум 100 раз были добавлены в «Закладки».

```sql
SELECT COUNT(DISTINCT p.id)
FROM stackoverflow.posts as p
JOIN stackoverflow.post_types AS pt ON p.post_type_id = pt.id
WHERE (p.score > 300 
    OR p.favorites_count >= 100) AND pt.type = 'Question'
```

| count |
| --- |
| 1355 |

### 2. Сколько в среднем в день задавали вопросов с 1 по 18 ноября 2008 включительно? Результат округлите до целого числа.

```sql
WITH new_tab AS 
(SELECT
    DATE_TRUNC('day', p.creation_date)::date AS cret_date,
    COUNT(p.id) AS count_id
FROM stackoverflow.posts AS p
JOIN stackoverflow.post_types AS pt ON pt.id = p.post_type_id
WHERE 
    pt.type = 'Question' 
    AND p.creation_date::date BETWEEN '01-11-2008' AND '18-11-2008'
GROUP BY cret_date)

SELECT ROUND(AVG(count_id))
FROM new_tab
```

| round |
| --- |
| 383 |

### 3. Сколько пользователей получили значки сразу в день регистрации? Выведите количество уникальных пользователей.

```sql
SELECT COUNT(DISTINCT u.id)
FROM stackoverflow.users AS u
JOIN stackoverflow.badges AS b ON u.id = b.user_id
WHERE u.creation_date::date = b.creation_date::date
```

| count |
| --- |
| 7047 |

### 4. Сколько уникальных постов пользователя с именем Joel Coehoorn получили хотя бы один голос?

```sql
WITH new_tab AS 
(SELECT *
FROM stackoverflow.users AS u
    JOIN stackoverflow.posts AS p ON u.id = p.user_id
    JOIN stackoverflow.votes AS v ON v.post_id = p.id)
    
SELECT COUNT(DISTINCT post_id)
FROM new_tab
WHERE display_name = 'Joel Coehoorn'
```

| count |
| --- |
| 12 |

### 5. Выгрузите все поля таблицы `vote_types` .

Добавьте к таблице поле `rank`, в которое войдут номера записей в обратном порядке. Таблица должна быть отсортирована по полю `id`.

```sql
SELECT *,
    ROW_NUMBER() OVER (ORDER BY id DESC)
FROM stackoverflow.vote_types
ORDER BY id
```

| id | name | row_number |
| --- | --- | --- |
| 1 | AcceptedByOriginator | 15 |
| 2 | UpMod | 14 |
| 3 | DownMod | 13 |
| 4 | Offensive | 12 |
| … |  |  |

### 6. Отберите 10 пользователей, которые поставили больше всего голосов типа `Close`.

Отобразите таблицу из двух полей: идентификатором пользователя и количеством голосов. Отсортируйте данные сначала по убыванию количества голосов, потом по убыванию значения идентификатора пользователя.

```sql
WITH new_tab AS 
(SELECT DISTINCT u.id,
    COUNT(v.id) AS vote_number
FROM stackoverflow.users AS u
JOIN stackoverflow.votes AS v ON u.id = v.user_id
JOIN stackoverflow.vote_types AS vt ON v.vote_type_id = vt.id
WHERE vt.name = 'Close'
GROUP BY u.id
ORDER BY vote_number DESC, id DESC),

new_tab_2 AS
(SELECT *, 
    ROW_NUMBER() OVER (ORDER BY vote_number DESC) AS row_numb
FROM new_tab)

SELECT id,
    vote_number
FROM new_tab_2
WHERE row_numb <= 10
```

| id | vote_number |
| --- | --- |
| 20646 | 36 |
| 14728 | 36 |
| 27163 | 29 |
| 41158 | 24 |
| … |  |

### 7. Отберите 10 пользователей по количеству значков, полученных в период с 15 ноября по 15 декабря 2008 года включительно.

Отобразите несколько полей:

- идентификатор пользователя;
- число значков;
- место в рейтинге — чем больше значков, тем выше рейтинг.

Пользователям, которые набрали одинаковое количество значков, присвойте одно и то же место в рейтинге.

Отсортируйте записи по количеству значков по убыванию, а затем по возрастанию значения идентификатора пользователя.

```sql
WITH tab_1 AS
(SELECT u.id,
    COUNT(b.id) AS cnt
FROM stackoverflow.users AS u
JOIN stackoverflow.badges AS b ON b.user_id = u.id
WHERE b.creation_date::date BETWEEN '15-11-2008' AND '15-12-2008'
GROUP BY u.id
ORDER BY cnt DESC),

tabl_2 AS 
(SELECT *,
    DENSE_RANK() OVER (ORDER BY cnt DESC) AS raiting,
    ROW_NUMBER() OVER (ORDER BY cnt DESC) AS hlp
FROM tab_1)

SELECT id, 
    cnt, 
    raiting
FROM tabl_2
WHERE hlp <= 10
```

| id | cnt | raiting |
| --- | --- | --- |
| 22656 | 149 | 1 |
| 34509 | 45 | 2 |
| 1288 | 40 | 3 |
| 5190 | 31 | 4 |

### 8. Сколько в среднем очков получает пост каждого пользователя?

Сформируйте таблицу из следующих полей:

- заголовок поста;
- идентификатор пользователя;
- число очков поста;
- среднее число очков пользователя за пост, округлённое до целого числа.

Не учитывайте посты без заголовка, а также те, что набрали ноль очков.

```sql
SELECT u.id,
     p.title,
     p.score,
     ROUND(AVG(p.score) OVER(PARTITION BY u.id))
FROM stackoverflow.users AS u
JOIN stackoverflow.posts AS p ON u.id = p.user_id
WHERE p.score != 0 AND p.title IS NOT NULL
```

| id | title | score | round |
| --- | --- | --- | --- |
| 1 | Diagnosing Deadlocks in SQL Server 2005 | 82 | 573 |
| 1 | How do I calculate someone's age in C#? | 1743 | 573 |
| 1 | Why doesn't IE7 copy <pre><code> blocks to the clipboard correctly? | 37 | 573 |
| … |  |  |  |

### 9. Отобразите заголовки постов, которые были написаны пользователями, получившими более 1000 значков.

Посты без заголовков не должны попасть в список.

```sql
WITH tabl AS
(SELECT p.title AS tit,
    COUNT(b.id) AS bad_num
FROM stackoverflow.users AS u
JOIN stackoverflow.badges AS b ON b.user_id = u.id
JOIN stackoverflow.posts AS p ON p.user_id = u.id
WHERE p.title IS NOT NULL
GROUP BY tit
ORDER BY bad_num DESC)

SELECT tit
FROM tabl
WHERE bad_num > 1000
```

| titl |
| --- |
| What's the hardest or most misunderstood aspect of LINQ? |
| What are the correct version numbers for C#? |
| … |

### 10. Напишите запрос, который выгрузит данные о пользователях из США (англ. United States).

Разделите пользователей на три группы в зависимости от количества просмотров их профилей:

- пользователям с числом просмотров больше либо равным 350 присвойте группу `1`;
- пользователям с числом просмотров меньше 350, но больше либо равно 100 — группу `2`;
- пользователям с числом просмотров меньше 100 — группу `3`.

Отобразите в итоговой таблице идентификатор пользователя, количество просмотров профиля и группу. Пользователи с нулевым количеством просмотров не должны войти в итоговую таблицу.

```sql
SELECT id,
    views,
    CASE
        WHEN views >= 350 THEN 1
        WHEN views < 350 AND  views >= 100 THEN 2
        WHEN views < 100 THEN 3
    END AS user_group
FROM stackoverflow.users AS u
	WHERE location LIKE '%United States%' AND views > 0
```

| id | views | user_group |
| --- | --- | --- |
| 3 | 24396 | 1 |
| 13 | 35414 | 1 |
| 23 | 757 | 1 |
| 25 | 3837 | 1 |
| … |  |  |

### 11. Дополните предыдущий запрос. Отобразите лидеров каждой группы — пользователей, которые набрали максимальное число просмотров в своей группе.

Выведите поля с идентификатором пользователя, группой и количеством просмотров. Отсортируйте таблицу по убыванию просмотров, а затем по возрастанию значения идентификатора.

```sql
WITH tab_1 AS
(SELECT id,
    views,
    CASE
        WHEN views >= 350 THEN 1
        WHEN views < 350 AND  views >= 100 THEN 2
        WHEN views < 100 THEN 3
    END AS user_group
FROM stackoverflow.users AS u
WHERE location LIKE '%United States%' AND views > 0
ORDER BY views DESC),

tab_2 AS
(SELECT *,
    MAX(views) OVER (PARTITION BY user_group) AS maxim
FROM tab_1)

SELECT id,
    user_group,
    views
FROM tab_2
WHERE maxim=views
```

| id | user_group | views |
| --- | --- | --- |
| 16587 | 1 | 62813 |
| 9094 | 2 | 349 |
| 9585 | 2 | 349 |
| … |  |  |

### 12. Посчитайте ежедневный прирост новых пользователей в ноябре 2008 года. Сформируйте таблицу с полями:

- номер дня;
- число пользователей, зарегистрированных в этот день;
- сумму пользователей с накоплением.

```sql
WITH tab AS
(SELECT ROW_NUMBER() OVER(ORDER BY creation_date::date) ,
    COUNT(id)
FROM stackoverflow.users
WHERE DATE_TRUNC('month', creation_date)::date = '2008-11-01'
    AND DATE_TRUNC('year', creation_date)::date = '2008-01-01'
GROUP BY creation_date::date)

SELECT *,
    SUM(count) OVER(ORDER BY row_number)
    
FROM tab
```

| row_number | count | sum |  |  |
| --- | --- | --- | --- | --- |
| 1 | 34 | 34 |  |  |
| 2 | 48 | 82 |  |  |
| … |  |  |  |  |

### 13. Для каждого пользователя, который написал хотя бы один пост, найдите интервал между регистрацией и временем создания первого поста.

Отобразите:

- идентификатор пользователя;
- разницу во времени между регистрацией и первым постом.

```sql
WITH tab AS 
(SELECT DISTINCT u.id,
    FIRST_VALUE(u.creation_date) OVER(PARTITION BY u.id ORDER BY u.creation_date) AS reg_day,
    FIRST_VALUE(p.creation_date) OVER (PARTITION BY p.user_id ORDER BY p.creation_date) AS cre_day
FROM stackoverflow.users AS u
JOIN stackoverflow.posts AS p ON u.id = p.user_id)

SELECT id,
    cre_day-reg_day AS dif
FROM tab
```

| id | dif |
| --- | --- |
| 1 | 9:18:29 |
| 2 | 14:37:03 |
| 3 | 3 days, 16:17:09 |
| 4 | 15 days, 5:44:22 |
| … |  |

### 14. Выведите общую сумму просмотров постов за каждый месяц 2008 года.

Если данных за какой-либо месяц в базе нет, такой месяц можно пропустить. Результат отсортируйте по убыванию общего количества просмотров.

```sql
SELECT DATE_TRUNC('month', creation_date)::DATE AS dt,
    SUM(views_count)
FROM stackoverflow.posts
WHERE DATE_TRUNC('year', creation_date) = '01-01-2008'
GROUP BY dt
ORDER BY sum DESC
```

| dt | sum |
| --- | --- |
| 2008-09-01 | 452928568 |
| 2008-10-01 | 365400138 |
| 2008-11-01 | 221759651 |
| … |  |

### 15. Выведите имена самых активных пользователей, которые в первый месяц после регистрации (включая день регистрации) дали больше 100 ответов.

Вопросы, которые задавали пользователи, не учитывайте. Для каждого имени пользователя выведите количество уникальных значений `user_id`. Отсортируйте результат по полю с именами в лексикографическом порядке.

```sql
SELECT display_name as name_us,
       COUNT(distinct u.id)
FROM stackoverflow.users u 
JOIN stackoverflow.posts p ON u.id = p.user_id
JOIN stackoverflow.post_types pt ON pt.id= p.post_type_id
 
WHERE pt.type = 'Answer' 
    AND (DATE_TRUNC('day', p.creation_date) >= DATE_TRUNC('day', u.creation_date) 
         AND DATE_TRUNC('day', p.creation_date) <= DATE_TRUNC('day', u.creation_date) + interval '1 month')
 
GROUP BY  name_us
HAVING count(p.id)>100
ORDER BY 1
```

| name_us | count |
| --- | --- |
| 1800 INFORMATION | 1 |
| Adam Bellaire | 1 |
| Adam Davis | 1 |

### 16. Выведите количество постов за 2008 год по месяцам.

Отберите посты от пользователей, которые зарегистрировались в сентябре 2008 года и сделали хотя бы один пост в декабре того же года. Отсортируйте таблицу по значению месяца по убыванию.

```sql
WITH tab AS
(SELECT DISTINCT u.id AS u_id
FROM stackoverflow.users AS u
JOIN stackoverflow.posts AS p ON p.user_id = u.id
WHERE (u.creation_date::date BETWEEN '2008-09-01' AND '2008-09-30') 
	AND (p.creation_date::date BETWEEN '2008-12-1' AND '2008-12-31'))
    
SELECT DISTINCT DATE_TRUNC('month', p.creation_date)::date,
       COUNT (p.id) OVER (PARTITION BY DATE_TRUNC('month', p.creation_date)) as p_cnt
FROM stackoverflow.posts AS p
JOIN tab AS tab ON p.user_id = tab.u_id
ORDER BY date_trunc DESC
```

| date_trunc | p_cnt |
| --- | --- |
| 2008-12-01 | 17641 |
| 2008-11-01 | 18294 |
| … |  |

### 17. Используя данные о постах, выведите несколько полей:

- идентификатор пользователя, который написал пост;
- дата создания поста;
- количество просмотров у текущего поста;
- сумму просмотров постов автора с накоплением.

Данные в таблице должны быть отсортированы по возрастанию идентификаторов пользователей, а данные об одном и том же пользователе — по возрастанию даты создания поста.

```sql
SELECT DISTINCT u.id,
    p.creation_date,
    SUM(p.views_count) AS v_c,
    SUM(p.views_count) OVER (PARTITION BY u.id ORDER BY p.creation_date)
FROM stackoverflow.posts AS p
JOIN stackoverflow.users AS u ON p.user_id = u.id

GROUP BY u.id, p.creation_date, p.views_count
ORDER BY u.id, p.creation_date ASC
```

| id | creation_date | v_c | sum |
| --- | --- | --- | --- |
| 1 | 2008-07-31 23:41:00 | 480476 | 480476 |
| 1 | 2008-07-31 23:55:38 | 136033 | 616509 |
| … |  |  |  |

### 18. Сколько в среднем дней в период с 1 по 7 декабря 2008 года включительно пользователи взаимодействовали с платформой?

Для каждого пользователя отберите дни, в которые он или она опубликовали хотя бы один пост. Нужно получить одно целое число — не забудьте округлить результат.

```sql
WITH tab AS 
(SELECT user_id, 
    COUNT(DISTINCT creation_date) AS cnt,
    creation_date::DATE AS test
FROM stackoverflow.posts
WHERE creation_date::date BETWEEN '2008-12-01' AND '2008-12-07'
GROUP BY user_id, test)

SELECT ROUND(AVG(cnt),0)
FROM tab
```

| round |
| --- |
| 2 |

### 19. На сколько процентов менялось количество постов ежемесячно с 1 сентября по 31 декабря 2008 года?

Отобразите таблицу со следующими полями:

- номер месяца;
- количество постов за месяц;
- процент, который показывает, насколько изменилось количество постов в текущем месяце по сравнению с предыдущим.

Если постов стало меньше, значение процента должно быть отрицательным, если больше — положительным. Округлите значение процента до двух знаков после запятой.

Напомним, что при делении одного целого числа на другое в PostgreSQL в результате получится целое число, округлённое до ближайшего целого вниз. Чтобы этого избежать, переведите делимое в тип `numeric`.

```sql
SELECT EXTRACT(MONTH FROM creation_date) AS dt,
    COUNT(p.id) AS cnt,
    ROUND((COUNT(id)::numeric/LAG(COUNT(id)) OVER (ORDER BY EXTRACT(MONTH FROM creation_date)))*100 - 100, 2)
    
FROM stackoverflow.posts p
WHERE DATE_TRUNC('month', p.creation_date)::date BETWEEN '2008-09-01' and '2008-12-01'
GROUP BY dt
```

| dt | cnt | round |
| --- | --- | --- |
| 9 | 70371 |  |
| 10 | 63102 | -10.33 |
| … |  |  |

### 20. Выгрузите данные активности пользователя, который опубликовал больше всего постов за всё время.

Выведите данные за октябрь 2008 года в таком виде:

- номер недели;
- дата и время последнего поста, опубликованного на этой неделе.

```sql
WITH tabl AS
(SELECT
    p.user_id AS u_id,
    p.creation_date AS cr_dt,
    date_part('week', p.creation_date) AS wk_num
FROM stackoverflow.posts p
WHERE DATE_TRUNC('MONTH', p.creation_date) = '2008-10-01' AND
                 p.user_id in
                 (SELECT p.user_id AS u_id
                  FROM stackoverflow.posts p
                  GROUP BY p.user_id
                  ORDER BY COUNT(p.id) DESC
                  LIMIT 1)
ORDER BY cr_dt ASC),
tab_2 AS       
(SELECT wk_num,
    LAST_VALUE(cr_dt) OVER(PARTITION BY wk_num)
FROM tabl)

SELECT *
FROM tab_2
GROUP BY last_value, wk_num
ORDER BY wk_num DESC
```

| wk_num | last_value |
| --- | --- |
| 44 | 2008-10-31 22:16:01 |
| 43 | 2008-10-26 21:44:36 |
