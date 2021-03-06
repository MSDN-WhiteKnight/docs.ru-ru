---
title: Особенности возможностей Windows Workflow Foundation
description: В этой статье описываются новые функции, которые .NET Framework 4 добавляет в Windows Workflow Foundation и сценарии, в которых эти функции могут быть полезны.
ms.date: 03/30/2017
ms.assetid: e84d12da-a055-45f6-b4d1-878d127b46b6
ms.openlocfilehash: ae15f3ed536967cb15d1a5913f9ca1eab8a510d9
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/15/2020
ms.locfileid: "90554610"
---
# <a name="windows-workflow-foundation-feature-specifics"></a>Особенности возможностей Windows Workflow Foundation

.NET Framework 4 добавляет ряд функций для Windows Workflow Foundation. В этом документе описываются некоторые новые функциональные возможности и приведены подробные сведения о сценариях, в которых они могут оказаться полезными.

## <a name="messaging-activities"></a>Действия обмена сообщениями

Действия обмена сообщениями ( <xref:System.ServiceModel.Activities.Receive> ,, <xref:System.ServiceModel.Activities.SendReply> <xref:System.ServiceModel.Activities.Send> , <xref:System.ServiceModel.Activities.ReceiveReply> ) используются для отправки и получения сообщений WCF из рабочего процесса. <xref:System.ServiceModel.Activities.Receive><xref:System.ServiceModel.Activities.SendReply>действия и используются для формирования операции службы Windows Communication Foundation (WCF), которая доступна через WSDL так же, как стандартные веб-службы WCF. <xref:System.ServiceModel.Activities.Send> и <xref:System.ServiceModel.Activities.ReceiveReply> используются для использования веб-службы, подобной WCF <xref:System.ServiceModel.ChannelFactory> ; Кроме того, для Workflow Foundation существует **Добавление ссылки на службуный** опыт, создающий предварительно настроенные действия.

### <a name="getting-started-with-messaging-activities"></a>Приступая к работе с действиями обмена сообщениями

- В Visual Studio 2012 создайте проект приложения службы рабочего процесса WCF. На полотне будут расположены <xref:System.ServiceModel.Activities.Receive> и <xref:System.ServiceModel.Activities.SendReply>.

- Щелкните проект правой кнопкой мыши и выберите **Добавление ссылки на службу**. Укажите существующий язык WSDL веб-службы и нажмите кнопку **ОК**. Создайте проект, чтобы отобразить созданные действия (реализованные с помощью <xref:System.ServiceModel.Activities.Send> и <xref:System.ServiceModel.Activities.ReceiveReply> ) на панели элементов.

- [Документация по службам рабочих процессов](../wcf/feature-details/workflow-services.md)

### <a name="messaging-activities-example-scenario"></a>Пример сценария действий обмена сообщениями

`BestPriceFinder`Служба обращается к нескольким службам авиакомпании, чтобы найти лучшую тарифную сумму для конкретного маршрута. Для реализации этого сценария потребуется использовать действия с сообщениями для получения цены, получения цен из внутренних служб и ответа на запрос цены с максимальной ценой. Также потребуется использовать другие готовые действия для создания бизнес-логики для вычисления лучшей цены.

## <a name="workflowservicehost"></a>WorkflowServiceHost

<xref:System.ServiceModel.WorkflowServiceHost>Является готовым узлом рабочего процесса, поддерживающим несколько экземпляров, конфигураций и сообщений WCF (хотя рабочие процессы не требуют использования обмена сообщениями для размещения). Он также реализует сохраняемость, отслеживание и контроль за экземплярами через набор поведений службы. Как и WCF <xref:System.ServiceModel.ServiceHost> , объект <xref:System.ServiceModel.WorkflowServiceHost> может быть размещен в консоли, в приложении WPF или в службе Windows или в веб-среде (как XAMLX-файл) в IIS или WAS.

### <a name="getting-started-with-workflow-service-host"></a>Приступая к работе со службой рабочего процесса

