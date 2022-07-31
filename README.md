# ursip
Analyzing TestEntity

- Класс TestEntity, который предполагается использовать в качестве сущности БД, представлен в виде класса данных (data class), такой тип класса не рекомендуется использовать в качестве Entity (ввиду особенностей компилятора Котлин). Я сделаю замену на обычный класс.

- В наименовании класса необходимо избегать постфикса Entity. Это не несет никакой смысловой нагрузки. По другим явным признакам итак будет ясно, что этот класс нужен для работы с БД. Поэтому в качестве наименования оставлю просто 'Test'.

- Тип данных String для поля documentDate не соответствуют наименованию. Если речь идет о дате, то нужно заменить на LocalDateTime.

- Не до конца понятно предназначение поля sortOrder. Возможно, имеется в виду некий тумблер: включено/выключено - тогда бы нужно было заменить тип на Boolean. А возможно имеется в виду режим сортировки: ASC, DESC - тогда я бы вынес это в отдельный класс как отдельный тип.
	
- Id-шники заявлены как UUID, но нужно ли их определять с таким типом? Они более 'тяжелые', и также, например, если необходимо будет сделать ту же сортировку (по id), то в случае с UUID результат будет сомнительный.

- Непонятно для чего заявлено поле testId, ведь есть же id, которое уже идентифицирует запись. Удалю testId.

- Поле testName нужно перенести в первичный конструктор, сделать свойством класса, для удобства инициализации объекта.

- Не указана стратегия генерации первичного ключа, т.е. нужно самому генерить ид, что неудобно.

- БД может состоять из множества схем. Поэтому можно указать схему для этой таблицы (хотя технически это необязательно).

- Само название сущности очень абстрактное, не понятно что оно отображает и как соотносится с полями, которые имеют более конкретные названия. 
1. Хочется вынести в отдельные таблицы такие сущности как Document и Dictionary (они имеют свои id-шники) и настроить связь между Test и ими. По моим рассуждениям поля document, dictionary - это не атрибуты класса Test. Возникают вопросы к чему будет относится поле sortOrder в таком случае (вероятнее, к Dictionary), также класс Document будет выглядеть неполным, имея только одно поле с датой, кроме того не ясно какую связь надо настраивать.
3. Или же тут имелось в виду стратегия наследования с единой таблицей (вся иерархическая структура в одном классе).
2. Или же id-шники Dictionary и Document - лишние, и остальные поля - часть Test'а. 
Так как нет понимания и ответов на все эти вопросы, но есть убеждение, что поля dictionary и document - не являются частью исследуемого класса, я просто их удалю, таким образом класс будет состоять из полей id и testName. 