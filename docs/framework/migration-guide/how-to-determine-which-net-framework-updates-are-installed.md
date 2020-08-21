---
title: Определение установленных обновлений и исправлений системы безопасности .NET Framework
description: Сведения об определении установленных на компьютере обновлений и исправлений безопасности платформы .NET Framework.
ms.date: 11/27/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- updates, determining for .NET Framework
- .NET Framework, determining updates
ms.assetid: 53c7b5f7-d47a-402a-b194-7244a696a88b
ms.openlocfilehash: 5c7bf48d5786530a9bcb69fb7cf605ac2c80a4eb
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/15/2020
ms.locfileid: "79181270"
---
# <a name="how-to-determine-which-net-framework-security-updates-and-hotfixes-are-installed"></a>Определение установленных обновлений и исправлений системы безопасности .NET Framework

В этой статье содержатся сведения об определении установленных на компьютере обновлений и исправлений безопасности платформы .NET Framework.

> [!NOTE]
> Для использования всех методов, представленных в этой статье, требуется учетная запись с правами администратора.

## <a name="use-registry-editor"></a>Использование редактора реестра

Установленные обновления и исправления безопасности для каждой версии .NET Framework на компьютере перечислены в реестре Windows. Для просмотра этих сведений можно использовать редактор реестра (*regedit.exe*).

1. Откройте программу **regedit.exe**. В Windows 8 и выше щелкните правой кнопкой мыши **Пуск**![Снимок экрана: клавиша с логотипом Windows](./media/how-to-determine-which-net-framework-updates-are-installed/windows-keyboard-logo.png "Windowskeyboardlogo") и выберите **Выполнить**. В поле **Открыть** введите **regedit**, а затем нажмите кнопку **ОК**.

2. В редакторе реестра откройте следующий подраздел:

     **HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Updates**

     Установленные обновления перечислены в подразделах, соответствующих версии .NET Framework, к которой они относятся. Каждому обновлению присваивается номер базы знаний Microsoft (KB).

В редакторе реестра версии .NET Framework и установленные обновления для каждой версии хранятся в разных подразделах. Сведения об обнаружении номеров установленных версий см. в статье [Практическое руководство. Определение установленных версий .NET Framework](how-to-determine-which-versions-are-installed.md).

## <a name="query-the-registry-using-code"></a>Отправка запросов в реестр с помощью кода

В следующем примере кода программным образом определяются обновления и исправления безопасности .NET Framework, установленные на компьютере:

[!code-csharp[ListUpdates](../../../samples/snippets/csharp/VS_Snippets_CLR/listupdates/cs/program.cs)]
[!code-vb[ListUpdates](../../../samples/snippets/visualbasic/VS_Snippets_CLR/listupdates/vb/program.vb)]

Выходные данные этого примера выглядят примерно следующим образом:

```console
Microsoft .NET Framework 4 Client Profile
  KB2468871
  KB2468871v2
  KB2478063
  KB2533523
  KB2544514
  KB2600211
  KB2600217
Microsoft .NET Framework 4 Extended
  KB2468871
  KB2468871v2
  KB2478063
  KB2533523
  KB2544514
  KB2600211
  KB2600217
```

## <a name="use-powershell-to-query-the-registry"></a>Отправка запросов в реестр с помощью PowerShell

В следующем примере кода показано определение установленных на компьютере обновлений и исправлений безопасности .NET Framework с помощью PowerShell:

```powershell
$DotNetVersions = Get-ChildItem HKLM:\SOFTWARE\WOW6432Node\Microsoft\Updates | Where-Object {$_.name -like
 "*.NET Framework*"}

ForEach($Version in $DotNetVersions){

   $Updates = Get-ChildItem $Version.PSPath
    $Version.PSChildName
    ForEach ($Update in $Updates){
       $Update.PSChildName
       }
}
```

Выходные данные этого примера выглядят примерно следующим образом:

```console
Microsoft .NET Framework 4 Client Profile
KB2468871
KB2468871v2
KB2478063
KB2533523
KB2544514
KB2600211
KB2600217
Microsoft .NET Framework 4 Extended
KB2468871
KB2468871v2
KB2478063
KB2533523
KB2544514
KB2600211
KB2600217
```

## <a name="see-also"></a>См. также раздел

- [Практическое руководство.Определение установленных версий платформы .NET Framework](how-to-determine-which-versions-are-installed.md)
- [Установка .NET Framework для разработчиков](../install/guide-for-developers.md)
- [Версии и зависимости](versions-and-dependencies.md)
