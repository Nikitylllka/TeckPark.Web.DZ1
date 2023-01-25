# Домашнее задание 3

## Проектирование модели данных

Целью домашнего задания является проектирование модели базы данных, наполнение ее тестовыми данными и и отображение этих данных на сайте. Таким образом будет создана read-only версия сайта.

### 1. Проектирование модели
Используя Django Models спроектировать схему вашего приложения. Создать модели для основных сущностей: вопрос, ответ, тег, профиль пользователя, лайк. Важно использовать правильные типы данных и проставить необходимые связи ([ForeignKey](https://docs.djangoproject.com/en/4.1/ref/models/fields/#foreignkey)) в моделях. Для вопросов создать свой Model Manager, в котором определить типичные выборки (“лучшие” и “новые” вопросы).

Поскольку мы будем использовать встроенную авторизацию Django (**django.contrib.auth**) - у нас будет готовая модель пользователя (**django.contrib.auth.models.User**), но необходимо создать дополнительную модель (Profile) в которую разместить дополнительные поля, например avatar. Модель Profile и User должны быть связаны один к
одному.

### 2. ModelManager и методы моделей
В коде view не должно быть кода бизнес логики (анти-паттерн fat controller). Все методы по выборке и сортировке данных - в ModelManager. Вся логика по выводу данных, например построение полного пути к вопросу - в методы модели. Пагинация - в отдельную функцию. Вьюшки должны состоять из 4-6 строк.

### 3. Наполнение данными
Для удобства дальнейшей разработки нужно наполнить базу тестовыми данными. Для этого нужно создать скрипт (а лучше это сделать в виде команды приложения - Management Command), в котором добавить необходимые объекты в базу. При добавлении удобно использовать уже созданные модели, а не писать сырой SQL.

Обратите внимание, что при указании связи в моделях можно использовать как id, так и объекты на которые вы ссылаетесь:
```Python
u = User.objects.get(pk=1)
q = Question(author=u, text='wut?') # ссылка на объект
q.save()
a = Answer(question_id=1, text='blabla') # ссылка по id
a.save()
```
В базе данных в любом случае будут хранится поля вида author_id и question_id.

Требования к объему данных:
- Пользователи > 10 000.
- Вопросы > 100 000.
- Ответы > 1 000 000.
- Тэги > 10 000.
- Оценки пользователей > 2 000 000.

**Внимание!** Management command должна быть вида: 
```Bash
python manage.py fill_db [ratio]
```
Где num_users — коэффициент заполнения сущностей. Соответственно, после применения команды в базу должно быть добавлено:
 - пользователей — равное ratio;
 - вопросов — ratio * 10;
 - ответы — ratio * 100;
 - тэгов - ratio;
 - оценок пользователей - ratio * 200;

### 4. Отображение данных
Разработать view для отображения следующих страниц:

- Список новых вопросов;
- Список “лучших” вопросов;
- Список вопросов по тегу;
- Страница 1 вопроса со списком ответов.

Для страниц не существующих объектов нужно возвращать 404 страницу. В коде view (контроллеров) не должно быть дублирующегося кода. Использовать функцию пагинации созданную на предыдущем занятии.

### 5. Использование СУБД
В качестве СУБД по умолчанию в Django используется SQLite. Это встраиваемая СУБД, которая представляет собой файл в каталоге с вашим Django-проектом. Такое решение удобно для отладки, но не может использоваться "в продакшене".

Требуется отказаться от стандартной СУБД SQLite в пользу иной реляционной СУБД на ваш выбор: MySQL или PostgreSQL. Для этого необходимо развернуть локально инстанс СУБД на вашей машине и внести небольшие изменения в конфигурацию проекта Django. См. документацию по ссылкам ниже.

### 6. Баллы

#### Максимальные баллы за ДЗ - 16 баллов

Проектирование модели - 5:

- правильные адекватные типы данных и внешние ключи - 1;
- своя модель пользователя - 1;
- таблицы тегов, лайков - 1;
- query-set'ы для типовых выборок: новые вопросы, популярные, по тегу - 2.

Наполнение базы тестовыми данными - 3:

- скрипт для наполнения данными - 1;
- использование django management commands - 1;
- соблюдение требований по объему данных - 1.

Отображение списка вопросов - 3:

- список новых вопросов - 1;
- список популярных - 1;
- список вопросов по тегу - 1.

Отображение страницы вопроса - 3:

- общее - 3.

Использование СУДБ - 2:

- MySQL или PostgreSQL - 2.

### 7. Полезные ссылки
- Туториал по [проектированию моделей в Django](https://docs.djangoproject.com/en/4.1/intro/tutorial02/);
- Подробная [документация по моделям](https://docs.djangoproject.com/en/4.1/topics/db/models/);
- Скрипты - [Management Commands](https://docs.djangoproject.com/en/4.1/howto/custom-management-commands/).
- Подключение MySQL к Django - [Databases - MySQL](https://docs.djangoproject.com/en/4.1/ref/databases/#mysql-notes)
- Как поднять MySQL на своей машине с помощью Docker - [Basic Steps for MySQL Server Deployment with Docker](https://dev.mysql.com/doc/mysql-installation-excerpt/8.0/en/docker-mysql-getting-started.html)
