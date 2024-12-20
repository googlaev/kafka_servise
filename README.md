```
WITH time_intervals AS (
    SELECT
        generate_series(
            (SELECT MIN(DATE_TRUNC('minute', recorded_at)) FROM hantos),
            (SELECT MAX(DATE_TRUNC('minute', recorded_at)) FROM hantos),
            '3 minutes'::interval  -- Укажите нужный интервал, например, 3 минуты
        ) AS recorded_at
),
aggregated_data AS (
    SELECT
        DATE_TRUNC('minute', recorded_at) AS recorded_at,
        SUM(total_count) AS total_count_sum
    FROM
        hantos
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
    total_count_sum,
    PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY total_count_sum) OVER (ORDER BY recorded_at ROWS BETWEEN 2 PRECEDING AND CURRENT ROW) AS median_total_count
FROM
    final_data
ORDER BY
    recorded_at;
```

# Работает
```
WITH time_intervals AS (
    SELECT
        generate_series(
            (SELECT MIN(DATE_TRUNC('minute', recorded_at)) FROM hantos),
            (SELECT MAX(DATE_TRUNC('minute', recorded_at)) FROM hantos),
            '3 minutes'::interval  -- Укажите нужный интервал, например, 3 минуты
        ) AS recorded_at
),
aggregated_data AS (
    SELECT
        DATE_TRUNC('minute', recorded_at) AS recorded_at,
        SUM(total_count) AS total_count_sum
    FROM
        hantos
    GROUP BY
        recorded_at
),
final_data AS (
    SELECT
        ti.recorded_at,
        COALESCE(ad.total_count_sum, 0) AS total_count_sum  -- Заполняем отсутствующие значения нулями
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
WITH time_intervals AS (
    SELECT
        generate_series(
            (SELECT MIN(DATE_TRUNC('minute', recorded_at)) FROM hantos),
            (SELECT MAX(DATE_TRUNC('minute', recorded_at)) FROM hantos),
            '3 minutes'::interval  -- Укажите нужный интервал, например, 3 минуты
        ) AS recorded_at
),
aggregated_data AS (
    SELECT
        DATE_TRUNC('minute', recorded_at) AS recorded_at,
        SUM(total_count) AS total_count_sum
    FROM
        hantos
    GROUP BY
        recorded_at
),
final_data AS (
    SELECT
        ti.recorded_at,
        COALESCE(ad.total_count_sum, NULL) AS total_count_sum  -- Используем NULL вместо 0
    FROM
        time_intervals ti
    LEFT JOIN
        aggregated_data ad ON ti.recorded_at = ad.recorded_at
)

SELECT
    recorded_at,
    COALESCE(total_count_sum, LAG(total_count_sum) OVER (ORDER BY recorded_at)) AS total_count_sum
FROM
    final_data
ORDER BY
    recorded_at;
```


```
WITH time_intervals AS (
    SELECT
        generate_series(
            (SELECT MIN(DATE_TRUNC('minute', recorded_at)) FROM hantos),
            (SELECT MAX(DATE_TRUNC('minute', recorded_at)) FROM hantos),
            '3 minutes'::interval  -- Укажите нужный интервал, например, 3 минуты
        ) AS recorded_at
),
aggregated_data AS (
    SELECT
        DATE_TRUNC('minute', recorded_at) AS recorded_at,
        SUM(total_count) AS total_count_sum
    FROM
        hantos
    GROUP BY
        recorded_at
),
final_data AS (
    SELECT
        ti.recorded_at,
        COALESCE(ad.total_count_sum, NULL) AS total_count_sum  -- Используем NULL вместо 0
    FROM
        time_intervals ti
    LEFT JOIN
        aggregated_data ad ON ti.recorded_at = ad.recorded_at
)

SELECT
    recorded_at,
    CASE
        WHEN total_count_sum IS NULL THEN LAG(total_count_sum) OVER (ORDER BY recorded_at)
        ELSE total_count_sum
    END AS total_count_sum
FROM
    final_data
ORDER BY
    recorded_at;
```

```
WITH time_intervals AS (
    SELECT
        generate_series(
            (SELECT MIN(DATE_TRUNC('minute', recorded_at)) FROM hantos),
            (SELECT MAX(DATE_TRUNC('minute', recorded_at)) FROM hantos),
            '3 minutes'::interval  -- Укажите нужный интервал, например, 3 минуты
        ) AS recorded_at
),
aggregated_data AS (
    SELECT
        DATE_TRUNC('minute', recorded_at) AS recorded_at,
        SUM(total_count) AS total_count_sum
    FROM
        hantos
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
        WHEN total_count_sum = 0 THEN LAG(total_count_sum) OVER (ORDER BY recorded_at)
        ELSE total_count_sum
    END AS total_count_sum
FROM
    final_data
ORDER BY
    recorded_at;
```


```
WITH time_intervals AS (
    SELECT
        generate_series(
            (SELECT MIN(DATE_TRUNC('minute', recorded_at)) FROM hantos),
            (SELECT MAX(DATE_TRUNC('minute', recorded_at)) FROM hantos),
            '1 minute'::interval  -- Укажите нужный интервал, например, 1 минуту
        ) AS recorded_at
),
aggregated_data AS (
    SELECT
        DATE_TRUNC('minute', recorded_at) AS recorded_at,
        SUM(total_count) AS total_count_sum
    FROM
        hantos
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
        WHEN total_count_sum = 0 AND LAG(total_count_sum) OVER (ORDER BY recorded_at) = 0 THEN NULL
        ELSE total_count_sum
    END AS total_count_sum
FROM
    final_data
ORDER BY
    recorded_at;
```