- В Visual Studio 2010 создайте проект приложения службы рабочего процесса WCF: этот проект будет настроен для использования <xref:System.ServiceModel.WorkflowServiceHost> в среде веб-узла.

- Чтобы разместить рабочий процесс, не связанный с обменом сообщениями, добавьте пользовательский класс <xref:System.ServiceModel.Activities.WorkflowHostingEndpoint>, который создаст экземпляр на основе сообщения.

- Экземплярами рабочих процессов можно управлять (приостанавливать или завершать их работу). Это делается путем добавления <xref:System.ServiceModel.Activities.WorkflowControlEndpoint> в <xref:System.ServiceModel.WorkflowServiceHost> с последующим вызовом <xref:System.ServiceModel.Activities.WorkflowControlClient>.

- Образцы <xref:System.ServiceModel.WorkflowServiceHost> приведены в следующих разделах:

  - [Выполнение](./samples/execution.md)

  - Приложение: [Управление приостановленным экземпляром](./samples/suspended-instance-management.md)

- [Общие сведения о размещении служб рабочих процессов](../wcf/feature-details/hosting-workflow-services-overview.md)

### <a name="workflowservicehost-scenario"></a>Сценарий WorkflowServiceHost

Служба Бестприцефиндер обращается к нескольким службам авиакомпании, чтобы найти лучшую тарифную сумму для конкретного маршрута. Для реализации этого сценария потребуется разместить рабочий процесс в <xref:System.ServiceModel.WorkflowServiceHost> . Она также будет использовать действия сообщений для получения ценового запроса, получения цен от внутренних служб и ответа на запрос цены с максимальной ценой.

## <a name="correlation"></a>Корреляция

Корреляцией называют два следующих явления:

- Способ группирования сообщений, т. е. связь между сообщением с запросом и ответом на него.

- Способ сопоставления порции данных с экземпляром службы.

### <a name="getting-started"></a>Приступая к работе

- Чтобы начать работу с корреляцией, создайте новый проект в Visual Studio. Создайте переменную типа <xref:System.ServiceModel.Activities.CorrelationHandle>.

- Примером корреляции для группирования сообщений является корреляция по схеме «запрос-ответ», группирующая сообщения.

  - В <xref:System.ServiceModel.Activities.Receive> действии щелкните <xref:System.ServiceModel.Activities.Receive.CorrelationInitializers%2A> свойство и добавьте <xref:System.ServiceModel.Activities.RequestReplyCorrelationInitializer> с помощью коррелатионхандле, созданного на первом шаге выше.

  - Создайте <xref:System.ServiceModel.Activities.SendReply> действие, щелкнув правой кнопкой мыши <xref:System.ServiceModel.Activities.Receive> и выбрав команду "Создать SendReply". Вставьте его в рабочий процесс после действия <xref:System.ServiceModel.Activities.Receive>.

- Примером сопоставления порции данных с экземпляром службы является корреляция на основе содержимого, сопоставляющая данные (например, идентификатор заказа) с определенным экземпляром рабочего процесса.

  - Для любого действия обмена сообщениями щелкните свойство `CorrelationInitializers` и добавьте <xref:System.ServiceModel.Activities.QueryCorrelationInitializer> с помощью переменной <xref:System.ServiceModel.Activities.CorrelationHandle>, созданной ранее. Дважды щелкните нужное свойство сообщения (например, OrderID) в раскрывающемся меню. Установите свойство `CorrelatesWith` в значение переменной <xref:System.ServiceModel.Activities.CorrelationHandle>, определенной выше.

- [Основная документация по корреляции](../wcf/feature-details/correlation.md)

### <a name="correlation-scenario"></a>Сценарий корреляции

Рабочий процесс обработки заказов используется для обработки нового создания заказа и обновления существующих заказов, которые находятся в процессе. Для реализации этого сценария потребуется разместить рабочий процесс в <xref:System.ServiceModel.WorkflowServiceHost> и использовать действия обмена сообщениями. Также требуется корреляция на основе, `orderId` чтобы гарантировать, что обновления вносятся в нужный рабочий процесс.

