```
2024/11/18 15:21:47 Ошибка при десериализации JSON для ключа kafka:3f4f4000: не удалось распарсить время '': parsing time "" as "2006-01-02 15:04:05": cannot parse "" as "2006"
2024/11/18 15:21:47 не удалось распарсить время '': parsing time "" as "2006-01-02 15:04:05": cannot parse "" as "2006"
2024/11/18 15:21:47 Ошибка при десериализации JSON для ключа kafka:3b34e38d: не удалось распарсить время '': parsing time "" as "2006-01-02 15:04:05": cannot parse "" as "2006"
2024/11/18 15:21:47 не удалось распарсить время '': parsing time "" as "2006-01-02 15:04:05": cannot parse "" as "2006"
2024/11/18 15:21:47 Ошибка при десериализации JSON для ключа kafka:a8971637: не удалось распарсить время '': parsing time "" as "2006-01-02 15:04:05": cannot parse "" as "2006"

```


```
/redis_pgsql_linux
2024/11/18 14:35:22 parsing time "2024-11-18 18:33:36" as "2006-01-02T15:04:05.999": cannot parse " 18:33:36" as "T"

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
