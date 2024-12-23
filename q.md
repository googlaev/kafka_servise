```
WITH time_intervals AS (
    -- Генерация временных интервалов
    SELECT generate_series(
        (SELECT MIN(recorded_at) FROM yamal), -- Минимальная временная метка
        (SELECT MAX(recorded_at) FROM yamal), -- Максимальная временная метка
        '3 minutes'::interval                -- Интервал 3 минуты
    ) AS recorded_at
)
SELECT
    ti.recorded_at,
    COALESCE(SUM(y.total_count), 0) AS total_count_sum
FROM
    time_intervals ti
LEFT JOIN
    yamal y 
    ON y.recorded_at >= ti.recorded_at
    AND y.recorded_at < ti.recorded_at + interval '3 minutes'
GROUP BY
    ti.recorded_at
ORDER BY
    ti.recorded_at;

```


```
SELECT 
    w.id AS well_id,
    COALESCE(SUM(d.total_count), 0) AS total_sum
FROM 
    wtable w
LEFT JOIN 
    dtable d ON w.id = d.well_id 
    AND d.recorded_at = '2024-12-20 19:12:05.337803+05'::timestamp with time zone
GROUP BY 
    w.id
ORDER BY 
    w.id;
```

```
WITH time_intervals AS (
    SELECT
        generate_series(
            (SELECT MIN(recorded_at) FROM yamal),
            (SELECT MAX(recorded_at) FROM yamal),
            '3 minutes'::interval
        ) AS recorded_at
),
aggregated_data AS (
    SELECT
        recorded_at,
        SUM(total_count) AS total_count_sum
    FROM
        yamal
    GROUP BY
        recorded_at
),
final_data AS (
    SELECT
        ti.recorded_at,
        COALESCE(ad.total_count_sum, 0) AS total_count_sum
    FROM
        time_intervals ti
    LEFT JOIN
        aggregated_data ad ON ti.recorded_at = ad.recorded_at
)

SELECT
    recorded_at,
    CASE
        WHEN total_count_sum = 0 AND LAG(total_count_sum) OVER (ORDER BY recorded_at) = 0 
             AND LAG(total_count_sum, 2) OVER (ORDER BY recorded_at) = 0 THEN 0
        ELSE total_count_sum
    END AS total_count_sum
FROM
    final_data
ORDER BY
    recorded_at;
```

```
"recorded_at","total_count_sum"
"2024-12-11 00:00:18.794281+05","18717"
"2024-12-11 00:03:18.794281+05","0"
"2024-12-11 00:06:18.794281+05","0"
"2024-12-11 00:09:18.794281+05","0"
"2024-12-11 00:12:18.794281+05","0"
"2024-12-11 00:15:18.794281+05","0"
"2024-12-11 00:18:18.794281+05","0"
"2024-12-11 00:21:18.794281+05","0"
"2024-12-11 00:24:18.794281+05","0"
"2024-12-11 00:27:18.794281+05","0"
"2024-12-11 00:30:18.794281+05","0"
"2024-12-11 00:33:18.794281+05","0"
"2024-12-11 00:36:18.794281+05","0"
"2024-12-11 00:39:18.794281+05","0"
"2024-12-11 00:42:18.794281+05","0"
"2024-12-11 00:45:18.794281+05","0"
"2024-12-11 00:48:18.794281+05","0"
"2024-12-11 00:51:18.794281+05","0"
"2024-12-11 00:54:18.794281+05","0"
"2024-12-11 00:57:18.794281+05","0"
"2024-12-11 01:00:18.794281+05","0"
"2024-12-11 01:03:18.794281+05","0"
"2024-12-11 01:06:18.794281+05","0"
"2024-12-11 01:09:18.794281+05","0"
"2024-12-11 01:12:18.794281+05","0"
"2024-12-11 01:15:18.794281+05","0"
"2024-12-11 01:18:18.794281+05","0"
"2024-12-11 01:21:18.794281+05","0"
"2024-12-11 01:24:18.794281+05","0"
"2024-12-11 01:27:18.794281+05","0"
"2024-12-11 01:30:18.794281+05","0"
"2024-12-11 01:33:18.794281+05","0"
"2024-12-11 01:36:18.794281+05","0"
"2024-12-11 01:39:18.794281+05","0"
"2024-12-11 01:42:18.794281+05","0"
"2024-12-11 01:45:18.794281+05","0"
"2024-12-11 01:48:18.794281+05","0"
"2024-12-11 01:51:18.794281+05","0"
"2024-12-11 01:54:18.794281+05","0"
"2024-12-11 01:57:18.794281+05","0"
"2024-12-11 02:00:18.794281+05","0"
"2024-12-11 02:03:18.794281+05","0"
"2024-12-11 02:06:18.794281+05","0"
"2024-12-11 02:09:18.794281+05","0"
"2024-12-11 02:12:18.794281+05","0"
```

