---
title: Как gRPC подходы к RPC-gRPC для разработчиков WCF
description: Сравнение основных функций WCF и gRPC.
ms.date: 09/02/2019
ms.openlocfilehash: b924d2f20b54de2d6476a0b27626d5dd7fd0986f
ms.sourcegitcommit: 5fb5b6520b06d7f5e6131ec2ad854da302a28f2e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/03/2019
ms.locfileid: "74711483"
---
# <a name="how-grpc-approaches-rpc"></a>Как gRPC работает с RPC

Windows Communication Foundation (WCF) и gRPC являются реализациями шаблона *удаленного вызова процедур* (RPC). Этот шаблон предназначен для вызова служб, работающих на другом компьютере или в другом процессе, без проблем, таких как вызовы методов в клиентском приложении. Хотя цель WCF и gRPC одинакова, сведения о реализации весьма различны.

В следующей таблице описано, как основные функции WCF связаны с gRPC и где можно найти более подробные объяснения.

| Компоненты | WCF | gRPC |
| -------- | --- | ---- |
| Цель | Разделите бизнес-код от реализации сети. | Разделите бизнес-код от определения интерфейса и реализации сети. |
| Определение служб и сообщений (главы 3-4)  | Контракт службы, контракт операции и контракт данных. | Использует файл с путем для объявления служб и сообщений. |
| Язык (главы 3-5) | Контракты, написанные на C# или Visual Basic. | Язык буфера протокола. |
| Формат сети (глава 3) | Настраиваемые, включая SOAP/XML, обычный XML, JSON и двоичный файл .NET. | Двоичный формат буфера протокола (хотя можно использовать другие форматы).
| Взаимодействие (глава 4) | При использовании протокола SOAP через HTTP. | Официальная поддержка: .NET, Java, Python, JavaScript, CC++/, Go, Руст, Ruby, SWIFT, DART, PHP. Неофициальная поддержка других языков в сообществе. |
| Сети (глава 4) | Настраивается во время выполнения. Переключение между NetTCP, HTTP и MSMQ. | HTTP/2, в настоящее время для TCP, только с ASP.NET Core gRPC. |
| Подход (глава 4) | Создание сериализации, десериализации и сетевого кода в базовых классах во время выполнения. | Создание сериализации, десериализации и сетевого кода в базовых классах во время сборки. |
| Безопасность (глава 6) | Проверка подлинности, WS-Security, шифрование сообщений. | Учетные данные, ASP.NET Core безопасность, TLS в сети. |

>[!div class="step-by-step"]
>[Назад](grpc-overview.md)
>[Вперед](interface-definition-language.md)
