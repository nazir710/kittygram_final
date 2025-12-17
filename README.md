#  Kittygram

Kittygram — социальная сеть для обмена фотографиями любимых питомцев.

Функционал: регистрация и авторизация, возможность добавить нового котика на сайт или
изменить существующего, просмотреть записи других пользователей. 

Настроены:
- WSGI-сервер Gunicorn для работы с бэкенд-приложением проекта;
- веб-сервер Nginx для перенаправления запросов и работы со статикой;
- запуск проекта в контейнерах;
- автоматическое тестирование и деплой проекта на удалённый сервер с помощью GitHub
Actions;
- уведомление пользователя с помощью Telegram-бота после завершения деплоя.


## Стек:
- Frontend: HTML, CSS, React
- Backend: Python, Django
- База данных: PostgreSQL
- DevOps: Docker, GitLab


## Описание CI/CD:
Для деплоя проекта на сервер используется GitHub Actions. Деплой происходит в следующем порядке:
- загрузка проекта в GitHub
- загрузка образов на Ваш аккаунт в DockerHub
- загрузка образов из DockerHub на сервер и сборка контейнеров.
Для настройки деплоя необходимо настроить следующие секреты в GitHub Actions:
- DOCKER_PASSWORD - пароль от аккаунта в DockerHub
- DOCKER_USERNAME - имя пользователя в DockerHub
- HOST - IP-адрес вашего сервера
- SECRET_KEY - секретный код Джанго проекта
- SSH_KEY - закрытый SSH-ключ для доступа к серверу
- SSH_PASSPHRASE - значение для доступа к серверу
- USER - имя пользователя на сервере


## Команды локального развертывания с Докером:
1) Клонирование репозитория: git clone https://github.com/nazir710/kittygram_final.git
2) Перейдите в директорию проекта: cd kittygram_final
3) Cоздайте виртуальное окружение: python -m venv venv
4) Активируйте виртуальное окружение: source venv/Scripts/activate
5) Установите/обновите пакетный менеджер: python -m pip install --upgrade pip
6) Установите зависимости: pip install -r requirements.txt
7) Заполнените файл окружения .env:
- POSTGRES_USER=<имя пользователя>
- POSTGRES_PASSWORD=<пароль пользователя>
- POSTGRES_DB=<имя базы данных>
- DB_HOST=<имя контейнера>
- DB_PORT=<порт>
- SECRET_KEY=<секретный код Джанго>
- DEBUG=False 
- ALLOWED_HOSTS=<'IP-адрес сервера, 127.0.0.1, localhost, домен'>

## Подъем контейнеров в Докере
Находясь в папке /kittygram_final при активированном виртуальном окружении выполните команду: 
- docker-compose up

## Подготовка базы данных
1) Выполните миграции: 
- docker exec kittygram_backend-1 python manage.py makemigrations
- docker exec kittygram_backend-1 python manage.py migrate 
2) Создайте суперпользователя:
- docker exec kittygram_backend-1 python manage.py createsuperuser

## Сборка статики
В новом терминале в папке /kittygram_final при активированном виртуальном окружении и запущенных контейнерах выполните команды:
- docker compose exec backend python manage.py collectstatic
- docker compose exec backend cp -r /app/collected_static/. /backend_static/static/

## Запуск сервера
1) Подготовьте сервер к загрузке проекта:
Установите на сервер пакетный менеджер и утилиту для создания виртуального окружения:
- sudo apt install python3-pip python3-venv -y
Установите docker и docker compose:
- sudo apt update
- sudo apt install curl
- curl -fSL https://get.docker.com -o get-docker.sh
- sudo sh ./get-docker.sh
- sudo apt install docker-compose-plugin
Создайте папку проекта:
- mkdir foodgram
Перейдите в папку проекта: 
- cd kittygram
Создайте файл окружения и заполните его:
- touch .env
- nano .env
2) При активировнном виртуальном окружении выполните команду: git push
