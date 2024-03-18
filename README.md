# Node.js AND Postgres. Rest API		
		
## Для реализации проекта:		
### Установка на ПК node.js		
### Установка на ПК PostgreSQL		
### Установка на ПК Postman		
### npm install express		
### npm install pg		
### npm install -D nodemon		
		
## 1	Создание основного файла.	
	1	Создаем файл index.js.
	2	Импортируем express.
	3	Зададим порт (можно из process || статический).
	4	Из express создадим сам сервер.
	5	Подключим слушатель на нашем порту.
		
## 2	Скрипты для старта приложения.	
	1	В файле package.json пишем скрипт для запуска через node (start).
	2	В файле package.json пишем скрипт для запуска через nodemon (dev).
		
## 3	Запрос для проверки.	
	1	Создать get для проверки работоспособности сервера.
		
## 4	Для подключения к Базе Данных.	
	1	Создаем файл db.js.
	2	Получаем poll из установленного пакета pg.
	3	Создаем объект этого класса.
	4	Экспортируем объект класса.
		
## 5	Настраиваем объект класса Poll.	
	1	Первое поле - это пользователь от которого мы будем подключатся к БД (user).
	2	Далее пароль, который вы указывали при создании (password).
	3	Далее указываем хост. В нашем случае - это локальная машина (host).
	4	Далее указываем порт. По умолчанию при установке он указывается 5432 (port).
	5	Последнее свойство - это название базы данных(database). (его пока у нас нет, запишем, когда создадим БД).
		
## 6	Создание Базы Данных.	
	1	Открыть powershell и перейти по адресу установленного postgres в папку bin.
	2	командой .\psql --version убеждаемся что postgresql установлен.
	3	Заходим в СУБД (.\psql -U postgres).
	4	Вводим пароль от пользователя.
	5	Создать новую БД node_postgres_practic (create database node_postgres_practic).
	6	Подключаемся к Базе Данных (\connect node_postgres_practic).
	7	Переключаем кодировку на 1251 (psql \! chcp 1251).
	8	Теперь же мы можем добавить в параметры pool database: "node_postgres_practic".
		
## 7	Для большего понимания можно создать схему сущностей проекта.	
	1	Создать схему с сущностями можно на сайте draw.io.
	2	Создать две сущности person и post.
	3	Создать поля сущностей person (id, name, surname).
	4	Создать поля сущностей post (id, title, content, user_id).
	5	Указать связь таблиц (один-ко-многим).
		
## 8	Создание таблиц сущностей в Базе Данных.	
	1	Создать файл database.sql.
	2	Написать запрос на создание таблицы person.
	3	Написать запрос на создание таблицы post.
	4	Сделать запрос на создание таблицы person в powershell.
	5	Сделать запрос на создание таблицы post в powershell.
		
## 9	Запросы для person.	
	1	Создадим папку controller для работы с запросами.
	2	Создадим файл user.controller.js.
	3	Создадим класс внутри которого будем определять методы class UserController.
	4	Первый метод будет создание пользователя (createUser).
	5	Вторая функция будет возвращать клиенту всех пользователей (getUsers).
	6	Третья функция будет возвращать конкретного пользователя по id (getOneUser).
	7	Четвертая функция будет обновлять данные о пользователе по id (updateUser).
	8	Пятая функция будет удалять пользователя по id (deleteUser).
	9	экспортировать ОБЪЕКТ данного контроллера.
		
## 10	Маршруты для person.	
	1	Создадим папку routes для работы с маршрутами, по которым надо будет отправлять запросы.
	2	Создадим файл user.routes.js.
	3	Получаем из express роутер.
	4	Создаем объект этого класса.
	5	Добавим экспорт нашей переменной router.
	6	Импортируем объект controller.
	7	Для каждой из функций определим маршрут по которому она будет отрабатывать.
	8	Первая функция - это функция создания пользователя (post запрос).
	9	Далее у нас будет два get запроса: получить всех пользователей и получить одного пользователя.
	10	Put запрос для обновления данных.
	11	И delete запрос для удаления пользователя. Так же тут мы можем через двоеточие указать параметр.
		
## 11	Регистрируем роутер.	
	1	Переходим в файл index.js.
	2	Удаляем get запрос, который был для проверки.
	3	Далее у app воспользуемся функцией use в которую первым параметром передаем url по которому этот роутер будет обрабатывать ("/api"), вторым параметром передаем уже сам роутер.
	4	Указываем expressб что нужно будет распаршивать json строку (app.use(express.json())).
		
## 12	Реализация функций user.controller.	
	1	Начнем с функции createUser.
	2	С помощью деструктуризацией получим из тела запроса имя и фамилию (req.body).
	3	затем выводим в логи и ответом на клиент вернем просто ok (res.json("ok")).
	4	Проверить через postman.
		
## 13	Реализуем записать пришедших данных в базу данных.	
	1	В файле user.controller.js импортируем db который мы создавали ранее.
	2	В функции createUser после получения данных из тела создаем нового пользователя.
	3	Создаем нового пользователя с помощью db, вызовем функцию query с sql запросом в параметре (INSERT INTO person (name, surname) values ($1, $2) RETURNING *, [1, 2]).
	4	Ответом на клиент вернем пользователя, а точнее только данные которые находятся в rows[0].
	5	Проверить через postman.
		
## 14	Реализация функции получения всех пользователей getUsers.	
	1	Создаем новую переменную в которую мы будем помещать пользователей и так же пишем запрос.
	2	На клиент будем возвращать поле rows.
	3	Проверить через postman.
		
## 15	Реализация функции получение конкретного пользователя getOneUser.	
	1	Получаем id из параметров запроса (req.params.id).
	2	С помощью запроса выделяем пользователя из db.
	3	И на клиент вернем этого пользователя.
	4	Проверить через postman.
		
## 16	Реализация функции изменения пользователя по id. Функция updateUser.	
	1	Получаем из запроса id новое имя name и новую фамилию surname.
	2	Далее пишем запрос (UPDATE person set name = $1, surname = $2 where id = $3 RETURNING *).
	3	И возвращаем на клиент результат запроса.
	4	Проверить через postman.
		
## 17	Реализация функции удалениz пользователя deleteUser.	
	1	Узнаем id пользователя из запроса.
	2	Создаем запрос на удаление (DELETE FROM person WHERE id = $1).
	3	Возвращаем результат.
	4	Проверить через postman.
		
## 18	Файлы для работы с post.	
	1	Создаем файл post.controller.js.
	2	В нем создаем класс PostController.
	3	Экспортируем новый объект этого класса.
	4	Создаем две функции для создания и получения createPost и getPostsByUser.
		
## 19	Создаем файл для post.routers.	
	1	Импортировать Router из express.
	2	Cоздать новый объект класса Router.
	3	Импортировать postController.
	4	Создать post запрос для создания постов.
	5	Создать get запрос для получения постов пользователя.
	6	Экспортировать роутер.
	7	Регистрируем этот роутер в index.js файле.
	8	Запускаем метод use у app для postRouter.
		
## 20	Реализации функций post.controller.js.	
	1	При создании файла из тела функции достаем title и content.
	2	Делаем запрос на создание поста.
	3	Возвращаем на клиент созданный пост.
	4	Создадим несколько постов с помощью postman.
	5	Проверить через postman.
		
## 21	Реализация функции для получения всех постов пользователя.	
	1	Получаем id клиента из запроса.
	2	Выбираем все посты с помощью запроса.
	3	Возвращаем на клиент эти посты.
	4	Проверить через postman.
