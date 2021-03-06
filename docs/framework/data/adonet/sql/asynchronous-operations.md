---
title: Асинхронные операции
ms.date: 03/30/2017
ms.assetid: e7d32c3c-bf78-4bfc-a357-c9e82e4a4b3c
ms.openlocfilehash: f94a33b1ff06b5433f61687b8e28096ea37b412a
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/24/2020
ms.locfileid: "91197590"
---
# <a name="asynchronous-operations"></a>Асинхронные операции

Некоторые операции баз данных, например выполнение команд, могут занимать значительное количество времени. В таких случаях однопотоковые приложения должны блокировать другие операции и ожидать завершения команды перед возобновлением собственных операций. В противоположность этому при назначении длительной операции в фоновый поток основной поток может оставаться активным в течение всей операции. Например, в приложении Windows делегирование длительной операции в фоновый поток позволяет потоку пользовательского интерфейса оставаться в рабочем процессе во время выполнения операции.  
  
 .NET Framework предоставляет несколько асинхронных шаблонов конструирования, которые разработчики могут использовать для извлечения преимуществ из фоновых потоков и освобождает пользовательский интерфейс высокоприоритетных потоков от выполнения других операций. ADO.NET поддерживает те же шаблоны конструирования в его классе <xref:System.Data.SqlClient.SqlCommand>. В частности, методы <xref:System.Data.SqlClient.SqlCommand.BeginExecuteNonQuery%2A>, <xref:System.Data.SqlClient.SqlCommand.BeginExecuteReader%2A>и <xref:System.Data.SqlClient.SqlCommand.BeginExecuteXmlReader%2A>, связанные с методами <xref:System.Data.SqlClient.SqlCommand.EndExecuteNonQuery%2A>, <xref:System.Data.SqlClient.SqlCommand.EndExecuteReader%2A> и <xref:System.Data.SqlClient.SqlCommand.EndExecuteXmlReader%2A>, обеспечивают асинхронную поддержку.  
  
> [!NOTE]
> Асинхронное программирование является основной возможностью .NET Framework, а ADO.NET пользуется всеми преимуществами стандартных шаблонов конструирования. Дополнительные сведения о разных видах асинхронной техники, доступных разработчикам, см. в статье [Асинхронный вызов синхронных методов](../../../../standard/asynchronous-programming-patterns/calling-synchronous-methods-asynchronously.md).  
  
 Хотя использование асинхронной техники при помощи возможностей ADO.NET не требует какого-либо дополнительного специального рассмотрения, имеется большая вероятность того, что большинство разработчиков будет использовать асинхронные возможности в ADO.NET, а не в других областях .NET Framework. Важно знать о преимуществах и проблемах создания многопотоковых приложений. В приведенных в этом разделе примерах указываются некоторые важные проблемы, которые необходимо учитывать при создании приложений с многопоточной функциональностью.  
  
## <a name="in-this-section"></a>в этом разделе  

 [Приложения Windows, использующие обратные вызовы](windows-applications-using-callbacks.md)  
 Содержит пример, демонстрирующий безопасное выполнение асинхронной команды с правильной обработкой взаимодействия с формой и ее содержимым из отдельного потока.  
  
 [ASP.NET приложения, использующие дескрипторы ожидания](aspnet-apps-using-wait-handles.md)  
 Содержит пример, демонстрирующий, как выполнять несколько одновременных команд со страницы ASP.NET, используя дескрипторы ожидания для управления операцией при завершении всех команд.  
  
 [Опрос в консольных приложениях](polling-in-console-applications.md)  
 Содержит пример, демонстрирующий использование опроса для ожидания завершения выполнения асинхронной команды из консольного приложения. Этот метод также допустимый в библиотеке классов или другом приложении без пользовательского интерфейса.  
  
## <a name="see-also"></a>См. также раздел

- [SQL Server и ADO.NET](index.md)
- [Асинхронный вызов синхронных методов](../../../../standard/asynchronous-programming-patterns/calling-synchronous-methods-asynchronously.md)
- [Общие сведения об ADO.NET](../ado-net-overview.md)