```
WITH time_intervals AS (
    SELECT
        generate_series(
            (SELECT MIN(DATE_TRUNC('minute', recorded_at)) FROM hantos),
            (SELECT MAX(DATE_TRUNC('minute', recorded_at)) FROM hantos),
            '1 minute'::interval  -- Укажите нужный интервал, например, 1 минуту
        ) AS recorded_at
),
aggregated_data AS (
    SELECT
        DATE_TRUNC('minute', recorded_at) AS recorded_at,
        SUM(total_count) AS total_count_sum
    FROM
        hantos
    GROUP BY
        recorded_at
)

SELECT
    ti.recorded_at,
    COALESCE(ad.total_count_sum, 0) AS total_count_sum  -- Заполняем отсутствующие значения нулями
FROM
    time_intervals ti
LEFT JOIN
    aggregated_data ad ON ti.recorded_at = ad.recorded_at
ORDER BY
    ti.recorded_at;
```
```
WITH time_intervals AS (
    SELECT
        generate_series(
            (SELECT MIN(DATE_TRUNC('minute', recorded_at)) FROM meretoyaha),
            (SELECT MAX(DATE_TRUNC('minute', recorded_at)) FROM meretoyaha),
            '1 minute'::interval
        ) AS recorded_at
),
aggregated_data AS (
    SELECT
        DATE_TRUNC('minute', recorded_at) AS recorded_at,
        SUM(total_count) AS total_count_sum
    FROM
        meretoyaha
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
        WHEN total_count_sum = 0 THEN LAG(total_count_sum) OVER (ORDER BY recorded_at)
        ELSE total_count_sum
    END AS total_count_sum
FROM
    final_data
ORDER BY
    recorded_at;
```

```
WITH time_intervals AS (
    SELECT
        generate_series(
            (SELECT MIN(DATE_TRUNC('minute', recorded_at)) FROM meretoyaha),
            (SELECT MAX(DATE_TRUNC('minute', recorded_at)) FROM meretoyaha),
            '1 minute'::interval
        ) AS recorded_at
),
aggregated_data AS (
    SELECT
        DATE_TRUNC('minute', recorded_at) AS recorded_at,
        SUM(total_count) AS total_count_sum
    FROM
        meretoyaha
    GROUP BY
        recorded_at
)

SELECT
    ti.recorded_at,
    COALESCE(ad.total_count_sum, 0) AS total_count_sum  -- Заполняем отсутствующие значения нулями
FROM
    time_intervals ti
LEFT JOIN
    aggregated_data ad ON ti.recorded_at = ad.recorded_at
ORDER BY
    ti.recorded_at;
```

```
SELECT sum(total_count),recorded_at  FROM meretoyaha 

GROUP BY  recorded_at
```
```
Status: 500. Message: db query error: pq: column w.well_idКрасноленинское does not exist

```
```
ERROR:  operator does not exist: character varying = bigint
LINE 9:     AND w.well_id NOT IN (
                          ^
HINT:  No operator matches the given name and argument types. You might need to add explicit type casts. 

SQL-состояние: 42883
```
```
SELECT
    w.well_id,
    w.well_name,
    w.cluster_name
FROM
    wells_and_clusters w
WHERE
    w.field_name = 'Зимнее'  -- Фильтруем по конкретному месторождению
    AND w.well_id NOT IN (
        SELECT
            h.well_id
        FROM
            hantos h
        WHERE
            h.recorded_at >= NOW() - INTERVAL '30 minutes'  -- Данные обновлялись в последние 30 минут
    );
```

```
SELECT
    w.well_id,
    w.well_name,
    w.cluster_name,
    SUM(h.total_count) AS total_count,
    DATE_TRUNC('minute', h.recorded_at) AS recorded_at
FROM
    wells_and_clusters w
JOIN
    hantos h ON w.well_id::text = h.well_id::text  -- Приведение типов к строке
WHERE
    w.field_name = 'Зимнее'  -- Фильтруем по месторождению
GROUP BY
    w.well_id, w.well_name, w.cluster_name, recorded_at
ORDER BY
    recorded_at, w.well_id;
```
```
ERROR:  operator does not exist: character varying = bigint
LINE 10:     hantos h ON w.well_id = h.well_id
                                   ^
HINT:  No operator matches the given name and argument types. You might need to add explicit type casts. 

SQL-состояние: 42883
Символ: 215
```

```
SELECT
    w.well_id,
    w.well_name,
    w.cluster_name,
    SUM(h.total_count) AS total_count,
    DATE_TRUNC('minute', h.recorded_at) AS recorded_at
FROM
    wells_and_clusters w
JOIN
    hantos h ON w.well_id = h.well_id
WHERE
    w.field_name = 'Зимнее'  -- Фильтруем по месторождению
GROUP BY
    w.well_id, w.well_name, w.cluster_name, recorded_at
ORDER BY
    recorded_at, w.well_id;
```

```
WITH aggregated_data AS (
    SELECT
        DATE_TRUNC('minute', recorded_at) + INTERVAL '10 minutes' * (EXTRACT(MINUTE FROM recorded_at)::int / 10) AS interval_start,
        total_count
    FROM
        hantos 
    WHERE
        well_id IN (
            SELECT
                CAST(w.well_id AS bigint)
            FROM
                wells_and_clusters w
                JOIN fields f ON w.field_name = f.field_name
            WHERE
                f.field_name = 'Зимнее'  -- Замените на нужное название месторождения
        )
)

SELECT
    interval_start,
    PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY total_count) AS median_total_count
FROM
    aggregated_data
GROUP BY
    interval_start
ORDER BY
    interval_start;
```

```
WITH aggregated_data AS (
    SELECT
        DATE_TRUNC('minute', recorded_at) AS minute,
        total_count
    FROM
        hantos 
    WHERE
        well_id IN (
            SELECT
                CAST(w.well_id AS bigint)
            FROM
                wells_and_clusters w
                JOIN fields f ON w.field_name = f.field_name
            WHERE
                f.field_name = 'Зимнее'  -- Замените на нужное название месторождения
        )
)

SELECT
    minute,
    PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY total_count) AS median_total_count
FROM
    aggregated_data
GROUP BY
    minute
ORDER BY
    minute;
```

