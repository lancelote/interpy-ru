# Однострочники

В этой главе я продемонстрирую несколько однострочных команд, которые могут
быть очень полезны.

## Простой веб-сервер

Когда-нибудь хотели быстро передать файл по сети? Тогда вам повезло. В Python
есть такая возможность. Перейдите в директорию, которую хотите расшарить по
сети, и наберите следующую команду в терминале:

```bash
# Python 2
python -m SimpleHTTPServer

# Python 3
python -m http.server
```

## Аккуратный вывод в консоль

Вы можете выводить списки и словари аккуратно отформатированными в консоль. Вот нужный код:

```python
from pprint import pprint

my_dict = {'name': 'Yasoob', 'age': 'undefined', 'personality': 'awesome'}
print(dir(my_dict))
# ['__add__', '__class__', '__contains__', '__delattr__', ..., 'sort']

pprint(dir(my_dict))
# ['__add__',
#  '__class__',
#  '__contains__',
#  '__delattr__',
#  '__delitem__',
#  ...
#  'sort']
```

Это особенно удобно для вложенных словарей. Помимо этого, если вам нужно вывести содержимое JSON файла в терминал в удобочитаемом формате, то:

```bash
cat file.json | python -m json.tool
```

## Профилирование скрипта

Это может быть очень полезно для определения узких мест производительности ваших
программ:

```bash
python -m cProfile my_script.py
```

Примечание: `cProfile` это ускоренная реализация `profile`, написанная на C.

## CSV в JSON

Выполните следующую команду:

```bash
python -c "import csv,json;print json.dumps(list(csv.reader(open('csv_file.csv'))))"
```

Не забудьте заменить `csv_file.csv` на желаемое имя файла.

## Сглаживание списка

Вы можете легко и просто сгладить список, содержащий вложенные списки с помощью
`itertools.chain.from_iterable` из пакета `itertools`. Вот простой пример:

```python
a_list = [[1, 2], [3, 4], [5, 6]]
print(list(itertools.chain.from_iterable(a_list)))
# Вывод: [1, 2, 3, 4, 5, 6]

# или
print(list(itertools.chain(*a_list)))
# Вывод: [1, 2, 3, 4, 5, 6]
```

## Однострочные конструкторы

Позволяют избежать больших кусков повторяющегося кода при инициализации класса:

```python
class A(object):
    def __init__(self, a, b, c, d, e, f):
        self.__dict__.update({k: v for k, v in locals().items() if k != 'self'})
```

Другие подобные однострочники можно найти на [официальном сайте](https://wiki.python.org/moin/Powerful%20Python%20One-Liners).