```
WITH time_intervals AS (
    SELECT
        generate_series(
            (SELECT MIN(recorded_at) FROM yamal),
            (SELECT MAX(recorded_at) FROM yamal),
            '3 minutes'::interval  -- Укажите нужный интервал, например, 3 минуты
        ) AS recorded_at
),
aggregated_data AS (
    SELECT
        recorded_at,
        SUM(total_count) AS total_count_sum
    FROM
        yamal
    GROUP BY
        recorded_at
),
final_data AS (
    SELECT
        ti.recorded_at,
        COALESCE(ad.total_count_sum, 0) AS total_count_sum
    FROM
        time_intervals ti
    LEFT JOIN
        aggregated_data ad ON ti.recorded_at = ad.recorded_at
)

SELECT
    recorded_at,
    CASE
        WHEN total_count_sum = 0 AND LAG(total_count_sum) OVER (ORDER BY recorded_at) = 0 THEN 0
        ELSE total_count_sum
    END AS total_count_sum
FROM
    final_data
ORDER BY
    recorded_at;
```


```
wells_and_clusters
"well_id","well_name","cluster_name","field_name"
"10000134","1Н","0","Оренбургское"
"10000234","2Н","0","Оренбургское"
"10000334","3Н","0","Оренбургское"
"10000734","7Н","0","Оренбургское"
"10000934","9Н","0","Оренбургское"
"10001034","10Н","0","Оренбургское"
"10001134","11Н","0","Оренбургское"
"10001234","12Н","0","Оренбургское"
"10001634","16Н","0","Оренбургское"
"10001834","18Н","0","Оренбургское"
"10002034","20Н","0","Оренбургское"
"10002134","21Н","0","Оренбургское"
"10002234","22Н","0","Оренбургское"
"10002334","23Н","0","Оренбургское"
"10002434","24Н","0","Оренбургское"
"10002534","25Н","0","Оренбургское"
"10002634","26Н","0","Оренбургское"
"10003234","32Н","0","Оренбургское"
"10003934","39Н","0","Оренбургское"
"10005634","56Н","0","Оренбургское"
"10009900","99","0","Оренбургское"
"10030000","300","0","Оренбургское"
"10030100","301","0","Оренбургское"
"10033600","336","0","Оренбургское"
"10039603","396G","2","Оренбургское"
"10039703","397G","2","Оренбургское"
"10039903","399G","2","Оренбургское"
"10040003","400G","1","Оренбургское"
"10040103","401G","2","Оренбургское"
"10040303","403G","1","Оренбургское"
"10040403","404G","1","Оренбургское"
"10040503","405G","1","Оренбургское"
"10043800","438G","2","Оренбургское"
"10044003","440G","2","Оренбургское"
"10044203","442G","2","Оренбургское"
"10044303","443G","2","Оренбургское"
"10050134","501Н","0","Оренбургское"
"10052634","526Н","0","Оренбургское"
```

