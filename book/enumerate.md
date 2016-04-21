# Enumerate

`enumerate` это встроенная в Python функция. Её пользу не передать одной строкой.
В то же время большинство новичков и многие опытные разработчики не знакомы с
ней. Она позволяет нам итерировать по объекту с параллельным автоматическим
счетчиком. Вот пример:

```python
for counter, value in enumerate(some_list):
    print(counter, value)
```

Это не все. `enumerate` принимает также необязательные аргументы, с помощью
которых она становится еще полезнее.

```python
my_list = ['apple', 'banana', 'grapes', 'pear']
for c, value in enumerate(my_list, 1):
    print(c, value)

# Output:
# 1 apple
# 2 banana
# 3 grapes
# 4 pear
```

Необязательный аргумент позволяет задавать начальное значение счетчика. Вы
также можете создать список кортежей, содержащих индекс и элемент используя
список. Пример:

```python
my_list = ['apple', 'banana', 'grapes', 'pear']
counter_list = list(enumerate(my_list, 1))
print(counter_list)
# Output: [(1, 'apple'), (2, 'banana'), (3, 'grapes'), (4, 'pear')]
```
