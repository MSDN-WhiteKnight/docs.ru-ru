---
description: -reference (параметры компилятора C#)
title: -reference (параметры компилятора C#)
ms.date: 07/20/2015
f1_keywords:
- /reference
helpviewer_keywords:
- /r compiler option [C#]
- reference compiler option [C#]
- r compiler option [C#]
- /reference compiler option [C#]
- -r compiler option [C#]
- metadata import [C#]
- public type information [C#]
- -reference compiler option [C#]
ms.assetid: 8d13e5b0-abf6-4c46-bf71-2daf2cd0a6c4
ms.openlocfilehash: cd7346ae4094a84a398306394f771e040dd7b72f
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/24/2020
ms.locfileid: "91193794"
---
# <a name="-reference-c-compiler-options"></a>-reference (параметры компилятора C#)

Параметр **-reference** предписывает компилятору импортировать сведения типа [public](../keywords/public.md) из указанного файла в текущий проект. Это позволяет ссылаться на метаданные из указанных файлов сборки.  
  
## <a name="syntax"></a>Синтаксис  
  
```console  
-reference:[alias=]filename  
-reference:filename  
```  
  
## <a name="arguments"></a>Аргументы  

 `filename`  
 Имя файла, содержащего манифест сборки. Чтобы импортировать данные из нескольких файлов, включите отдельный параметр **-reference** для каждого файла.  
  
 `alias`  
 Допустимый идентификатор C#, который представляет корневое пространство имен, содержащее все пространства имен сборки.  
  
## <a name="remarks"></a>Remarks  

 Чтобы импортировать данные из нескольких файлов, включите параметр **-reference** для каждого файла.  
  
 Импортируемые файлы должны содержать манифест; выходной файл следует компилировать с одним из параметров [-target](./target-compiler-option.md), отличных от [-target:module](./target-module-compiler-option.md).  
  
 **-r** является краткой формой **-reference**.  
  
 Для импорта метаданных из выходного файла, который не содержит манифест сборки, используется параметр [-addmodule](./addmodule-compiler-option.md).  
  
 При ссылке на сборку (сборку А), которая, в свою очередь, ссылается на другую сборку (сборку Б), необходимо ссылаться на сборку Б в указанных ниже случаях.  
  
- Используемый в сборке A тип наследует от типа сборки Б или реализует интерфейс из этой сборки.  
  
- Вызывается поле, свойство, событие или метод, которые имеют тип возвращаемых данных или тип параметра из сборки Б.  
  
 Для указания каталога, где находится одна или несколько ссылок на сборки, используется параметр [-lib](./lib-compiler-option.md). В разделе, посвященном параметру **-lib**, также рассматриваются каталоги, в которых компилятор ищет сборки.  
  
 Чтобы компилятор мог распознавать тип в сборке (не в модуле), ему следует указать принудительно разрешать типы. Это можно сделать, определив экземпляр типа. Возможны и другие способы разрешения компилятором имен типов в сборке. Например, если тип наследуется от типа в сборке, его имя будет распознаваться компилятором.  
  
 Иногда бывает необходимо сослаться на две различные версии одного компонента из одной сборки. Для этого следует использовать подпараметр псевдонима для параметра **-reference** для каждого файла, чтобы различать два этих файла. Этот псевдоним используется в качестве квалификатора имени компонента и разрешается в компонент в одном из файлов.  
  
 Файл ответов csc (RSP-файл), который ссылается на часто используемые сборки .NET Framework, используется по умолчанию. Параметр [-noconfig](./noconfig-compiler-option.md) позволяет запретить компилятору использовать файл csc.rsp.  
  
> [!NOTE]
> В Visual Studio используйте диалоговое окно **Добавление ссылки**. Для получения дополнительной информации см. [Практическое руководство. Добавление и удаление ссылок с помощью диспетчера ссылок](/visualstudio/ide/how-to-add-or-remove-references-by-using-the-reference-manager). Чтобы обеспечить эквивалентное поведение при добавлении ссылок с помощью `-reference` и диалогового окна **Добавление ссылки**, свойству **Внедрить типы взаимодействия** добавляемой сборки должно быть задано значение **False**. **True** является значением по умолчанию для этого свойства.  
  
## <a name="example"></a>Пример  

 В этом примере показано, как использовать [псевдоним extern](../keywords/extern-alias.md).  
  
 В примере компилируется файл исходного кода и импортируются метаданные из сборок `grid.dll` и `grid20.dll`, которые были скомпилированы ранее. Два DLL-файла содержат разные версии одного компонента, поэтому для компиляции файла исходного кода используются два параметра **-reference** с подпараметрами псевдонимов. Параметры выглядят следующим образом:  

```console
-reference:GridV1=grid.dll -reference:GridV2=grid20.dll  
```
  
 При этом устанавливаются внешние псевдонимы `GridV1` и `GridV2`, которые используются посредством оператора `extern`.  
  
```csharp  
extern alias GridV1;  
extern alias GridV2;  
// Using statements go here.  
```  
  
 После выполнения описанных выше действий можно ссылаться на элемент управления "сетка" из файла `grid.dll`, предваряя имя элемента управления префиксом `GridV1`, как показано в следующем коде.  
  
```csharp  
GridV1::Grid  
```  
  
 Кроме того, можно ссылаться на элемент управления "сетка" из файла `grid20.dll`, предваряя имя элемента управления префиксом `GridV2`, как показано в следующем коде.  
  
```csharp  
GridV2::Grid
```  
  
## <a name="see-also"></a>См. также раздел

- [Параметры компилятора C# ](./index.md)
- [Управление свойствами проектов и решений](/visualstudio/ide/managing-project-and-solution-properties)
