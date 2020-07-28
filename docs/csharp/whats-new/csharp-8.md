---
title: Новые возможности в C# 8.0. Руководство по языку C#
description: Обзор новых функций, доступных в C# 8.0.
ms.date: 04/07/2020
ms.openlocfilehash: 14df381e17fe89bd862f97522c7efd814857e71e
ms.sourcegitcommit: 0fa2b7b658bf137e813a7f4d09589d64c148ebf5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/14/2020
ms.locfileid: "86309408"
---
# <a name="whats-new-in-c-80"></a>Новые возможности C# 8.0

В C# 8.0 добавлены следующие функции и улучшения языка C#:

- [Члены только для чтения](#readonly-members)
- [Методы интерфейса по умолчанию](#default-interface-methods)
- [Улучшения сопоставления шаблонов](#more-patterns-in-more-places):
  - [выражения switch](#switch-expressions);
  - [шаблоны свойств](#property-patterns);
  - [шаблоны кортежей](#tuple-patterns);
  - [позиционные шаблоны](#positional-patterns).
- [Объявления using](#using-declarations).
- [Статические локальные функции](#static-local-functions).
- [Удаляемые ссылочные структуры](#disposable-ref-structs).
- [Ссылочные типы, допускающие значение NULL](#nullable-reference-types)
- [Асинхронные потоки](#asynchronous-streams).
- [Асинхронные высвобождаемые типы](#asynchronous-disposable)
- [Индексы и диапазоны](#indices-and-ranges).
- [Присваивание объединения со значением NULL](#null-coalescing-assignment)
- [Неуправляемые сконструированные типы](#unmanaged-constructed-types)
- [Выражение stackalloc во вложенных выражениях](#stackalloc-in-nested-expressions)
- [Улучшение буквальных интерполированных строк](#enhancement-of-interpolated-verbatim-strings)

C# 8.0 поддерживается в **.NET Core 3.x** и **.NET Standard 2.1**. Дополнительные сведения см. в статье [Управление версиями языка C#](../language-reference/configure-language-version.md).

В остальных разделах этой статьи кратко описываются эти возможности. Здесь приведены ссылки на эти подробные руководства и обзоры (если они доступны). Эти функции можно изучить в своей среде с помощью глобального средства `dotnet try`:

1. Установите глобальное средство [dotnet-try](https://github.com/dotnet/try/blob/master/README.md#setup).
1. Клонируйте репозиторий [dotnet/try-samples](https://github.com/dotnet/try-samples).
1. Для репозитория *try-samples* установите в качестве текущего каталога подкаталог *csharp8*.
1. Запустите `dotnet try`.

## <a name="readonly-members"></a>Члены только для чтения

Модификатор `readonly` можно применить к членам структуры. Он означает, что член не изменяет состояние. Это более детализированный способ, чем применение модификатора `readonly` в объявлении `struct`.  Рассмотрим следующую изменяемую структуру:

```csharp
public struct Point
{
    public double X { get; set; }
    public double Y { get; set; }
    public double Distance => Math.Sqrt(X * X + Y * Y);

    public override string ToString() =>
        $"({X}, {Y}) is {Distance} from the origin";
}
```

Как и большинство структур, метод `ToString()` не изменяет состояние. Это можно указать, добавив модификатор `readonly` в объявление `ToString()`:

```csharp
public readonly override string ToString() =>
    $"({X}, {Y}) is {Distance} from the origin";
```

Предыдущее изменение создает предупреждение компилятора, так как `ToString` обращается к свойству `Distance`, которое не помечено как `readonly`:

```console
warning CS8656: Call to non-readonly member 'Point.Distance.get' from a 'readonly' member results in an implicit copy of 'this'
```

Компилятор выдает предупреждение, когда ему требуется создать защитную копию.  Свойство `Distance` не изменяет состояние, поэтому вы можете устранить это предупреждение, добавив модификатор `readonly` в объявление:

```csharp
public readonly double Distance => Math.Sqrt(X * X + Y * Y);
```

Обратите внимание, что модификатор `readonly` необходим для свойства только для чтения. Компилятор не предполагает, что методы доступа `get` не изменяют состояние; `readonly` необходимо объявить явно. Автоматические реализуемые свойства являются исключением; компилятор будет рассматривать все автоматические реализуемые методы получения как `readonly`, поэтому не нужно добавлять модификатор `readonly` к свойствам `X` и `Y`.

Компилятор принудительно применяет правило, в соответствии с которым члены `readonly` не изменяют состояние. Следующий метод не будет компилироваться, если не удалить модификатор `readonly`:

```csharp
public readonly void Translate(int xOffset, int yOffset)
{
    X += xOffset;
    Y += yOffset;
}
```

Эта функция позволяет указать намерение вашего проекта, чтобы компилятор мог применить его и выполнить оптимизацию на основе намерения.

Дополнительные сведения см. в разделе о [членах экземпляров `readonly`](../language-reference/builtin-types/struct.md#readonly-instance-members) в статье [Типы структур](../language-reference/builtin-types/struct.md).

## <a name="default-interface-methods"></a>Методы интерфейса по умолчанию

Теперь вы можете добавлять члены в интерфейсы и предоставлять реализацию для этих членов. Эта возможность языка позволяет разработчикам API добавлять методы в интерфейс в более поздних версиях, не нарушая исходный код или совместимость на уровне двоичного кода с существующими реализациями этого интерфейса. Существующие реализации *наследуют* реализацию по умолчанию. Эта функция также позволяет C# взаимодействовать с API-интерфейсами, предназначенными для Android или Swift, которые поддерживают аналогичные функциональные возможности. Методы интерфейса по умолчанию также поддерживают сценарии, аналогичные функции языка "признаки".

Методы интерфейса по умолчанию влияют на многие сценарии и элементы языка. В нашем первом учебнике рассматривается [изменение интерфейса с помощью реализаций по умолчанию](../tutorials/default-interface-methods-versions.md). Выпуск общедоступных версий обновлений других руководств и справочника ожидается в ближайшее время.

## <a name="more-patterns-in-more-places"></a>Дополнительные шаблоны в нескольких расположениях

Возможность **сопоставления шаблонов** позволяет работать с шаблонами в зависимости от формата в связанных, но различных типах данных. В C# 7.0 появился синтаксис для шаблонов типа и шаблонов константы, использующий выражение [`is`](../language-reference/keywords/is.md) и инструкцию [`switch`](../language-reference/keywords/switch.md). Эти функции представляют первые пробные шаги на пути к поддержке парадигм программирования, где данные и функции разделены. По мере того как отрасль переходит на использование микрослужб и других облачных архитектур, необходимы также средства других языков.

В C# 8.0 расширены эти возможности, так что вы можете использовать дополнительные выражения шаблонов в нескольких расположениях в коде. Учитывайте эти функции, когда данные и функциональные возможности представлены отдельно. Рассмотрите возможность сопоставления шаблонов, когда алгоритмы зависят от фактов, отличных от типа среды выполнения объекта. Эти методы предоставляют другой способ выражения проектов.

Помимо новых шаблонов в новых расположениях, в C# 8.0 добавлены **рекурсивные шаблоны**. Результат выражения любого шаблона — это выражение языка. Рекурсивный шаблон является просто выражением шаблона, которое применяется к результатам другого выражения шаблона.

### <a name="switch-expressions"></a>Выражения switch

Часто инструкция [`switch`](../language-reference/keywords/switch.md) возвращает значение в каждом из блоков `case`. **Выражения switch** позволяет использовать более краткий синтаксис выражения. В нем меньше повторяющихся ключевых слов `case` и `break`, а также меньше фигурных скобок.  Например, рассмотрим следующее перечисление, которое выводит список цветов радуги:

```csharp
public enum Rainbow
{
    Red,
    Orange,
    Yellow,
    Green,
    Blue,
    Indigo,
    Violet
}
```

Если для приложения определен тип `RGBColor`, который создается на основе компонентов `R`, `G` и `B`, вы можете преобразовать значение `Rainbow` в RGB-значение, используя следующий метод, содержащий выражение switch:

```csharp
public static RGBColor FromRainbow(Rainbow colorBand) =>
    colorBand switch
    {
        Rainbow.Red    => new RGBColor(0xFF, 0x00, 0x00),
        Rainbow.Orange => new RGBColor(0xFF, 0x7F, 0x00),
        Rainbow.Yellow => new RGBColor(0xFF, 0xFF, 0x00),
        Rainbow.Green  => new RGBColor(0x00, 0xFF, 0x00),
        Rainbow.Blue   => new RGBColor(0x00, 0x00, 0xFF),
        Rainbow.Indigo => new RGBColor(0x4B, 0x00, 0x82),
        Rainbow.Violet => new RGBColor(0x94, 0x00, 0xD3),
        _              => throw new ArgumentException(message: "invalid enum value", paramName: nameof(colorBand)),
    };
```

Здесь представлено несколько улучшений синтаксиса:

- Переменная расположена перед ключевым словом `switch`. Другой порядок позволяет визуально легко отличить выражение switch от инструкции switch.
- Элементы `case` и `:` заменяются на `=>`. Это более лаконично и интуитивно понятно.
- Случай `default` заменяется пустой переменной `_`.
- Тексты являются выражениями, а не инструкциями.

Сравните это с эквивалентным кодом, где используется классическая инструкция `switch`:

```csharp
public static RGBColor FromRainbowClassic(Rainbow colorBand)
{
    switch (colorBand)
    {
        case Rainbow.Red:
            return new RGBColor(0xFF, 0x00, 0x00);
        case Rainbow.Orange:
            return new RGBColor(0xFF, 0x7F, 0x00);
        case Rainbow.Yellow:
            return new RGBColor(0xFF, 0xFF, 0x00);
        case Rainbow.Green:
            return new RGBColor(0x00, 0xFF, 0x00);
        case Rainbow.Blue:
            return new RGBColor(0x00, 0x00, 0xFF);
        case Rainbow.Indigo:
            return new RGBColor(0x4B, 0x00, 0x82);
        case Rainbow.Violet:
            return new RGBColor(0x94, 0x00, 0xD3);
        default:
            throw new ArgumentException(message: "invalid enum value", paramName: nameof(colorBand));
    };
}
```

### <a name="property-patterns"></a>Шаблоны свойств

**Шаблон свойств** позволяет сопоставлять свойства исследуемого объекта. Рассмотрим сайт электронной коммерции, на котором должен вычисляться налог с продаж по адресу покупателя. Это вычисление не является основной задачей класса `Address`. Оно меняется со временем и, скорее всего, чаще, чем изменения формата адреса. Сумма налога с продаж зависит от свойства `State` адреса. В следующем методе используется шаблон свойства для вычисления налога с продаж по адресу и цене:

```csharp
public static decimal ComputeSalesTax(Address location, decimal salePrice) =>
    location switch
    {
        { State: "WA" } => salePrice * 0.06M,
        { State: "MN" } => salePrice * 0.075M,
        { State: "MI" } => salePrice * 0.05M,
        // other cases removed for brevity...
        _ => 0M
    };
```

При сопоставлении шаблонов создается сокращенный синтаксис для выражения этого алгоритма.

### <a name="tuple-patterns"></a>Шаблоны кортежей

Некоторые алгоритмы зависят от нескольких наборов входных данных. **Шаблоны кортежей** позволяют переключаться между несколькими значениями, выраженными как [кортежи](../language-reference/builtin-types/value-tuples.md).  В следующем примере кода показано выражение switch для игры *камень, ножницы, бумага*:

```csharp
public static string RockPaperScissors(string first, string second)
    => (first, second) switch
    {
        ("rock", "paper") => "rock is covered by paper. Paper wins.",
        ("rock", "scissors") => "rock breaks scissors. Rock wins.",
        ("paper", "rock") => "paper covers rock. Paper wins.",
        ("paper", "scissors") => "paper is cut by scissors. Scissors wins.",
        ("scissors", "rock") => "scissors is broken by rock. Rock wins.",
        ("scissors", "paper") => "scissors cuts paper. Scissors wins.",
        (_, _) => "tie"
    };
```

В сообщении указан победитель. В случае отклонения представляются три комбинации, ведущие к ничьей, или другие текстовые входные данные.

### <a name="positional-patterns"></a>Позиционные шаблоны

Некоторые типы включают метод `Deconstruct`, свойства которого деконструируются на дискретные переменные. Если метод `Deconstruct` доступен, можно использовать **позиционные шаблоны** для проверки свойств объекта и использовать эти свойства для шаблона.  Рассмотрим следующий класс `Point`, который содержит метод `Deconstruct` для создания дискретных переменных для `X` и `Y`:

```csharp
public class Point
{
    public int X { get; }
    public int Y { get; }

    public Point(int x, int y) => (X, Y) = (x, y);

    public void Deconstruct(out int x, out int y) =>
        (x, y) = (X, Y);
}
```

Кроме того, нужно учитывать следующее перечисление, представляющее различные позиции квадранта:

```csharp
public enum Quadrant
{
    Unknown,
    Origin,
    One,
    Two,
    Three,
    Four,
    OnBorder
}
```

В следующем методе используется **позиционный шаблон** для извлечения значений `x` и `y`. Затем используется предложение `when` для определения `Quadrant` точки:

```csharp
static Quadrant GetQuadrant(Point point) => point switch
{
    (0, 0) => Quadrant.Origin,
    var (x, y) when x > 0 && y > 0 => Quadrant.One,
    var (x, y) when x < 0 && y > 0 => Quadrant.Two,
    var (x, y) when x < 0 && y < 0 => Quadrant.Three,
    var (x, y) when x > 0 && y < 0 => Quadrant.Four,
    var (_, _) => Quadrant.OnBorder,
    _ => Quadrant.Unknown
};
```

Шаблон пустой переменной в предыдущем операторе switch совпадает с выражением, если `x` или `y`, но не оба, имеет значение 0. Выражение switch должно создавать значение или исключение. Если ни один из вариантов не совпадает, выражение switch создает исключение. Компилятор создает предупреждение, если в выражении switch не охватываются все возможные случаи.

Ознакомиться с методами сопоставления шаблонов можно в [этом подробном учебнике](../tutorials/pattern-matching.md).

## <a name="using-declarations"></a>Объявления using

**Объявление using** — это объявление переменной, которому предшествует ключевое слово `using`. Оно сообщает компилятору, что объявляемая переменная должна быть удалена в конце области видимости. Для примера рассмотрим следующий код, в котором записывается текстовый файл:

```csharp
static int WriteLinesToFile(IEnumerable<string> lines)
{
    using var file = new System.IO.StreamWriter("WriteLines2.txt");
    // Notice how we declare skippedLines after the using statement.
    int skippedLines = 0;
    foreach (string line in lines)
    {
        if (!line.Contains("Second"))
        {
            file.WriteLine(line);
        }
        else
        {
            skippedLines++;
        }
    }
    // Notice how skippedLines is in scope here.
    return skippedLines;
    // file is disposed here
}
```

В приведенном выше примере файл удаляется при достижении закрывающей фигурной скобки метода. Это конец области, в котором объявляется `file`. Приведенный выше код эквивалентен следующему коду, в котором используется классическая [инструкция using](../language-reference/keywords/using-statement.md):

```csharp
static int WriteLinesToFile(IEnumerable<string> lines)
{
    // We must declare the variable outside of the using block
    // so that it is in scope to be returned.
    int skippedLines = 0;
    using (var file = new System.IO.StreamWriter("WriteLines2.txt"))
    {
        foreach (string line in lines)
        {
            if (!line.Contains("Second"))
            {
                file.WriteLine(line);
            }
            else
            {
                skippedLines++;
            }
        }
    } // file is disposed here
    return skippedLines;
}
```

В приведенном выше примере файл удаляется при достижении закрывающей фигурной скобки, связанной с инструкцией `using`.

В обоих случаях компилятор создает вызов метода `Dispose()`. Компилятор создает ошибку, если выражение в инструкции `using` нельзя удалить.

## <a name="static-local-functions"></a>Статические локальные функции

Теперь вы можете добавить модификатор `static` для локальных функций, чтобы убедиться, что локальная функция не захватывает (не ссылается на) какие-либо переменные из области видимости. Это приводит к возникновению ошибки `CS8421`: "A static local function can't contain a reference to \<variable>" (Статическая локальная функция не может содержать ссылку на \<variable>).

Рассмотрим следующий код. Локальная функция `LocalFunction` обращается к переменной `y`, объявленной в области видимости (метод `M`). Таким образом `LocalFunction` не может объявляться с помощью модификатора `static`:

```csharp
int M()
{
    int y;
    LocalFunction();
    return y;

    void LocalFunction() => y = 0;
}
```

Следующий код содержит статическую локальную функцию. Она может быть статической, так как она не обращается ко всем переменным в области видимости:

```csharp
int M()
{
    int y = 5;
    int x = 7;
    return Add(x, y);

    static int Add(int left, int right) => left + right;
}
```

## <a name="disposable-ref-structs"></a>Удаляемые ссылочные структуры

Объект `struct`, объявленный с помощью модификатора `ref`, не может реализовывать интерфейсы и поэтому не может реализовать <xref:System.IDisposable>. Таким образом, чтобы объект `ref struct` можно было удалить, у него должен быть доступный метод `void Dispose()`. Эта функция также применима к объявлениям `readonly ref struct`.

## <a name="nullable-reference-types"></a>Ссылочные типы, допускающие значение null

Внутри контекста заметок, допускающих значение null, любая переменная ссылочного типа считается переменной **ссылочного типа, не допускающего значения null**. Если вы хотите указать, что переменная может принимать значение null, необходимо добавить к имени типа `?`, чтобы объявить переменную как переменную **ссылочного типа, допускающего значения null**.

Для ссылочных типов, не допускающих значение null, компилятор использует анализ потока, чтобы убедиться, что локальные переменные инициализируются в значение, отличное от null, при объявлении. Поля должны быть инициализированы во время построения. Компилятор создает предупреждение, если переменная не задается вызовом ни к одному из доступных конструкторов или инициализатором. Кроме того, ссылочным типам, не допускающим значение null, нельзя задать значение, которое может быть null.

Ссылочные типы, допускающие значение null, не проверяются на предмет того, назначено ли им значение null или получили ли они его после инициализации. Тем не менее, компилятор использует анализ потока, чтобы убедиться, что любая переменная ссылочного типа, допускающего значение null, проверяется на наличие значения null, прежде чем обратиться к ней или назначить ей ссылочный тип, не допускающий значение null.

Дополнительные сведения о функции см. в обзоре [ссылочных типов, допускающих значение null](../nullable-references.md). Попробуйте самостоятельно применить эту возможность в новом приложении в этом [руководстве по ссылочным типам, допускающим значение null](../tutorials/nullable-reference-types.md). Дополнительные сведения о шагах переноса существующей базы кода для использования ссылочных типов, допускающих значение null, см. в [этом руководстве](../tutorials/upgrade-to-nullable-references.md).

## <a name="asynchronous-streams"></a>Асинхронные потоки

Начиная с C# версии 8.0 можно создавать и использовать потоки асинхронно. В методе, который возвращает асинхронный поток, есть три свойства:

1. Он объявлен с помощью модификатора `async`.
1. Он возвращает интерфейс <xref:System.Collections.Generic.IAsyncEnumerable%601>.
1. Метод содержит инструкции `yield return` для возвращения последовательных элементов в асинхронном потоке.

Для использования асинхронного потока требуется добавить ключевое слово `await` перед ключевым словом `foreach` при перечислении элементов потока. Для добавления ключевого слова `await` требуется, чтобы метод, который перечисляет асинхронный поток, был объявлен с помощью модификатора `async` и возвращал тип, допустимый для метода `async`. Обычно это означает возвращение структуры <xref:System.Threading.Tasks.Task> или <xref:System.Threading.Tasks.Task%601>. Это также может быть структура <xref:System.Threading.Tasks.ValueTask> или <xref:System.Threading.Tasks.ValueTask%601>. Метод может использовать и создавать асинхронный поток. Это означает, что будет возвращен интерфейс <xref:System.Collections.Generic.IAsyncEnumerable%601>. Следующий код создает последовательность чисел от 0 до 19 с интервалом 100 мс между генерированием каждого числа:

```csharp
public static async System.Collections.Generic.IAsyncEnumerable<int> GenerateSequence()
{
    for (int i = 0; i < 20; i++)
    {
        await Task.Delay(100);
        yield return i;
    }
}
```

Вы бы перечислили последовательность с использованием инструкции `await foreach`:

```csharp
await foreach (var number in GenerateSequence())
{
    Console.WriteLine(number);
}
```

Вы можете попробовать асинхронные потоки самостоятельно в нашем руководстве по [созданию и использованию асинхронных потоков](../tutorials/generate-consume-asynchronous-stream.md). Элементы потока по умолчанию обрабатываются в захваченном контексте. Чтобы отключить захват контекста, используйте метод расширения <xref:System.Threading.Tasks.TaskAsyncEnumerableExtensions.ConfigureAwait%2A?displayProperty=nameWithType>. Дополнительные сведения о контекстах синхронизации и захвате текущего контекста см. в [статье](../../standard/asynchronous-programming-patterns/consuming-the-task-based-asynchronous-pattern.md), посвященной использованию асинхронной модели на основе задач.

## <a name="asynchronous-disposable"></a>Асинхронные высвобождаемые типы

Начиная с C# 8.0 язык поддерживает асинхронные освобождаемые типы, реализующие интерфейс <xref:System.IAsyncDisposable?displayProperty=nameWithType>. Для работы с асинхронно освобождаемым объектами используется инструкция `await using`. Дополнительные сведения см. в статье [Реализация метода DisposeAsync](../../standard/garbage-collection/implementing-disposeasync.md).

## <a name="indices-and-ranges"></a>Индексы и диапазоны

Диапазоны и индексы обеспечивают лаконичный синтаксис для доступа к отдельным элементам или диапазонам в последовательности.

Поддержка языков опирается на два новых типа и два новых оператора:

- <xref:System.Index?displayProperty=nameWithType> представляет индекс в последовательности.
- Оператор `^` (индекс с конца), который указывает, что индекс указан относительно конца последовательности.
- <xref:System.Range?displayProperty=nameWithType> представляет вложенный диапазон последовательности.
- Оператор диапазона `..`, который задает начало и конец диапазона в качестве своих операндов.

Начнем с правил для использования в индексах. Рассмотрим массив `sequence`. Индекс `0` совпадает с `sequence[0]`. Индекс `^0` совпадает с `sequence[sequence.Length]`. Обратите внимание, что `sequence[^0]` создает исключение так же, как и `sequence[sequence.Length]`. Для любого числа `n` индекс `^n` совпадает с `sequence.Length - n`.

Диапазон указывает *начало* и *конец* диапазона. Начало диапазона является включающим, но конец диапазона является исключающим, то есть *начало* включается в диапазон, а *конец* не включается. Диапазон `[0..^0]` представляет весь диапазон так же, как `[0..sequence.Length]` представляет весь диапазон.

Рассмотрим несколько примеров. Обратите внимание на следующий массив, который помечен индексом от начала и от конца:

```csharp
var words = new string[]
{
                // index from start    index from end
    "The",      // 0                   ^9
    "quick",    // 1                   ^8
    "brown",    // 2                   ^7
    "fox",      // 3                   ^6
    "jumped",   // 4                   ^5
    "over",     // 5                   ^4
    "the",      // 6                   ^3
    "lazy",     // 7                   ^2
    "dog"       // 8                   ^1
};              // 9 (or words.Length) ^0
```

Вы можете получить последнее слово с помощью индекса `^1`:

```csharp
Console.WriteLine($"The last word is {words[^1]}");
// writes "dog"
```

Следующий код создает поддиапазон со словами "quick", "brown" и "fox". Он включает в себя элементы от `words[1]` до `words[3]`. Элемент `words[4]` в диапазон не входит.

```csharp
var quickBrownFox = words[1..4];
```

Следующий код создает поддиапазон со словами "lazy" и "dog". Он включает элементы `words[^2]` и `words[^1]`. Конечный индекс `words[^0]` не включен:

```csharp
var lazyDog = words[^2..^0];
```

В следующих примерах создаются диапазоны, которые должны быть открыты для начала, конца или в обоих случаях:

```csharp
var allWords = words[..]; // contains "The" through "dog".
var firstPhrase = words[..4]; // contains "The" through "fox"
var lastPhrase = words[6..]; // contains "the", "lazy" and "dog"
```

Вы также можете объявить диапазоны как переменные:

```csharp
Range phrase = 1..4;
```

Затем вы можете использовать диапазон внутри символов `[` и `]`:

```csharp
var text = words[phrase];
```

Индексы и диапазоны поддерживаются не только массивами. Можно также использовать индексы и диапазоны со [строкой](../language-reference/builtin-types/reference-types.md#the-string-type) (<xref:System.Span%601> или <xref:System.ReadOnlySpan%601>). Дополнительные сведения см. в разделе [Поддержка типа для индексов и диапазонов](../tutorials/ranges-indexes.md#type-support-for-indices-and-ranges).

Вы можете изучить сведения об индексах и диапазонах адресов в руководстве [Индексы и диапазоны](../tutorials/ranges-indexes.md).

## <a name="null-coalescing-assignment"></a>Присваивание объединения со значением NULL

В C# 8.0 появился оператор присваивания объединения со значением NULL `??=`. Оператор `??=` можно использовать для присваивания значения правого операнда левому операнду только в том случае, если левый операнд принимает значение `null`.

```csharp
List<int> numbers = null;
int? i = null;

numbers ??= new List<int>();
numbers.Add(i ??= 17);
numbers.Add(i ??= 20);

Console.WriteLine(string.Join(" ", numbers));  // output: 17 17
Console.WriteLine(i);  // output: 17
```

Дополнительные сведения см. в статье [Операторы ?? и ??=](../language-reference/operators/null-coalescing-operator.md).

## <a name="unmanaged-constructed-types"></a>Неуправляемые сконструированные типы

В C# 7.3 и более ранних версиях сконструированный тип (тип, содержащий по крайней мере один аргумент типа) не может быть [неуправляемым типом](../language-reference/builtin-types/unmanaged-types.md). Начиная с C# 8.0, сконструированный тип значения является неуправляемым, если он содержит поля исключительно неуправляемых типов.

Например, при наличии следующего определения универсального типа `Coords<T>`:

```csharp
public struct Coords<T>
{
    public T X;
    public T Y;
}
```

тип `Coords<int>` является неуправляемым в C# 8.0 и более поздних версиях. Как и для любого неуправляемого типа, вы можете создать указатель на переменную этого типа или [выделить блок памяти в стеке](../language-reference/operators/stackalloc.md) для экземпляров этого типа:

```csharp
Span<Coords<int>> coordinates = stackalloc[]
{
    new Coords<int> { X = 0, Y = 0 },
    new Coords<int> { X = 0, Y = 3 },
    new Coords<int> { X = 4, Y = 0 }
};
```

Дополнительные сведения см. в разделе [Неуправляемые типы](../language-reference/builtin-types/unmanaged-types.md).

## <a name="stackalloc-in-nested-expressions"></a>Выражение stackalloc во вложенных выражениях

Начиная с C# 8.0, если результат выражения [stackalloc](../language-reference/operators/stackalloc.md) имеет тип <xref:System.Span%601?displayProperty=nameWithType> или <xref:System.ReadOnlySpan%601?displayProperty=nameWithType>, можно использовать выражение `stackalloc` в других выражениях:

```csharp
Span<int> numbers = stackalloc[] { 1, 2, 3, 4, 5, 6 };
var ind = numbers.IndexOfAny(stackalloc[] { 2, 4, 6, 8 });
Console.WriteLine(ind);  // output: 1
```

## <a name="enhancement-of-interpolated-verbatim-strings"></a>Улучшение буквальных интерполированных строк

Порядок маркеров `$` и `@` в буквальных [интерполированных](../language-reference/tokens/interpolated.md) строках может быть любым: и `$@"..."`, и `@$"..."` являются допустимыми буквальными интерполированными строками. В более ранних версиях C# маркер `$` должен располагаться перед маркером `@`.