```
EXPLAIN ANALYZE
WITH time_intervals AS (
    SELECT generate_series(
        NOW() - INTERVAL '1 hour',
        NOW(),
        INTERVAL '3 minutes'
    ) AS interval_start
)
SELECT
    ti.interval_start AS recorded_at,
    COALESCE(SUM(y.total_count), 0) AS total_sum
FROM
    time_intervals ti
LEFT JOIN yamal y ON y.recorded_at >= ti.interval_start
                  AND y.recorded_at < ti.interval_start + INTERVAL '3 minutes'
GROUP BY
    ti.interval_start
ORDER BY
    ti.interval_start;
```
```
"well_id","total_count","recorded_at"
"500240800",51,"2024-12-20 19:12:05.337803+05"
"500240900",9,"2024-12-20 19:12:05.337803+05"
"500241000",4,"2024-12-20 19:12:05.337803+05"
"500241400",33,"2024-12-20 19:12:05.337803+05"
"500276400",5,"2024-12-20 19:12:05.337803+05"
"500276500",1,"2024-12-20 19:12:05.337803+05"
"500277400",11,"2024-12-20 19:12:05.337803+05"
"500277600",24,"2024-12-20 19:12:05.337803+05"
"500277900",22,"2024-12-20 19:12:05.337803+05"
"500278000",4,"2024-12-20 19:12:05.337803+05"
"500278100",10,"2024-12-20 19:12:05.337803+05"
"500278200",13,"2024-12-20 19:12:05.337803+05"
"500279700",1,"2024-12-20 19:12:05.337803+05"
"500280300",9,"2024-12-20 19:12:05.337803+05"
"500400100",1,"2024-12-20 19:12:05.337803+05"
"500400300",3,"2024-12-20 19:12:05.337803+05"
"500402400",2,"2024-12-20 19:12:05.337803+05"
"500402795",2,"2024-12-20 19:12:05.337803+05"
"500404500",6,"2024-12-20 19:12:05.337803+05"
"500404700",2,"2024-12-20 19:12:05.337803+05"
"500405500",12,"2024-12-20 19:12:05.337803+05"
"500405600",1,"2024-12-20 19:12:05.337803+05"
"500405700",3,"2024-12-20 19:12:05.337803+05"
"500405900",2,"2024-12-20 19:12:05.337803+05"
"500406400",6,"2024-12-20 19:12:05.337803+05"
"500406500",1,"2024-12-20 19:12:05.337803+05"
"500406595",4,"2024-12-20 19:12:05.337803+05"
"500406700",1,"2024-12-20 19:12:05.337803+05"
"500406795",3,"2024-12-20 19:12:05.337803+05"
"500406800",1,"2024-12-20 19:12:05.337803+05"
"500407400",1,"2024-12-20 19:12:05.337803+05"
"500407500",2,"2024-12-20 19:12:05.337803+05"
"500407595",1,"2024-12-20 19:12:05.337803+05"
"500407695",1,"2024-12-20 19:12:05.337803+05"
"500407800",1,"2024-12-20 19:12:05.337803+05"
"500408500",13,"2024-12-20 19:12:05.337803+05"
"500408600",3,"2024-12-20 19:12:05.337803+05"
"500408700",2,"2024-12-20 19:12:05.337803+05"
"500408795",2,"2024-12-20 19:12:05.337803+05"
"500408800",1,"2024-12-20 19:12:05.337803+05"
"500409400",14,"2024-12-20 19:12:05.337803+05"
"500409595",2,"2024-12-20 19:12:05.337803+05"
"500409600",1,"2024-12-20 19:12:05.337803+05"
"500409695",1,"2024-12-20 19:12:05.337803+05"
"500409800",1,"2024-12-20 19:12:05.337803+05"
"500409895",1,"2024-12-20 19:12:05.337803+05"
"500410500",2,"2024-12-20 19:12:05.337803+05"
"500410600",1,"2024-12-20 19:12:05.337803+05"
"500410700",2,"2024-12-20 19:12:05.337803+05"
"500410795",3,"2024-12-20 19:12:05.337803+05"
"500410800",1,"2024-12-20 19:12:05.337803+05"
"500411695",22,"2024-12-20 19:12:05.337803+05"
"500411800",1,"2024-12-20 19:12:05.337803+05"
```

Исправленный запрос:
Убедимся, что данные агрегируются корректно и сопоставляются с временными интервалами:
```
sql
Копировать код
WITH time_intervals AS (
    SELECT generate_series(
        (SELECT MIN(recorded_at) FROM yamal),  -- Начало диапазона
        NOW(),                                 -- Конец диапазона
        INTERVAL '3 minutes'                   -- Шаг времени
    ) AS interval_start
),
merged_data AS (
    SELECT
        ti.interval_start AS recorded_at,
        COALESCE(SUM(y.total_count), 0) AS total_sum  -- Сумма total_count или 0
    FROM
        time_intervals ti
    LEFT JOIN
        yamal y
    ON
        y.recorded_at >= ti.interval_start 
        AND y.recorded_at < ti.interval_start + INTERVAL '3 minutes'
    GROUP BY
        ti.interval_start
    ORDER BY
        ti.interval_start
)
SELECT
    recorded_at AS time,   -- Grafana ожидает колонку с названием `time`
    total_sum AS value     -- Значение суммы за интервал
FROM
    merged_data;
```
Шаги для отладки:
Проверьте данные в yamal: Убедитесь, что временные метки находятся в диапазоне:

