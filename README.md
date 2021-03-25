# test-task
The backend test task for "ЭС-АЙ Безопасность"
# 0
* GET-запрос отправляется через URL, а POST-запрос через тело запроса, следовательно данные более защищены
* PUT-запрос используется для обновления данных. При PUT-запросе, в отличие от POST, результат будет всегда одинаковым при идентичных данных
* Delete - запрос на удаление данных
* 301 код ответа - ресурс был перемещен, возможно в ответе будет присутствовать новый URL
* Cookies - фрагмент данных, который отправляется сервером браузеру на хранение и последующую отправку обратно на сервер.
  В основном используются для персонализации, корзины, логинов и т.д.
  Сессии применяются для отслеживания состояния между браузером и сервером. Отличие между cookies и сессиями в том, что cookies полностью
  хранятся в браузере, а при сессиях в cookies хранится только идентификатор сессии, вся информация хранится на сервере. Также cookies в 
  браузере пользователя хранится безвременно, а сессии по умолчанию 15 мин.

# 1
https://ya.ru (https - протокол, в отличие от http имеет шифрование, ya.ru - host, доменное имя)
1. Браузер должен найти ip сервера, на котором находится сайт, на основе host он ищет:
    * в истории подключений, в браузере
    * в ОС в файле hosts
    * в кэше роутера
    * отправляет запрос к DNS-серверам
    * для http-запроса - 80 порт, для https - 443 порт
2. Когда сервер найден, браузер пытается установить соединение в большинстве случаев путём TCP протокола.
   Браузер и сервер обмениваются специальными запросами:
    1. браузер отправляет запрос на установку соединения с браузером - SYN-пакет
    2. сервер отвечает запросом подтверждение получения пакета - SYN/ACK-пакет
    3. браузер отправляет запрос с подтверждением - ACK-пакет
    * Т.к. мы используем протокол https, то устанавливается доверенное соединение путем handshaking, обменом сертификатами.
    Соединение установлено.
3. Браузер формирует HTTP-запрос на получение нужного контента, который состоит из:
    1. Метода запроса (GET, POST, DELETE, PUT, HEAD и т.д.)
    2. Пути к необходимому ресурсу
    3. Версии HTTP-протокола
    4. Имя host, т.к. на одном сервере могут находиться несколько веб-серверов, разделенных по host
    5. Другие заголовки вида ключ-значение
    6. Тело запроса
    GET / HTTP/1.0
    host: ya.ru
    ...
4. Сервер обрабатывает запрос
5. Сервер отправляет ответ браузеру, который состоит из:
    1. Версии HTTP-протокола
    2. Кода ответа (100 - информационные, 200 - все ок, 300 - перенаправление, 400 - ошибка на стороне клиента, 500 - ошибка на стороне сервера)
    3. Краткое описание кода состояния
    4. Другие заголовки вида ключ-значение
    5. Тело запроса
    HTTP/1.0 200 OK
    ...
6. Браузер обрабатывает ответ и рендерит веб-страницу.
   Сначала только основную структуру HTML-страницы, затем путем дополнительных GET-запросов картинки, таблицы стилей, картинки и т.д.

# 2
    import random
    arr = [num for num in range(0, 20)]
    random.shuffle(arr)

    print(arr)
    gt_num = []
    num = input("Input your num: ")
    num = int(num)
    arr = filter(lambda x: x > num, arr)
    arr = list(arr)
    arr = arr[:3]
    print(arr)


# 3
    phrase = "Taking my dog for a walk is fun"
    phrase = phrase.lower()
    phrase = set(phrase)
    phrase = filter(lambda x: x not in "aeoui ", phrase)
    phrase = list(phrase)
    print (phrase)

# 4
    my_dict = {"one": 1, "two": 2, "three": 3, "four": [4]}
    my_dict = my_dict.items()
    my_dict = list(my_dict)
    my_dict = map(lambda x: (str(x[1]), x[0]), my_dict)
    my_dict = list(my_dict)
    my_dict = {x[0]:x[1] for x in my_dict}
    print(my_dict)

# 5
if __name__=="__main__"
Эта конструкция служит для разделения кода, который отрабатывает при запуске самого файла
и который отрабатывает при импортировани файла для использования его функций отдельно

# 6
Декораторы - это функции, которые меняют поведение функций без внутреннего вмешательства в них.
Они удобны в случае, когда, например, 30-ти функциям необходимо добавить какой-нибудь общий функционал.

# 7
Второй вариант, т.к. строки матрицы, созданной первым вариантом, будут ссылаться на один и тот же список,
т.е. эти строки будут одной и той же строкой. Замена элемента в одной строке заменит соответствующий элемент
во всех строках.

# 8
    class Executor(models.Model):
        tasks = models.ManyToManyField("Task")

    Executor.objects.filter(id=5).first().tasks.filter(status="closed", exec_date__range=["2018-03-01", "2018-03-10"])

