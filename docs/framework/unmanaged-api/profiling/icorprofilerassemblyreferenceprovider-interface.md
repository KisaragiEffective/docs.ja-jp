---
title: ICorProfilerAssemblyReferenceProvider インターフェイス
ms.date: 03/30/2017
api_name:
- ICorProfilerAssemblyReferenceProvider
api_location:
- mscorwks.dll
api_type:
- COM
ms.assetid: 17205116-66e1-4acc-8f01-532fb3867028
topic_type:
- apiref
ms.openlocfilehash: 13a298451c3e8e1c5809cc1cb222acb5ab85714b
ms.sourcegitcommit: b11efd71c3d5ce3d9449c8d4345481b9f21392c6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2020
ms.locfileid: "76866690"
---
# <a name="icorprofilerassemblyreferenceprovider-interface"></a>ICorProfilerAssemblyReferenceProvider インターフェイス
[.NET Framework 4.5.2 以降のバージョンでのみでサポート]  
  
 プロファイラーが[ICorProfilerCallback:: ModuleLoadFinished](icorprofilercallback-moduleloadfinished-method.md)コールバックに追加するアセンブリ参照の共通言語ランタイム (CLR) を通知できるようにします。  
  
## <a name="methods"></a>メソッド  
  
|メソッド|説明|  
|------------|-----------------|  
|[AddAssemblyReference メソッド](icorprofilerassemblyreferenceprovider-addassemblyreference-method.md)|プロファイラーが[Moduleloadfinished](icorprofilercallback-moduleloadfinished-method.md)コールバックに追加する予定のアセンブリ参照を CLR に通知します。|  
  
## <a name="remarks"></a>コメント  
 CLR は、 [ICorProfilerCallback6:: GetAssemblyReferences](icorprofilercallback6-getassemblyreferences-method.md)コールバックでプロファイラーに `ICorProfilerAssemblyReferenceProvider` インターフェイスオブジェクトを渡します。 これにより、プロファイラーは、後で[ICorProfilerCallback:: ModuleLoadFinished 終了](icorprofilercallback-moduleloadfinished-method.md)した後に追加することを計画しているアセンブリ参照を CLR に通知できます。 コールバック。 これにより、CLR のアセンブリ参照クロージャ ウォーカーの正確性とアセンブリが共有可能かどうかを判断するアルゴリズムが強化されます。  
  
 このインターフェイスは、このインターフェイスオブジェクトをプロファイラーに渡す[ICorProfilerCallback6:: GetAssemblyReferences](icorprofilercallback6-getassemblyreferences-method.md)コールバックでのみ使用できます。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー** : CorProf.idl、CorProf.h  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v452plus](../../../../includes/net-current-v452plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [プロファイリングのインターフェイス](profiling-interfaces.md)