## <a name="simplified-configuration"></a>Упрощенная конфигурация

Схема конфигурации WCF является сложной и предоставляет пользователям много сложностей для поиска функций. В мы настроили [!INCLUDE[netfx_current_short](../../../includes/netfx-current-short-md.md)] помощь пользователям WCF по настройке служб с помощью следующих функций:

- Устранена необходимость явной настройки каждой службы отдельно. Если вы не настроили какие-либо \<service> элементы для службы, и ваша служба не определяет программно любую конечную точку, в службу будет автоматически добавлен набор конечных точек, по одному для каждого базового адреса службы и по контракту, реализованному службой.

- Позволяет пользователю определять для привязок WCF и поведений значения по умолчанию, которые будут применяться к службам без явно заданной конфигурации.

- Стандартные конечные точки определяют повторно используемые, заранее настроенные конечные точки, имеющие фиксированные значения для одного или нескольких свойств (адрес, привязка и контракт), а также позволяют определить пользовательские свойства.

- Наконец, <xref:System.ServiceModel.Configuration.ConfigurationChannelFactory%601> позволяет централизованно управлять конфигурацией клиента WCF, что полезно в сценариях, в которых конфигурация выбирается или изменяется после времени загрузки домена приложения.

### <a name="getting-started"></a>Приступая к работе

- [Руководство разработчика WCF 4.0](/previous-versions/dotnet/articles/ee354381(v=msdn.10))

- [Производство канала настройки](xref:System.ServiceModel.Configuration.ConfigurationChannelFactory%601)

- [Элемент стандартной конечной точки](xref:System.ServiceModel.Configuration.StandardEndpointElement)

- [Улучшения конфигурации службы в .NET Framework 4](/archive/blogs/endpoint/service-configuration-improvements-in-net-4)

- [Распространенная ошибка пользователей в .NET 4: опечатки в имени конфигурации служб WF и WCF](/archive/blogs/endpoint/common-user-mistake-in-net-4-mistyping-the-wfwcf-service-configuration-name)

### <a name="simplified-configuration-scenarios"></a>Сценарии упрощенной конфигурации

- Опытный разработчик ASMX хочет начать использовать WCF. Однако WCF кажется слишком сложным. Что означает вся информация, которую нужно указать в файле конфигурации? В .NET 4 можно вообще не использовать файл конфигурации.

- Существующий набор служб WCF очень сложно настраивать и обслуживать. Файл конфигурации содержит тысячи строк XML-кода, которые очень опасно трогать. Потребуется помощь, чтобы уменьшить объем кода и сделать его более управляемым.

## <a name="data-contract-resolver"></a>Арбитр контрактов данных

В .NET 3.5 имелись некоторые ограничения в разработке известных типов.

- Было невозможно динамически добавлять известные типы во время сериализации и десериализации.

- Сериализаторы не могли работать с неизвестными данными в xsi:type.

- Пользователи не смогли указать xsi:type, который должен появиться в канале, например уменьшить размер экземпляра сериализации в канале.

[DataContractResolver](../wcf/samples/datacontractresolver.md) решает эти проблемы в .NET 4,5.

### <a name="getting-started"></a>Приступая к работе

- [Документация по API арбитра контрактов данных](xref:System.Runtime.Serialization.DataContractResolver)

- [Представление арбитра контрактов данных](/archive/blogs/youssefm/configuring-known-types-dynamically-introducing-the-datacontractresolver)

- Примеры:

  - [DataContractResolver](../wcf/samples/datacontractresolver.md)

  - [Атрибут KnownAssemblyAttribute](../wcf/samples/knownassemblyattribute.md)

### <a name="data-contract-resolver-scenarios"></a>Сценарии арбитра контрактов данных

- Как избежать необходимости объявлять десятки объектов <xref:System.Runtime.Serialization.KnownTypeAttribute> в службе.

- Уменьшение размера большого двоичного объекта XML.

## <a name="flowchart"></a>Блок-схема

