---
title: Предотвращение атак воспроизведения, когда служба WCF размещена на веб-ферме
ms.date: 03/30/2017
ms.assetid: 1c2ed695-7a31-4257-92bd-9e9731b886a5
ms.openlocfilehash: 07743795effd3da9a241fd778756dd92d99d78f6
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/09/2020
ms.locfileid: "84596790"
---
# <a name="preventing-replay-attacks-when-a-wcf-service-is-hosted-in-a-web-farm"></a>Предотвращение атак воспроизведения, когда служба WCF размещена на веб-ферме
При использовании безопасности сообщений служба WCF предотвращает атаки путем воспроизведения NONCE из входящего сообщения и проверки внутренней`InMemoryNonceCache` на наличие созданного NONCE. Если он есть, то сообщение удаляется как повторное. Когда служба WCF размещается в веб-ферме, свойство `InMemoryNonceCache` не разделяется всеми узлами веб-фермы и служба уязвима для атак с повторением пакетов.  Для устранения подобной угрозы данный сценарий WCF 4.5 предоставляет точку расширяемости, которая позволяет реализовать собственный кэш NONCE. Это осуществляется наследованием класса от абстрактного класса <xref:System.ServiceModel.Security.NonceCache>.  
  
## <a name="noncecache-implementation"></a>Реализация Нонцекаче  
 Чтобы реализовать собственный кэш общего NONCE, унаследуйте класс от <xref:System.ServiceModel.Security.NonceCache> и переопределите метод <xref:System.ServiceModel.Security.NonceCache.CheckNonce%2A> и <xref:System.ServiceModel.Security.NonceCache.TryAddNonce%2A>. <xref:System.ServiceModel.Security.NonceCache.CheckNonce%2A> будет проверять, существует ли указанный NONCE в кэше. <xref:System.ServiceModel.Security.NonceCache.TryAddNonce%2A> будет пытаться добавить NONCE в кэш. После реализации класса создается его экземпляр, который назначается объекту <xref:System.ServiceModel.Channels.LocalClientSecuritySettings.NonceCache%2A> для обнаружения повторных сообщений на стороне клиента и назначается объекту <xref:System.ServiceModel.Channels.LocalServiceSecuritySettings.NonceCache%2A> для обнаружения подобных атак на стороне сервера. Для этой возможности нет стандартной конфигурации параметров.  
  
## <a name="see-also"></a>Дополнительно

- [Безопасность сообщений](message-security-in-wcf.md)
- [SymmetricSecurityBindingElement](../diagnostics/wmi/symmetricsecuritybindingelement.md)