```
WITH aggregated_data AS (
    SELECT
        DATE_TRUNC('minute', recorded_at) AS minute,
        SUM(total_count) AS total_count_sum
    FROM
        hantos 
    WHERE
        well_id IN (
            SELECT
                CAST(w.well_id AS bigint)
            FROM
                wells_and_clusters w
                JOIN fields f ON w.field_name = f.field_name
            WHERE
                f.field_name = 'Зимнее'  -- Замените на нужное название месторождения
        )
    GROUP BY
        minute
),
smoothed_data AS (
    SELECT
        minute,
        total_count_sum,
        AVG(total_count_sum) OVER (
            ORDER BY minute 
            ROWS BETWEEN 2 PRECEDING AND CURRENT ROW  -- Скользящее среднее за 3 значения
        ) AS smoothed_value
    FROM
        aggregated_data
)

SELECT
    minute,
    smoothed_value
FROM
    smoothed_data
ORDER BY
    minute;
```
```
SELECT
    DATE_TRUNC('minute', recorded_at) + INTERVAL '10 minutes' * (EXTRACT(MINUTE FROM recorded_at)::int / 10) AS interval_start,
    AVG(total_count) AS average_total_count
FROM
    hantos
WHERE
    well_id IN (
        SELECT
            CAST(w.well_id AS bigint)
        FROM
            wells_and_clusters w
            JOIN fields f ON w.field_name = f.field_name
        WHERE
            f.field_name = 'Зимнее'  -- Замените на нужное название месторождения
    )
GROUP BY
    interval_start
ORDER BY
    interval_start;
```

```
WITH aggregated_data AS (
    SELECT
        DATE_TRUNC('minute', recorded_at) AS minute,
        SUM(total_count) AS total_count_sum
    FROM
        hantos 
    WHERE
        well_id IN (
            SELECT
                CAST(w.well_id AS bigint)
            FROM
                wells_and_clusters w
                JOIN fields f ON w.field_name = f.field_name
            WHERE
                f.field_name = 'Зимнее'  -- Замените на нужное название месторождения
        )
    GROUP BY
        minute
),
smoothed_data AS (
    SELECT
        minute,
        total_count_sum,
        AVG(total_count_sum) OVER (
            ORDER BY minute 
            ROWS BETWEEN 3 PRECEDING AND CURRENT ROW  -- Учитываем 10 минут (3 минуты * 4)
        ) AS average_last_10_minutes
    FROM
        aggregated_data
)

SELECT
    DATE_TRUNC('minute', minute) AS minute,
    average_last_10_minutes
FROM
    smoothed_data
WHERE
    minute >= NOW() - INTERVAL '10 minutes'  -- Фильтруем последние 10 минут
ORDER BY
    minute;
```

```
WITH aggregated_data AS (
    SELECT
        DATE_TRUNC('minute', recorded_at) AS minute,
        SUM(total_count) AS total_count_sum
    FROM
        hantos 
    WHERE
        well_id IN (
            SELECT
                CAST(w.well_id AS bigint)
            FROM
                wells_and_clusters w
                JOIN fields f ON w.field_name = f.field_name
            WHERE
                f.field_name = 'Зимнее'  -- Замените на нужное название месторождения
        )
    GROUP BY
        minute
),
smoothed_data AS (
    SELECT
        minute,
        total_count_sum,
        AVG(total_count_sum) OVER (ORDER BY minute ROWS BETWEEN 2 PRECEDING AND CURRENT ROW) AS smoothed_value
    FROM
        aggregated_data
)

SELECT
    minute,
    smoothed_value
FROM
    smoothed_data
ORDER BY
    minute;
```
```
SELECT
    DATE_TRUNC('minute', recorded_at) AS minute,  -- Округляем время до минуты
    SUM(total_count) AS total_count_sum            -- Суммируем total_count
FROM
    hantos 
WHERE
    well_id IN (
        SELECT
            CAST(w.well_id AS bigint)
        FROM
            wells_and_clusters w
            JOIN fields f ON w.field_name = f.field_name
        WHERE
            f.field_name = 'Зимнее'  -- Замените на нужное название месторождения
    )
GROUP BY
    minute  -- Группируем по округленному времени
ORDER BY
    minute;  -- Сортируем по времени
```	

```
SELECT
    DATE_TRUNC('minute', recorded_at) AS minute,  -- Округляем время до минуты
    SUM(total_count) AS total_count_sum            -- Суммируем total_count
FROM
    hantos
WHERE
    recorded_at >= NOW() - INTERVAL '10 minutes'  -- Фильтруем записи за последние 10 минут
GROUP BY
    minute  -- Группируем по округленному времени
ORDER BY
    minute;  -- Сортируем по времени
```
```
WITH ranked_counts AS (
    SELECT
        recorded_at,
        total_count,
        ROW_NUMBER() OVER (ORDER BY recorded_at DESC) AS rn
    FROM (
        SELECT
            recorded_at,
            SUM(total_count) AS total_count
        FROM
            hantos
        GROUP BY
            recorded_at
    ) AS aggregated_counts
)

SELECT
    MAX(recorded_at) AS last_recorded_at,  -- Получаем последнее recorded_at
    AVG(total_count) AS average_last_three  -- Вычисляем среднее значение
FROM
    ranked_counts
WHERE
    rn <= 3  -- Берем только последние 3 записи
GROUP BY
    recorded_at;  -- Группируем по recorded_at, если нужно
```
```
import smtplib
from email.mime.text import MIMEText
from email.mime.multipart import MIMEMultipart

# Настройки
smtp_server = 'smtp.example.com'  # Замените на адрес вашего SMTP-сервера
smtp_port = 587                     # Обычно 587 для TLS
smtp_user = 'your_email@example.com'  # Ваш email
smtp_password = 'your_password'     # Ваш пароль

# Создание сообщения
sender_email = smtp_user
receiver_email = 'recipient@example.com'  # Замените на адрес получателя
subject = 'Тема сообщения'
body = 'Это тело сообщения.'

# Создание объекта сообщения
msg = MIMEMultipart()
msg['From'] = sender_email
msg['To'] = receiver_email
msg['Subject'] = subject

# Добавление текста сообщения
msg.attach(MIMEText(body, 'plain'))

try:
    # Подключение к SMTP-серверу
    server = smtplib.SMTP(smtp_server, smtp_port)
    server.starttls()  # Включение TLS
    server.login(smtp_user, smtp_password)  # Вход на сервер

    # Отправка сообщения
    server.sendmail(sender_email, receiver_email, msg.as_string())
    print('Email успешно отправлен!')

except Exception as e:
    print(f'Ошибка при отправке email: {e}')

finally:
    server.quit()  # Закрытие соединения
```