Блок-схема - это распространенный способ визуального представления проблем домена. Это новый стиль потока управления, представляемый в .NET 4. Основная особенность блок-схемы заключается в том, что в определенное время выполняется только одно действие. Блок-схемы могут представлять циклы и альтернативные результаты, но не могут стандартно представлять параллельное выполнение нескольких узлов.

### <a name="getting-started"></a>Приступая к работе

- В Visual Studio 2012 создайте консольное приложение рабочего процесса. В конструкторе рабочих процессов добавьте блок-схему.

- В блок-схеме используются следующие классы:

  - <xref:System.Activities.Statements.Flowchart>

  - <xref:System.Activities.Statements.FlowNode>

  - <xref:System.Activities.Statements.FlowDecision>

  - <xref:System.Activities.Statements.FlowStep>

  - <xref:System.Activities.Statements.FlowSwitch%601>

- Примеры:

  - [Обработка ошибок в действии блок-схемы с помощью TryCatch](./samples/fault-handling-in-a-flowchart-activity-using-trycatch.md)

  - [Процесс найма](./samples/hiring-process.md)

- Документация по конструктору:

  - [Конструкторы действий блок-схемы](/visualstudio/workflow-designer/flowchart-activity-designers)

### <a name="flowchart-scenarios"></a>Сценарии блок-схем

Действие блок-схемы можно использовать для реализации игры по угадыванию числа. Эта игра очень проста: компьютер выбирает случайное число, а игрок должен его угадать. Когда игрок отправляет каждое предположение, компьютер показывает их подсказку (т. е. "попробуйте меньшее число"). Если игрок находит число в течение менее 7 попыток, он получает от компьютера Специальный конгратулатион. Эта игра может быть реализована с помощью сочетания следующих процедурных действий:

- <xref:System.Activities.Statements.Sequence>

- <xref:System.Activities.Statements.While>

- <xref:System.Activities.Statements.Switch%601>

- <xref:System.Activities.Statements.TryCatch>

- <xref:System.Activities.Statements.Assign%601>

- <xref:System.Activities.Statements.If>

## <a name="procedural-activities-sequence-if-foreach-switch-assign-dowhile-while"></a>Процедурные действия (Sequence, If, ForEach, Switch, Assign, DoWhile, While)

Процедурные действия предоставляют механизм моделирования последовательного потока управления, используя знакомые программистам концепции. Эти действия включают в себя стандартные конструкции языка программирования и, при необходимости, обеспечивают языковую четность с помощью распространенных процедурных языков, таких как C# и Visual Basic.

### <a name="getting-started"></a>Приступая к работе

- В Visual Studio 2012 создайте консольное приложение рабочего процесса. Добавьте в конструкторе рабочих процессов процедурные действия.

- Примеры:

  - [Процесс найма](./samples/hiring-process.md)

  - [Процесс корпоративных закупок](./samples/corporate-purchase-process.md)

- Документация по конструктору:

  - [Конструктор действия Parallel](/visualstudio/workflow-designer/parallel-activity-designer)

  - [Конструктор действия ParallelForEach\<T>](/visualstudio/workflow-designer/parallelforeach-t-activity-designer)

### <a name="procedural-activity-scenarios"></a>Сценарии процедурных действий

- <xref:System.Activities.Statements.Parallel>: В системе управления документами интрасети есть рабочий процесс утверждения документов. Документы перед публикацией в интрасети должны утверждаться сотрудниками нескольких отделов. Не установлен порядок для утверждений. они могут происходить в любое время, когда документ находится на этапе «ожидается утверждение». Когда пользователь отправляет документ для проверки, он должен быть одобрен его прямым менеджером, администратором интрасети и диспетчером внутренней связи.

- <xref:System.Activities.Statements.ParallelForEach%601>: приложение WF управляет корпоративными закупками в большой компании. Корпоративные правила предписывают перед планированием каждой операции закупки произвести оценку трех разных поставщиков. Сотрудник из отдела покупателя выбирает трех поставщиков из списка поставщиков компании. После того как эти поставщики выбраны и проинформированы, компания ожидает от них коммерческих предложений. Предложения могут поступить в любом порядке. Чтобы реализовать этот сценарий в WF, выполните действие <xref:System.Activities.Statements.ParallelForEach%601> для всех поставщиков в коллекции, запросив от них коммерческие предложения. После того как все предложения собраны, выбирается и отображается лучшее из них.

