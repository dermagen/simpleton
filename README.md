## Описание языка программирования ПРОСТЕЦ


### 1. Формат входного текста программы

Входной текст программы состоит из блоков кода, которые могут быть предварены, разделены 
и завершены произвольными сопроводительными комментариями. Начало блока кода сигнализирует 
строка, начинающаяся с трех знаков тильды (`~~~`) за которой идет название языка заглавными 
буквами (`ПРОСТЕЦ`) и, факультативно, номером версии в виде целого числа или двух целых 
чисел разделенных точкой. Между тильдами и названием, а также между названием и номером 
версии могут вставляться знаки пробела. Окончание блока сигнализируется строкой 
тоже начинающейся с трех знаков тильды, но за которыми не следует ничего больше кроме 
возможных знаков пробела. Компилятор игнорирует все входные строки кроме строк внутри 
кодовых блоков; хотя разбиение самого кода на блоки при этом может быть произвольным, 
принято не прерывать блок посередине незавершенного выражения.   

> NB. Строки начинающиеся с трех тильд но либо не соответствующие описанию выше, либо не 
> следующие в указанном порядке считаются синтаксическими ошибками.

> NB. Интерпретатор языка стартующий в интерактивном режиме предполагает что оператор 
> будет вводить строки кода а не комментарии, поэтому ввод строки сигнализирующей начало 
> блока кода не требуется.
  
Последующее описание касается только содержимого блоков кода а не межблочных комментариев.
Межблочные комментарии могут форматироваться в соответствии с любыми удобными соглашениями
о документации. Исходный текст данного описания может служить примером легального
оформления входного текста программы (и был бы легальной программой, если бы его
кодовые блоки содержали легальный код).


### 2. Формат кода программы

