Структура данных ``set``
------------------------

``set`` это весьма полезная структура данных. ``set`` схож по поведению
со списком, за тем исключением, что множества не могут содержать одинаковые
значения. Это бывает очень кстати в определенных случаях. К примеру, вам может
потребоваться проверить список на наличие дубликатов. Существует два типичных
метода. Первый - использовать цикл ``for``. Как-то так:

.. code:: python

    some_list = ['a', 'b', 'c', 'b', 'd', 'm', 'n', 'n']

    duplicates = []
    for value in some_list:
        if some_list.count(value) > 1:
            if value not in duplicates:
                duplicates.append(value)

    print(duplicates)
    # Output: ['b', 'n']

Но есть и более простое решение при помощи множества. Вы можете сделать
что-нибудь такое:

.. code:: python

    some_list = ['a', 'b', 'c', 'b', 'd', 'm', 'n', 'n']
    duplicates = set([x for x in some_list if some_list.count(x) > 1])
    print(duplicates)
    # Output: set(['b', 'n'])

У множеств также есть несколько дополнительных методов. Вот некоторые
из них:

**Пересечение**

Вы можете найти пересечение двух множества. Например:

.. code:: python

    valid = set(['yellow', 'red', 'blue', 'green', 'black'])
    input_set = set(['red', 'brown'])
    print(input_set.intersection(valid))
    # Output: set(['red'])

**Разность**

Разность двух множеств можно найти следующим методом:

.. code:: python

    valid = set(['yellow', 'red', 'blue', 'green', 'black'])
    input_set = set(['red', 'brown'])
    print(input_set.difference(valid))
    # Output: set(['brown'])

Множества также можно создавать с помощью новой нотации:

.. code:: python

    a_set = {'red', 'blue', 'green'}
    print(type(a_set))
    # Output: <type 'set'>

Существуют и другими методы. Рекомендую ознакомится с ними в официальной
документации.
