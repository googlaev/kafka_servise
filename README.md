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
