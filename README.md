#GB. Базы данных и PyQT

#Урок 1. Полезные модули
1. Написать функцию host_ping(), в которой с помощью утилиты ping будет проверяться доступность сетевых узлов. Аргументом функции является список, в котором каждый сетевой узел должен быть представлен именем хоста или ip-адресом. В функции необходимо перебирать ip-адреса и проверять их доступность с выводом соответствующего сообщения («Узел доступен», «Узел недоступен»). При этом ip-адрес сетевого узла должен создаваться с помощью функции ip_address(). (Внимание! Аргументом сабпроцеса должен быть список, а не строка!!! Крайне желательно использование потоков.)
2. Написать функцию host_range_ping() (возможности которой основаны на функции из примера 1) для перебора ip-адресов из заданного диапазона. Меняться должен только последний октет каждого адреса. По результатам проверки должно выводиться соответствующее сообщение.
3. Написать функцию host_range_ping_tab(), возможности которой основаны на функции из примера 2. Но в данном случае результат должен быть итоговым по всем ip-адресам, представленным в табличном формате (использовать модуль tabulate). Таблица должна состоять из двух колонок и выглядеть примерно так:
Reachable
10.0.0.1
10.0.0.2

Unreachable
10.0.0.3
10.0.0.4

------------------ (факультативно) --------------------------

4. *Продолжаем работать над проектом «Мессенджер»:
a) Реализовать скрипт, запускающий два клиентских приложения: на чтение чата и на запись в него. Уместно использовать модуль subprocess).
b) Реализовать скрипт, запускающий указанное количество клиентских приложений.

5. *В следующем уроке мы будем изучать дескрипторы и метаклассы. Но вы уже сейчас можете перевести часть кода из функционального стиля в объектно-ориентированный. Создайте классы «Клиент» и «Сервер», а используемые функции превратите в методы классов.

#Урок 2. Дескрипторы и метаклассы

1. Реализовать метакласс ClientVerifier, выполняющий базовую проверку класса «Клиент» (для некоторых проверок уместно использовать модуль dis):
отсутствие вызовов accept и listen для сокетов;
использование сокетов для работы по TCP;
2. Реализовать метакласс ServerVerifier, выполняющий базовую проверку класса «Сервер»:
отсутствие вызовов connect для сокетов;
использование сокетов для работы по TCP.
3. Реализовать дескриптор для класса серверного сокета, а в нем — проверку номера порта. Это должно быть целое число (>=0). Значение порта по умолчанию равняется 7777. Дескриптор надо создать в отдельном классе. Его экземпляр добавить в пределах класса серверного сокета. Номер порта передается в экземпляр дескриптора при запуске сервера.

#Урок 3. Хранение данных в БД. ORM SQLAlchemy

1. Начать реализацию класса «Хранилище» для серверной стороны. Хранение необходимо осуществлять в базе данных. В качестве СУБД использовать sqlite. Для взаимодействия с БД можно применять ORM.
Опорная схема базы данных:
На стороне сервера БД содержит следующие таблицы:
a) клиент:
* логин;
* информация.
b) историяклиента:
* время входа;
* ip-адрес.
c) списокактивныхпользователей :
* idклиента;
* адрес:
* port;
* login_time.

#Урок 4. Хранение данных в БД (продолжение) и основы Qt
1. Продолжить реализацию класса хранилища для серверной стороны.
a) Реализовать функционал работы со списком контактов по протоколу JIM:
Получение списка контактов
Запрос к серверу:

{
"action": "get_contacts",
"time": <unix timestamp>,
"user_login": "login"
}

Положительный ответ сервера будет содержать список контактов:
{
"response": "202",
"alert": "[‘nick_1’, ‘nick_2’,...]"
}
Получение списка контактов — не самая частая операция при взаимодействии с сервером. Она должна выполняться после подключения и авторизации клиента. Инициируется им же. В процессе получения списка контактов клиент не должен инициировать другие запросы.
Добавление/удаление контакта в список контактов
Запрос к серверу:
{
"action": "add_contact" | "del_contact",
"user_id": "nickname",
"time": <unix timestamp>,
"user_login": "login"
}

Ответ сервера будет содержать одно сообщение с кодом результата и необязательной расшифровкой:
{
"response": xxx,
}
b) Реализовать хранение информации в БД на стороне клиента:
* списокконтактов;
* историясообщений.
Реализовать графический интерфейс для мессенджера, используя библиотеку PyQt. Реализовать графический интерфейс администратора сервера:
* отображение списка всех клиентов;
* отображение статистики клиентов;
* настройка сервера (подключение к БД, идентификация).

#Урок 5. Qt (продолжение), Qt и потоки
1. Реализовать графический интерфейс пользователя на стороне клиента:
Отображение списка контактов;
Выбор чата двойным кликом на элементе списка контактов;
Добавление нового контакта в локальный список контактов;
Отображение сообщений в окне чата;
Набор сообщения в окне ввода сообщения;
Отправка введенного сообщения.

#Урок 6. Безопасность
1. Реализовать аутентификацию пользователей на сервере.
2. *Реализовать декоратор @login_required, проверяющий авторизованность пользователя для выполнения той или иной функции.
3. Реализовать хранение паролей в БД сервера (пароли не хранятся в открытом виде — хранится хэш-образ от пароля с добавлением криптографической соли).
4. *Реализовать возможность сквозного шифрования сообщений (использовать асимметричный шифр, ключи которого хранятся только у клиентов).