```
WITH last_three_sums AS (
    SELECT
        recorded_at,
        SUM(total_count) AS total_count
    FROM
        hantos 
    WHERE
        well_id IN (
            SELECT
                CAST(w.well_id AS bigint)
            FROM
                wells_and_clusters w
                JOIN fields f ON w.field_name = f.field_name
            WHERE
                f.field_name = 'Зимнее' -- Замените на нужное название месторождения
        )
    GROUP BY
        recorded_at
    ORDER BY
        recorded_at DESC
    LIMIT 3  -- Получаем последние 3 записи
)

SELECT
    recorded_at,
    AVG(total_count) AS average_last_three_sums  -- Вычисляем среднее значение
FROM
    last_three_sums
GROUP BY
    recorded_at;  -- Группируем по recorded_at
```
```
WITH last_three_sums AS (
    SELECT
        recorded_at,
        SUM(total_count) AS total_count
    FROM
        hantos 
    WHERE
        well_id IN (
            SELECT
                CAST(w.well_id AS bigint)
            FROM
                wells_and_clusters w
                JOIN fields f ON w.field_name = f.field_name
            WHERE
                f.field_name = 'Зимнее' -- Замените на нужное название месторождения
        )
    GROUP BY
        recorded_at
    ORDER BY
        recorded_at DESC
    LIMIT 3  -- Получаем последние 3 записи
)

SELECT
    AVG(total_count) AS average_last_three_sums  -- Вычисляем среднее значение
FROM
    last_three_sums;
```


```
SELECT
  SUM(total_count) AS "Зимнее",
  recorded_at
FROM
  hantos 
WHERE
  well_id IN (
    -- напиши запрос что бы выводил список wellid и имя скавыажины по названию месторождения
    SELECT
      CAST(w.well_id AS bigint)
    FROM
      wells_and_clusters w
      JOIN fields f ON w.field_name = f.field_name
    WHERE
      f.field_name = 'Зимнее' -- Замените $1 на нужное название месторождения
  )
GROUP BY
  recorded_at;
```


```
-- Создание временной таблицы
CREATE TEMP TABLE temp_aggregated_counts (
    well_id BIGINT,
    total_count INT,
    recorded_at TIMESTAMP,
    company_id INT
);

-- Заполнение временной таблицы
INSERT INTO temp_aggregated_counts (well_id, total_count, recorded_at, company_id)
WITH aggregated_counts AS (
    SELECT 
        mc.well_id, 
        SUM(mc.count) AS total_count, 
        MAX(mc.timestamp) AS recorded_at,
        c.company_id
    FROM 
        message_counts mc
    JOIN 
        wells_and_clusters w ON mc.well_id::text = w.well_id
    JOIN 
        fields f ON w.field_name = f.field_name
    JOIN 
        companies c ON f.company_id = c.company_id
    WHERE 
        mc.timestamp >= NOW() - INTERVAL '10 minutes'
        AND c.company_id IN (1, 2, 3, 4, 5, 7, 8, 9, 10)
    GROUP BY 
        mc.well_id, c.company_id
)
SELECT well_id, total_count, recorded_at, company_id FROM aggregated_counts;

-- Вставка в таблицу 'vostok'
INSERT INTO vostok (well_id, total_count, recorded_at)
SELECT well_id, total_count, recorded_at 
FROM temp_aggregated_counts 
WHERE company_id = 1;

-- Вставка в таблицу 'orenburg'
INSERT INTO orenburg (well_id, total_count, recorded_at)
SELECT well_id, total_count, recorded_at 
FROM temp_aggregated_counts 
WHERE company_id = 2;

-- Вставка в таблицу 'zapolare'
INSERT INTO zapolare (well_id, total_count, recorded_at)
SELECT well_id, total_count, recorded_at 
FROM temp_aggregated_counts 
WHERE company_id = 3;

-- Вставка в таблицу 'hantos'
INSERT INTO hantos (well_id, total_count, recorded_at)
SELECT well_id, total_count, recorded_at 
FROM temp_aggregated_counts 
WHERE company_id = 4;

-- Вставка в таблицу 'messoyaha'
INSERT INTO messoyaha (well_id, total_count, recorded_at)
SELECT well_id, total_count, recorded_at 
FROM temp_aggregated_counts 
WHERE company_id = 7;

-- Вставка в таблицу 'yamal'
INSERT INTO yamal (well_id, total_count, recorded_at)
SELECT well_id, total_count, recorded_at 
FROM temp_aggregated_counts 
WHERE company_id = 8;

-- Вставка в таблицу 'meretoyaha'
INSERT INTO meretoyaha (well_id, total_count, recorded_at)
SELECT well_id, total_count, recorded_at 
FROM temp_aggregated_counts 
WHERE company_id = 9;

-- Вставка в таблицу 'nng'
INSERT INTO nng (well_id, total_count, recorded_at)
SELECT well_id, total_count, recorded_at 
FROM temp_aggregated_counts 
WHERE company_id = 10;
```


```
ERROR:  relation "aggregated_counts" does not exist
LINE 31: FROM aggregated_counts 
```

