## Задача 7 - работа с очередями / система оповещений по email
### Дедлайн: четверг (18:00)

**Ветка:** task7

## Для работы вам понядобятся роуты из 2 / 3 / 4 домашки

### Подзадачи:
1.  Зарегистрироваться на https://mailtrap.io/
2.  В .env вписать smtp сервер \ логин \ пароль \ etc. от mailtrap (после регистрации там будут инструкции)
3.  Все ваши письма (на любой email!) должны попадать в ваш inbox в mailtrap
4.  queue driver у вас должен быть - database (поищите в доках laravel вам нужно artisan командой создать специальную миграцию таблицы jobs - там будут храниться данные о ваших очередях) и что бы очередь работала в отдельном терминале выполняете php artisan queue:work --tries=3
5.  Все письма должны отправляться из очереди т.е. вы пушите в очередь Queue::push(new YourEmailJob)

### Вьюшки ваших писем
1. css писать не надо, html будет достаточно - вывести в самом простом виде, главное, чтобы вся нужная информация была

### Структура письма
у вас должен быть базовый template (resources/views/emails/layouts/base) ну и все содержимое в отдельной вьюшке 

## Список роутов в которые нужно добавить email оповещения (+ примеры)
1. POST     api/v0/user/{user}/group

**Заголовок письма: Добавлена новая группа - $groupName**

**Пример содержимого письма:**
```
была добавлена группа: %название группы%
```

2. PATCH    api/v0/users/profile/{userProfile}

**Заголовок письма: Обновлен профиль с id: $profileId**

**Пример содержимого письма:**
```
Обновился профиль с id: %profileId%:
$старое_имя => $новое_имя
```

3. PATCH    api/v1/user/{user}

**Заголовок письма: Обновлены данные пользователя с id: 32**

**Примеры содержимого письма:**
```
Пользователь с email someemail@mail.com обновил информацию пользователя с id: 32
Имя: John Fedor => John Doe
Роль: Администратор => Пользователь
Пользователь забанен!
```
```
Пользователь с email someemail@mail.com обновил информацию пользователя с id: 32
Имя: John Doe => John Fedor
Роль: Пользователь
```
если пользователь не забанен - последнюю строку (Пользователь забанен!) выводить не нужно

## Полезные ссылки
1. https://laravel.com/docs/5.8/mail
2. https://laravel.com/docs/5.8/queues
