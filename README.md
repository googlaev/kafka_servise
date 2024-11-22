
От: Шеховцова Ольга Руслановна
Отправлено: 21 ноября 2024 г. 16:36
Кому: Федоренко Оксана Васильевна; Максимова Екатерина Михайловна; Сутягина Фаина Алексеевна; Самойлова Евгения Васильевна; Колесникова Инна Сергеевна; Минибаев Рустам Расихович; Гуляев Иван Павлович
Тема: скрининг офис 23.11
 
Добрый день, коллеги!
Напоминаю, что вы записаны на скрининг на 23 ноября (суббота) в «Доктор Арбитайло», ул. Республики, дом № 40, корпус 1.
В заявленную дату необходимо явиться натощак к 07:50 - 08:00 по вышеуказанному адресу, при себе иметь паспорт, баночки с анализами (моча, кал - см памятку, если это требуется в соответствии с вашим полом и возрастом). Прохождение скрининга вы начнете со сдачи анализов, ретнген- и УЗ- исследований. 
Далее вас направят к узким специалистам, которые уже будут видеть результаты анализов и дадут рекомендации.
Таким образом, скрининг можно будет пройти за один день.
Памятка по подготовке к исследованиям во вложении.



```python
import smtplib
from email.mime.text import MIMEText
from email.mime.multipart import MIMEMultipart

# Параметры SMTP-сервера
smtp_server = 'smtp.example.com'
smtp_port = 587
smtp_user = 'your_email@example.com'
smtp_password = 'your_password'

# Параметры сообщения
from_email = 'your_email@example.com'
to_email = 'recipient@example.com'
subject = 'Test Email'
body = 'This is a test email sent from Python.'

# Создание сообщения
msg = MIMEMultipart()
msg['From'] = from_email
msg['To'] = to_email
msg['Subject'] = subject
msg.attach(MIMEText(body, 'plain'))

# Отправка сообщения
try:
    server = smtplib.SMTP(smtp_server, smtp_port)
    server.starttls()  # Устанавливаем защищенное соединение
    server.login(smtp_user, smtp_password)
    server.sendmail(from_email, to_email, msg.as_string())
    server.quit()
    print('Email sent successfully')
except Exception as e:
    print(f'Failed to send email: {e}')

```



```
global:
  scrape_interval: 15s
  scrape_timeout: 10s
  evaluation_interval: 15s
alerting:
  alertmanagers:
  - static_configs:
    - targets: []
    scheme: http
    timeout: 10s
    api_version: v1
scrape_configs:
- job_name: prometheus
  honor_timestamps: true
  scrape_interval: 15s
  scrape_timeout: 10s
  metrics_path: /metrics
  scheme: http
  static_configs:
  - targets:
    - localhost:9090

- job_name: 'ZPL_MONITOR'
    static_configs:
      - targets: ['10.83.150.217:10050']
      - targets: ['10.83.188.233:10050']
      - targets: ['10.83.184.202:10050']
      - targets: ['10.83.151.218:10050']


- job_name: 'YAMAL_ORION_MONITOR'
    static_configs:
      - targets: ['10.69.183.37:10050']

- job_name: 'MEGION_MONITOR'
    static_configs:
      - targets: ['10.22.4.49:10050']

- job_name: 'KAFKA_MONITOR'
    static_configs:
      - targets: ['10.82.47.151:10050']

- job_name: 'KAFKA_MONITOR_OSNOV'
    static_configs:
      - targets: ['10.82.47.170:10050']

- job_name: 'KAFKA_MONITOR_VOSTOK'
    static_configs:
      - targets: ['10.82.47.170:10051']

- job_name: 'KAFKA_MONITOR_MESH'
    static_configs:
      - targets: ['10.82.47.170:10052']

- job_name: 'KAFKA_MONITOR_ZPLDO'
    static_configs:
      - targets: ['10.82.47.170:10053']

- job_name: 'KAFKA_MONITOR_MRNGDO'
    static_configs:
      - targets: ['10.82.47.170:10054']


#  - job_name: 'ZPL_10.83.188.233_(WIN-VE0N6EVQAS7)_MONITOR'
#    static_configs:
#      - targets: ['10.83.188.233:10050']

#  - job_name: 'ZPL_10.83.184.202_(WIN-VE0N6EVQAS7)_MONITOR'
#    static_configs:
#      - targets: ['10.83.184.202:10050']

#  - job_name: 'ZPL_10.83.151.218_(WIN-VE0N6EVQAS7)_MONITOR'
#    static_configs:
#      - targets: ['10.83.151.218:10050']




 

- job_name: 'NNG_10.8.2.243_(NNG-NBK01-ASODUCASH)_MONITOR'
    static_configs:
      - targets: ['10.8.2.243:10050']

alerting:
  alertmanagers:
  - scheme: http
    static_configs:
    - targets: 
      - 'alertmanager:9093'

#  - job_name: 'nginx'
#    scrape_interval: 10s
#    static_configs:
#      - targets: ['nginxexporter:9113']

#  - job_name: 'aspnetcore'
#    scrape_interval: 10s
#    static_configs:
#      - targets: ['eventlog-proxy:5000', 'eventlog:5000']

```



```
11-19T05:57:25.600Z caller=main.go:549 level=error msg="Error loading config (--config.file=/etc/prometheus/prometheus.yml)" file=/etc/prometheus/prometheus.yml err="parsing YAML file /etc/prometheus/prometheus.yml: yaml: line 23: did not find expected key"

11-19T05:57:29.120Z caller=main.go:549 level=error msg="Error loading config (--config.file=/etc/prometheus/prometheus.yml)" file=/etc/prometheus/prometheus.yml err="parsing YAML file /etc/prometheus/prometheus.yml: yaml: line 23: did not find expected key"

11-19T05:57:31.177Z caller=main.go:549 level=error msg="Error loading config (--config.file=/etc/prometheus/prometheus.yml)" file=/etc/prometheus/prometheus.yml err="parsing YAML file /etc/prometheus/prometheus.yml: yaml: line 23: did not find expected key"

11-19T05:57:33.642Z caller=main.go:549 level=error msg="Error loading config (--config.file=/etc/prometheus/prometheus.yml)" file=/etc/prometheus/prometheus.yml err="parsing YAML file /etc/prometheus/prometheus.yml: yaml: line 23: did not find expected key"

11-19T05:57:35.385Z caller=main.go:549 level=error msg="Error loading config (--config.file=/etc/prometheus/prometheus.yml)" file=/etc/prometheus/prometheus.yml err="parsing YAML file /etc/prometheus/prometheus.yml: yaml: line 23: did not find expected key"

11-19T05:57:36.880Z caller=main.go:549 level=error msg="Error loading config (--config.file=/etc/prometheus/prometheus.yml)" file=/etc/prometheus/prometheus.yml err="parsing YAML file /etc/prometheus/prometheus.yml: yaml: line 23: did not find expected key"

11-19T05:57:39.692Z caller=main.go:549 level=error msg="Error loading config (--config.file=/etc/prometheus/prometheus.yml)" file=/etc/prometheus/prometheus.yml err="parsing YAML file /etc/prometheus/prometheus.yml: yaml: line 23: did not find expected key"

11-19T05:57:44.084Z caller=main.go:549 level=error msg="Error loading config (--config.file=/etc/prometheus/prometheus.yml)" file=/etc/prometheus/prometheus.yml err="parsing YAML file /etc/prometheus/prometheus.yml: yaml: line 23: did not find expected key"

```




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