```
WITH aggregated_counts AS (
    SELECT 
        mc.well_id, 
        SUM(mc.count) AS total_count, 
        MAX(mc.timestamp) AS recorded_at,  -- Используем MAX для получения последнего timestamp
        c.company_id
    FROM 
        message_counts mc
    JOIN 
        wells_and_clusters w ON mc.well_id::text = w.well_id
    JOIN 
        fields f ON w.field_name = f.field_name
    JOIN 
        companies c ON f.company_id = c.company_id
    WHERE 
        mc.timestamp >= NOW() - INTERVAL '10 minutes'
        AND c.company_id IN (1, 2, 3, 4, 5, 7, 8, 9, 10)
    GROUP BY 
        mc.well_id, c.company_id
)

-- Вставка в таблицу 'vostok'
INSERT INTO vostok (well_id, total_count, recorded_at)
SELECT well_id, total_count, recorded_at 
FROM aggregated_counts 
WHERE company_id = 1;

-- Вставка в таблицу 'orenburg'
INSERT INTO orenburg (well_id, total_count, recorded_at)
SELECT well_id, total_count, recorded_at 
FROM aggregated_counts 
WHERE company_id = 2;

-- Вставка в таблицу 'zapolare'
INSERT INTO zapolare (well_id, total_count, recorded_at)
SELECT well_id, total_count, recorded_at 
FROM aggregated_counts 
WHERE company_id = 3;

-- Вставка в таблицу 'hantos'
INSERT INTO hantos (well_id, total_count, recorded_at)
SELECT well_id, total_count, recorded_at 
FROM aggregated_counts 
WHERE company_id = 4;

-- Вставка в таблицу 'messoyaha'
INSERT INTO messoyaha (well_id, total_count, recorded_at)
SELECT well_id, total_count, recorded_at 
FROM aggregated_counts 
WHERE company_id = 7;

-- Вставка в таблицу 'yamal'
INSERT INTO yamal (well_id, total_count, recorded_at)
SELECT well_id, total_count, recorded_at 
FROM aggregated_counts 
WHERE company_id = 8;

-- Вставка в таблицу 'meretoyaha'
INSERT INTO meretoyaha (well_id, total_count, recorded_at)
SELECT well_id, total_count, recorded_at 
FROM aggregated_counts 
WHERE company_id = 9;

-- Вставка в таблицу 'nng'
INSERT INTO nng (well_id, total_count, recorded_at)
SELECT well_id, total_count, recorded_at 
FROM aggregated_counts 
WHERE company_id = 10;
```
```
WITH aggregated_counts AS (
    SELECT 
        mc.well_id, 
        SUM(mc.count) AS total_count, 
        MAX(mc.timestamp) AS recorded_at,  -- Используем MAX для получения последнего timestamp
        c.company_id
    FROM 
        message_counts mc
    JOIN 
        wells_and_clusters w ON mc.well_id::text = w.well_id
    JOIN 
        fields f ON w.field_name = f.field_name
    JOIN 
        companies c ON f.company_id = c.company_id
    WHERE 
        mc.timestamp >= NOW() - INTERVAL '10 minutes'
        AND c.company_id IN (1, 2, 3, 4, 5, 8, 9, 10)
    GROUP BY 
        mc.well_id, c.company_id
)
INSERT INTO nng (well_id, total_count, recorded_at)
SELECT 
    well_id, 
    total_count, 
    recorded_at
FROM 
    aggregated_counts
WHERE 
    company_id IN (1, 2, 3, 4, 5, 8, 9, 10);
```


```
ERROR:  column "mc.timestamp" must appear in the GROUP BY clause or be used in an aggregate function
LINE 5:         mc.timestamp AS recorded_at,
                ^ 

SQL-состояние: 42803
```


```
WITH aggregated_counts AS (
    SELECT 
        mc.well_id, 
        SUM(mc.count) AS total_count, 
        mc.timestamp AS recorded_at,
        c.company_id
    FROM 
        message_counts mc
    JOIN 
        wells_and_clusters w ON mc.well_id::text = w.well_id  -- Приведение mc.well_id к тексту
    JOIN 
        fields f ON w.field_name = f.field_name
    JOIN 
        companies c ON f.company_id = c.company_id
    WHERE 
        mc.timestamp >= NOW() - INTERVAL '10 minutes'
        AND c.company_id IN (1, 2, 3, 4, 5, 8, 9, 10)
    GROUP BY 
        mc.well_id, c.company_id
)
INSERT INTO nng (well_id, total_count, recorded_at)
SELECT 
    well_id, 
    total_count, 
    recorded_at
FROM 
    aggregated_counts
WHERE 
    company_id IN (1, 2, 3, 4, 5, 8, 9, 10);
```

```
WITH aggregated_counts AS (
    SELECT 
        well_id, 
        SUM(count) AS total_count, 
        "timestamp" AS recorded_at,
        CASE 
            WHEN c.company_id = 1 THEN 'vostok'
            WHEN c.company_id = 2 THEN 'orenburg'
            WHEN c.company_id = 3 THEN 'zapolare'
			WHEN c.company_id = 4 THEN 'hantos'
			WHEN c.company_id = 7 THEN 'messoyaha'
			WHEN c.company_id = 8 THEN 'yamal'
			WHEN c.company_id = 9 THEN 'meretoyaha'
			WHEN c.company_id = 10 THEN 'nng'
        END AS target_table
    FROM 
        message_counts mc
    JOIN 
        wells_and_clusters w ON mc.well_id = w.well_id
    JOIN 
        fields f ON w.field_name = f.field_name
    JOIN 
        companies c ON f.company_id = c.company_id
    WHERE 
        mc.timestamp >= NOW() - INTERVAL '10 minutes'
        AND c.company_id IN (1,2,3,4,5,8,9,10)  -- Замените на нужные идентификаторы дочерних обществ
    GROUP BY 
        well_id, c.company_id
)
INSERT INTO nng (well_id, total_count, recorded_at)
SELECT well_id, total_count, recorded_at FROM aggregated_counts WHERE target_table = 'vostok'
UNION ALL
SELECT well_id, total_count, recorded_at FROM aggregated_counts WHERE target_table = 'orenburg'
UNION ALL
SELECT well_id, total_count, recorded_at FROM aggregated_counts WHERE target_table = 'zapolare'
UNION ALL
SELECT well_id, total_count, recorded_at FROM aggregated_counts WHERE target_table = 'hantos'
UNION ALL
SELECT well_id, total_count, recorded_at FROM aggregated_counts WHERE target_table = 'messoyaha'
UNION ALL
SELECT well_id, total_count, recorded_at FROM aggregated_counts WHERE target_table = 'yamal'
UNION ALL
SELECT well_id, total_count, recorded_at FROM aggregated_counts WHERE target_table = 'meretoyaha'
UNION ALL
SELECT well_id, total_count, recorded_at FROM aggregated_counts WHERE target_table = 'nng';
```


