markdown
# Проект по фильтрации и сортировке данных

## Описание проекта

Данный проект представляет собой скрипт на Python, который реализует функции для фильтрации и сортировки списка словарей, содержащих информацию о различных элементах, а также функции для работы с банковскими транзакциями. Основные цели проекта:

- Фильтрация данных по ключу `state`.
- Сортировка данных по ключу `date` в формате ISO.
- Фильтрация транзакций по валюте.
- Получение описаний транзакций.
- Генерация номеров банковских карт.

Этот проект может быть полезен при работе с коллекциями данных, где необходимо быстро получать нужные элементы, сортировать их и работать с финансами.

## Установка

Для начала работы с проектом вам потребуется:

1. Установить [Python](https://www.python.org/downloads/) (рекомендуется версия 3.6 и выше).
2. Клонировать репозиторий на локальный компьютер:
```
git clone git@github.com:ваш_пользователь/название_репозитория.git
```
3. Перейдите в директорию проекта

## Использование

### Импорт функций

```
from your_module_name import filter_by_state, sort_by_date, filter_by_currency, transaction_descriptions, card_number_generator
```
### Функции

#### 1. `filter_by_state(data: List[Dict[str, Any]], state: str = "EXECUTED") -> List[Dict[str, Any]]`

**Описание:** Фильтрует список словарей по значению ключа `state`.

**Параметры:**

- `data` (List[Dict[str, Any]]): Список словарей, который нужно фильтровать.
- `state` (str): Значение ключа `state` для фильтрации. По умолчанию `'EXECUTED'`.

**Возвращает:** Новый список словарей, содержащих только те, у которых ключ `state` соответствует заданному значению.

**Пример:**

data = [
{"id": 41428829, "state": "EXECUTED", "date": "2019-07-03T18:35:29.512364"},
{"id": 939719570, "state": "EXECUTED", "date": "2018-06-30T02:08:58.425572"},
{"id": 594226727, "state": "CANCELED", "date": "2018-09-12T21:27:25.241689"},
{"id": 615064591, "state": "CANCELED", "date": "2018-10-14T08:21:33.419441"},
]

executed_items = filter_by_state(data)
print(executed_items)

# Вывод: [{'id': 41428829, 'state': 'EXECUTED', 'date': '2019-07-03T18:35:29.512364'}, {'id': 939719570, 'state': 'EXECUTED', 'date': '2018-06-30T02:08:58.425572'}]

#### 2. `sort_by_date(data: List[Dict[str, Any]], reverse: bool = True) -> List[Dict[str, Any]]`

**Описание:** Сортирует список словарей по дате.

**Параметры:**

- `data` (List[Dict[str, Any]]): Список словарей, который нужно сортировать.
- `reverse` (bool): Параметр для определения порядка сортировки. По умолчанию `True` (убывание). Если `False`, то сортировка в порядке возрастания.

**Возвращает:** Новый список словарей, отсортированный по дате.

**Пример:**

sorted_executed_items = sort_by_date(executed_items)
print(sorted_executed_items)

# Вывод: [{'id': 41428829, 'state': 'EXECUTED', 'date': '2019-07-03T18:35:29.512364'}, {'id': 939719570, 'state': 'EXECUTED', 'date': '2018-06-30T02:08:58.425572'}]

#### 3. `filter_by_currency(transactions: List[Dict], currency: str) -> Generator[Union[Dict, str], None, None]`

**Описание:** Эта функция возвращает итератор, который поочередно выдает транзакции, где валюта операции соответствует заданной. Если транзакции не найдены, возвращает строку "Нет значений".

**Параметры:**
- `transactions`: список словарей с транзакциями.
- `currency`: строка, представляющая нужную валюту.

**Возвращает:** генератор, который выдает либо словарь с транзакцией, либо строку.

**Пример:**

usd_transactions = filter_by_currency(transactions, "USD")
for transaction in usd_transactions:
print(transaction)

#### 4. `transaction_descriptions(transactions: List[Dict]) -> Generator[str, None, None]`

**Описание:** Эта функция принимает список словарей с транзакциями и возвращает описание каждой операции по очереди. Если описание отсутствует, возвращает "Описание отсутствует".

**Параметры:**
- `transactions`: список словарей с транзакциями.

**Возвращает:** генератор строк, содержащих описания транзакций.

**Пример:**

descriptions = transaction_descriptions(transactions)
for description in descriptions:
print(description)

#### 5. `card_number_generator(start: int, stop: int) -> Generator[str, None, None]`

**Описание:** Генератор, который выдает номера банковских карт в формате: `XXXX XXXX XXXX XXXX`. Номера генерируются на основе заданного диапазона.

**Параметры:**
- `start`: начальное значение диапазона (включительно).
- `stop`: конечное значение диапазона (включительно).

**Возвращает:** генератор строк, представляющих номера карт.

**Пример:**

for card_number in card_number_generator(1, 5):
print(card_number)

### Декоратор логирования

В текущей версии проекта был добавлен декоратор под названием `log`, который позволяет автоматически записывать в файл информацию о вызовах функций, их аргументах и возвращаемых значениях. Это может быть полезно для отладки и мониторинга работы программного обеспечения.

#### Как использовать декоратор

Чтобы использовать декоратор `log`, вам нужно импортировать его из модуля `src.decorators` и применить его к вашей функции. Вот пример:
```
from src.decorators import log

@log("test_log.txt")
def divide(x, y):
return x / y
```

#### Описание функциональности

- **Запись вызовов функций:** Декоратор будет записывать в указанный файл (`test_log.txt`) текстовую информацию о каждом вызове функции, включая имя функции, переданные ей аргументы и возвращаемое значение.
  
- **Обработка ошибок:** Если в функции возникает ошибка (например, деление на ноль), декоратор также запишет информацию об ошибке в файл, включая аргументы, с которыми произошла ошибка.

#### Примеры использования

##### Успешный вызов:

```
divide(10, 2)
```

*Запись в файл `test_log.txt`:*

Запуск функции: divide с аргументами: (10, 2), {}
Функция: divide вернула: 5.0

##### Ошибка:

divide(10, 0)


*Запись в файл `test_log.txt`:*


Запуск функции: divide с аргументами: (10, 0), {}
Ошибка в функции: divide с аргументами: (10, 0), {}


### Установка

(Опишите, как установить проект или необходимые зависимости, если это необходимо.)

### Запуск тестов

Для запуска тестов используется `pytest`. Запустите следующие команды в терминале:

```commandline
pytest tests/test_decorators.py
```

## Заключение

Этот проект предоставляет удобные функции для фильтрации и сортировки данных в формате словаря. Он может быть полезен в различных сценариях работы с данными, таких как анализ, обработка и представление информации.

Для предложений и вопросов, пожалуйста, свяжитесь с [ваша_электронная_почта]. 

## Лицензия

Этот проект лицензирован под [Вашей лицензией].

### Примечания:
- Замените `ваш_пользователь` и `название_репозитория` на ваши данные.
- Позаботьтесь о добавлении лицензии, если планируете делиться кодом публично.

# Тестирование

В этом проекте используются юнит-тесты для проверки корректности работы функций.

## Зависимости

Перед запуском тестов убедитесь, что у вас установлены необходимые зависимости. Для этого вам потребуется `pytest`. Установить его можно с помощью следующей команды:
```
pip install pytest
```

## Запуск тестов

1. Убедитесь, что вы находитесь в корневой директории проекта, где расположен файл, который вы хотите протестировать (или где находятся тестовые файлы).
2. Запустите тесты с помощью команды:
```
pytest
```
3. Для запуска тестов с дополнительной информацией и подробным выводом вы можете использовать флаг `-v` (verbose):
```
pytest -v
```
4. Если вы хотите запустить тесты только для конкретного файла, вы можете указать его после команды `pytest`:
```commandline
pytest tests/test_file.py
```
## Результаты тестов

После запуска тестов вы увидите вывод, который покажет, какие тесты прошли, а какие провалились. Если тесты не прошли, обратите внимание на сообщения об ошибках для устранения проблем.

### Примечания:
- Замените `tests/test_file.py` на имя вашего файла с тестами.
- Убедитесь, что ваша структура каталогов и названия файлов соответствуют тем, что вы указали в разделе тестирования.