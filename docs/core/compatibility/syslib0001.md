---
title: Предупреждение SYSLIB0001
description: Сведения об устаревших элементах, которые приводят к появлению предупреждения во время компиляции SYSLIB0001.
ms.topic: reference
ms.date: 10/20/2020
ms.openlocfilehash: 58c16879b7d91598ea0848bb0ba95f8fa0200cfe
ms.sourcegitcommit: dfcbc096ad7908cd58a5f0aeabd2256f05266bac
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/21/2020
ms.locfileid: "92333138"
---
# <a name="syslib0001-the-utf-7-encoding-is-insecure"></a>SYSLIB0001. Кодировка UTF-7 небезопасна

Кодировка UTF-7 больше не используется широко в приложениях, и многие спецификации теперь [запрещают использовать ее](https://security.stackexchange.com/a/68609/3573) при обмене. Кроме того, иногда она [служит вектором атаки](https://cve.mitre.org/cgi-bin/cvekey.cgi?keyword=utf-7) в приложениях, которые не предусматривают получение данных в этой кодировке. Корпорация Майкрософт предостерегает от использования класса <xref:System.Text.UTF7Encoding?displayProperty=nameWithType>, так как он не выполняет обнаружение ошибок.

В связи с этим следующие API помечены как устаревшие, начиная с версии .NET 5.0. При использовании этих API во время компиляции создается предупреждение `SYSLIB0001`.

- Свойство<xref:System.Text.Encoding.UTF7?displayProperty=nameWithType>
- Конструкторы <xref:System.Text.UTF7Encoding.%23ctor%2A>

## <a name="workarounds"></a>Обходные пути

- Если вы используете <xref:System.Text.Encoding.UTF7?displayProperty=nameWithType> или <xref:System.Text.UTF7Encoding> в своем формате протокола или файла:

  Перейдите на использование <xref:System.Text.Encoding.UTF8?displayProperty=nameWithType> или <xref:System.Text.UTF8Encoding>. Кодировка UTF-8 является отраслевым стандартом, поддерживающим различные языки, операционные системы и среды выполнения. Использование UTF-8 упрощает дальнейшее обслуживание кода и улучшает его совместимость с остальной экосистемой.

- Если вы сравниваете экземпляр <xref:System.Text.Encoding> со свойством <xref:System.Text.Encoding.UTF7?displayProperty=nameWithType>:

  Вместо этого рекомендуется выполнять проверку по кодовой странице UTF-7 — `65000`. Сравнение с кодовой страницей позволяет избежать предупреждения, а также обрабатывать некоторые пограничные случаи, например, когда вызывается `new UTF7Encoding()` или создается подкласс типа.

  ```csharp
  void DoSomething(Encoding enc)
  {
      // Don't perform the check this way.
      // It produces a warning and misses some edge cases.
      if (enc == Encoding.UTF7)
      {
          // Encoding is UTF-7.
      }

      // Instead, perform the check this way.
      if (enc != null && enc.CodePage == 65000)
      {
          // Encoding is UTF-7.
      }
  }
  ```

## <a name="see-also"></a>См. также

- [Пути к коду в кодировке UTF-7 устарели](3.1-5.0.md#utf-7-code-paths-are-obsolete)
