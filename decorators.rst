Декораторы
----------

Декораторы - важная часть Python. Если коротко: они являются функциями, которые
изменяют работу других функций. Они помогают делать код короче и более
"питонистичным". Большинство новичков не знает где их использовать, так что
я расскажу о нескольких случаях, когда декораторы помогут написать лаконичный
код.

Для начала рассмотрим как написать свой собственный декоратор.

Это будет самым сложным моментом в теме, поэтому мы будем продвигаться шаг за
шагом, так что вы сможете все полностью понять.

Все в Python является объектом
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Для начала краткая ретроспектива относительно функций в Python:

.. code:: python

    def hi(name="yasoob"):
        return "hi " + name

    print(hi())
    # output: 'hi yasoob'

    # Мы можем присвоить функцию переменной:
    greet = hi
    # Мы не используем здесь скобки, поскольку наша задача не вызвать функцию,
    # а передать её объект переменной. Теперь попробуем запустить

    print(greet())
    # output: 'hi yasoob'

    # Давайте посмотрим что произойдет, если мы удалим оригинальную функцию
    del hi
    print(hi())
    # outputs: NameError

    print(greet())
    # outputs: 'hi yasoob'

Определение функций внутри функций
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Итак, это были основы работы с функциями. Теперь продвинемся на шаг дальше.
В Python разрешено объявлять функции внутри других функций:

.. code:: python

    def hi(name="yasoob"):
        print("now you are inside the hi() function")

        def greet():
            return "now you are in the greet() function"

        def welcome():
            return "now you are in the welcome() function"

        print(greet())
        print(welcome())
        print("now you are back in the hi() function")

    hi()
    # output:now you are inside the hi() function
    #        now you are in the greet() function
    #        now you are in the welcome() function
    #        now you are back in the hi() function

    # Пример демонстрирует, что при вызове hi() вызываются также функции
    # greet() и welcome(). Кроме того, две последние функции недоступны
    # извне hi():

    greet()
    #outputs: NameError: name 'greet' is not defined

Теперь мы знаем, что возможно определять функции внутри других функций. Другими
словами: мы можем создавать вложенные функции. Теперь вам нужно познакомиться
с еще одной возможностью функций: возвращать другие функции.

Возвращение функции из функции
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Нам не обязательно исполнять функцию, определенную внутри другой функции, сразу,
мы можем вернуть её в качестве возвращаемого значения:

.. code:: python

    def hi(name="yasoob"):
        def greet():
            return "now you are in the greet() function"

        def welcome():
            return "now you are in the welcome() function"

        if name == "yasoob":
            return greet
        else:
            return welcome

    a = hi()
    print(a)
    # outputs: <function greet at 0x7f2143c01500>

    # Это наглядно демонстрирует, что переменная `a` теперь указывает на
    # функцию greet() в функции hi().Теперь попробуйте вот это

    print(a())
    # outputs: now you are in the greet() function

Давайте еще раз пробежимся по коду. Через условный оператор мы возвращаем из
функции объекты ``greet`` и ``welcome``, а не ``greet()`` и ``welcome()``.
Почему? Потому что скобки означают вызов функции, без них мы просто передаем сам
объект функции. Достаточно ясно? Давайте я чуть подробнее остановлюсь на
этом. Когда мы пишем ``a = hi()``, функция ``hi()`` иполняется и (поскольку
имя по умолчанию yasoob) возвращается функция ``greet``. Если мы изменим код
на ``a = hi(name="ali")``, то будет возвращена функция ``welcome``. Мы также
можем набрать ``hi()()``, что вернет *now you are in the greet() function*.

Передаем функцию в качестве аргумента другой функции
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: python

    def hi():
        return "hi yasoob!"

    def doSomethingBeforeHi(func):
        print("I am doing some boring work before executing hi()")
        print(func())

    doSomethingBeforeHi(hi)
    # outputs:I am doing some boring work before executing hi()
    #         hi yasoob!

Теперь у нас есть все необходимые знания для изучения работы декораторов.
Декораторы позволяют нам исполнять определенный код до и после исполнения
конкретной фунции.

Пишем наш первый декоратор
^^^^^^^^^^^^^^^^^^^^^^^^^^

В прошлом примере мы по сути уже написали декоратор! Давайте изменим его и
сделаем немного более полезным:

.. code:: python

    def a_new_decorator(a_func):

        def wrapTheFunction():
            print("I am doing some boring work before executing a_func()")

            a_func()

            print("I am doing some boring work after executing a_func()")

        return wrapTheFunction

    def a_function_requiring_decoration():
        print("I am the function which needs some decoration to remove my foul smell")

    a_function_requiring_decoration()
    # outputs: "I am the function which needs some decoration to remove my foul smell"

    a_function_requiring_decoration = a_new_decorator(a_function_requiring_decoration)
    # Теперь функция a_function_requiring_decoration обернута в wrapTheFunction()

    a_function_requiring_decoration()
    # outputs: I am doing some boring work before executing a_function_requiring_decoration()
    #          I am the function which needs some decoration to remove my foul smell
    #          I am doing some boring work after executing a_function_requiring_decoration()

Все ясно? Мы просто использовали принципы, с которыми познакомились выше.
Это то, чем и занимаются декораторы в Python! Они "обертывают" функцию и
модифицируют её поведение определенным образом. Сейчас вы можете спросить
почему мы не используем в коде символ @. Это просто более короткий способ
декорировать функции. Вот как мы можем модифицировать пример выше с
использованием @:

.. code:: python

    @a_new_decorator
    def a_function_requiring_decoration():
        """Hey you! Decorate me!"""
        print("I am the function which needs some decoration to "
              "remove my foul smell")

    a_function_requiring_decoration()
    #outputs: I am doing some boring work before executing a_func()
    #         I am the function which needs some decoration to remove my foul smell
    #         I am doing some boring work after executing a_func()

    # Выражение @a_new_decorator это сокращенная версия следующего кода:
    a_function_requiring_decoration = a_new_decorator(a_function_requiring_decoration)

Надеюсь, теперь у вас есть базовое представление о логике работы декораторов
в Python. Однако, у нашего кода есть одна проблема. Если мы исполним:

.. code:: python

    print(a_function_requiring_decoration.__name__)
    # Output: wrapTheFunction

Мы этого не ожидали! Имя функции должно быть
"a\_function\_requiring\_decoration". В реальности наша функция была
заменена на wrapTheFunction. Она перезаписала имя и строку документации
оригинальной функции. К счастью, Python предоставляет нам простой инструмент
для обхода этой проблемы - ``functools.wraps``. Давайте исправим
предыдущий пример, используя ``functools.wraps``:

.. code:: python

    from functools import wraps

    def a_new_decorator(a_func):
        @wraps(a_func)
        def wrapTheFunction():
            print("I am doing some boring work before executing a_func()")
            a_func()
            print("I am doing some boring work after executing a_func()")
        return wrapTheFunction

    @a_new_decorator
    def a_function_requiring_decoration():
        """Hey yo! Decorate me!"""
        print("I am the function which needs some decoration to "
              "remove my foul smell")

    print(a_function_requiring_decoration.__name__)
    # Output: a_function_requiring_decoration

Так намного лучше. Давайте двигаться дальше и знакомиться с конкретными
вариантами использования декораторов.

**Макет:**

.. code:: python

    from functools import wraps
    def decorator_name(f):
        @wraps(f)
        def decorated(*args, **kwargs):
            if not can_run:
                return "Function will not run"
            return f(*args, **kwargs)
        return decorated

    @decorator_name
    def func():
        return("Function is running")

    can_run = True
    print(func())
    # Output: Function is running

    can_run = False
    print(func())
    # Output: Function will not run

Примечание: ``@wraps`` принимает на вход функцию для декорирования и
добавляет функциональность копирования имени, строки документации, списка
аргументов и т.д. Это открывает доступ к свойствам декорируемой функции из
декоратора.

Варианты использования
~~~~~~~~~~~~~~~~~~~~~~

Теперь давайте рассмотрим области, где декораторы действительно показывают
себя и существенно упрощают работу.

Авторизация
~~~~~~~~~~~

Декораторы могут использоваться в веб-приложениях для проверки авторизации
пользователя, перед тем как открывать ему доступ к функционалу. Они активно
используются в веб-фреймворках Flask и Django. Вот пример проверки авторизации
на декораторах:

**Пример:**

