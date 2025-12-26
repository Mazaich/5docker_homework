# 5docker_homework

Домашнее задание к занятию 5. «Практическое применение Docker» - Машаев Роман

Задача 0
Убедитесь что у вас НЕ(!) установлен docker-compose, для этого получите следующую ошибку от команды docker-compose --version
Command 'docker-compose' not found, but can be installed with:

sudo snap install docker          # version 24.0.5, or
sudo apt  install docker-compose  # version 1.25.0-1

See 'snap info docker' for additional versions.
В случае наличия установленного в системе docker-compose - удалите его.
2. Убедитесь что у вас УСТАНОВЛЕН docker compose(без тире) версии не менее v2.24.X, для это выполните команду docker compose version


ОТВЕТ:

![Скриншот выполнения](https://github.com/Mazaich/5docker_homework/blob/main/Screenshot_2025-12-25_22_37_00.png?raw=true)

Задача 1
Сделайте в своем GitHub пространстве fork репозитория.

Создайте файл Dockerfile.python на основе существующего Dockerfile:

Используйте базовый образ python:3.12-slim
Обязательно используйте конструкцию COPY . . в Dockerfile
Создайте .dockerignore файл для исключения ненужных файлов
Используйте CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "5000"] для запуска
Протестируйте корректность сборки
(Необязательная часть, *) Изучите инструкцию в проекте и запустите web-приложение без использования docker, с помощью venv. (Mysql БД можно запустить в docker run).

ОТВЕТ:

