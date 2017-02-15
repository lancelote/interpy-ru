# Collections

В стандартную библиотеку Python входит модуль `collections` содержащий
дополнительные структуры данных. Мы поговорим о некоторых и обсудим их пользу.

А конкретно:

- `defaultdict`
- `OrderedDict`
- `counter`
- `deque`
- `namedtuple`
- `enum.Enum` (вне модуля; Python 3.4+)

## `defaultdict`

Я использую `defaultdict` время от времени. В отличие от `dict` нам не
нужно проверять существует ли ключ в словаре или нет. В результате мы можем
писать следующий код:

```python
from collections import defaultdict

colours = (
    ('Yasoob', 'Yellow'),
    ('Ali', 'Blue'),
    ('Arham', 'Green'),
    ('Ali', 'Black'),
    ('Yasoob', 'Red'),
    ('Ahmed', 'Silver'),
)

favourite_colours = defaultdict(list)

for name, colour in colours:
    favourite_colours[name].append(colour)

print(favourite_colours)

# Вывод:
# defaultdict(<type 'list'>,
#    {'Arham': ['Green'],
#     'Yasoob': ['Yellow', 'Red'],
#     'Ahmed': ['Silver'],
#     'Ali': ['Blue', 'Black']
# })
```

Другим популярным случаем использования `defaultdict` является добавление
элементов в список внутри словаря. Если ключ не существует в словаре, то вы
упрётесь в `KeyError`. `defaultdict` позволяет обойти эту проблему
аккуратным образом. Для начала, позвольте привести пример использования
`dict` с исключением `KeyError`, а затем мы посмотрим на пример с
`defaultdict`.

**Проблема:**

```python
some_dict = {}
some_dict['colours']['favourite'] = "yellow"
# Вызывает KeyError: 'colours'
```

**Решение:**

```python
import collections
tree = lambda: collections.defaultdict(tree)
some_dict = tree()
some_dict['colours']['favourite'] = "yellow"
# Работает без ошибок
```

Вы можете вывести в консоль `some_dict` используя `json.dumps`. Вот пример:

```python
import json
print(json.dumps(some_dict))
# Вывод: {"colours": {"favourite": "yellow"}}
```

## `OrderedDict`

`OrderedDict` сохраняет элементы в порядке добавление в словарь. Иизменение
значения ключа не изменяет его позиции. При этом удаление и повторное
добавление перенесет ключ в конец словаря.

**Проблема:**

```python
colours = {"Red": 198, "Green": 170, "Blue": 160}
for key, value in colours.items():
    print(key, value)
# Вывод:
#   Red 198
#   Blue 160
#   Green 170
#
# Элементы выводятся в произвольном порядке
```

**Решение:**

```python
from collections import OrderedDict

colours = OrderedDict([("Red", 198), ("Green": 170), ("Blue": 160)])
for key, value in colours.items():
    print(key, value)
# Вывод:
#   Red 198
#   Green 170
#   Blue 160
#
# Порядок элементов сохранен
```

## `counter`

`Counter` позволяет подсчитывать частоту определенных элементов. К примеру,
мы можем использовать его, чтобы посчитать сколько любимых цветов у каждого
человека:

```python
from collections import Counter

colours = (
    ('Yasoob', 'Yellow'),
    ('Ali', 'Blue'),
    ('Arham', 'Green'),
    ('Ali', 'Black'),
    ('Yasoob', 'Red'),
    ('Ahmed', 'Silver'),
)

favs = Counter(name for name, colour in colours)
print(favs)
# Вывод: Counter({
#    'Yasoob': 2,
#    'Ali': 2,
#    'Arham': 1,
#    'Ahmed': 1
# })
```

Мы также можем посчитать частоту строк в файле. Пример:

```python
with open('filename', 'rb') as f:
    line_count = Counter(f)
print(line_count)
```

## `deque`

`deque` предлагает нам двустороннюю очередь, которая позволяет добавлять
и удалять элементы с обеих сторон. Для начала, вам нужно импортировать
модуль `deque` из библиотеки `collections`:

```python
from collections import deque
```

Теперь мы можем создать экземпляр двусторонней очереди:

```python
d = deque()
```

Очередь работает подобно списку в Python и имеет схожие методы. Например, вы
можете:

```python
d = deque()
d.append('1')
d.append('2')
d.append('3')

print(len(d))
# Вывод: 3

print(d[0])
# Вывод: '1'

print(d[-1])
# Вывод: '3'
```

Мы можем отрезать элементы с обеих сторон очереди:

```python
d = deque(range(5))
print(len(d))
# Вывод: 5

d.popleft()
# Вывод: 0

d.pop()
# Вывод: 4

print(d)
# Вывод: deque([1, 2, 3])
```

Мы также можем ограничить число элементов, которые может хранить очередь.
Таким образом при достижении максимального числа элементов очередь начнет
отрезать элементы с другого конца. Это проще объяснить на примере:

```python
d = deque(maxlen=30)
```

Теперь, когда мы попытаемся добавить 31-й элемент - очередь отрежет первый элемент
с другого конца. Вы также можете добавлять элементы к очереди с обоих концов:

```python
d = deque([1,2,3,4,5])
d.extendleft([0])
d.extend([6,7,8])
print(d)
# Вывод: deque([0, 1, 2, 3, 4, 5, 6, 7, 8])
```

## `namedtuple`

Вы уже должны быть знакомы с кортежами. Кортеж в Python это неизменяемый
список, который позволяет хранить объекты, разделенные запятой. Они практически
идентичны спискам, за исключением нескольких важных особенностей. В отличие от
списков, **вы не можете изменить элемент кортежа**. В то же время
вы можете обращаться к элементам кортежа по индексам:

```python
man = ('Ali', 30)
print(man[0])
# Вывод: Ali
```

Отлично, так что же тогда `namedtuples`? Этот модуль открывает доступ к
удобной структуре данных для простых задач. С помощью именованных кортежей вам
не обязательно использовать индексы для обращения к элементам кортежа. Вы
можете думать об именованных кортежах как о словарях, но в отличие от словарей
они неизменяемы.

```python
from collections import namedtuple

Animal = namedtuple('Animal', 'name age type')
perry = Animal(name="perry", age=31, type="cat")

print(perry)
# Вывод: Animal(name='perry', age=31, type='cat')

print(perry.name)
# Вывод: 'perry'
```

Теперь вы можете видеть, что мы можем обращаться к элементам именованного кортежа при
помощи их имени и `.` (точки). Давайте чуть подробнее на этом остановимся.
Именованный кортеж имеет два обязательных аргумента. Это имя самого кортежа и
имена полей кортежа. В примере выше имя нашего кортежа `Animal`, имена
полей соответственно: `name`, `age` и `type`. Именованный кортеж позволяет
создавать **само-документированные** кортежи. Вы сможете легко понять код
при первом же взгляде на него. И, поскольку вы не привязаны к индексам, у вас
открывается больше возможностей по поддержке своего кода. Помимо этого,
**именованные кортежи не создают словари для каждого экземпляра**, они легковесны и
не требуют больше памяти чем обычные кортежи. Это делает их быстрее словарей.
Тем не менее, помните, что как и в случае с обычными кортежами, **именованный
кортеж неизменяем**. Это означает, что такой код работать не будет:

```python
from collections import namedtuple

Animal = namedtuple('Animal', 'name age type')
perry = Animal(name="perry", age=31, type="cat")
perry.age = 42

# Вывод: Traceback (most recent call last):
#           File "", line 1, in
#        AttributeError: can't set attribute
```

Вы должны использовать именованные кортежи для улучшения читаемости кода.
Они **обратносовместимы с обычными кортежами**. Это значит, что вы можете
использовать численные индексы с именованными кортежами:

```python
from collections import namedtuple

Animal = namedtuple('Animal', 'name age type')
perry = Animal(name="perry", age=31, type="cat")
print(perry[0])
# Вывод: perry
```

И последнее, вы можете сконвертировать именованный кортеж в словарь. Вот так:

```python
from collections import namedtuple

Animal = namedtuple('Animal', 'name age type')
perry = Animal(name="Perry", age=31, type="cat")
print(perry._asdict())
# Вывод: OrderedDict([('name', 'Perry'), ('age', 31), ...
```

## `enum.Enum` (Python 3.4+)

Другой полезной структурой данных является `enum`. Он доступен в модуле
`enum`, начиная с Python 3.4 (также в PyPI как бекпорт под именем `enum34`).
Enums ([перечисляемый тип](https://en.wikipedia.org/wiki/Enumerated_type))
это простой способ организации разных вещей.

Давайте рассмотрим именованный кортеж `Animal` из прошлого примера. У него
есть поле `type`. Проблема в том, что его тип - строка. Это создаёт нам
несколько проблем. Что если пользователь ввёл `Cat`, поскольку нажал Shift?
Или `CAT`? Или `kitten`?

Перечисление может помочь обойти эту проблему, позволив не использовать строки.
Рассмотрим пример:

```python
from collections import namedtuple
from enum import Enum

class Species(Enum):
    cat = 1
    dog = 2
    horse = 3
    aardvark = 4
    butterfly = 5
    owl = 6
    platypus = 7
    dragon = 8
    unicorn = 9
    # Список продолжается...

    # Нам безразличен возраст животного, так что мы используем синонимы
    kitten = 1
    puppy = 2

Animal = namedtuple('Animal', 'name age type')
perry = Animal(name="Perry", age=31, type=Species.cat)
drogon = Animal(name="Drogon", age=4, type=Species.dragon)
tom = Animal(name="Tom", age=75, type=Species.cat)
charlie = Animal(name="Charlie", age=2, type=Species.kitten)

# А теперь несколько тестов
>>> charlie.type == tom.type
True
>>> charlie.type
<Species.cat: 1>
```

Так у нас куда меньше шансов допустить ошибку. При этом мы должны быть
конкретны и использовать только перечисление для определения полей.

Существует три способа получения доступа к перечисляемым элементам. Например,
все три метода, представленные ниже, дадут вам значения поля `cat`:

```python
Species(1)
Species['cat']
Species.cat
```

Это было короткое погружение в библиотеку `collections`. Обязательно
ознакомьтесь с официальной документацией после чтения этой главы.