# 9
В Postgres реализованы 3 уровня изоляци:
  1. Read Committed (по умолчанию): запрос в транзакции видит "снимок" данных до начала запроса. 
  Защита от чтения незафиксированных данных. Два последовательных запроса могут видеть разные данные, 
  если между ними другая транзакция закомитит изменения.
  2. Запрос в Repeatable read, в отличие от Read Committed, видит "снимок" данных до начала транзакции.
  Два последовательных запроса видят одинаковые данные. 
  3. Serializable - самый строгий уровень изоляции. Тразакции выполняются как бы последовательно, а не параллельно.

Задача блокировок - упорядочить конкурентный доступ к данным.
  1. Блокировки на уровне таблиц. Например режим ACCESS SHARE автоматически задаётся при выполнении SELECT.
  Он конфликтует только с режимом ACCESS EXCLUSIVE. Пока выполняется транзакция с определенным режимом блокировки,
  другие транзакции с конфликтующими режимиами будут ожидать её завершения.
  2. Блокировки на уровне строк ограниичивают только запись в строки, но не влияют на выборку.
  3. Рекомендательные блокировки

# 10
Транзакция - это набор команд, объединенных в одну операцию, и если что-то помешает завершить транзакцию,
ни один результат не сохранится в БД.

# 11
Хранимая процедура - набор последовательных запросов. Необходима для удобства и улучшения производительности повторяемых задач.

    CREATE TABLE person_cash (
      person_id int PRIMARY KEY,
      cash int
    );

    CREATE TABLE transactions (
      transaction_id SERIAL PRIMARY KEY,
      person_id int,
      fund int
    );

    CREATE FUNCTION person_cash_update() RETURNS TRIGGER AS $simple_trigger$
    BEGIN
    UPDATE person_cash SET cash = cash + NEW.fund WHERE person_cash.person_id = NEW.person_id;
    RETURN NULL;
    END;
    $simple_trigger$ LANGUAGE plpgsql;

    CREATE TRIGGER simple_trigger 
    AFTER INSERT ON transactions
    FOR EACH ROW
    EXECUTE PROCEDURE person_cash_update();

    insert into person_cash values (1, 15), (2, 0);
    select * from person_cash;

    insert into transactions (person_id, fund) values (1, 15), (2, 40);
    select * from transactions;
    
# 12
"///////////////////////"

# 13
    import requests
    import re

    url = "https://www.tut.by/"
    r = requests.get(url)
    domain = '.'.join(url.split('.')[-2:])
    if domain[-1] == "/":
        domain = domain[:-1]
    pattern = re.compile(r'<a [\w\"\=]* ?href="(https?:\/\/)([\w\.-]+)(\/[\w\.\?&\=-]*\/?)*') 
    lst = [''.join(x) for x in re.findall(pattern, r.text)]
    lst = filter(lambda x: domain not in x.split("/")[2], lst)
    lst = list(lst)
    print(lst)

# 14
    def my_func_1(num):
        return {"1": 2, "2": 1}.get(str(num))

    def my_func_2(num):
        return num ^ 3

    print(my_func_1(2))
    print(my_func_2(2))

# 15
    class Books(models.Model):
        authors = models.ManyToManyField("Author")

    Books.objects.annotate(total_authors=Count("authors")).filter(total_authors__lt=3)

# 16
Стек, очередь, связанный список, двоичное дерево поиска, множества, словари

# 17
В университете работал с MYSql, SqlServer, SQLlite. Postgres более гибкий и имеет больше возможностей.
Redis - NOSql БД хранящая данные в памяти под видом ключ-значение.
Нормализация БД - это её упрощение, повышение производительности и удобства использования.
Основные нормальные формы:
 1. Первая нормальная форма. Нет повторяющихся строк, нет массивов, данные атомарны.
 2. Вторая нормальная форма. Таблица в первой нормальной форме, есть первичный ключ,
все неключевые столбцы должны зависеть только от первичного ключа.
 3. Третья нормальная форма. Таблица во второй нормальной форме, все неключевые столбцы зависят
от первичного ключа нетранзитивно.
 4. Нормальная форма Бойса-Кодда. Таблица в третьей нормальной форме и нет зависимости составного
первичного ключа от неключевых столбцов.

# 18
Пишу для себя сайт магазин настольных игр на Django, также ознакамливался с Flask.
ORM - Object-Relational Mapping - технология, которая связывает БД с языками программирования. 

# 19
Если это два счета одного банка, то одной транзакцией спишется с одного счета деньги и другому зачислится сумма.
Если несколько транзакций, то первая наложит блокировку на данные, а остальные будут ожидать ее завершения.
Если это счета разных банков, то фиксируется разница в корреспондентских счетах банков. Все сообщения
попадают в систему, которая отслеживает все платежи и затем в определенные сроки рассчитывает нетто-сумму,
которую каждый из банков должен отдать другому банку. После этого они проводят между собой определенные операции.

# 20
    def round_func(cash):
        if not isinstance(cash, int):
            raise ValueError('Enter the right amount of cash')
        min_val = 50
        bill_num = cash // min_val
        cash_rest = cash - bill_num * min_val
        return {"The quantity of bills": bill_num, "The remain of cash": cash_rest}

    print(round_func(149))

# 21
Был опыт работы с github. Просматривал команды GIT. Удаление коммита - (git reset --hard коммит). Rebase не сохраняет историю коммитов,
а merge сохраняет и при слиянии веток они выстраиваются в нужную последовательность.
