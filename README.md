# Домашнее задание к занятию «Работа с данными (DDL/DML)» - Кощеев Иван

### Задание 1
1.1. Поднимите чистый инстанс MySQL версии 8.0+. Можно использовать локальный сервер или контейнер Docker.

```

docker run --name mysql8 -e MYSQL_ROOT_PASSWORD=root -p 3306:3306 -d mysql:8


```
B подключимся к mysql


```

sudo docker exec -it mysql8 mysql -u root -p

```



1.2. Создайте учётную запись sys_temp. 

```

CREATE USER 'sys_temp'@'localhost' IDENTIFIED BY 'password';

```

1.3. Выполните запрос на получение списка пользователей в базе данных. (скриншот)

```

SELECT user, host FROM mysql.user;

```

![image1](https://github.com/SirSeoPro/11-02/blob/main/1.png)

1.4. Дайте все права для пользователя sys_temp. 

```

GRANT ALL PRIVILEGES ON *.* TO 'sys_temp'@'localhost' WITH GRANT OPTION;
FLUSH PRIVILEGES;

```

1.5. Выполните запрос на получение списка прав для пользователя sys_temp. (скриншот)

```

SHOW GRANTS FOR 'sys_temp'@'localhost';

```

![image2](https://github.com/SirSeoPro/11-02/blob/main/2.png)

1.6. Переподключитесь к базе данных от имени sys_temp.

Для смены типа аутентификации с sha2 используйте запрос: 
```sql
ALTER USER 'sys_test'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
```
1.6. По ссылке https://downloads.mysql.com/docs/sakila-db.zip скачайте дамп базы данных.

Скачаем архив, разархивируем. </br>

1.7. Восстановите дамп в базу данных.

Выполним, в sql:

```

CREATE DATABASE sakila;
USE sakila;

```

Вернёмся в bash. И выполним: 

```

cat /home/ivon/mysql/sakila-db/sakila-schema.sql | docker exec -i mysql8 mysql -u sys_temp -p'password' sakila
cat /home/ivon/mysql/sakila-db/sakila-data.sql | docker exec -i mysql8 mysql -u sys_temp -p'password' sakila


```

1.8. При работе в IDE сформируйте ER-диаграмму получившейся базы данных. При работе в командной строке используйте команду для получения всех таблиц базы данных. (скриншот)

![image3](https://github.com/SirSeoPro/11-02/blob/main/3.png)

![image5](https://github.com/SirSeoPro/11-02/blob/main/5.png)

*Результатом работы должны быть скриншоты обозначенных заданий, а также простыня со всеми запросами.*


### Задание 2
Составьте таблицу, используя любой текстовый редактор или Excel, в которой должно быть два столбца: в первом должны быть названия таблиц восстановленной базы, во втором названия первичных ключей этих таблиц. Пример: (скриншот/текст)
```
Название таблицы | Название первичного ключа
customer         | customer_id
```
### Ответ:

```
SELECT TABLE_NAME, COLUMN_NAME
FROM information_schema.KEY_COLUMN_USAGE
WHERE CONSTRAINT_NAME = 'PRIMARY'
AND TABLE_SCHEMA = 'sakila';

```

![image4](https://github.com/SirSeoPro/11-02/blob/main/4.png)
