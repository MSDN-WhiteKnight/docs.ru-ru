---
ms.openlocfilehash: a635e2ed6a735b5234c92fd8f5ffa1685fe9373e
ms.sourcegitcommit: 8bfeb5930ca48b2ee6053f16082dcaf24d46d221
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/18/2020
ms.locfileid: "88558190"
---
### <a name="microsoftdotnetplatformabstractions-package-removed"></a>Удален пакет Microsoft.DotNet.PlatformAbstractions

Создание новых версий [Microsoft.DotNet.PlatformAbstractions NuGet package](https://www.nuget.org/packages/Microsoft.DotNet.PlatformAbstractions/) не предусматривается.

#### <a name="change-description"></a>Описание изменений

Ранее новые версии библиотеки <xref:Microsoft.DotNet.PlatformAbstractions?displayProperty=fullName> создавались вместе с новыми версиями .NET Core. В дальнейшем добавление новых функций в библиотеку не предусматривается, как и выпуск новых основных версий. Однако работа и обслуживание существующих версий библиотеки не прекращаются.

Библиотека <xref:Microsoft.DotNet.PlatformAbstractions?displayProperty=fullName> пересекается с интерфейсами API, которые уже установлены в пространствах имен System\*. Кроме того, некоторые API <xref:Microsoft.DotNet.PlatformAbstractions> не разрабатывались с тем же уровнем детализации и не были рассчитаны на такую же долгосрочную поддержку, что и остальная часть System.\* Необходимо загрузить и установить пакет NuGet LINQ to Twitter для работы с этим учебником. Например, <xref:Microsoft.DotNet.PlatformAbstractions> использует перечисление `Platform` для описания текущей платформы операционной системы. От такой модели перечисления отказались при разработке API <xref:System.Runtime.InteropServices.RuntimeInformation.IsOSPlatform(System.Runtime.InteropServices.OSPlatform)?displayProperty=nameWithType>, чтобы обеспечить поддержку новых платформ и гибкую настройку с учетом новых возможностей.

Сценарии, которые активируются библиотекой <xref:Microsoft.DotNet.PlatformAbstractions?displayProperty=fullName>, теперь могут обходиться без нее. Существующие версии будут продолжать работать даже в .NET 5.0 и более поздних версиях и обслуживаться наряду с предыдущими версиями .NET Core. Однако новые функции в библиотеку не добавляются. Вместо этого новые функции будут добавляться в другие библиотеки и API-интерфейсы.

#### <a name="version-introduced"></a>Представленная версия

5.0, предварительная версия 6

#### <a name="recommended-action"></a>Рекомендованное действие

- Вы сможете по-прежнему использовать более ранние версии библиотеки, если они соответствуют вашим требованиям.

- Если старые версии не соответствуют вашим требованиям, замените используемые API `PlatformAbstractions` на рекомендованные версии.

  | API `PlatformAbstractions` | Рекомендуемая замена |
  |-|-|
  | `ApplicationEnvironment.ApplicationBasePath` | <xref:System.AppContext.BaseDirectory?displayProperty=nameWithType> |
  | <xref:Microsoft.DotNet.PlatformAbstractions.HashCodeCombiner> | <xref:System.HashCode?displayProperty=nameWithType> |
  | `RuntimeEnvironment.GetRuntimeIdentifier()` | <xref:System.Runtime.InteropServices.RuntimeInformation.RuntimeIdentifier?displayProperty=nameWithType> |
  | `RuntimeEnvironment.OperatingSystemPlatform` | <xref:System.Runtime.InteropServices.RuntimeInformation.IsOSPlatform(System.Runtime.InteropServices.OSPlatform)?displayProperty=nameWithType> |
  | `RuntimeEnvironment.RuntimeArchitecture` | <xref:System.Runtime.InteropServices.RuntimeInformation.ProcessArchitecture?displayProperty=nameWithType> |
  | `RuntimeEnvironment.OperatingSystem` | <xref:System.Runtime.InteropServices.RuntimeInformation.OSDescription?displayProperty=nameWithType> |
  | `RuntimeEnvironment.OperatingSystemVersion` | <xref:System.Runtime.InteropServices.RuntimeInformation.OSDescription?displayProperty=nameWithType> и <xref:System.Environment.OSVersion?displayProperty=nameWithType> |

  > [!NOTE]
  > Большинство сценариев использования `RuntimeEnvironment.OperatingSystem` и `RuntimeEnvironment.OperatingSystemVersion` ориентированы на отображение, например на отображение для пользователя, ведение журнала и телеметрических данных. Не рекомендуется принимать решения по среде выполнения, основываясь на версии операционной системы (ОС). <xref:System.Environment.OSVersion?displayProperty=nameWithType> теперь [возвращает верную версию](../../../../docs/core/compatibility/corefx.md#environmentosversion-returns-the-correct-operating-system-version) для операционных систем Windows и macOS. Однако в отношении большинства дистрибутивов Unix "версия ОС" не является однозначным понятием. Например, она может указывать на версию ядра Linux или версию дистрибутива. Для большинства платформ Unix <xref:System.Environment.OSVersion?displayProperty=nameWithType> и <xref:System.Runtime.InteropServices.RuntimeInformation.OSDescription?displayProperty=nameWithType> возвращают версию, которая возвращается `uname`. Сведения об имени и версии дистрибутива Linux приведены в файле */etc/os-release*.

#### <a name="category"></a>Категория

Библиотеки Core .NET

#### <a name="affected-apis"></a>Затронутые API

- `Microsoft.DotNet.PlatformAbstractions.ApplicationEnvironment.ApplicationBasePath`
- <xref:Microsoft.DotNet.PlatformAbstractions.HashCodeCombiner?displayProperty=fullName>
- `Microsoft.DotNet.PlatformAbstractions.RuntimeEnvironment.GetRuntimeIdentifier()`
- `Microsoft.DotNet.PlatformAbstractions.RuntimeEnvironment.OperatingSystem`
- `Microsoft.DotNet.PlatformAbstractions.RuntimeEnvironment.OperatingSystemPlatform`
- `Microsoft.DotNet.PlatformAbstractions.RuntimeEnvironment.OperatingSystemVersion`
- `Microsoft.DotNet.PlatformAbstractions.RuntimeEnvironment.RuntimeArchitecture`

<!--

#### Affected APIs

- `P:Microsoft.DotNet.PlatformAbstractions.ApplicationEnvironment.ApplicationBasePath`
- `T:Microsoft.DotNet.PlatformAbstractions.HashCodeCombiner`
- `M:Microsoft.DotNet.PlatformAbstractions.RuntimeEnvironment.GetRuntimeIdentifier`
- `P:Microsoft.DotNet.PlatformAbstractions.RuntimeEnvironment.OperatingSystem`
- `P:Microsoft.DotNet.PlatformAbstractions.RuntimeEnvironment.OperatingSystemPlatform`
- `P:Microsoft.DotNet.PlatformAbstractions.RuntimeEnvironment.OperatingSystemVersion`
- `P:Microsoft.DotNet.PlatformAbstractions.RuntimeEnvironment.RuntimeArchitecture`

-->
