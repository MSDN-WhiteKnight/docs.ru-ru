---
title: Как определить сериализуемость объекта .NET Standard
description: Показывается, как определить, может ли тип .NET Standard быть сериализован во время выполнения.
ms.date: 10/20/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- serializing objects
- objects, serializing steps
ms.openlocfilehash: a425d44ac3b58a568bd51e638f28a2b76ced9dec
ms.sourcegitcommit: 67ebdb695fd017d79d9f1f7f35d145042d5a37f7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/20/2020
ms.locfileid: "92223993"
---
# <a name="how-to-determine-if-a-net-standard-object-is-serializable"></a>Как определить сериализуемость объекта .NET Standard

.NET Standard — это спецификация, определяющая типы и элементы, которые должны присутствовать в конкретных реализациях .NET, соответствующих этой версии стандарта. Однако .NET Standard не определяет, является ли тип сериализуемым. Типы, определенные в библиотеке .NET Standard, не помечаются атрибутом <xref:System.SerializableAttribute>. Но вместо этого некоторые реализации .NET, например .NET Framework и .NET Core, могут беспрепятственно определять, является ли конкретный тип сериализуемым.

Если вы разработали библиотеку, предназначенную для .NET Standard, ваша библиотека может использоваться в любой реализации .NET, поддерживающей .NET Standard. Это означает, что вы не можете заранее знать, является ли конкретный тип сериализуемым. Вы сможете определить, является ли он сериализуемым, только во время выполнения.

Чтобы во время выполнения определить, является ли объект сериализуемым, получите значение свойства <xref:System.Type.IsSerializable> объекта <xref:System.Type>, который представляет тип этого объекта. В следующем примере показано, как это делается. Определяется метод расширения `IsSerializable(Object)`, который указывает, может ли быть сериализован любой экземпляр <xref:System.Object>.

[!code-csharp[is-a-type-serializable](~/samples/snippets/standard/serialization/is-serializable/csharp/program.cs#2)]
[!code-vb[is-a-type-serializable](~/samples/snippets/standard/serialization/is-serializable/vb/library.vb#2)]

Затем в этот метод можно передать любой объект, чтобы определить, можно ли его сериализовать и десериализовать в текущей реализации .NET, как показано в следующем примере:

[!code-csharp[test-is-a-type-serializable](~/samples/snippets/standard/serialization/is-serializable/csharp/program.cs#1)]
[!code-vb[test-is-a-type-serializable](~/samples/snippets/standard/serialization/is-serializable/vb/program.vb#1)]

## <a name="see-also"></a>См. также

- [Двоичная сериализация](binary-serialization.md)
- <xref:System.SerializableAttribute?displayProperty=nameWithType>
- <xref:System.Type.IsSerializable?displayProperty=nameWithType>
