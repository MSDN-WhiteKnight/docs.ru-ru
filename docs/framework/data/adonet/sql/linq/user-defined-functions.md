---
title: Определяемые пользователем функции
ms.date: 03/30/2017
ms.assetid: 3304c9b2-5c7a-4a95-9d45-4f260dcb606e
ms.openlocfilehash: 061e07ba91a1742c90a594bf42f12e64172b2481
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/24/2020
ms.locfileid: "91164120"
---
# <a name="user-defined-functions"></a>Определяемые пользователем функции

[!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] использует методы в объектной модели для представления пользовательских функций. Чтобы назначить методы в качестве функций, следует применить атрибут <xref:System.Data.Linq.Mapping.FunctionAttribute>, а где необходимо - атрибут <xref:System.Data.Linq.Mapping.ParameterAttribute>. Дополнительные сведения см. [в разделе Объектная модель LINQ to SQL](the-linq-to-sql-object-model.md).  
  
 Чтобы избежать <xref:System.InvalidOperationException>, пользовательские функции в [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] должны быть представлены в одной из следующих форм.  
  
- Функция, упакованная в качестве вызова метода с правильными атрибутами сопоставления. Дополнительные сведения см. в разделе [сопоставление на основе атрибутов](attribute-based-mapping.md).  
  
- Статический метод SQL, характерный для [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)].  
  
- Функция, поддерживаемая методом .NET Framework.  
  
 В темах данного раздела показано формирование и вызов этих методов в приложении при самостоятельном написании кода. Разработчики, использующие Visual Studio, обычно используют реляционный конструктор объектов для соотнесения определяемых пользователем функций.  
  
## <a name="in-this-section"></a>в этом разделе  

 [Практическое руководство. Как применять определяемые пользователем скалярные функции](how-to-use-scalar-valued-user-defined-functions.md)  
 Описано, как реализовать функцию, возвращающую скалярные значения.  
  
 [Практическое руководство. Как применять определяемые пользователем возвращающие табличное значение функции](how-to-use-table-valued-user-defined-functions.md)  
 Содержит описание способов реализации функции, возвращающей табличные значения.  
  
 [Практическое руководство. Как вызывать встроенные определяемые пользователем функции](how-to-call-user-defined-functions-inline.md)  
 Содержит описание преобразования встроенных вызовов в функции и представляет различия при внутреннем осуществлении вызова.