.. code:: python

    from functools import wraps

    def requires_auth(f):
        @wraps(f)
        def decorated(*args, **kwargs):
            auth = request.authorization
            if not auth or not check_auth(auth.username, auth.password):
                authenticate()
            return f(*args, **kwargs)
        return decorated

Журналирование
~~~~~~~~~~~~~~

Журналирования - другая область, в которой декораторы находя широкое применение.
Вот пример:

.. code:: python

    from functools import wraps

    def logit(func):
        @wraps(func)
        def with_logging(*args, **kwargs):
            print(func.__name__ + " was called")
            return func(*args, **kwargs)
        return with_logging

    @logit
    def addition_func(x):
       """Do some math."""
       return x + x


    result = addition_func(4)
    # Output: addition_func was called

Уверен, вы уже думаете о каком-нибудь хитром использовании декораторов.

Декораторы с аргументами
^^^^^^^^^^^^^^^^^^^^^^^^

Тогда подумайте вот о чем, является ли ``@wraps`` также декоратором? Но,
``@wraps`` же принимает аргумент, как нормальная функция. Тогда почему бы нам
не сделать что-то похожее с нашими декораторами?

Когда мы используем синтаксис ``@my_decorator`` мы применяем декорирующую
функцию с декорируемой функцией в качестве параметра. Как вы помните, все в
Python является объектом, в том числе и функции! Помня это, мы можем писать
функции, возвращающие декорирующие функции.

Вложенные декораторы внутри функции
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Давайте вернемся к нашему примеру с журналированием и напишем декоратор, который
позволит нам задавать файл для сохранения логов:

.. code:: python

    from functools import wraps

    def logit(logfile='out.log'):
        def logging_decorator(func):
            @wraps(func)
            def wrapped_function(*args, **kwargs):
                log_string = func.__name__ + " was called"
                print(log_string)
                # Открываем логфайл и записваем данные
                with open(logfile, 'a') as opened_file:
                    # Мы записываем логи в конкретный файл
                    opened_file.write(log_string + '\n')
            return wrapped_function
        return logging_decorator

    @logit()
    def myfunc1():
        pass

    myfunc1()
    # Output: myfunc1 was called
    # Файл out.log создан и содержит строку выше

    @logit(logfile='func2.log')
    def myfunc2():
        pass

    myfunc2()
    # Output: myfunc2 was called
    # Файл func2.log создан и содержит строку выше

Декораторы из классов
~~~~~~~~~~~~~~~~~~~~~

Теперь наш журналирующий декоратор находится на продакшене, однако, когда
отдельные части приложения являются критичными, мы определенно хотим отзываться на
возникающие ошибки как можно быстрее. Давайте предположим, что иногда мы
просто хотим записывать логи в файл, а иногда мы хотим получать сообщения об
ошибках по email, сохраняя логи в тоже время. Это подходящий случай
для использования наследования, однако, до сих пор мы встречали только
декораторы-функции.

К счастью, классы также можно использовать для создания декораторов. Давайте
опробуем эту методику:

.. code:: python

    class logit(object):
        def __init__(self, logfile='out.log'):
            self.logfile = logfile

        def __call__(self, func):
            log_string = func.__name__ + " was called"
            print(log_string)
            # Открываем логфайл и записваем данные
            with open(self.logfile, 'a') as opened_file:
                # Мы записываем логи в конкретный файл
                opened_file.write(log_string + '\n')
            # Отправляем сообщение
            self.notify()

        def notify(self):
            # Только записываем логи
            pass

Такое решение имеет дополнительно преимущество в краткости, в сравнении с
вложенными функциями, при этом синтаксис декорирования функции остается
прежним:

.. code:: python

    @logit()
    def myfunc1():
        pass

Теперь давайте возьмем подкласс ``logit`` и добавим функционал отправки email
(эта тема не будет здесь рассмотрена):

.. code:: python

    class email_logit(logit):
        '''
        Реализация logit для отправки писем администраторам при вызове
        функции
        '''
        def __init__(self, email='admin@myproject.com', *args, **kwargs):
            self.email = email
            super(logit, self).__init__(*args, **kwargs)

        def notify(self):
            # Отправляем письмо в self.email
            # Реализация не будет здесь приведена
            pass


``@email_logit`` будет работать также как и ``@logit``, при этом отправляя
сообщения на почту администратору помимо журналирования.