Код программы состоит из отдельных строк оканчивающихся символом возврата каретки.
Разбивка кода на строки в некоторых специально оговоренных местах кода может
иметь грамматический смысл; при этом символ возврата каретки выступает в качестве
грамматического разделителя особого рода, отличающегося от разделителя-пробела или
разделителя-символа табуляции. Таковое отличие может быть нивелировано посредством
вставки перед концом строки символа обратной косой черты (`\`), сообщающего машинному
исполнителю кода что в данном месте конец строки должен быть проигнорирован и
следующая строка должна считаться продолжением текущей. Между символом обратной косой черты
и возвратом каретки может находиться пробельный материал в виде последовательности
знаков пробела и знаков табуляции; вся цепочка символов начинающаяся с `\` и заканчивающаяся
возвратом каретки считается «знаком склейки», логически эквивалентным одному пробелу
посреди регулярного кода и полностью изымаемым из последовательности входных
символов посреди изображения строчного литерала (см. ниже).

За пределами изображений строчных литералов входные строки кода могут оканчиваться
терминальными построчными комментариями. Подобно знаку склейки строк, терминальный
построчный комментарий считается логически эквивалентным одному пробелу; подобный
комментарий начинается с восклицательного знака (`!`) и заканчивается символом
непосредственно предшествующим возврату каретки на той же строке. Сам возврат каретки 
при этом не входит в комментарий и играет свою обычную роль.  

>  NB. Знаки склейки и терминальные построчные комментарии могут соседствовать в одной строке; 
> при этом т.п.к., являясь эквивалентом пробела, должен завершать пробельный материал
> располагающийся между обратной косой чертой и возвратом каретки. Обратное вложение
> (косая черта в конце т.п.к.) считается синтаксической ошибкой.


### 3. Выражения

Программный код состоит из так называемых _выражений_; выражения могут содержать
другие выражения (подвыражения) и соединяться в выражения более высокого уровня.
Выражения делятся на две категории, определяющие их использование внутри других
выражений: _формулы_ и _команды_.

Формулы как правило описывают вычисление возвращающее одно значение, _результат_ формулы с
данными значениями. Одна формула может быть подвыражением другой формулы, пользующейся 
её результатом для вычисления своего результата.

Команды строятся вокруг выражений либо не возвращающих ни одного значения 
(вычисляемых ради побочных эффектов наподобие изменения памяти или записи на диск), 
либо возвращающих больше одного значения (и использующихся лишь в ограниченном наборе 
мест где этим значениям может быть найдено применение), либо не возвращающихся вообще 
(например прекращающих исполнение программы), либо изменяющих контекст в котором 
вычисляются другие выражения.

>  NB. Дихотомия формулы/команды играет в основном чисто грамматическую роль. Формулы
> могут свободно вкладываться друг в друга без специальных вспомогательных синтаксических
> элементов; команды сочетаются иным образом и напрямую частью формул быть не могут.


### 3.1. Формулы

Формулы строятся по образцу традиционных арифметических и логических выражений.
Элементарные формулы состоят из единственного элемента, например числа или имени
(имена и буквальные представления чисел, литер и строк приведены в Приложении 1).
Более сложные формулы строятся из более простых при помощи знаков операций и 
вызовов функций; параметры функций должны быть окружены круглыми скобками,
которые одновременно применяются для управления старшинством операций:

~~~ ПРОСТЕЦ 1

log(y) * (x + 1)

~~~

Старшинство операций определяет как группируются подвыражения, окруженные знаками операций
с двух сторон. Например, операция умножения `*` «старше» операции сложения `+`, поэтому
подвыражения выражения `x + y * z` группируются как `(x + (y * z))`, т.е. `y` между 
двумя знаками операций «достается» более старшему знаку, в данном случае умножению.

Знаки операций участвующие в формулах разбиваются по старшинству на 10 категорий. Категории
получили свои названия по своим самым характерным операциям, но не следует придавать
этим названием слишком большое значение — в каждую категорию могут входить самые
разные знаки, не связанные ничем кроме грамматического старшинства. Каждая категория
имеет единую для всех знаков группы _ассоциированность_, определяющую как группируются
выражения в которых подвыражение находится между двумя знаками одной и той же категории.
Лево-ассоциированные операции отдают такое подвыражение левому знаку (например
выражение `x - y - z` группируется как `((x - y) - z)`, а право-ассоциированные — 
правому. Операции группы 1 _одноместные_ т.е. принимают только один аргумент
справа; остальные операции двухместные, т.е. принимают два аргумента, левый и
правый.

   | Старшинство | Название категории | Операции | Ассоциация |
   |:------------|:--------------------|:----------------------|:------------|
   | 1 | изменения знака | `-` `+` `(-)` `(+)` `~` `@` `.` `(~)` | (одноместные) |
   | 2 | умножения | `*` `/` `(*)` `(/)` `(\)` `(&)` `(&~)` `{&}` `{&~}` | влево |
   | 3 | сложения | `+` `-` `(+)` `(-)` `(\|)` `(^)` `{\|}` `{^}` | влево |
   | 4 | минимум/максимум | `<>` `><` `(<<)` `(>>)` | влево |
   | 5 | отношения | `<` `>` `<=` `>=` `{<}` `{>}` `{<=}` `{>=}` `{<-}` | влево |
   | 6 | равенства | `==` `/=` `{=}` `{/=}` `[=]` `[/=]` | влево |
   | 7 | логические конъюнкции | `&` | влево |
   | 8 | логические дизъюнкции | `\|` | влево |
   | 9 | порождение новых | `#` `##` | вправо |
   | 10 | абстракции | `=>` | вправо |


Вызов функции имеет неявно-операторный синтаксис — в качестве неявного
знака операции разделяющего вызываемое выражение (например имя функции) и
его аргументы (список формул разделенных запятыми заключенный в круглые скобки)
выступает не имеющий внешнего выражения знак операции суперпозиции. Этот неявный
знак имеет приоритет выше всех перечисленных, так что выражение
`x * f (y)` ожидаемо группируется как `(x * (f (y)))`. Суперпозиция лево-ассоциирована,
поэтому `f(x)(y)` группируется как `(f(x))(y)`.

>  NB. Аргументы вызова функции или процедуры должны всегда писаться в скобках, 
>  даже если аргумент один: вместо математической нотации `sin x` требуется писать `sin(x)`, 
>  также как вместо неявного умножения `ab` требуется писать явное `a * b`.

Отдельным видом формул являются так называемые _кортежи_, соединяющие одиночные
значения-результаты нескольких формул в один результат из нескольких значений.
Кортеж выглядит так же как и список аргументов для вызова функции, т.е. 
список формул разделенных запятыми заключенный в круглые скобки `(`_ф_`,` ...`)`. 
Кортеж может быть пустым, содержать один аргумент, или содержать больше одного 
аргумента. Все формулы-аргументы должны возвращать ровно одно значение. Только кортеж 
из  одного элемента может быть использован как часть другой формулы. Кортежи не
являются самостоятельными объектами; их функция состоит только в передаче в функцию
или возврате из функции нескольких значений одновременно.

Формулы-операции разбиваются на смысловые группы по типу принимаемых значений и
результатов. Далее следует краткое описание основных групп.


### 3.1.1. Арифметические операции (смешанная арифметика)

Арифметические операции оперируют числами. Все операции этой группы принимают как целые
так и вещественные числа в качестве аргументов; если все аргументы такой операции целые
и результат тоже может быть представлен в виде целого числа (что зависит от реализации), то
результат будет тоже целым, иначе результат — вещественное число.

> NB. Все реализации должны поддерживать целые числа в диапазоне -536870912 .. 536870911
> включительно. Вещественные числа как правило представлены в виде чисел плавающей арифметики
> двойной точности и операции с ними не являются абсолютно точными в математическом смысле.

Изменения знака (одноместные): `-` `+`

Умножение и деление: `*` `/`

Сложение и вычитание: `+` `-`

Минимум и максимум: `><` `<>`

Сравнения: `<` `>` `<=` `>=`

Проверки на равенство/неравенство: `==` `/=`


### 3.1.2. Арифметические операции (целочисленная арифметика)

Целочисленные операции принимают только целые аргументы и всегда
возвращают целые результаты. Целочисленное деление `(/)` округляет в
сторону нуля, целочисленный остаток `(\)` возвращает парное к нему
значение, так что для всех x и y для которых обе операции определены,
выполняется равенство `х (/) y (+) х (\) y == y`.

Изменения знака (одноместные): `(-)` `(+)`

Умножение, деление и остаток: `(*)` `(/)` `(\)`

Сложение и вычитание: `(+)` `(-)`

Побитная инверсия: `(~)`

Побитное «и», «или», исключающего «или», маска: `(&)` `(|)` `(^)` `(&~)`

Побитные сдвиги: `(<<)` `(>>)`


### 3.1.3. Операции над последовательностями

Взятие первого элемента непустой последовательности (одноместная): `@`

Взятие остатка непустой последовательности без первого элемента (одноместная) : `.`

Присоединение элемента в начало последовательности: `#`

Конкатенация двух последовательностей: `##`

Проверки на равенство/неравенство: `[=]` `[/=]`


### 3.1.4. Операции над множествами

Пересечение и вычитание: `{&}` `{&~}`

Объединение, симметричная разность: `{|}` `{^}`

Проверка принадлежности: `{<-}`

Проверки на подмножество: `{<}` `{<=}`

Проверки на надмножество: `{>}` `{>=}`

Проверки на равенство/неравенство: `{=}` `{/=}`


### 3.1.5. Логические операции

Логические операции принимают и возвращают логические значения. Вычисление
аргументов логического «и» и логического «или» производится слева направо
и прекращается как только результат операции оказывается полностью определён.

Логическая инверсия («не», одноместная): `~`

Логическая конъюнкция («и»): `&`

Логическая дизъюнкция («или»): `|`


### 3.1.6. Функциональная абстракция

Формулы вида _и_ `=>` _ф_ и `(`_и_`,` ...`)` `=>` _ф_ описывают безымянные
функции. Первый вариант эквивалентен записи `(`_и_`,` ...`)` `=>` _ф_ и тоже
описывает функцию с одним аргументом. Все имена аргументов _и_`,` ...
должны быть различными; при вызове функции с соответствующим числом
аргументов эти имена получат результаты вычисления аргументов в качестве
значении и могут использоваться при вычисления тела функции _ф_, на которое
распространяется зона их видимости.

Вычисление формулы-функции создает функцию-объект, обладающую теми же
возможностями по передаче и размещению в памяти, что и все остальные
объекты. Внешние имена, на которые ссылается тело функции _ф_, продолжают
быть доступны для вычисления _ф_ когда бы и откуда бы функция не была
вызвана.


### 3.2. Команды

Команды, как и формулы, строятся при помощи знаков операций и вложенных выражений.
Однако, в отличие от формул, команды дополнительно могут использовать одновременно 
два разделительных знака, тем самым соединяя три выражения. Формат такой трёхместной 
операции X _з_ Y `;` Z или X _з_ Y `,` Z где _з_ это знак операции; два выражения 
справа всегда разделяются либо запятой либо точкой с запятой.
  
Несмотря на то, что команды выступают отдельно от формул, все команды устроены
так чтобы из них была возможность собрать выражение возвращающее значение и тем
самым пригодное в качестве подвыражения формулы. Для этой цели грамматика языка
устроена так, что последним выражением в двух- или трёхместной команде может
быть либо формула (и тогда результатом вычисления всей команды может стать результат
этой формулы), либо другая команда, вычисляемая после и в контексте заданном
остальными выражениями команды. Там самым команды собираются в цепочку, оканчивающуюся 
на формулу; такая цепочка, будучи заключена в круглые скобки `(` `)` или эквивалентные 
им ограничители, с точки зрения грамматики является формулой и может использоваться 
в тех же местах где ипользуются формулы описанные в предыдущем разделе.

Знаки операций команд по старшинству находятся на самой нижней, 11-й ступени
иерархии. Все они право-ассоциированы, так что каждая команда включает в себя
команду, являющуюся её последним выражением, а формула оканчивающая
шлейф команд оказывается вложенной глубже всех. 


   | Старшинство | Название категории | Операции | Ассоциация |
   |:------------|:--------------------|:----------------------|:------------|
   | 11 | двухместные команды | `;`  `:` `<:` `>:` | вправо |
   | 11 | трёхместные команды | `:= ;` `-> ;` `= ;` `= ,` | вправо по последнему выражению |


Поскольку команды играют важнейшую роль в формировании кода программы, 
необходимо разобрать основные из них подробнее.


### 3.2.1. Команда последовательного исполнения ( двухместная операция `;` )

Команда вида _ф_ `;` _в_ где _ф_ это произвольная формула, а _в_ - произвольная
команда или формула предназначена для упорядочивания побочных эффектов. Формула
_ф_, являясь формулой грамматически, может тем не менее отличаться по своему
поведению от того что мы обычно понимаем под математической формулой, т.е. вычисления
дающего один и тот же результат при одних и тех же параметрах, не зависящего
от состояния системы и не влияющего на это состояние. Вычисления другого
рода могут иметь подобного рода зависимости, и команда последовательного
исполнения гарантирует что все побочные эффекты от вычисления _ф_ будут
завершены до начала вычисления _в_. Результат или результаты вычисления _ф_,
если таковые имеются, игнорируются; результатами всей команды являются
результаты _в_.

Типичным примером подобного рода формул являются выражения ввода-вывода,
требующие контроля за последовательностью исполнения.

### 3.2.2. Команда присваивания ( трёхместная операция `:=` `;` )

Команда вида _и_ `:=` _ф_ `;` _в_ где _и_ это имя ранее определенной переменной, 
_ф_ — произвольная формула а _в_ — произвольная команда или формула предназначена 
для присваивания переменной нового значения. Вычисление команды начинается с 
вычисления формулы _ф_, которая должна вернуть один результат, который далее
записывается в качестве нового значения переменной _и_.
Команда присвоения гарантирует что все побочные эффекты от исполнения _ф_ и 
изменения значения переменной _и_ будут завершены до начала вычисления _в_. 
Результатами всей команды являются результаты _в_.

> NB. Присваивание не является формулой поскольку не имеет собственного значения.
> Императивные языки, в которых присваивание играет центральную роль, стремятся
> упростить его использование и делают его выражением, но в данном случае такой
> необходимости нет.

### 3.2.3. Команда выбора ( трёхместная операция `->` `;` )

Команда вида _ф1_ `->` _ф2_ `;` _в_ где _ф1_ и _ф2_ — произвольные формулы, а
_в_ - произвольная команда или формула позволяет принять решение о передаче
управления в зависимости от логического результата. Вычисление команды начинается
с вычисления формулы _ф1_ которое должно возвратить логический результат.
Если возвращенное значение истинно, результатом команды будет результат
(или результаты) вычисления формулы _ф2_,  иначе будет произведено
вычисление выражения _в_.

Грамматическая несимметричность двух ветвей команды выбора обусловлена
частотой использования ветвей; одним из характерных способов определения
результата является перебор вариантов:

~~~ ПРОСТЕЦ

(x < 0 -> -1; x > 0 -> 1; 0)   ! возвращает знак х в виде целого числа

~~~


### 3.2.4. Команда определения имён ( трёхместная операция `=` `;` )

Как для удобства написания кода, так и для экономии на времени повторных
вычислений, часто бывает удобно поименовать результаты вычислений. Этим целям
служит команда именования, имеющая вид _о_ `=` _ф_ `;` _в_, где _о_ — образец,
содержащий определяемое локальное имя или имена, _ф_ — формула, задающая начальные
значения, а _в_ — формула или команда исполняющаяся в контексте предоставляющем
доступ к новым именам. Прежде чем рассмотреть детали грамматики образцов
следует упомянуть что зона видимости определяемых имён зависит от образца.
Образцы, определяющие одно имя, делают это имя видимым как внутри _ф_,
так и внутри _в_, в то время как образцы, определяющие группу имён,
делают эти имена видимыми только внутри _в_, в то время как _ф_ 
вычисляется в том же контексте, что и сама именующая команда (т.е.
внешнем по отношению к ней).

Простейшим образцом является одно имя, _и_. Формула _ф_ вычисляется
в контексте где это имя доступно но не имеет определенного значения
(и стало быть не может быть использовано во время вычисления); 
результат формулы по окончанию её вычисления становится значением
имени.

~~~ ПРОСТЕЦ 1

(х = log(y)*2; y+1)  ! возвращает  log(y)*2+1

~~~

Разновидностью образца, определяющего одно имя, является образец,
выглядящий как вызов этого имени как функции с другими именами в
качестве аргументов, т.е. _и_ `(`_и1_`,` ...`)`. Эта форма служит для более 
наглядного определения локальной функции или процедуры, и может быть заменена 
на простейшую форму с образцом-именем и оператором функциональной абстракции:

~~~ ПРОСТЕЦ

(z(x, y) = log(x)+log(y); z(1.0, 3.0))      ! возвращает log(1.0)+log(3.0)

(z = (x, y) => log(x)+log(y); z(1.0, 3.0))  ! тот же результат

~~~

Более сложным образцом является образец, определяющий группу имён
одновременно. Он выглядит как кортеж — список имён разделенных запятой, заключенный
в круглые скобки, т.е. `(`_и1_`,` ...`)`. Определяющим значением для такой формы 
может служить любое выражение, вычисление которого возвращает нужное количество
результатов:

~~~ ПРОСТЕЦ

(range(x, d) = (x-d, x+d); (х, y) = range(10, 1); y-х)  ! возвращает 2

~~~

Вторая, вложенная, команда в приведенном примере определяет два имени
одновременно, пользуясь определением функции предоставленным предыдущей
командой. 

Как описано выше, имена, определяемые посредством группового определения,
не видны внутри определяющего выражения. Это дает возможность определять
одно и то же имя снова и снова, давая ему значение, вычисленное с использованием
«старого» имени, или менять значения местами, определяя новые имена
при помощи старых:

~~~ ПРОСТЕЦ

(x = 1; (x) = x+1; (x) = x+1; x)          ! возвращает 3

(х = 1; y = 2; (x, y) = (y, x); 10*x+y)   ! возвращает 21 

~~~

Первый из примеров показывает что для группового определения имён
не требуется чтобы имён было больше одного. В предельном случае
имён может не быть вообще, хоть подобное определение может служить
разве что для демонстрации того что определяющее выражение не возвращает
ни одного результата.

Как и в случае команда последовательного исполнения, все побочные
эффекты случающиеся в процессе вычисления определяющей формулы _ф_
происходят до того как начинается вычисление последнего выражения _в_.


### 3.2.5. Команда совместного определения имён ( трёхместная операция `=` `,` )  
 
Данная команда имеет вид _о_ `=` _ф_ `,` _ок_, где _о_ — образец, содержащий одно определяемое имя, 
_ф_ — формула, задающая начальное значение, а _ок_ — команда определения имён с
образцом определяющим одно имя или другая команда совместного определения имён.
Грамматически такие команды образуют свою (под-)цепочку заканчивающуюся на команде обычного
определения одного имени (трёхместная операция `=` `;`). Вычисление такой команды во всём 
подобно вычислению команды определения одного имени описанной выше, за исключением следущей
детали: зоной видимости для _всех_ имён определенных в цепочке команд соединённых операцией
`=` `,` становится _весь_ набор их определяющих выражений. Это, в частности, позволяет 
определять группу взаимно-рекурсивных функций. Пример:

~~~ ПРОСТЕЦ

(even(x) = x <  1 |  odd(x-1),    ! зона видимости соединяется со следующей командой 
  odd(x) = x >= 1 & even(x-1);    ! последняя команда в цепочке с совместной зоной видимости
 even(5))                         ! проверка на чётность не проходит

~~~

Как и раньше, все побочные эффекты случающиеся в процессе вычисления определяющей формулы _ф_
происходят до того как начинается вычисление следующей команды _<ок>_. Факт того, что в
зону видимости _всех_ определяющих формул цепочки команд с совместной зоной видимости
входят _все_ определяемые имена цепочки, по-прежнему не позволяет в процессе вычисления
определяющих значений обращаться к значениям имён ещё эти значения не получивших (упоминание
имени в теле функции без исполнения этой функции таковым обращением не является). Получение
именем значения тоже считается побочным эффектом и также происходит последовательно
по цепочке команд.
 

### 3.2.6. Команда метки ( двухместная операция `:` )  

Команда метки предназначена для обозначения места в цепочке команд, к которому
можно вернуться для повторного исполнения, т.е. для организации _цикла_. Однако,
в отличие от меток в большинстве других языков программирования, команда метки в 
языке ПРОСТЕЦ позволяет передавать новые значения переменных цикла не через 
изменение переменных, а напрямую, через параметры метки. В сущности, такая
метка является ничем иным как локально определённой функцией, каждое обращение
к которой не прерывает исполнение а является вызовом функции.

Команда метки имеет вид _им_`(`_и_ `=` _ф_`,` ...`)` `:` _в_, где _им_ это имя метки, 
`(`_и_ `=` _ф_`,` ...`)` это возможно пустая последовательность определений имён параметров,
а _в_ — формула или команда, попадающая в зону видимости имени метки _им_ и имён
параметров _и_....

Неформально, команда ведёт себя следующим образом: сначала вычисляются значения 
формул _ф_... и вводятся локальные имена _и_..., получающие эти значения
в качестве начальных. Вместе с этим, вводится локальное имя _им_, получающее
в качестве значения функцию позволяющую вызвать _новую итерацию_ с новыми
значениями этих параметров. Далее происходит вычисление внутреннего выражения
_в_, входящего в зону видимости всех новых имен. Если вычисление этого
выражения не приводит к вызову имени метки _им_ с новыми параметрами, то
команда метки завершается и результат _в_ становится её результатом. Если же
в процессе исполнения такой вызов происходит, каждый подобный вызов приводит к
повторному исполнению _в_ с новыми значениями локальных имён _и_..., 
причём к исполнению _возвращающему значение_ а не просто передающему
управление подобно оператору `goto`.

Формально, смысл команды метки определяется её трансформацией в 
определение и вызов локальной рекурсивной функции. Команда вида 

_им_`(`_и_ `=` _ф_`,` ...`)` `:` _в_ 

полностью эквивалентна команде 

_им_`(`_и_`,` ...`)` `=` `(`_в_`)` `;` _им_`(`_ф_`,` ...`)`

Пример:

~~~ ПРОСТЕЦ

 ! итеративное определение функции факториал
 fact(x) = (f(x=x, v=1): x > 1 -> f(x-1, x*v); v)

~~~

>  NB. Для того чтобы управляющие структуры отложенных вычислений не 
>  накапливались в памяти, вызов функции-метки должен находиться в позиции
>  из которой производится возврат результата команды (её так называемой
> _хвостовой позиции_ — см. ниже).


### 3.2.7. Команда запоминания цепочки возврата ( двухместная операция `<:` )  

В процессе исполнения программы каждый вызов функции или операции возвращающей
результат может находиться в одной из двух типов грамматической позиции: (1)
позиции накопления аргументов для другого вызова, и (2) позиции возвращения
значения выражения. Позиции в которых находятся подвыражения формул как правило
являются позициями первого рода, так как возвращаемые значения ожидаются для
последующей передачи в качестве аргументов охватывающей формуле. Позиции
в которых находятся последние выражения цепочки команд и формулы справа от
стрелки `->` в командах выбора находятся в позиции типа (2).

По мере продвижения вычислений, каждое вычисление выражения сопровождается
неявной последовательностью позиций типа (1) (_цепочки возврата_), определяющей 
куда именно в общем вычислении необходимо передать результаты данного выражения. 
Как только вычисление выражения заканчивается, его результаты передаются цепочке
возврата для продолжения вычислений. Вычисления выражений в позициях типа (2) 
(известных как _хвостовые позиции_) не приводят к удлинению цепочки возврата.
 
Для прямой манипуляции с цепочками возврата в языке существует две команды.
Команда запоминания цепочки возврата имеет вид _и_ `<:` _в_, где
_и_ это новое локальное имя, а _в_ — любая формула или команда. 
В начале вычисления данной команды её неявная цепочка возврата делается
явной и доступной внутри _в_ под локальным именем _и_. Далее
следует вычисление _в_, с той же неявной цепочкой возврата,
которой в «обычной» ситуации и передаются результаты вычисления _в_.
Однако ситуация может и не быть «обычной» — для этого существует парная
команда нелокального возврата.

### 3.2.8. Команда нелокального возврата ( двухместная операция `:>` )  

Команда вида _ф_ `:>` _фв_ где _ф_ и _фв_ — произвольные формулы
предназначена для осуществления нелокального перехода к ранее запомненной
цепочке возврата. Работает она следующим образом: сначала вычисляется
формула _фв_, результатом которой должна быть цепочка возврата, ранее
полученная при помощи вышеописанной команды запоминания `<:`. Далее,
неявная цепочка возврата сопровождающая вычисление всей команды
заменяется только что вычисленной и формула _ф_ вычисляется уже
с ней, отправляя ей свои результаты. 

Неформально, эффект нелокального возврата выглядит так: результаты
_ф_ не возвращаются в обычное место как бы положенное по тексту программы,
а «ныряют» и «выныривают» в качестве результатов команды `<:` в которой
данная цепочка возвратов _фв_ была запомнена. То есть вычисления продолжаются
в совершенно другом месте, так, будто никаких вызовов с момента запоминания
цепочки не происходило вовсе (однако происходили все побочные эффекты:
присвоение переменным, изменение объектов в памяти, ввод-вывод и т.д.).

Заметим, что запомненная цепочка запоминается целиком и может
быть использована даже когда действующая цепочка сокращается до
одного вызова верхнего уровня. Цепочки возврата являются полноценными
неизменяемыми объектами, которые можно использовать наравне с любыми 
другими. 

> NB. В реализации языка цепочки возврата могут быть представлены как 
> процедуры и в такой реализации возвращаемые результаты можно было бы
> передать в функцию цепочки возврата явно в качестве аргументов,
> т.е. `(х, 1, y+z) :> f` эквивалентно `f(х, 1, y+z)`. Запись в виде
> операции `:>`, однако, не просто делает нелокальный переход более
> заметным в тексте программы; если возвращаемое значение является
> вложенным вызовом, запись `f(foo())` вызовет `foo` с удлиннённой
> цепочкой возврата, чтобы уже потом её заменить по возвращении из foo(), 
> в то время как запись `foo() :> f` сразу подставит нужную. 
> Разница станет заметна если внутри foo() текущая цепочка запоминается, 
> например для реализации _сопрограмм_ — в одном случае цепочки будут 
> удлинняться при каждом переключении, в другом нет.

~~~ ПРОСТЕЦ

 ! факториал с нелокальным выходом
 fact(x) = (res <: f(x=x, v=1): х < 1 -> v :> res; f(x-1, x*v))

~~~


### 4. Выражения верхнего уровня

Код программы, полученный соединением содержимого блоков кода в
единый непрерывный набор строк, представляет собой последовательность
выражений специального вида, называемых _выражениями верхнего уровня_.
В свою очередь, они подразделяются на формулы верхнего уровня и 
определения верхнего уровня. И те и другие грамматически напоминают
команды верхнего уровня, но имеют свою специфику. 


### 4.1. Формулы верхнего уровня

Формулы верхнего уровня имеют вид _ф_ `;` где _ф_ — произвольная
формула. Результат вычисления _ф_ в интерактивном режиме выводится
на экран, в других режимах игнорируется.


### 4.2. Определения верхнего уровня

Определения верхнего уровня грамматически тождественны командам 
определения локальных имён, за исключением того, что последнее
выражение <в> после точки с запятой `;` не пишется.

> NB. Точнее сказать, последним выражением является весь
> оставшийся код программы, но так как программа обрабатывается
> _поэлементно_, удобно дробить её код немного иначе, допустив
> в данном месте грамматическую вольность без изменения общего смысла.

В этом урезанном виде могут быть записаны все виды определений
имён пишущихся посредством трёхместной операции `=` `;`. Область
видимости всех имён заданных на верхнем уровне включает в себя
всю программу целиком, поэтому в дополнительной конструкции для
совместного определения имён нужды нет.

В интерактивном режиме определение обрабатывается и определённые
имена становятся доступными для использования сразу по окончании 
обработки.

В остальных режимах исполнение программы начинается со специально
помеченной процедуры называемой _процедурой входа_.

### ПРИЛОЖЕНИЯ

### Приложение 1. Грамматика языка

В этом приложении приводятся правила задающие полную грамматику
языка. Правила пишутся в расширенной БНФ-нотации: в дополнение
к знаку ::= разделяющему левую и правую часть правила и знаку |
разделяющему альтернативы, используются следующие соглашения:

\| ... \|  — неформальное повторение, пропущенные правила

{ _х_ }  — элемент х является необязательным (может повторяться 0 или 1 раз)

_х_ ...  — элемент х может повторяться 0 или больше раз

_х_ `,` ...  — элемент х может повторяться 0 или больше раз; повторения разделяются запятой

Элементы грамматики записываются в фигурных скобках. Некоторые из
них, например <СИМВОЛ НОВОЙ СТРОКИ>, определены за пределами приведенной
грамматики; названия таких элементов пишутся с заглавной бувы.

### П 1.1. Лексические элементы

Правила для лексических элементов не подразумевают никаких дополнительных 
символов между элементами грамматики, все элементы пишутся последовательно
в соответствии с описанием. Все повторы элементов покрывают максимально
длинную часть входного текста.

### П 1.1.1. Комментарии и пробелы

<пробел> ::= <ПРОБЕЛ> | <ТАБУЛЯЦИЯ>

<комментарий> ::= `!` <ЛЮБОЙ СИМВОЛ КРОМЕ НОВОЙ СТРОКИ> ...

<пробел или комментарий> ::= <пробел> | <комментарий>

<внутристрочный пробельный материал> ::= <пробел или комментарий> ...

<комментарий с новой строкой> ::= <комментарий> <СИМВОЛ НОВОЙ СТРОКИ>

<пробел или комментарий с новой строкой> ::= <пробел> | <комментарий с новой строкой>

<пробельный материал> ::= <пробел или комментарий с новой строкой> ...


### П 1.1.2. Константы

<цифра> ::= `0` | ... | `9`

<цифра 1-9> ::= `1` | ... | `9`

<двоичная цифра> ::= `0` | `1`

<восьмеричная цифра> ::= `0` | ... | `7`

<шестнадцатеричная цифра> ::= `0` | ... | `9` | `A` | ... | `F` | `a` | ... | `f`

<целое> ::= `0` | <цифра 1-9> <цифра> ...

<двоичное целое> ::= `2'` <двоичная цифра> <двоичная цифра> ...

<восьмеричное целое> ::= `8'` <восьмеричная цифра> <восьмеричная цифра> ...

<десятеричное целое> ::= `10'` <цифра> <цифра> ...

<шестнадцатеричное целое> ::= `16'` <шестнадцатеричная цифра> <шестнадцатеричная цифра> ...

<целое число> ::= <целое> | <двоичное целое> | <восьмеричное целое> | <десятеричное целое> | <шестнадцатеричное целое>

<знак> ::= `+` | `-`

<показатель степени> ::= `e` | `E` | `*10^`

<порядок> ::= <показатель степени> { <знак> } <целое>

<вещественное число> ::= <целое> `.` <цифра> <цифра> ... { <порядок> } | <целое> <порядок>

<численная константа> ::= <вещественное число> | <целое число>

<логическая константа> ::= `'0` | `'1`

<описание литеры> ::= <ЛЮБОЙ СИМВОЛ КРОМЕ ' " ‹ › « » ~ И УПРАВЛЯЮЩИХ> | <код символа>

<код символа> ::= <код кавычки> | <код двойной кавычки> | <код левой кавычки> | <код правой кавычки> | <код левой двойной кавычки> | <код правой двойной кавычки> | <код тильды> | <код табуляции> | <код новой строки> | <универсальный код>

<код кавычки> ::= `~'`

<код двойной кавычки> ::= `~"`

<код левой кавычки> ::= `~‹`

<код правой кавычки> ::= `~›`

<код левой двойной кавычки> ::= `~«`

<код правой двойной кавычки> ::= `~»`

<код тильды> ::= `~~`

<код табуляции> ::= `~|`

<код новой строки> ::= `~%`

<универсальный код> ::= `~` <целое число> `;`

<литерная константа> ::= `'` <описание литеры> `'` | `‹` <описание литеры> `›`

<строковая константа> ::= `"` <описание литеры> ... `"` | `«` <описание литеры> ... `»`

<константа> := <численная константа> |  <логическая константа> | <литерная константа> | <строковая константа>


### П 1.1.3. Имена и символы операций

Имена не могут содержать прописных букв, но могут состоять из нескольких
слов разделенных одним пробелом. Слова из прописных букв могут использоваться
только в качестве символов операций и служебных слов.

> N.B. Полный набор букв, поддерживаемых конкретной реализацией языка, называется
> _азбукой реализации_. Конкретная реализация может накладывать дополнительные
> ограничения на допустимые имена.

<строчная буква> ::= `a` | ... | `z` | <ДРУГАЯ СТРОЧНАЯ БУКВА>

<строчная буква, цифра или подчерк> ::= <строчная буква> | <цифра> | `_`

<слово> ::= <строчная буква> <строчная буква, цифра или подчерк> ...

<слово или число> ::= <слово> | <число>

<пробел, дальше слово или число> ::= <ПРОБЕЛ> <слово или число>

<имя> ::= <слово> <пробел, дальше слово или число> ... 

<прописная буква> ::= `A` | ... | `Z` | <ДРУГАЯ ПРОПИСНАЯ БУКВА>

<буквенный символ операции> ::= <прописная буква> <прописная буква> ...

<символ операции> ::= <буквенный символ> | `-` | `+` | .... | `:>`

### П 1.2. Грамматические элементы

Правила для грамматических элементов подразумевают возможность разделения
любых двух элементов пробельным материалом (т.е. элементом <пробельный материал>).
Все повторы элементов покрывают максимально длинную часть входного текста.

<выражение> ::= <формула> | <команда>

### П 1.2.1. Элементарные формулы

<эф> ::= <константа> | <имя> | `(` <команда> `)` | <кортеж> | <вызов>

<кортеж> ::= `(` <формула> `,` ... `)`

<вызов> ::= <эф> <кортеж>

### П 1.2.2. Формулы группы изменения знака (одноместные операторы)

<зф> ::= <зоп> <зф> | <эф>

<зоп> ::= `-` | `+` | `(-)` | `(+)` | `~` | `@` | `.` | `(~)`

### П 1.2.3. Формулы группы умножения

<уф> ::=  <уф> <уоп> <зф> | <зф>

<уоп> ::= `*` | `/` | `(*)` | `(/)` | `(\)` | `(&)` | `(&~)` | `{&}` | `{&~}`

### П 1.2.4. Формулы группы сложения

<сф> ::=  <сф> <соп> <уф> | <уф>

<соп> ::= `+` | `-` | `(+)` | `(-)` | `(|)` | `(^)` | `{|}` | `{^}`

### П 1.2.5. Формулы группы минимум-максимум

<мф> ::=  <мф> <моп> <сф> | <сф>

<моп> ::= `<>` | `><` | `(<<)` | `(>>)`

### П 1.2.6. Формулы группы отношения

<оф> ::=  <оф> <ооп> <мф> | <мф>

<ооп> ::= `<` | `>` | `<=` | `>=` | `{<}` | `{>}` | `{<=}` | `{>=}` | `{<-}`

### П 1.2.7. Формулы группы равенства

<рф> ::=  <рф> <роп> <оф> | <оф>

<роп> ::= `==` | `/=` | `{=}` | `{/=}` | `[=]` | `[/=]`

### П 1.2.8. Формулы группы логической конъюнкции

<кф> ::=  <кф> <коп> <рф> | <рф>

<коп> ::= `&`

### П 1.2.9. Формулы группы логической дизъюнкции

<дф> ::=  <дф> <доп> <кф> | <кф>

<доп> ::= `|`

### П 1.2.10. Формулы группы порождения новых последовательностей

<нф> ::=  <нф> <ноп> <дф> | <дф>

<ноп> ::= `#` | `##`

### П 1.2.11. Формулы группы абстракции

<аф> ::=  <нф> <аоп> <аф> | <нф>

<ноп> ::= `=>`

> NB. Более детальный грамматический разбор должен ограничить
> вид возможных левых частей операции `=>` так:
>
> <имя> | `(` <имя> `,` ... `)`
>
> заодно проверив что среди имён в скобках нет дубликатов

На этом грамматическая башня формул завершается

<формула> ::= <аф>


### П 1.2.12. Команды

Все команды имеют один и тот же приоритет. Для удобства
можно описать грамматику команд в упрощенном виде, не вдаваясь
в подробности структуры левых подвыражений. Ограничения на отдельные
операторы будет дано в примечаниях.

<команда> ::= <двухместная команда> | <трёхместная команда с ;> | <трёхместная команда с ,> | <формула>

<двухместная команда> ::= <формула> <дмоп> <команда>

<дмоп> ::= `;` | `:` | `<:` | `:>`

<трёхместная команда с ;> ::= <формула> <тмоп с ;> <формула> `;` <команда> 
<трёхместная команда с ,> ::=  <формула> <тмоп с ,> <формула> `,` <команда>

<тмоп с ;> ::= `=` | `:=` | `->` 

<тмоп с ,> ::= `=`

