---
title: ICorProfilerInfo7 インターフェイス
ms.date: 03/30/2017
ms.assetid: cf37c462-73c5-412a-a7f8-bb26ca746313
ms.openlocfilehash: f80f310c10bae33583cb7cd2048ede4f5efbe14c
ms.sourcegitcommit: b11efd71c3d5ce3d9449c8d4345481b9f21392c6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2020
ms.locfileid: "76861750"
---
# <a name="icorprofilerinfo7-interface"></a>ICorProfilerInfo7 インターフェイス
[.NET Framework 4.6.1 以降のバージョンでのみでサポート]  
  
 新しく定義されたメタデータをモジュールに適用し、メモリ内シンボルストリームへのアクセスを提供するメソッドを提供する[ICorProfilerInfo6](icorprofilerinfo6-interface.md)のサブクラスです。  
  
## <a name="methods"></a>メソッド  
  
|メソッド|説明|  
|------------|-----------------|  
|[ApplyMetaData メソッド](icorprofilerinfo7-applymetadata-method.md)|`IMetadataEmit::Define*` メソッドによって新たに定義されたメタデータを、指定したモジュールに適用します。|  
|[GetInMemorySymbolsLength メソッド](icorprofilerinfo7-getinmemorysymbolslength-method.md)|メモリ内シンボルストリームの長さを返します。|  
|[ReadInMemorySymbols](icorprofilerinfo7-readinmemorysymbols.md)|メモリ内シンボルストリームからバイトを読み取ります。|  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー** : CorProf.idl、CorProf.h  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v461plus](../../../../includes/net-current-v461plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [プロファイリングのインターフェイス](profiling-interfaces.md)
