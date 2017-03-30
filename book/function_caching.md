# Кэширование функций

Кэширование функций позволяет кэшировать возвращаемые значения функций в
зависимости от аргументов. Это может помочь сэкономить время при работе
с вводом/выводом на повторяющихся данных. До Python 3.2 мы должны были бы
написать собственную реализацию. В Python 3.2+ появился декоратор `lpu_cache`,
который позволяет быстро кэшировать возвращаемые функцией значения.

Давайте посмотрим, как мы можем воспользоваться кэшированием в Python 3.2+ и
в более старых версиях.

## Python 3.2+

Реализуем функцию расчета n-ого числа Фибоначчи с использованием `lru_cache`:

```python
from functools import lru_cache

@lru_cache(maxsize=32)
def fib(n):
    if n < 2:
        return n
    return fib(n - 1) + fib(n - 2)

>>> print([fib(n) for n in range(10)])
# Вывод: [0, 1, 1, 2, 3, 5, 8, 13, 21, 34]
```

Аргумент `maxsize` сообщает `lru_cache` сколько последних значений
запоминать.

Мы также можем легко очистить кэш:

```python
fib.cache_clear()
```

## Python 2+

В старых версиях языка также есть несколько путей достижения схожего эффекта.
Вы можете сами реализовать любой тип кэширования. Это зависит только от ваших
потребностей. Вот базовое решение:

```python
from functools import wraps

def memoize(function):
    memo = {}
    @wraps(function)
    def wrapper(*args):
        if args in memo:
            return memo[args]
        else:
            rv = function(*args)
            memo[args] = rv
            return rv
    return wrapper

@memoize
def fibonacci(n):
    if n < 2: return n
    return fibonacci(n - 1) + fibonacci(n - 2)

fibonacci(25)
```

[Отличная статья](https://www.caktusgroup.com/blog/2015/06/08/testing-client-side-applications-django-post-mortem/)
от Caktus Group, в которой они рассказывают историю бага в Django, который
появился из-за `lru_cache`. Рекомендую к ознакомлению.
