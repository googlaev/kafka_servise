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
Таблица companies (Дочерние общества)
```
CREATE TABLE companies (
    company_id SERIAL PRIMARY KEY,
    company_name VARCHAR(255) NOT NULL
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
