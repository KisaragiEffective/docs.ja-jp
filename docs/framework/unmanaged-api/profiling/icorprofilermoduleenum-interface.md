---
title: ICorProfilerModuleEnum インターフェイス
ms.date: 03/30/2017
api_name:
- ICorProfilerModuleEnum
api_location:
- mscorwks.cll
api_type:
- COM
f1_keywords:
- ICorProfilerModuleEnum
helpviewer_keywords:
- ICorProfilerModuleEnum interface [.NET Framework profiling]
ms.assetid: 24d0fcfa-1601-4fba-868f-da8c97303467
topic_type:
- apiref
ms.openlocfilehash: 2713fa90240cb0bf41f455b6ed65d76c568a2cf8
ms.sourcegitcommit: b11efd71c3d5ce3d9449c8d4345481b9f21392c6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2020
ms.locfileid: "76868266"
---
# <a name="icorprofilermoduleenum-interface"></a>ICorProfilerModuleEnum インターフェイス
アプリケーションまたはプロファイラーによってロードされたモジュールのコレクションを順番に反復処理するためのメソッドを提供します。  
  
## <a name="methods"></a>メソッド  
  
|メソッド|説明|  
|------------|-----------------|  
|[Clone メソッド](icorprofilermoduleenum-clone-method.md)|この `ICorProfilerModuleEnum` インターフェイスのコピーへのインターフェイス ポインターを取得します。|  
|[GetCount メソッド](icorprofilermoduleenum-getcount-method.md)|アプリケーションによって読み込まれたマネージド モジュールの数を取得します。|  
|[Next メソッド](icorprofilermoduleenum-next-method.md)|オブジェクトのシーケンシャル コレクションから、列挙子の現在の位置以降にある指定した数の隣接するモジュールを取得します。|  
|[Reset メソッド](icorprofilermoduleenum-reset-method.md)|列挙子のカーソルをシーケンスの開始位置に移動します。|  
|[Skip メソッド](icorprofilermoduleenum-skip-method.md)|指定した数の要素がスキップされるように、この列挙子のカーソルの位置を進めます。|  
  
## <a name="remarks"></a>コメント  
 `ICorProfilerModuleEnum` インターフェイスは列挙子です。 このインターフェイスにより、配列の受信側は、受信側に適した速度で送信側から要素をプルできます。 つまり、受信側は配列要素のフローを明示的に制御できるため、大きな配列をメソッド パラメーターとして渡す場合に関連する問題を回避できます。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー** : CorProf.idl、CorProf.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ICorProfilerInfo インターフェイス](icorprofilerinfo-interface.md)
- [プロファイリングのインターフェイス](profiling-interfaces.md)
- [EnumModules メソッド](icorprofilerinfo3-enummodules-method.md)
