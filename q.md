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