sql
Копировать код
```
SELECT recorded_at, total_count
FROM yamal
WHERE recorded_at >= NOW() - INTERVAL '1 hour'
ORDER BY recorded_at;
```
Проверьте сгенерированные интервалы: Убедитесь, что generate_series создает правильные временные интервалы:

sql
Копировать код
```
SELECT generate_series(
    (SELECT MIN(recorded_at) FROM yamal),
    NOW(),
    INTERVAL '3 minutes'
) AS interval_start;
```
Проверьте сопоставление данных: Проверьте, соответствуют ли временные метки данным в yamal:

sql
Копировать код
```
SELECT ti.interval_start, COUNT(y.well_id) AS match_count
FROM (
    SELECT generate_series(
        (SELECT MIN(recorded_at) FROM yamal),
        NOW(),
        INTERVAL '3 minutes'
    ) AS interval_start
) ti
LEFT JOIN yamal y
ON y.recorded_at >= ti.interval_start 
   AND y.recorded_at < ti.interval_start + INTERVAL '3 minutes'
GROUP BY ti.interval_start
ORDER BY ti.interval_start;
```
Настройте точность временных данных: Если временные метки содержат секунды или миллисекунды, округлите их в запросе:

sql
Копировать код
```
SELECT
    DATE_TRUNC('minute', recorded_at) AS truncated_time,
    SUM(total_count) AS total_sum
FROM yamal
WHERE recorded_at >= NOW() - INTERVAL '1 hour'
GROUP BY truncated_time
ORDER BY truncated_time;
```
Дополнительные рекомендации:
Настройте Grafana:

Проверьте, какой временной интервал установлен в Grafana (например, 3 минуты).
Убедитесь, что метрики возвращают данные в колонке time для Grafana.
Проверка данных в merged_data: Выполните промежуточный SELECT на merged_data:

sql
Копировать код
``
SELECT * FROM merged_data;
```
Если это не поможет, дайте подробности: какие данные в таблице yamal, какой интервал вы используете в Grafana, и какой шаг времени установлен для отображения.



chatgpt
```
WITH time_intervals AS (
    SELECT generate_series(
        (SELECT MIN(recorded_at) FROM yamal),  -- Начало диапазона
        NOW(),                                 -- Конец диапазона
        INTERVAL '3 minutes'                  -- Шаг времени
    ) AS recorded_at
),
merged_data AS (
    SELECT
        ti.recorded_at,
        COALESCE(SUM(y.total_count), 0) AS total_sum  -- Если данных нет, используем 0
    FROM
        time_intervals ti
    LEFT JOIN
        yamal y
    ON
        ti.recorded_at = y.recorded_at
    GROUP BY
        ti.recorded_at
    ORDER BY
        ti.recorded_at
)
SELECT
    recorded_at AS time,   -- Grafana ожидает колонку с названием `time`
    total_sum AS value     -- Значение суммы за интервал
FROM
    merged_data;

```
kersor
```
WITH time_intervals AS (
    SELECT
        generate_series(
            (SELECT MIN(recorded_at) FROM yamal),
            (SELECT MAX(recorded_at) FROM yamal),
            '3 minutes'::interval  -- Укажите нужный интервал, например, 3 минуты
        ) AS recorded_at
),
aggregated_data AS (
    SELECT
        recorded_at,
        SUM(total_count) AS total_count_sum
    FROM
        yamal
    GROUP BY
        recorded_at
),
final_data AS (
    SELECT
        ti.recorded_at,
        COALESCE(ad.total_count_sum, 0) AS total_count_sum
    FROM
        time_intervals ti
    LEFT JOIN
        aggregated_data ad ON ti.recorded_at = ad.recorded_at
)

SELECT
    recorded_at,
    CASE
        WHEN total_count_sum = 0 AND LAG(total_count_sum) OVER (ORDER BY recorded_at) = 0 THEN 0
        ELSE total_count_sum
    END AS total_count_sum
FROM
    final_data
ORDER BY
    recorded_at;
```