## <a name="invokemethod"></a>InvokeMethod

Действие <xref:System.Activities.Statements.InvokeMethod> позволяет вызывать открытые методы для объектов или типов в области. Оно поддерживает вызов методов экземпляров и статических методов с параметрами или без параметров (включая массивы параметров) и универсальных методов. Оно также позволяет выполнять метод синхронно и асинхронно.

### <a name="getting-started"></a>Приступая к работе

- В Visual Studio 2012 создайте консольное приложение рабочего процесса. Добавьте действие <xref:System.Activities.Statements.InvokeMethod> в конструкторе рабочих процессов и настройте для него метод экземпляра и статический метод.

- Документация по конструктору: [конструктор действий InvokeMethod](/visualstudio/workflow-designer/invokemethod-activity-designer)

### <a name="invokemethod-scenarios"></a>Сценарии InvokeMethod

- Необходимо вызвать метод для объекта в области. Например, в словарь необходимо добавить значение. Вызывается метод Add для экземпляра словаря, указываются ключ и значение.

- Метод необходимо вызывать для унаследованного объекта CLR. Вместо создания пользовательского действия для заключения вызова унаследованного класса в оболочку, если он находится в области во время выполнения рабочего процесса, можно использовать <xref:System.Activities.Statements.InvokeMethod>.

## <a name="error-handling-activities"></a>Действия по обработке ошибок

