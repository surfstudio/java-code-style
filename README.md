# Java Code Style

Почти полностью основан на [Google Java Style Guide](https://google.github.io/styleguide/javaguide.html). Он был переведен, а также убраны моменты, которые отличаются для нас.

## Содержание

* [Файлы с исходным кодом: основное](#Файлы-с-исходным-кодом)
    * [Имена файлов](#Имена-файлов)
    * [Кодировка](#Кодировка)
    * [Спецсимволы](#Спецсимволы)
* [Структура файлов](#Структура-файлов)
    * [Имя пакета](#Имя-пакета)
    * [Импорты](#Импорты-классов)
    * [Описание классов](#Описание-классов)
* [Форматирование](#Форматирование)
    * [Фигурные скобки](#Фигурные-скобки)
    * [Блоки](#Отступы-слева-для-блоков)
    * [Одно выражение на строку](#Одно-выражение-на-строку)
    * [Длина строки](#Длина-строки)
    * [Перенос строки](#Перенос-строки)
    * [Пустое место](#Пустое-место)
    * [Специфические конструкции](#Специфические-конструкции)
* [Именование](#Именование)
    * [Все идентификаторы](#Правила-для-всех-идентификаторов)
    * [Пакеты](#Имена-пакетов)
    * [Классы](#Имена-классов)
    * [Методы](#Имена-методов)
    * [Константы](#Имена-констант)
    * [Неконстантные поля](#Имена-неконстантных-полей)
    * [Параметры](#Имена-параметров)
    * [Локальные переменные](#Имена-локальных-переменных)
    * [Переменные типа](#Имена-переменных-типов)
    * [Camel Case](#Определение-camel-case)
* [Практики программирования](#Практики-программирования)

## Файлы с исходным кодом

### Имена файлов
Имя файла с исходным кодом состоит из регистро-чувствительного имени класса верхнего уровня, который располагается в этом файле, плюс `.java` расширение. Класс верхнего уровня в файле может быть только один.

### Кодировка
Файлы с исходным кодом хранятся в кодировке **UTF-8**.

### Спецсимволы
#### Символы пустого места
Кроме символов перевода строки, только ASCII-пробел (0x20) встречается в файлах. Это означает:
* Все остальные символы пустого места эскейпятся (escaped).
* Табы **не** используются для отступов.

#### Escaped-последовательности символов
Для любого символа, который имеет специальную [escape-последовательность](http://docs.oracle.com/javase/tutorial/java/data/characters.html) (b, \t, \n, \f, \r, \", \' и \\), эта последовательность используется вместо соответствующего octal (например \012) или Unicode-символа (например \u000a).

#### Не-ASCII символы
Для остальных не-ASCII символов, используется или сам Unicode-символ (например ∞) или соответствующий Unicode escape-символ (например \u221e). Выбор символа зависит только о того, насколько легко код с ним читать и понимать, хотя Unicode escape-символы вне строковых литералов и комментариев вставлять не рекомендуется.

Совет: в случае использования Unicode escape-символа, и иногда даже когда используется сам Unicode символ, поясняющие комментарии могут быть полезны.

Пример | Комментарии
------------ | -------------
String unitAbbrev = "μs"; | Лучший вариант: все ясно даже без комментария
String unitAbbrev = "\u03bcs"; // "μs" | Разрешается, но нет причины так делать
String unitAbbrev = "\u03bcs"; // Greek letter mu, "s" | Разрешается, но странно выглядит и неустойчиво к ошибкам
String unitAbbrev = "\u03bcs"; | Плохо: читатель не поймет, что это такое
return '\ufeff' + content; // byte order mark | Хорошо: используется escape-символ для непечатных символов, и есть коммент

Совет: не делайте ваш код менее читаемым просто из боязни, что какая-либо программа не сможет обработать не-ASCII символы верно. Если такое случается, значит программа **работает неправильно** и должна быть **починена**.

## Структура файлов

Файл с исходным кодом состоит из:
* Лицензия или информация о копирайте, если есть
* Имя пакета
* Импорты классов
* Только один класс верхнего уровня

Секции разделяются одной пустой линией.

### Имя пакета

Имя пакета не разделяется переносом строки. Заданная нами [длина строки](#Длина-строки) не распространяется на строку с именем пакета.

### Импорты классов

Использование wildcard и static импортов не рекомендуется. Исключение: если ситуация оправдана, и все испортируемые методы и константы близки по смыслу к классу в данном файле.

Импорт не разделяется переносом строки. Заданная нами [длина строки](#Длина-строки) не распространяется на строку с импортом класса.

#### Порядок импортов
Нет специальных правил на порядок импортов. Придерживаемся стиля, заданного по-умолчанию в используемой всеми IDE.

## Описание классов
Каждый класс верхнего уровня находится в отдельном файле с его именем.
### Порядок расположения членов класса
Члены классы располагаются в таком порядке:
* static-поля
* поля
* конструкторы
* static-методы
* перекрытые (overriden) методы
* методы
* get/set методы
* внутренние классы

#### Не разрываем последовательность перегруженных методов
Если у класса есть несколько конструкторов или методов с одним и тем же именем, они располагаются друг за другом, без включения других полей/методов

## Форматирование

### Фигурные скобки

#### Ставим скобки, даже если это необязательно
Скобки используются с `if`, `else`, `for`, `do` и `while`, даже если их тело пустое или содержит только 1 оператор.

#### Непустые блоки: стиль K & R
Скобки ставятся в соответствии со стилем Кернигана и Ричи ([Египетские скобки](http://www.codinghorror.com/blog/2012/07/new-programming-jargon.html)) для непустых блоков:
* Нет перевода строки до открывающей скобки
* Перевод строки после открывающей скобки
* Перевод строки до закрывающей скобки
* Перевод строки после закрывающей скобки, если эта скобка заканчивает оператор, тело метода, конструктор или класс. Например, нет перевода строки после закрывающей скобки, если после неё стоит `else` или точка с запятой.

Пример:
```java
return () -> {
  while (condition()) {
    method();
  }
};

return new MyClass() {
  @Override public void method() {
    if (condition()) {
      try {
        something();
      } catch (ProblemException e) {
        recover();
      }
    } else if (otherCondition()) {
      somethingElse();
    } else {
      lastThing();
    }
  }
};
```
Исключения делаются для Enum-классов.

#### Пустые блоки не укорачиваем
Ставим перевод строки и комментарий, даже если тело блока пустое:
```java
void doNext() {
  // do nothing
}
```

### Отступы слева для блоков
Каждый раз, когда мы открываем новый блок, отступ увеличивается на 2 пробела. Когда блок заканчивается, отступ возвращается до прежнего значения. Отступ применяется и для комментариев в блоке.

### Одно выражение на строку
Каждое выражение или оператор заканчивается переводом строки.

### Длина строки
Длина строки в коде имеет лимит в 120 символов. Кроме исключений, любая строка больше лимита должна быть разбита на 2 строки, как описано в (#Перевод строки).
Исключения:
* Строки, для которых невозможно соблюсти лимит (например, длинный URL в Javadoc)
* Имя пакета и импорты (см. #Имя пакета и #Импорты)
* Команды, которые можно скопировать в командную строку для выполнения (shell)

### Перенос строки
Нет точной формулы или алгоритма для перевода строк в каждой ситуации. Часто есть несколько способов.
Основная цель - сделать код более понятным и ясным, а необязательно укладывающимся в минимальное число строк.
Совет: хотя одна из целей использования переноса строк - соблюсти лимит длины строки в коде, часто автор может перенести код на другую строку, даже если он укладывался в лимит.
Совет: выделение метода или локальной переменной может решить проблему без необходимости переносить строку.

#### Отступы для перенесенных строк - 8 пробелов
При переносе строк, каждая перенесенная строка после самой первой отступает слева на 8 пробелов от начальной строки. В общем случае, отступ для 2 перенесенных строк будет одинаковым только если начинаются с синтаксически параллельных конструкций.

### Пустое место

#### Вертикальное пустое место
Одна пустая строка стоит:
* Между последовательными членами класса: поля, конструкторы, методы, вложенные классы, static-инициализация, инициализация экземпляров классов.
  * Исключение: пустая строка между 2 полями опциональна (если нет другого кода между ними). Такие пустые строки могут отделять логические группы полей.
  * Исключение: пустые строки между Enum-константами описаны в разделе про Enum-классы.
* Между выражениями, чтобы выделить логические подсекции.
* Согласно требованиям в других разделах code style (см. Структура файлов, Импорты).
* *Опционально* перед первым членом класса или после последнего.

Несколько пустых строк подряд разрешается, но не обязательно.

#### Горизонтальное пустое место

Одиночный ASCII-пробел встречается в следующих местах (помимо требования синтаксиса языка, литералов, комментов и javadoc): 
* Отделяет любое зарезервированное слово, такое как *catch*, *for* или *if*, от открывающей круглой скобки, которая идет далее в этой строке
* Отделяет любое зарезервированное слово, такое как *else* или *catch*, от закрывающей фигурной скобки, которая предшествует этому слову в этой строке
* Перед любой открывающей фигурной скобкой ({), кроме:
  * ```@SomeAnnotation({a, b})``` (нет пустого места)
* По обеим сторонам любого бинарного или тернарного оператора. Это относится и к подобным символам:
  * Амперсанд в пересечении типов: ```<T extends Foo & Bar>```
  * Вертикальная черта (pipe) для блока *catch*, который обрабатывает несколько исключений: ```catch (FooException | BarException e)```
  * Двоеточие (:) в *for* ("foreach")
  * Стрелка в лямбда-выражении: ```(String str) -> str.length()```
* Но не относится к:
  * Два двоеточия (::) для ссылки на метод, в записи вида ```Object::toString```
  * Для точки-разделителя при вызове метода, в записи вида ```object.toString()```
* После закрывающей скобки в приведении типов
* С двух сторон двойного слеша (//), который начинает комментарий в конце строки.
* Между типом и переменной в определении: ```List<String> list```

Правила относятся к пробелам внутри строки, а не отступам (оговариваются в другом разделе).

#### Горизонтальное выравнивание не используется

Горизонтальное выравнивание - практика добавления разного числа пробелов в коде с целью того, чтобы некоторые конструкции располагались точно под другими конструкциями на вышестоящей строке. Мы не используем горизонтальное выравнивание, и рекомендуется убирать его при рефакторинге.
Пример:
```java
private int x; // это нормально
private Color color; // и это тоже

private int   x;      // неправильно
private Color color;  // неправильно, хотя color и стоит под x
```
Выравнивание может улучшить читаемость, но создает проблемы для дальнейшей поддержки. Допустим, последующее изменение затрагивает всего 1 строку. Часто такое изменение сбивает выравнивание, что выглядит уже не так хорошо. Но чаще разработчик стремится поддержать порядок выравнивания и меняет оступы в других строках, инициируя каскадную серию форматирований. Это однострочное изменение теперь имеет "радиус поражения". Это бесполезная работа, также это ухудшает читаемость истории версий, замедляет ревьюеров и затрудняет разрешение merge-конфликтов. 

### Специфические конструкции

#### Классы Enum
После каждой запятой после определения Enum-константы нужен перевод строки. Дополнительно допускается пустая строка после этого. Пример:
```java
private enum Answer {
  YES {
    @Override 
    public String toString() {
      return "yes";
    }
  },
  NO,
  MAYBE
}
```
Enum-классы в остальном подчиняются общим правилам оформления классов.

#### Определение переменных

##### В каждой строке определяется только одна переменная
Каждое определение (поле или локальная переменная) состоит только из одной переменной: определения вида **int a, b;** не используются.

##### Переменные определяются по мере надобности
Локальные переменные не обязательно должны определятся в начале блока или метода. Вместо этого, он определяются как можно ближе к месту их первого использования, чтобы уменьшить область действия переменной. Локальные переменные как правило сразу инициализируются.

##### Массивы
Квадратные скобки ставятся рядом с типом, а не самой переменной: String[] args, но не String args[].

#### Блоки Switch
Внутри скобок в switch-блоке располагаются одна или несколько групп операторов. Каждая группа содержит одну или несколько меток (например FOO: или default:), за которой следуют операторы.

##### Отступы
Как и с другими блоками, оступ для содержимого составляет +2 пробела.
После метки стоит перевод строки, и отступ снова увеличивается на +2, как в случае с открытием нового блока. Следующая метка находится на предыдущем уровне, как будто блок закрылся.
##### Провалы в следующие метки: не используются
Внутри switch-блока, группа операторов заканчивается break, continue, return или выбросом исключения. Провалы в следующие метки недопустимы - это сильно снижает читаемость, и сложно визуально выделить и отследить логику провала по меткам. Пример:
```java
switch (input) {
  case 1:
  case 2:
    prepareOneOrTwo();
    // неправильно, тут нет возврата из switch
  case 3:
    handleOneTwoOrThree();
    break;
  default:
    handleLargeNumber(input);
}
```
Исключения делается только для пустых меток (пример выше с *case 1:*).
Для каждого switch должна быть default-метка, даже если она не содержит кода.

### Аннотации

Аннотации, применяемые к классу, методу или конструктору ставятся сразу после блока документации, и находятся на отдельной строке (по одной аннотации на строке). Отступ слева после аннотации не увеличивается. Пример:
```java
@Override
@Nullable
public String getNameIfPresent() { ... }
```
Нет специальных правил для форматирования аннтотаций на параметрах, локальных переменных, типах.

#### Комментарии

Блочные комментарии имеют тот же отступ слева, что и окружающий их код. Они могут быть записаны в стиле /* ... */ или // ... Для многострочных комментариев, последующие строки могут начинаться со * , которая выравнена со звездочкой на предыдущей строке.
Примеры:
```java
/*
 * Вот так                     
 * нормально писать.                     
 */

// И так
// тоже.

/* И даже так
 * можно писать. */
```
Комменты не должны быть заключены в прямоугольники из звездочек или других символов.
Совет: когда пишете многострочные комментарии, используйте стиль /* ... */ для того, чтобы автоматические форматтеры переносили строки когда нужно. Большинство форматтеров не могут сделать такое для //... 

#### Модификаторы

Модификаторы классов и методов ставятся в порядке, рекомендованном Java Language Specification:

*public protected private abstract default static final transient volatile synchronized native strictfp*

#### Числовые литералы

Числовые литералы типа long используют большую букву L, никогда не используется нижний регистр (чтобы избежать путаницы с цифрой 1). Например, надо писать 3000000000L вместо 3000000000l.

## Именование

#### Правила для всех идентификаторов

Для идентификаторов используются только ASCII буквы и цифры, и в малом числе исключений, нижнее подчеркивание ( _ ). Поэтому каждое валидное имя идентификатора попадает под регулярное выражение *\w+* .
В нашем стиле кодирования специальные префиксы и суффиксы, такие как *name_*, *mName*, *s_name* и *kName*, не используются.

#### Имена пакетов

Имена пакетов пишутся в нижнем регистре, слова просто последовательно конкатенируются друг с другом (без нижних подчеркиваний). Например, *com.example.deepspace*, а не *com.example.deepSpace* или *com.example.deep_space*.

#### Имена классов

Имена классов пишутся в *UpperCamelCase*.

Имена классов - это обычно существительные или фразы с существительными. Например, *Character* или *ImmutableList*. Интерфейсы тоже могут называться существительными или фразами с существительными (например, *List*), но иногда могут быть прилагательными или фразами с прилагательными (например, *Readable*).

Нет специальных правил для именования аннотаций.

Тестовые классы начинаются с имени класса, который они тестируют, и заканчиваются на *Test*. Например, *HashTest* или *HashIntegrationTest*.

#### Имена методов

Имена методов пишутся в *lowerCamelCase*.

Имена методов - обычно глаголы или фразы с глаголами. Например, *sendMessage* или *stop*.

Нижние подчеркивания могут быть в тестовых методах JUnit для отделения логических компонентов имени. Типичный шаблон - `test<MethodUnderTest>_<state>`, например *testPop_emptyStack*. Нет Одного Правильного Способа для именования тестовых методов.

#### Имена констант

Имена констант используют *CONSTANT_CASE*: все буквы в верхнем регистре, слова отделяются нижним подчеркиванием. Но что есть константа?

Каждая константа является static final полем, но не все static final поля являются константами. Прежде чем именовать переменную константой, подумайте реально ли она является ей. Например, если видимое состояние экземпляра обьекта может измениться, это точно не константа. Просто мысли о том, что объект не будет меняться в процессе работы, не достаточно. Примеры:

```java
// Константы
static final int NUMBER = 5;
static final ImmutableList<String> NAMES = ImmutableList.of("Ed", "Ann");
static final Joiner COMMA_JOINER = Joiner.on(','); // потому что Joiner - immutable
static final SomeMutableType[] EMPTY_ARRAY = {};
enum SomeEnum { ENUM_CONSTANT }

// Не константы
static String nonFinal = "non-final";
final String nonStatic = "non-static";
static final Set<String> mutableCollection = new HashSet<String>();
static final ImmutableSet<SomeMutableType> mutableElements = ImmutableSet.of(mutable);
static final Logger logger = Logger.getLogger(MyClass.getName());
static final String[] nonEmptyArray = {"these", "can", "change"};
```
Имена констант - обычно существительные или фразы с существительными.

#### Имена неконстантных полей

Не константы (static или нет) пишутся в *lowerCamelCase*. Эти имена - обычно существительные или фразы с существительными. Например, *computedValues* или *index*.

#### Имена параметров

Имена параметров пишутся в *lowerCamelCase*. Односимвольных имен параметров в публичных методах следует избегать.

#### Имена локальных переменных

Имена локальных переменных пишутся в *lowerCamelCase*. Даже когда они final и immutable, локальные переменные не надо путать с константами, и именовать их как константы.

#### Имена переменных типов

Каждая переменная типа именуется одним из 2 способов:
* Одна большая буква, опционально с цифрой после (например E, T, X, T2).
* Имя, похожее на имя класса, с буквой T после (например: RequestT, FooBarT).

#### Определение Camel Case

Иногда есть несколько способов конвертировать английскую фразу в camel case, например для аббревиатур или необычных имен типа "IPv6" или "iOS". Чтобы имя было предсказуемо, предлагается такая схема.
* Берем имя или фразу
* Конвертируем её в набор ASCII символов и убираем все апострофы. Например, "Müller's algorithm" может стать "Muellers algorithm".
* Делим результат на слова, разбивая по пробелам или другим знакам препинания (обычно дефисы). Если одно из слов уже употребляется в camel case, разбиваем его (например, AdWords становится ad words). Слова типа "iOS" не попадают под это правило, потому что они на самом деле не camel case.
* Приводим все к нижнему регистру (включая аббревиатуры), затем приводим первую букву к верхнему регистру:
  * каждого слова, если хотим получить upper camel case
  * каждого слова, кроме первого, если хотим получить lower camel case
* Наконец, соединяем все слова, получая результирующее имя.

Примите во внимание, что регистр букв в оригинальных словах не принимается во внимание. Примеры:

Оригинальная фраза | Правильно | Неправильно
------------ | ------------- | -------------
"XML HTTP request" | XmlHttpRequest | XMLHTTPRequest
"new customer ID" | newCustomerId | newCustomerID
"inner stopwatch" | innerStopwatch | innerStopWatch
"supports IPv6 on iOS?" | supportsIpv6OnIos | supportsIPv6OnIOS
"YouTube importer" | YouTubeImporter | YoutubeImporter

Примечание: некоторые слова имеют несколько написаний через дефис в английском языке, например оба "nonempty" и "non-empty" верны. Таким образом, имена методов *checkNonempty* и *checkNonEmpty* тоже оба верны.

## Практики программирования

#### @Override: всегда используется

Метод всегда помечается аннотацией @Override, если это верно с точки зрения языка. Это включает в себя:
* Метод класса, переопределяющий метод суперкласса
* Метод класса, реализующий метод интерфейса
* Метод интерфейса, переопределяющий метод суперинтерфейса

Исключение: @Override можно опустить, если метод родителя обьявлен как @Deprecated.

#### Перехватываемые исключения: не игнорируются

Кроме случая, описываемого ниже, очень редко является корректным ничего не делать в ответ на перехваченное исключение (типовые способы: логгировать его, если же обьявлено "невозможным", выбросить заново как AssertionError).

Когда реально ничего не надо делать в catch-блоке, причина указывается в поясняющем комментарии:
```java
try {
  int i = Integer.parseInt(response);
  return handleNumericResponse(i);
} catch (NumberFormatException ok) {
  // it's not numeric; that's fine, just continue
}
return handleTextResponse(response);
```

Исключение: в тестах, перехваченное исключение может игнорироваться без комментария, если имя переменной начинается с *expected*. Это часто встречается, когда надо убедиться, что тестируемый код бросает исключение нужного типа, поэтому комментарий излишен.
```java
try {
  emptyStack.pop();
  fail();
} catch (NoSuchElementException expected) {
}
```

#### Статические члены класса: указываются через имя класса

Когда нужно указать или сослаться на статическое поле или метод класса, оно определяется с именем класса, а не с ссылкой на обьект или выражение, возвращающее объект типа этого класса.

```java
Foo aFoo = ...;
Foo.aStaticMethod(); // good
aFoo.aStaticMethod(); // bad
somethingThatYieldsAFoo().aStaticMethod(); // very bad
```

#### Finalizers: не используются

Очень редко надо переопределять `Object.finalize`.

Совет: не делайте этого. Если позарез надо, сначала прочитайте и поймите [Effective Java](http://books.google.com/books?isbn=8131726592) Item 7, "Avoid Finalizers", а после этого *не делайте* этого.