```
ERROR:  operator does not exist: bigint = character varying
LINE 19:         wells_and_clusters w ON mc.well_id = w.well_id
                                                    ^
HINT:  No operator matches the given name and argument types. You might need to add explicit type casts. 
```


```
WITH aggregated_counts AS (
    SELECT 
        well_id, 
        SUM(count) AS total_count, 
        CURRENT_TIMESTAMP AS recorded_at,
        CASE 
            WHEN c.company_id = 3 THEN 'nng'
            WHEN c.company_id = 4 THEN 'yamal'
            WHEN c.company_id = 5 THEN 'orenburg'
        END AS target_table
    FROM 
        message_counts mc
    JOIN 
        wells_and_clusters w ON mc.well_id = w.well_id
    JOIN 
        fields f ON w.field_name = f.field_name
    JOIN 
        companies c ON f.company_id = c.company_id
    WHERE 
        mc.timestamp >= NOW() - INTERVAL '10 minutes'
        AND c.company_id IN (3, 4, 5)  -- Замените на нужные идентификаторы дочерних обществ
    GROUP BY 
        well_id, c.company_id
)
INSERT INTO nng (well_id, total_count, recorded_at)
SELECT well_id, total_count, recorded_at FROM aggregated_counts WHERE target_table = 'nng'
UNION ALL
SELECT well_id, total_count, recorded_at FROM aggregated_counts WHERE target_table = 'yamal'
UNION ALL
SELECT well_id, total_count, recorded_at FROM aggregated_counts WHERE target_table = 'orenburg';
```

```
   */10 * * * * /var/service/kafka_redis/script_db/insert_do/insert_do.sh >> /var/service/kafka_redis/logs/insert_do.log 2>&1
   0 0 * * 3 /var/service/kafka_redis/script_db/delete_do_7days/script.sh >> /var/service/kafka_redis/logs/delete_do.log 2>&1
   0 */4 * * * /var/service/kafka_redis/script_db/delete_message_counts_6_hours/script.sh >> /var/service/kafka_redis/logs/delete_message_counts.log 2>&1
```

```                                                                               /tmp/crontab.90EHEB                                                                                             
*/10 * * * * /var/service/kafka_redis/script_db/insert_do/insert_do.sh
0 0 * * 3 /var/service/kafka_redis/script_db/delete_do_7days/script.sh
0 */4 * * * /var/service/kafka_redis/script_db/delete_message_counts_6_hours/script.sh


```
```
дек 18 10:00:21 localhost.localdomain crond[2651005]: No configuration file found at /root/.esmtprc or /etc/esmtprc
дек 18 10:00:21 localhost.localdomain crond[2651007]: No configuration file found at /root/.esmtprc or /etc/esmtprc
дек 18 10:00:21 localhost.localdomain crond[2651009]: No configuration file found at /root/.esmtprc or /etc/esmtprc
дек 18 10:00:21 localhost.localdomain crond[2651027]: No configuration file found at /root/.esmtprc or /etc/esmtprc
дек 18 10:00:21 localhost.localdomain crond[2651031]: No configuration file found at /root/.esmtprc or /etc/esmtprc
дек 18 10:00:21 localhost.localdomain crond[2651033]: No configuration file found at /root/.esmtprc or /etc/esmtprc
дек 18 10:00:21 localhost.localdomain crond[2651035]: No configuration file found at /root/.esmtprc or /etc/esmtprc
дек 18 10:00:21 localhost.localdomain crond[2651037]: No configuration file found at /root/.esmtprc or /etc/esmtprc
дек 18 10:01:01 localhost.localdomain CROND[2651118]: (root) CMD (run-parts /etc/cron.hourly)
дек 18 10:01:01 localhost.localdomain run-parts[2651121]: (/etc/cron.hourly) starting 0anacron
дек 18 10:01:01 localhost.localdomain run-parts[2651127]: (/etc/cron.hourly) finished 0anacron
дек 18 10:01:01 localhost.localdomain CROND[2651117]: (root) CMDEND (run-parts /etc/cron.hourly)
lines 45753-45802/45802 (END)

```

```
SELECT well_id, parameter_id, count, "timestamp"
	FROM public.message_counts

	WHERE timestamp >= NOW() - INTERVAL '45 minutes'
		
```


```
#!/bin/bash
```
```
дек 16 11:39:15 localhost.localdomain systemd[1]: insert_db.service: Failed with result 'exit-code'.
дек 16 11:39:15 localhost.localdomain systemd[1]: Failed to start insert_db.service - insert_db.
дек 16 11:50:01 localhost.localdomain (rt_do.sh)[12982]: insert_db.service: Failed to execute /var/service/kafka_redis/script_db/insert_do/insert_do.sh: Exec format error
дек 16 11:50:01 localhost.localdomain (rt_do.sh)[12982]: insert_db.service: Failed at step EXEC spawning /var/service/kafka_redis/script_db/insert_do/insert_do.sh: Exec format error

```
```
systemctl list-timers
```
```
sudo systemctl daemon-reload
sudo systemctl enable my-script.timer
sudo systemctl start my-script.timer
```

## Удаление данных раз в 6 часов
```sudo nano /etc/systemd/system/my-script.service```
```
[Unit]
Description=My Script Service

[Service]
Type=oneshot
ExecStart=/path/to/your/script.sh
```
```sudo nano /etc/systemd/system/my-script.timer ```
```
[Unit]
Description=Run My Script Every 6 Hours

[Timer]
OnBootSec=10min
OnUnitActiveSec=6h
Unit=my-script.service

[Install]
WantedBy=timers.target
```

