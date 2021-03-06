---
title: Метод ICorProfilerInfo5::SetEventMask2
ms.date: 03/30/2017
dev_langs:
- cpp
api_name:
- IcorProfilerInfo5.SetEventMask2
api_location:
- mscorwks.dll
api_type:
- COM
ms.assetid: 05dbbe2b-049c-4a60-be69-2ad7a949405e
topic_type:
- apiref
ms.openlocfilehash: 8027cdcde8281c363207e309bf65fcd90c03b626
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/08/2020
ms.locfileid: "84495636"
---
# <a name="icorprofilerinfo5seteventmask2-method"></a>Метод ICorProfilerInfo5::SetEventMask2
[Поддерживается в .NET Framework 4.5.2 и более поздних версиях.]  
  
 Определяет значение, указывающее типы событий, для которых профилировщик хочет получать уведомления о событиях от среды CLR. Он предоставляет больше функций, чем метод [ICorProfilerInfo:: SetEventMask](icorprofilerinfo-seteventmask-method.md) .  
  
## <a name="syntax"></a>Синтаксис  
  
```cpp
HRESULT SetEventMask2(        [in] DWORD dwEventsLow,        [in] DWORD dwEventsHigh  
);  
```  
  
## <a name="parameters"></a>Параметры  
 `dwEventsLow`  
 [в] 4-байтовое значение, определяющее категории событий. Каждый бит управляет отдельной возможностью, поведением или типом события. Биты описаны в перечислении [COR_PRF_MONITOR](cor-prf-monitor-enumeration.md) .  
  
 `dwEventsHigh`  
 [в] 4-байтовое значение, определяющее категории событий.  Каждый бит управляет отдельной возможностью, поведением или типом события. Биты описаны в перечислении [COR_PRF_HIGH_MONITOR](cor-prf-high-monitor-enumeration.md) .  
  
## <a name="remarks"></a>Примечания  
 Метод `SetEventMask2` используется для установки обратных вызовов, на которые подписывается профилировщик. Как правило, вызывается метод [GetEventMask2](icorprofilerinfo5-geteventmask2-method.md) , чтобы определить, какие биты заданы, ВЫПОЛНЯЮТ логическое или из `pdwEventsLow` его `pdwEventsHigh` значений и любых новых битов, которые необходимо задать, а затем вызовите `SetEventMask2` метод.  
  
 Этот метод является рекомендуемым альтернативой методу [SetEventMask](icorprofilerinfo-seteventmask-method.md) .  
  
## <a name="requirements"></a>Требования  
 **Платформы:** см. раздел [Требования к системе](../../get-started/system-requirements.md).  
  
 **Заголовок:** CorProf.idl, CorProf.h  
  
 **Библиотека:** CorGuids.lib  
  
 **.NET Framework версии:**[!INCLUDE[net_current_v452plus](../../../../includes/net-current-v452plus-md.md)]  
  
## <a name="see-also"></a>См. также

- [Интерфейс ICorProfilerInfo5](icorprofilerinfo5-interface.md)
- [Метод GetEventMask2](icorprofilerinfo5-geteventmask2-method.md)
