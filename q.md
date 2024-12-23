```
EXPLAIN ANALYZE
WITH time_intervals AS (
    SELECT generate_series(
        NOW() - INTERVAL '1 hour',
        NOW(),
        INTERVAL '3 minutes'
    ) AS interval_start
),
aggregated_data AS (
    SELECT
        ti.interval_start AS recorded_at,
        COALESCE(SUM(y.total_count), 0) AS total_sum
    FROM
        time_intervals ti
    LEFT JOIN LATERAL (
        SELECT total_count
        FROM yamal
        WHERE yamal.recorded_at >= ti.interval_start
          AND yamal.recorded_at < ti.interval_start + INTERVAL '3 minutes'
    ) y ON true
    GROUP BY
        ti.interval_start
    ORDER BY
        ti.interval_start
)
SELECT
    recorded_at AS time,
    total_sum AS value
FROM
    aggregated_data;

```


```
WITH time_intervals AS (
    SELECT generate_series(
        (SELECT MIN(recorded_at) FROM yamal),  -- Начало временного диапазона
        NOW(),                                 -- Конец временного диапазона
        INTERVAL '3 minutes'                   -- Шаг интервала
    ) AS interval_start
),
aggregated_data AS (
    SELECT
        ti.interval_start AS recorded_at,
        COALESCE(SUM(y.total_count), 0) AS total_sum -- Сумма total_count (если нет данных, ставим 0)
    FROM
        time_intervals ti
    LEFT JOIN yamal y
    ON y.recorded_at >= ti.interval_start 
       AND y.recorded_at < ti.interval_start + INTERVAL '3 minutes'
    GROUP BY
        ti.interval_start
    ORDER BY
        ti.interval_start
)
SELECT
    recorded_at AS time,   -- Grafana ожидает колонку `time`
    total_sum AS value     -- Суммарное значение за интервал
FROM
    aggregated_data;

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