<xref:System.Activities.Statements.TryCatch>Действие предоставляет механизм для перехвата исключений, происходящих во время выполнения набора вложенных операций (аналогично конструкции try/catch в C# и Visual Basic). <xref:System.Activities.Statements.TryCatch> обеспечивает обработку исключений на уровне рабочего процесса. При возникновении необработанного исключения рабочий процесс прерывается и блок finally не выполняется. Такое поведение согласуется с C#.

### <a name="getting-started"></a>Приступая к работе

- В Visual Studio 2012 создайте консольное приложение рабочего процесса. В конструкторе рабочих процессов добавьте действие <xref:System.Activities.Statements.TryCatch>.

- Пример. [Обработка ошибок в действии блок-схемы с помощью TryCatch](./samples/fault-handling-in-a-flowchart-activity-using-trycatch.md)

- Документация по конструктору: [Обработка ошибок конструкторы действий](/visualstudio/workflow-designer/error-handling-activity-designers)

### <a name="error-handling-scenarios"></a>Сценарии обработки ошибок

При возникновении ошибки должен быть выполнен набор действий и определенная логика. Если логика обработки ошибок обнаружила, что ошибку устранить нельзя, то исключение будет выдано повторно и проблема будет обрабатываться родительским действием (или узлом).

## <a name="pick-activity"></a>Действие Pick

Действие <xref:System.Activities.Statements.Pick> обеспечивает моделирование потока управления на основе событий в WF. Класс <xref:System.Activities.Statements.Pick> содержит множество ветвей, каждая из которых перед выполнением ожидает наступления конкретного события. В такой конфигурации действие <xref:System.Activities.Statements.Pick> ведет себя подобно <xref:System.Activities.Statements.Switch%601>, в котором действие выполнено только для одного события из набора прослушиваемых событий. Всеми ветвями управляют события, и появившееся событие запускает соответствующую ветвь. Все другие ветви отменяются и прекращают ожидать передачу данных, связанных с событиями.

### <a name="getting-started"></a>Приступая к работе

- В Visual Studio 2012 создайте консольное приложение рабочего процесса. В конструкторе рабочих процессов добавьте действие <xref:System.Activities.Statements.Pick>.

- Пример. [Использование действия "скомплектовать"](./samples/using-the-pick-activity.md)

- Документация по конструктору: [Выбор конструктора действий](/visualstudio/workflow-designer/pick-activity-designer)

### <a name="pick-scenario"></a>Сценарий Pick

Пользователь должен получать приглашение ввести данные. В обычных обстоятельствах разработчик будет использовать вызов метода <xref:System.Console.ReadLine%2A> , например, для запроса ввода данных пользователем. Проблема в данном случае состоит в том, что программа должна ожидать ввода данных пользователем. В этом сценарии для разблокирования блокирующего действия необходимо время ожидания. В распространенном сценарии задача должна быть выполнена в течение заданного промежутка времени. Использование времени ожидания для блокирующего действия - это сценарий, где действие Pick имеет очень большое значение.

## <a name="wcf-routing-service"></a>Служба маршрутизации WCF

Служба Routing Service разработана как универсальный программный маршрутизатор, позволяющий управлять передачей сообщений WCF между клиентами и службами. Служба Routing Service позволяет отделить клиентов от служб, что обеспечивает гораздо большую свободу в отношении конфигураций, которые вы можете поддерживать, и гибкости, которую вы используете при рассмотрении размещения служб. В .NET 3,5 Клиенты и службы тесно связаны. клиенту пришлось узнать обо всех службах, которые он должен поговорить и где они были размещены. Кроме того, в WCF в .NET Framework 3,5 имелись следующие ограничения.

- Обработка ошибок была сложной задачей, так как эта логика должна была быть жестко закодирована в клиенте.

- Клиенты и службы должны были всегда использовать одни и те же привязки.

- Службы редко были хорошо факторизованы: проще настроить клиент для работы с одной службой, реализующей все, что нужно, чем выбирать из множества служб.

Служба маршрутизации в .NET 4 призвана упростить решение этих проблем. Новая служба маршрутизации имеет следующие функциональные возможности:

1. Маршрутизация на основе содержимого (Объекты <xref:System.ServiceModel.Dispatcher.MessageFilter> исследуют сообщение и определяют, куда оно должно быть отправлено.)

2. Мост протокола (сообщение транспорта &)

3. Обработка ошибок (маршрутизатор перехватывает исключения и при возникновении сбоя переходит к резервным конечным точкам)

4. Динамическое (в памяти) обновление <xref:System.ServiceModel.Dispatcher.MessageFilterTable%601> и конфигурации маршрутизации.

### <a name="getting-started"></a>Приступая к работе

1. Документация: [Маршрутизация](../wcf/feature-details/routing.md)

2. Примеры: [службы маршрутизации &#91;примеры WCF&#93;](../wcf/samples/routing-services.md)

3. Блог. [правила маршрутизации](/archive/blogs/RoutingRules/) .

### <a name="routing-scenarios"></a>Сценарии маршрутизации

Служба маршрутизации полезна в следующих сценариях.

- Клиенты могут работать с несколькими службами, не обращаясь к ним непосредственно.

- Клиенты могут выполнять дополнительные операции с запросами, чтобы определить, куда их направить.

- Распределение операций, выполняемых клиентом, между несколькими реализациями службы без оптимизации кода клиента.

- Клиенты и службы могут согласовать различные привязки с различными параметрами безопасности.

- Клиенты могут быть более устойчивыми к сбоям или недоступности служб.

## <a name="wcf-discovery"></a>Обнаружение WCF

Обнаружение WCF — это технология платформы, которая позволяет внедрить механизм обнаружения в инфраструктуру приложений. Его можно использовать для того, чтобы сделать службы доступными для обнаружения и настроить на клиентах поиск служб. Конечные точки больше не нужно жестко прописывать в клиентах, что позволяет делать приложения более надежными и отказоустойчивыми. Обнаружение - это превосходная основа для создания возможностей автоматической настройки приложений.

Продукт создается по стандарту WS-Discovery. Она разработана как взаимодействующая, расширяемая и универсальная. Продукт поддерживает два режима функционирования.

1. Управляемый: в сети имеется сущность, осведомленная о существующих службах; клиенты направляют запросы непосредственно к ней. Такая схема работы аналогична Active Directory.

2. Нерегламентированный: клиенты используют многоадресные сообщения для обнаружения служб.

Более того, сообщения обнаружения не зависят от сетевого протокола; их можно использовать с любым протоколом, поддерживающим требования режима. Например, многоадресные сообщения обнаружения могут отправляться по каналу UDP или любой другой сети, поддерживающей многоадресную передачу сообщений. Эти точки проектирования в сочетании с гибкостью функций позволяют адаптировать обнаружение специально для вашего решения.

### <a name="getting-started"></a>Приступая к работе

- Документация: [Обнаружение WCF](../wcf/feature-details/wcf-discovery.md)

- Примеры: [Обнаружение (примеры)](../wcf/samples/discovery-samples.md)

### <a name="discovery-scenarios"></a>Сценарии обнаружения

Разработчик не хочет жестко задавать конечные точки, хотя неизвестно, когда служба будет доступна. Вместо этого разработчик хочет выбирать службу во время выполнения. Компоненты в приложении должны быть лучше разделены, более надежны и доступны для автоматической настройки.

## <a name="tracking"></a>Отслеживание

Отслеживание рабочих процессов позволяет понять выполнение экземпляра рабочего процесса. События отслеживания выдаются из рабочего процесса на уровне экземпляра рабочего процесса и при выполнении действий в рабочем процессе. Участник отслеживания в рабочем процессе должен быть добавлен к узлу рабочего процесса для подписки на записи отслеживания. Записи отслеживания фильтруются с помощью профиля отслеживания. В .NET Framework предоставляется участник отслеживания ETW (Event Tracing для Windows), а в файле machine.config устанавливается базовый профиль.

### <a name="getting-started"></a>Приступая к работе

1. В Visual Studio 2010 создайте новый проект служебного приложения рабочего процесса WCF. Пара <xref:System.ServiceModel.Activities.Receive> и <xref:System.ServiceModel.Activities.SendReply> будет расположена на полотне для запуска.

2. Откройте файл web.config и добавьте поведение отслеживания трассировки событий Windows без профиля.

    1. Будет использован профиль по умолчанию.

    2. Откройте средство просмотра событий и включите канал аналитики на следующем узле: **Просмотр событий**, **журналы приложений и служб**, **Microsoft**, **Windows**, **сервер приложений — приложения**. Щелкните правой кнопкой мыши **аналитика** и выберите **Включить журнал**.

    3. Запустите службу рабочего процесса.

    4. Просмотрите события отслеживания рабочего процесса в средстве просмотра событий.

3. Примеры: [Отслеживание](./samples/tracking.md)

4. Концептуальная документация: [Отслеживание и трассировка рабочих процессов](workflow-tracking-and-tracing.md)

## <a name="sql-workflow-instance-store"></a>Хранилище экземпляров рабочих процессов SQL

<xref:System.Activities.DurableInstancing.SqlWorkflowInstanceStore> - это реализация хранилища экземпляров на основе SQL Server. Хранилище экземпляров служит для хранения состояния запущенного экземпляра вместе со всеми данными, необходимыми для загрузки и возобновления работы экземпляра. Узел службы сохраняет состояние экземпляра в хранилище экземпляров при сохранении рабочего процесса, а также загружает состояние экземпляра при получении сообщения для этого экземпляра или после истечения срока действия действий задержки.

### <a name="getting-started"></a>Приступая к работе

1. В Visual Studio 2012 создайте рабочий процесс, содержащий неявное или неявное <xref:System.Activities.Statements.Persist> действие. Добавьте поведение <xref:System.Activities.DurableInstancing.SqlWorkflowInstanceStore> на узел службы рабочего процесса. Это можно сделать в коде или в файле конфигурации приложения.

2. Примеры: [сохраняемость](/previous-versions/dotnet/netframework-4.0/dd699769(v%3dvs.100))

3. Концептуальная документация: [хранилище экземпляров рабочих процессов SQL](sql-workflow-instance-store.md).