## Выставка данных раз в 10 минут
```
[Unit]
Description=My Script Service

[Service]
Type=oneshot
ExecStart=/home/user/myscript.sh
```
```
[Unit]
Description=Run My Script Every 10 Minutes

[Timer]
OnBootSec=10min
OnUnitActiveSec=10min
Unit=my-script.service

[Install]
WantedBy=timers.target
```

```
INSERT INTO nng (well_id, total_count, recorded_at)
SELECT well_id, SUM(count) AS total_count, CURRENT_TIMESTAMP
FROM message_counts
WHERE timestamp >= NOW() - INTERVAL '10 minutes'
AND well_id IN (
SELECT 
    w.well_id 
   
FROM 
    wells_and_clusters w
JOIN 
    fields f ON w.field_name = f.field_name
JOIN 
    companies c ON f.company_id = c.company_id
WHERE 
    c.company_id = 3  -- Замените $1 на нужный идентификатор дочернего общества
)  -- Укажите нужные well_id
GROUP BY well_id;

```


```
-- напиши запрос что бы выводил список wellid и имя скавыажины по названию месторождения
SELECT 
    w.well_id, 
    w.well_name
FROM 
    wells_and_clusters w
JOIN 
    fields f ON w.field_name = f.field_name
WHERE 
    f.field_name = $1;  -- Замените $1 на нужное название месторождения
```
```
-- с выводящий название скважин и wellid по указаному дочернему обществу
SELECT 
    w.well_id, 
    w.well_name
FROM 
    wells_and_clusters w
JOIN 
    fields f ON w.field_name = f.field_name
JOIN 
    companies c ON f.company_id = c.company_id
WHERE 
    c.company_id = $1;  -- Замените $1 на нужный идентификатор дочернего общества
```

```
CREATE TABLE wells_and_clusters (
    well_id VARCHAR(50) PRIMARY KEY,      -- Уникальный идентификатор для каждой скважины (текстовый)
    well_name VARCHAR(255) NOT NULL,       -- Название скважины
    cluster_name VARCHAR(255) NOT NULL,     -- Название куста
    field_name VARCHAR(255) NOT NULL        -- Название месторождения
);
```

```
CREATE TABLE clusters_and_wells (
    id SERIAL PRIMARY KEY,
    cluster_name VARCHAR(255) NOT NULL,
    well_name VARCHAR(255),
    field_id INT NOT NULL,
    FOREIGN KEY (field_id) REFERENCES fields(field_id) ON DELETE CASCADE
);
```
```
INSERT INTO clusters_and_wells (cluster_name, well_name, field_id) VALUES 
    ('Cluster 1', 'Well 1', 1),
    ('Cluster 1', 'Well 2', 1),
    ('Cluster 2', 'Well 3', 1),
    ('Cluster 2', 'Well 4', 1),
    ('Cluster 3', NULL, 1),  -- Куст без скважин
    ('Cluster 3', 'Well 5', 1);
```



2. Объяснение структуры
companies: Таблица для хранения информации о дочерних обществах. Каждое общество имеет уникальный идентификатор company_id и имя company_name.
fields: Таблица для хранения месторождений. Каждое месторождение имеет уникальный идентификатор field_id, имя field_name и внешний ключ company_id, который ссылается на таблицу companies.
clusters: Таблица для хранения кустов. Каждый куст имеет уникальный идентификатор cluster_id, имя cluster_name и внешний ключ field_id, который ссылается на таблицу fields.
wells: Таблица для хранения скважин. Каждая скважина имеет уникальный идентификатор well_id, имя well_name и внешний ключ cluster_id, который ссылается на таблицу clusters.

3. Пример вставки данных
Вот пример, как можно вставить данные в эти таблицы:
```
-- Вставка дочерних обществ
INSERT INTO companies (company_name) VALUES ('Company A');
INSERT INTO companies (company_name) VALUES ('Company B');

-- Вставка месторождений
INSERT INTO fields (field_name, company_id) VALUES ('Field 1', 1);
INSERT INTO fields (field_name, company_id) VALUES ('Field 2', 1);
INSERT INTO fields (field_name, company_id) VALUES ('Field 3', 2);

-- Вставка кустов
INSERT INTO clusters (cluster_name, field_id) VALUES ('Cluster 1', 1);
INSERT INTO clusters (cluster_name, field_id) VALUES ('Cluster 2', 1);
INSERT INTO clusters (cluster_name, field_id) VALUES ('Cluster 3', 2);
INSERT INTO clusters (cluster_name, field_id) VALUES ('Cluster 4', 3);

-- Вставка скважин
INSERT INTO wells (well_name, cluster_id) VALUES ('Well 1', 1);
INSERT INTO wells (well_name, cluster_id) VALUES ('Well 2', 1);
INSERT INTO wells (well_name, cluster_id) VALUES ('Well 3', 2);
INSERT INTO wells (well_name, cluster_id) VALUES ('Well 4', 3);
INSERT INTO wells (well_name, cluster_id) VALUES ('Well 5', 4);
```

Таблица companies (Дочерние общества)
```
CREATE TABLE companies (
    company_id SERIAL PRIMARY KEY,
    company_name VARCHAR(255) NOT NULL
);
```
Таблица fields (Месторождения)
```
CREATE TABLE fields (
    field_id SERIAL PRIMARY KEY,
    field_name VARCHAR(255) NOT NULL,
    company_id INT REFERENCES companies(company_id) ON DELETE CASCADE
);
```
Таблица clusters (Кусты)
```
CREATE TABLE clusters (
    cluster_id SERIAL PRIMARY KEY,
    cluster_name VARCHAR(255) NOT NULL,
    field_id INT REFERENCES fields(field_id) ON DELETE CASCADE
);
```
Таблица wells (Скважины)
```
CREATE TABLE wells (
    well_id SERIAL PRIMARY KEY,
    well_name VARCHAR(255) NOT NULL,
    cluster_id INT REFERENCES clusters(cluster_id) ON DELETE CASCADE
);
```
```

*/5 * * * * /home/your_username/run_query.sh
```
```
   #!/bin/bash

   # Выполнение SQL-запроса в контейнере PostgreSQL
   docker exec -i my_postgres_container psql -U postgres -d my_database -c "INSERT INTO my_table (column1, column2) VALUES ('value1', 'value2');"
```