![Скриншот выполнения](https://github.com/Mazaich/5docker_homework/blob/main/Screenshot_2025-12-25_23_16_15.png?raw=true)
![Скриншот выполнения](https://github.com/Mazaich/5docker_homework/blob/main/Screenshot_2025-12-26_00_01_48.png?raw=true)


Задача 2 (*)
Создайте в yandex cloud container registry с именем "test" с помощью "yc tool" . Инструкция
Настройте аутентификацию вашего локального docker в yandex container registry.
Соберите и залейте в него образ с python приложением из задания №1.
Просканируйте образ на уязвимости.
В качестве ответа приложите отчет сканирования.

ОТВЕТ:
![Скриншот выполнения](https://github.com/Mazaich/5docker_homework/blob/main/Screenshot_2025-12-26_00_34_05.png?raw=true)
![Скриншот выполнения](https://github.com/Mazaich/5docker_homework/blob/main/Screenshot_2025-12-26_01_08_20.png?raw=true)
![Скриншот выполнения](https://github.com/Mazaich/5docker_homework/blob/main/Screenshot_2025-12-26_01_09_01.png?raw=true)


Задача 3
Изучите файл "proxy.yaml"
Создайте в репозитории с проектом файл compose.yaml. С помощью директивы "include" подключите к нему файл "proxy.yaml".
Опишите в файле compose.yaml следующие сервисы:
web. Образ приложения должен ИЛИ собираться при запуске compose из файла Dockerfile.python ИЛИ скачиваться из yandex cloud container registry(из задание №2 со *). Контейнер должен работать в bridge-сети с названием backend и иметь фиксированный ipv4-адрес 172.20.0.5. Сервис должен всегда перезапускаться в случае ошибок. Передайте необходимые ENV-переменные для подключения к Mysql базе данных по сетевому имени сервиса web
db. image=mysql:8. Контейнер должен работать в bridge-сети с названием backend и иметь фиксированный ipv4-адрес 172.20.0.10. Явно перезапуск сервиса в случае ошибок. Передайте необходимые ENV-переменные для создания: пароля root пользователя, создания базы данных, пользователя и пароля для web-приложения.Обязательно используйте уже существующий .env file для назначения секретных ENV-переменных!
Запустите проект локально с помощью docker compose , добейтесь его стабильной работы: команда curl -L http://127.0.0.1:8090 должна возвращать в качестве ответа время и локальный IP-адрес. Если сервисы не стартуют воспользуйтесь командами: docker ps -a  и docker logs <container_name> . Если вместо IP-адреса вы получаете информационную ошибку --убедитесь, что вы шлете запрос на порт 8090, а не 5000.
Подключитесь к БД mysql с помощью команды docker exec -ti <имя_контейнера> mysql -uroot -p<пароль root-пользователя>(обратите внимание что между ключем -u и логином root нет пробела. это важно!!! тоже самое с паролем) . Введите последовательно команды (не забываем в конце символ ; ): show databases; use <имя вашей базы данных(по-умолчанию example)>; show tables; SELECT * from requests LIMIT 10;.
Остановите проект. В качестве ответа приложите скриншот sql-запроса.


ОТВЕТ:
![Скриншот выполнения](https://github.com/Mazaich/5docker_homework/blob/main/Screenshot_2025-12-26_02_14_30.png?raw=true)


Задача 4
Запустите в Yandex Cloud ВМ (вам хватит 2 Гб Ram).
Подключитесь к Вм по ssh и установите docker.
Напишите bash-скрипт, который скачает ваш fork-репозиторий в каталог /opt и запустит проект целиком.
Зайдите на сайт проверки http подключений, например(или аналогичный): https://check-host.net/check-http и запустите проверку вашего сервиса http://<внешний_IP-адрес_вашей_ВМ>:8090. Таким образом трафик будет направлен в ingress-proxy. Трафик должен пройти через цепочки: Пользователь → Internet → Nginx → HAProxy → FastAPI(запись в БД) → HAProxy → Nginx → Internet → Пользователь
(Необязательная часть) Дополнительно настройте remote ssh context к вашему серверу. Отобразите список контекстов и результат удаленного выполнения docker ps -a
Повторите SQL-запрос на сервере и приложите скриншот и ссылку на fork.


ОТВЕТ:

![Скриншот выполнения](https://github.com/Mazaich/5docker_homework/blob/main/Screenshot_2025-12-26_03_38_20.png?raw=true)
![Скриншот выполнения](https://github.com/Mazaich/5docker_homework/blob/main/Screenshot_2025-12-26_03_39_52.png?raw=true)
![Скриншот выполнения](https://github.com/Mazaich/5docker_homework/blob/main/Screenshot_2025-12-26_03_44_19.png?raw=true)
![Скриншот выполнения](https://github.com/Mazaich/5docker_homework/blob/main/Screenshot_2025-12-26_04_20_52.png?raw=true)

Сыылка на fork: https://github.com/Mazaich/shvirtd-example-python.git

Задача 6
Скачайте docker образ hashicorp/terraform:latest и скопируйте бинарный файл /bin/terraform на свою локальную машину, используя dive и docker save.
Предоставьте скриншоты действий .


ОТВЕТ:
 
![Скриншот выполнения](https://github.com/Mazaich/5docker_homework/blob/main/Снимок%20экрана%20от%202025-12-26%2005-23-05.png?raw=true)
![Скриншот выполнения](https://github.com/Mazaich/5docker_homework/blob/main/Снимок%20экрана%20от%202025-12-26%2005-23-16.png?raw=true)
![Скриншот выполнения](https://github.com/Mazaich/5docker_homework/blob/main/Снимок%20экрана%20от%202025-12-26%2005-26-54.png?raw=true)


Задача 6.1
Добейтесь аналогичного результата, используя docker cp.
Предоставьте скриншоты действий .

ОТВЕТ:
![Скриншот выполнения](https://github.com/Mazaich/5docker_homework/blob/main/Screenshot_2025-12-26_16_46_04.png?raw=true)


 Можно создать новый образ, который в качестве промежуточного шага копирует нужный файл из целевого, а затем извлечь этот файл из нового образа.


Задача 6.2 (**)
Предложите способ извлечь файл из контейнера, используя только команду docker build и любой Dockerfile.
Предоставьте скриншоты действий .

![Скриншот выполнения](https://github.com/Mazaich/5docker_homework/blob/main/Screenshot_2025-12-26_17_13_45.png?raw=true)
![Скриншот выполнения](https://github.com/Mazaich/5docker_homework/blob/main/Screenshot_2025-12-26_17_15_10.png?raw=true)


