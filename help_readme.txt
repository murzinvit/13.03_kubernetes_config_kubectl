import psycopg2
from psycopg2.extensions import ISOLATION_LEVEL_AUTOCOMMIT

# Устанавливаем соединение с postgres
connection = psycopg2.connect(user="postgres", password="postgres")
connection.set_isolation_level(ISOLATION_LEVEL_AUTOCOMMIT)

# Создаем курсор для выполнения операций с базой данных
cursor = connection.cursor()
sql_create_database = 
# Создаем базу данных
cursor.execute('create database news')
# Закрываем соединение
cursor.close()
connection.close()

ioakim.ru


kubectl expose svc frontend-service --type=NodePort --name=front-svc
kubectl expose svc backend-service --type=NodePort --name=back-svc

psql -U postgres -W

CREATE DATABASE news;

\c

CREATE TABLE news
(
    id SERIAL PRIMARY KEY,
    title CHARACTER VARYING(30),
    short_description CHARACTER VARYING(30),
    description CHARACTER VARYING(30),
    preview CHARACTER VARYING(30)
);

GRANT ALL PRIVILEGES ON DATABASE "news" to postgres;


GRANT ALL PRIVILEGES ON ALL TABLES IN SCHEMA public TO "postgres";


INSERT INTO news (title, short_description, description, preview) VALUES ('name1', 'esc_1', 'pres_1');


pg_dump news -U postgres -W postgres > /tmp/news.dump
