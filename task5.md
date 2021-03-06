## Задача 5
### Дедлайн: понедельник 11 марта (12:00)

**Ветка:** task5

## Для работы с GitHub API установить: https://packagist.org/packages/knplabs/github-api

### Новые таблицы (добавить миграции и модели)
1. github_users

    1. username - string - имя пользователя на github например. KnpLabs

2. github_reposiories

    1. github_id integer - id репозитория на github
    2. github_user_id - foreign key (github_users.id)
    3. name - название репозитория ex. github-api
    4. description string
    5. private boolean
    6. language string

3. github_issues
    1. id
    2. github_id integer - id этой issue на гитхабе
    3. repository_id - foreign key (github_reposiories.id)
    4. title - string
    5. number - integer
    6. state - string

### Подзадачи:
1.  Установить knplabs/github-api

# Список роутов (результат)

1. GET     api/v1/github/{userName}/{repositoryName}/issues

### Параметры (+ валидация) - * обязательные параметры
1. fromDb      - boolean
1. page     - string  номер страницы
2. perPage  - integer кол-во элементов на страницу от 1 до 10


**Пример ответа:**
```
    {
      "success": true,
      "data": {
        "issues": {
            [
                "id": 1,
                "github_id": 230038500,
                "repository_id": 1,
                "number": 590,
                "title": "Native support for the retry plugin",
                "state": "closed"
            ],
            ...
        }
      }
    }
```

2. GET     api/v1/github/{userName}/repositories

### Параметры (+ валидация) - * обязательные параметры
1. fromDb      - boolean 
2. page     - string  номер страницы
3. perPage  - integer кол-во элементов на страницу от 1 до 10

**Пример ответа:**
```
    {
      "success": true,
      "data": {
        "repositories": {
            [
                "id": 1, // id в базе
                "github_id": 230038500,
                "name": "bad-jokes",
                "description" => "A React Native toy project (Hackathon #7)."
            ],
            ...
        }
      }
    }
```

3. GET     api/v1/github/{userName}/issues/search

### Параметры (+ валидация) - * обязательные параметры
1. fromDb   - boolean 
2. page     - string  номер страницы 
3. perPage  - integer кол-во элементов на страницу от 1 до 10

Параметры для поиска:

4. title - string - LIKE %title% поиск
5. state - string - поиск по state
6. number integer поиск по number 

### ! Искать можно по всем параметрам одновременно

**Пример ответа:**
```
    {
      "success": true,
      "data": {
        "issues": {
            [
                "id": 1,
                "github_id": 230038542,
                "repository_id": 1,
                "number": 590,
                "title": "Native support for the retry plugin",
                "state": "closed"
            ],
            ...
        }
      }
    }
```

4. GET     api/v1/github/{userName}/repositories/search

### Параметры (+ валидация) - * обязательные параметры
1. fromDb   - boolean 
2. page     - string  номер страницы 
3. perPage  - integer кол-во элементов на страницу от 1 до 10

Параметры для поиска:

4. title - string - LIKE %title% поиск
5. private - boolean
6. language - string LIKE %language% поиск

### ! Искать можно по всем параметрам одновременно

**Пример ответа:**
```
    {
      "success": true,
      "data": {
        "repositories": {
            [
                "id": 1, // id в базе
                "github_id": 230038500,
                "name": "bad-jokes",
                "description" => "A React Native toy project (Hackathon #7)."
                "private": false,
                "language": "PHP"
            ],
            ...
        }
      }
    }
```

## Требования к роутам
1. Во всех роутах должен этой домашки должен быть параметр:
fromDB - boolean если он = true - вы должны выдать результат в том же формате, только достать из базы
если нет - делаете запрос к Github API, пишете результат в базу и затем вывод
2. Выносите валидацию в реквесты
https://laravel.com/docs/master/validation#form-request-validation
3. Если параметр для поиска не передан - делать выборку любых значений

## Полезные ссылки
1. Документация пакета: 
https://github.com/KnpLabs/php-github-api/tree/master/doc
https://github.com/KnpLabs/php-github-api/blob/master/doc/issues.md
2. https://laravel.com/docs/5.7/collections

### Доп инфа
1. Для удобной работы с этим API и данными под ваши задачи вам скорее всего придется написать модуль \ несколько модулей (просто класс в app\Support добавьте =) ).
Ограничений никаких нет - старайтесь поменьше кода дублировать и всегда думайте как ваш код потом будет разбирать другой человек, который проекта не знает.
Итог цикл всегда будет такой: 1. получение \ обновление данных если необходимо 2. фильтрация данных 3. вывод
2. Все запросы к внешним API (GitHub в нашем случае) - должны быть обернуты в try catch

# Вопросы \ Ответы
### если опять  fromDB false после записи в базу мы снова обращаемся к ней что бы получить данные или сразу выводим те которые туда записали? (глупо конечно, но так с пагинацией проще будет)
нет
### вот это требование "Во всех роутах должен этой домашки должен быть параметр: fromDB - boolean если он = true - вы должны выдать результат в том же формате, только достать из базы если нет - делаете запрос к Github API, пишете результат в базу и затем вывод" Если fromDB false то мы делаем запрос к github api и пишем в базу, но если в базе уже есть такие данные, то мы не записываем в бд и сразу выдаем результат из бд или выводим ошибку или дописываем одинаковые записи в бд?
Если в базе уже есть нужные данные - запрос делать не надо
### Там в последних двух запросах, нужно искать issue и репозитории по параметрам, но в самом поиске гитхаба нету поиска например по number у issue, а у репозиториев нету поиска по "LIKE %language%" (по аналогии title in:title). Я так понимаю надо получать результаты поиска по параметрам для которых предусмотрен поиск и потом вручную фильтровать результаты?
так тоже можно, если лишние данные окажутся в базе - не страшно. Еще можно фильтовать массивы в объекте которые приходит с API, перед тем как писать в базу
### тоесть если fromDb = false, то поиск с помощью github search api по заданной строке поиска ( например для поиска репозиториев "user:$userName is:$private) и для тех параметров которых не предусмотрен поиск ("LIKE %language%") дополнительно фильтровать, и если найденных данных нету в базе записать в нее, так?
да
