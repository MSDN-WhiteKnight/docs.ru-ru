---
title: Предупреждение SYSLIB0007
description: Сведения об устаревших элементах, которые приводят к появлению предупреждения во время компиляции SYSLIB0007.
ms.topic: reference
ms.date: 10/20/2020
ms.openlocfilehash: d5410a3b3d33515e2ee6f578cad2f4deaec9c25d
ms.sourcegitcommit: dfcbc096ad7908cd58a5f0aeabd2256f05266bac
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/21/2020
ms.locfileid: "92333126"
---
# <a name="syslib0007-default-implementations-of-cryptography-algorithms-not-supported"></a>SYSLIB0007. Реализации алгоритмов шифрования по умолчанию не поддерживаются

Система конфигурации шифрования, представленная в .NET Framework, больше не используется в .NET Core и .NET 5 и более поздних версий, поскольку она не позволяет обеспечить необходимую гибкость шифрования. Требования к обратной совместимости .NET также не позволяют платформе обновлять определенные API шифрования, чтобы обеспечить поддержку улучшений технологий шифрования. В связи с этим следующие API помечены как устаревшие, начиная с версии .NET 5.0. При использовании этих API во время компиляции создается предупреждение `SYSLIB0007`.

- <xref:System.Security.Cryptography.AsymmetricAlgorithm.Create?displayProperty=fullName>
- <xref:System.Security.Cryptography.HashAlgorithm.Create?displayProperty=fullName>
- <xref:System.Security.Cryptography.HMAC.Create?displayProperty=fullName>
- <xref:System.Security.Cryptography.KeyedHashAlgorithm.Create?displayProperty=fullName>
- <xref:System.Security.Cryptography.SymmetricAlgorithm.Create?displayProperty=fullName>

## <a name="workaround"></a>Обходной путь

- Рекомендуется заменить вызовы к устаревшим API вызовами методов фабрики для конкретных алгоритмов, например <xref:System.Security.Cryptography.Aes.Create?displayProperty=nameWithType>. Таким образом, вы будете полностью контролировать алгоритмы, для которых создаются экземпляры.

- Если необходимо обеспечить совместимость с существующими полезными данными, создаваемыми приложениями .NET Framework, которые используют устаревшие API, используйте предложенные в следующей таблице альтернативные варианты. В этой таблице приводится сопоставление алгоритмов .NET Framework по умолчанию с их эквивалентами в .NET 5 и более поздних версий.

  | .NET Framework | Совместимый эквивалент в .NET Core или .NET 5.0 и более поздних версий | Remarks |
  | - | - | - |
  | <xref:System.Security.Cryptography.AsymmetricAlgorithm.Create?displayProperty=nameWithType> | <xref:System.Security.Cryptography.RSA.Create?displayProperty=nameWithType> | |
  | <xref:System.Security.Cryptography.HashAlgorithm.Create?displayProperty=nameWithType> | <xref:System.Security.Cryptography.SHA1.Create?displayProperty=nameWithType> | Алгоритм SHA-1 считается ненадежным. По возможности рекомендуется использовать более надежный алгоритм. Дополнительные сведения вы можете получить у своего консультанта по вопросам безопасности. |
  | <xref:System.Security.Cryptography.HMAC.Create?displayProperty=nameWithType> | <xref:System.Security.Cryptography.HMACSHA1.%23ctor> | Алгоритм HMACSHA1 не рекомендуется к применению для большинства современных приложений. По возможности рекомендуется использовать более надежный алгоритм. Дополнительные сведения вы можете получить у своего консультанта по вопросам безопасности. |
  | <xref:System.Security.Cryptography.KeyedHashAlgorithm.Create?displayProperty=nameWithType> | <xref:System.Security.Cryptography.HMACSHA1.%23ctor> | Алгоритм HMACSHA1 не рекомендуется к применению для большинства современных приложений. По возможности рекомендуется использовать более надежный алгоритм. Дополнительные сведения вы можете получить у своего консультанта по вопросам безопасности. |
  | <xref:System.Security.Cryptography.SymmetricAlgorithm.Create?displayProperty=nameWithType> | <xref:System.Security.Cryptography.Aes.Create?displayProperty=nameWithType> |

## <a name="see-also"></a>См. также

- [Критические изменения в области шифрования](cryptography.md#instantiating-default-implementations-of-cryptographic-abstractions-is-not-supported)
