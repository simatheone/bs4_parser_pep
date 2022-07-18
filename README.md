# PEP Parser Project :male_detective:

## Оглавление
- [PEP Parser Project :male_detective:](#pep-parser-project-male_detective)
  - [Оглавление](#оглавление)
  - [Дополнительные библиотеки](#дополнительные-библиотеки)
  - [Структура проекта](#структура-проекта)
  - [Документация парсера](#документация-парсера)
  - [Описание проекта](#описание-проекта)
  - [Запуск проекта](#запуск-проекта)

## Дополнительные библиотеки
:stew: BeautifulSoup, :bar_chart: tqdm, :calendar: prettytable, :card_index_dividers: requests-cache

## Структура проекта
```
bs4_parser_pep
 ├── src/
     ├── __init__.py
     ├── configs.py
     ├── constants.py
     ├── exceptions.py
     ├── main.py
     ├── outputs.py
     └── utils.py
 ├── tests/
 ├── .flake8
 ├── .gitignore
 ├── README.md
 ├── pytest.ini
 └── requirements.txt
```

## Документация парсера
```
Парсер документации Python

positional arguments:
  {whats-new,latest-versions,download,pep}
                        Режимы работы парсера

optional arguments:
  -h, --help            show this help message and exit
  -c, --clear-cache     Очистка кеша
  -o {pretty,file}, --output {pretty,file}
                        Дополнительные способы вывода данных
```

## Описание проекта
Парсер собирает иформацию сайта документации Python ```https://docs.python.org/3/``` и каталога с PEP'ми ```https://peps.python.org/```.

Парсера поддерживает 4 режима работы и 3 мода вывода результатов работы.

Режимы работы:
- ```whats-new```
- ```latest-version```
- ```download```
- ```pep```

Моды по выводу итогов парсинга:
- ```-o pretty``` - вывод результатов в консоль в виде таблицы;
- ```-o file``` - вывод результатов в виде **.csv** файла, который сохраняется в директорию ***/results***;
- без указания команды по выводу результатов, итоги выводтся в консоль в строчку.

Дополнительные опциональные аргументы можно узнать из [Документации парсера](#документация-парсера) или вызвать файл ```main.py``` c аргументом ```-h```.

Вся информация по парсингу пишется в логи, уровни **INFO** и **ERROR**.
RotatingFileHandler подчищает устаревшие логи: ```maxBytes=10 ** 6```, ```backupCount=5```.

---
<details><summary>Подробнее о режимах работы парсера:</summary>
<p>

Режим работы ```whats-new``` сканирует страницу ```https://docs.python.org/3/```, раздел ***"Docs by version"***, и собирает ссылки на каждую версию ***Python***. Далее сканирует карточку каждой версии ***Python*** и выводит информацию: ссылка на статью, заголовок, редактор, автор.

```
Пример:

+----------------------------------------------+---------------------------+------------------------------------------------+
| Ссылка на статью                             | Заголовок                 | Редактор, Автор                                |
+----------------------------------------------+---------------------------+------------------------------------------------+
| https://docs.python.org/3/whatsnew/3.10.html | What’s New In Python 3.10 |  Release 3.10.5  Date July 18, 2022  Editor ...|
| ...                                          | ...                       |  ...                                           |
+----------------------------------------------+---------------------------+------------------------------------------------+
```
---

Режим работы ```latest-version``` сканирует страницу ```https://docs.python.org/3/```, раздел ***"Docs by version"*** и выводит информацию о **Python**: ссылку на документацию, версия и статус.

```
Пример:

+--------------------------------------+--------------+----------------+
| Ссылка на документацию               | Версия       | Статус         |
+--------------------------------------+--------------+----------------+
| https://docs.python.org/3.12/        | 3.12         | in development |
| ...                                  | ...          |  ...           |
| https://docs.python.org/3.9/         | 3.9          | stable         |
| ...                                  | ...          |  ...           |
+--------------------------------------+--------------+----------------+
```
---
Режим работы ```download``` сканирует страницу ```https://docs.python.org/3/download.html``` и скачивает PDF-файл документации zip-архивом. Архив сохраняется в директорию ***/downloads***.

---
Режим работы ```pep``` сканирует страницу ```https://peps.python.org/```, собирает статусы всех **PEP**'ов, ссылки на каждый **PEP** и подсчитывает общее количество **PEP**'ов.
Так как статусы на общей странице **PEP**'ов различаются со статусом в карточке каждого **PEP**'a, парсер дополнительно проходит по карточке каждого **PEP**'a и собирает его статус, параллельно сравнивая со статусом из общей таблицы с **PEP**'ми. Если статусы различиются, то информация записывается в логи, уровень **INFO**.

```
Пример:

Несовпадающие статусы:
https://www.python.org/dev/peps/pep-number
Статус в карточке: Superseded
Ожидаемые статусы: ['Accepted', 'Active']
```
---
</p>
</details>


## Запуск проекта
1. Клонировать репозиторий:
```bash
git clone https://github.com/Simatheone/bs4_parser_pep.git
```

2. Создать виртуальное окружение:
```bash
python3 -m venv venv
```

3. Активировать виртуальное окружение и установить зависимости из ```requirements.txt```:
```bash
source venv/bin/activate
```

```bash
pip install -r requirements.txt
```

4. Запустить ```main.py``` и ознакомиться с [документацией парсера](#документация-парсера):
```bash
python3 main.py -h
```
