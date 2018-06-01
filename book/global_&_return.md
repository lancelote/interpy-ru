# Global и Return

Вы, вероятно, встречали функции с ключевым словом `return` в конце. Знаете
что оно означает? В целом то же самое что и в других языках. Давайте
посмотрим на эту маленькую функцию:

```python
def add(value1, value2):
    return value1 + value2

result = add(3, 5)
print(result)
# Вывод: 8
```

Функция выше принимает два значения и возвращает их сумму. Мы также можем
переписать её таким образом:

```python
def add(value1,value2):
    global result
    result = value1 + value2

add(3, 5)
print(result)
# Вывод: 8
```

Как несложно заметить, мы используем глобальную переменную `result`. Что это
означает? К глобальным переменным можно обращаться в том числе и извне области
видимости функции. Позвольте продемонстрировать это следующим примером:

```python
# Сначала без глобальной переменной
def add(value1, value2):
    result = value1 + value2

add(2, 4)
print(result)

# Вот чёрт, мы наткнулись на исключение. Но почему?
# Интерпретатор Python говорит нам, что не существует
# переменной с именем result. Так произошло, поскольку
# переменная result доступна исключительно из функции,
# где она была определена, если переменная не является
# глобальной.
Traceback (most recent call last):
  File "", line 1, in
    result
NameError: name 'result' is not defined

# Попробуем исправить код, сделав result глобальной
# переменной
def add(value1, value2):
    global result
    result = value1 + value2

add(2, 4)
print(result)
# Вывод: 6
```

Во второй раз ошибок быть не должно. В реальной жизни от глобальных переменных
стоит держаться подальше, они только усложняют жизнь, захламляя глобальную
область видимости.

## Возврат нескольких значений

Что если мы хотим вернуть две переменные из функции вместо одной? Есть
несколько способов, которыми пользуются начинающие разработчики. Первый из
них - это использование глобальных переменных. Вот подходящий пример:

```python
def profile():
    global name
    global age
    name = "Danny"
    age = 30

profile()
print(name)
# Вывод: Danny

print(age)
# Вывод: 30
```

> **Примечание:** как я уже писал, данный метод использовать не рекомендуется.
Повторяю, не используйте вышеуказанный метод!

Другим популярным методом является возврат кортежа, списка или словаря с
требуемыми значениями.

```python
def profile():
    name = "Danny"
    age = 30
    return (name, age)

profile_data = profile()
print(profile_data[0])
# Вывод: Danny

print(profile_data[1])
# Вывод: 30
```

Или общепринятое сокращение:

```python
def profile():
    name = "Danny"
    age = 30
    return name, age

profile_name, profile_age = profile()
print(profile_name)
# Вывод: Danny

print(profile_age)
# Вывод: 30
```

Не забывайте - в примере выше возвращается кортеж (несмотря на отсутствие скобок), а не отдельные значения. Если вы хотите пойти на один шаг дальше, то попробуйте использовать [namedtuple](https://docs.python.org/3/library/collections.html#collections.namedtuple). Пример:

```python
from collection import namedtuple

def profile():
    Person = namedtuple('Person', 'name age')
	return Person(name="Danny", age=31)

# Использование как namedtuple
p = profile()
print(p, type(p))
# Person(name='Danny', age=31) <class '__main__.Person'>
print(p.name)
# Danny
print(p.age
# 31

# Использование как простого кортежа
p = profile()
print(p[0])
# Danny
print(p[1])
# 31

# Немедленная распаковка
name, age = profile()
print(name)
# Danny
print(age)
# 31
```

Вместе с возвращением списка или словаря кортежи являются лучшим подходом к решению проблемы. Не используйте глобальные переменные, если точно не уверены в том, что делаете. `global` может быть неплохим вариантом в отдельных редких случаях, но точно не всегда.