```
SELECT 
    FROM_UNIXTIME(1280718960) AS date1,
    FROM_UNIXTIME(1280719740) AS date2,
    FROM_UNIXTIME(1280721360) AS date3;
```
```
1280718960
1280719740
1280721360
```

```
-- 1. Создание таблицы vostok
CREATE TABLE vostok (
    well_id BIGINT,
    total_count INTEGER,
    recorded_at TIMESTAMPTZ DEFAULT CURRENT_TIMESTAMP  -- Поле для хранения текущего времени
);

-- 2. Вставка значений из запроса с текущим временем
INSERT INTO vostok (well_id, total_count, recorded_at)
SELECT well_id, SUM(count) AS total_count, CURRENT_TIMESTAMP
FROM message_counts
WHERE timestamp >= NOW() - INTERVAL '10 minutes'
AND well_id IN (2540257976, 2540432133)  -- Укажите нужные well_id
GROUP BY well_id;
```
```
docker exec -it my_postgres bash -c "sed -i \"s/^#timezone = 'UTC'/timezone = 'Asia/Yekaterinburg'/\" /var/lib/postgresql/data/postgresql.conf"
```

```
ALTER DATABASE your_database_name SET timezone = 'Asia/Yekaterinburg';
```


```
   SELECT timestamp, count
   FROM message_counts
   ORDER BY timestamp DESC
   LIMIT 10;
```
```
-- Создание новой таблицы shingindkoe
CREATE TABLE IF NOT EXISTS shingindkoe (
    total_count INTEGER,
    recorded_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Вставка данных в таблицу shingindkoe для конкретных well_id
INSERT INTO shingindkoe (total_count, recorded_at)
SELECT SUM(count) AS total_count, CURRENT_TIMESTAMP
FROM message_counts
WHERE timestamp >= NOW() - INTERVAL '10 minutes'
AND well_id IN (/* Укажите конкретные well_id здесь, например: */ 2540257976, 2540432133);

```



```
1
1
2
2
3
3
4
4
5
5
6
6
6
7
7
8
8
9
9
10
10
11
11
12
12
14
14
15
15
18
18
19
19
20
21
22
23
23
24
25
27
28
29
30
30
30
31
32
32
33
33
34
34
35
35
36
37
38
39
39
39
39
40
41
41
41
42
42
42
42
42
43
44
44
45
45
46
46
47
48
48
48
49
50
51
52
54
55
56
57
58
59
60
61
61
73
73
75
81
90
8031
```
```
2024/12/02 11:08:01 Error creating consumer: kafka: client has run out of available brokers to talk to: dial tcp: missing address

```
```
2024/11/15 15:28:47 parsing time "" as "2006-01-02T15:04:05.999": cannot parse "" as "2006"

```
```
#!/bin/bash

# Пароль для всех пользователей
PASSWORD="12345678"

# Создание 20 пользователей
for i in {1..20}; do
    USERNAME="user$i"
    sudo useradd -m -s /bin/bash "$USERNAME"
    echo "$USERNAME:$PASSWORD" | sudo chpasswd
done

echo "20 пользователей создано успешно."

```
```
s = """
"as","v","c","d"
1,2,3,4
45,33,22,111
"""
def str_to_list_of_dicts(s):
    # Удаляем лишние пробелы и разбиваем строку на отдельные строки
    lines = [line.strip() for line in s.strip().split('\n')]

    # Разбиваем первую строку на ключи
    keys = [key.strip('"') for key in lines[0].split(',')]

    # Инициализируем пустой список для словарей
    dict_list = []

    # Проходим по оставшимся строкам и создаем словари
    for line in lines[1:]:
        values = [value.strip() for value in line.split(',')]
        if len(values) == len(keys):
            dict_list.append(dict(zip(keys, values)))

    # Выводим результат
  
    return dict_list
a = str_to_list_of_dicts(s)
print(a)
```
https://github.com/docker/awesome-compose/tree/master/django
https://github.com/bitnami/containers/blob/main/bitnami/redis/docker-compose.yml




# kafka_servise
{{now | dateFormat "DD/MM/YYYY HH:mm:ss"}}
{{now | strftime('%Y-%m-%d %H:%M:%S')}}

Создайте файл юнита Systemd: Создайте новый файл в директории /etc/systemd/system/, например export_kafka.service, и укажите следующий содержимое:
plaintext

Copy
[Unit]
Description=My Go Service
After=network.target

[Service]
Type=simple
ExecStart=/путь/к/вашему/исполняемому/файлу/export_kafka
Restart=always

[Install]
WantedBy=multi-user.target
Обязательно замените /путь/к/вашему/исполняемому/файлу/ на фактический путь к вашему исполняемому файлу.
Запустите службу и настройте автозапуск: После создания файла службы, выполните следующие команды для запуска вашей службы и настройки автозапуска:
bash

Copy
sudo systemctl daemon-reload
sudo systemctl start export_kafka
sudo systemctl enable export_kafka
Управление службой:
Для запуска службы: sudo systemctl start export_kafka
Для остановки службы: sudo systemctl stop export_kafka
Для перезапуска службы: sudo systemctl restart export_kafka
Для отключения автозапуска: sudo systemctl disable export_kafka
Просмотр логов службы: Вы можете просмотреть журналы службы с помощью journalctl:
bash

Copy
sudo journalctl -u export_kafka
После выполнения этих шагов ваш исполняемый файл export_kafka будет запускаться как служба при загрузке системы и будет автоматически перезапускаться в случае сбоя.
