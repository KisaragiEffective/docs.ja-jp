---
title: ICorProfilerInfo4 インターフェイス
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo4
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo4
helpviewer_keywords:
- ICorProfilerInfo4 interface [.NET Framework profiling]
ms.assetid: 80a5308e-c22f-4201-ba89-31cc8562515b
topic_type:
- apiref
ms.openlocfilehash: c287b630aee58c6795ef405cc1801149e220fd51
ms.sourcegitcommit: b11efd71c3d5ce3d9449c8d4345481b9f21392c6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2020
ms.locfileid: "76868422"
---
# <a name="icorprofilerinfo4-interface"></a>ICorProfilerInfo4 インターフェイス
コードプロファイラーが共通言語ランタイム (CLR) と通信してイベント監視と要求情報を制御するために使用するメソッドを提供します。 を確認しています。 `ICorProfilerInfo4` インターフェイスは、他の `ICorProfilerInfo` インターフェイスの拡張です。 これには、just-in-time (JIT) の再コンパイルをサポートする新しいメソッドが用意されており、.NET Framework 4.5 に追加されています。  
  
## <a name="methods"></a>メソッド  
  
|メソッド|説明|  
|------------|-----------------|  
|[EnumJITedFunctions2 メソッド](icorprofilerinfo4-enumjitedfunctions2-method.md)|以前に JIT コンパイルおよび JIT 再コンパイルされたすべての関数の列挙子を返します。|  
|[EnumThreads メソッド](icorprofilerinfo4-enumthreads-method.md)|プロファイリングされたプロセス内のすべてのマネージスレッドのコレクションを順番に反復処理するメソッドを提供する列挙子を取得します。|  
|[GetCodeInfo3 メソッド](icorprofilerinfo4-getcodeinfo3-method.md)|指定した関数の JIT 再コンパイル バージョンに関連付けられているネイティブ コードの範囲を取得します。|  
|[GetFunctionFromIP2 メソッド](icorprofilerinfo4-getfunctionfromip2-method.md)|マネージコード命令ポインターを、指定された関数の JIT 再コンパイルバージョンにマップします。|  
|[GetILToNativeMapping2 メソッド](icorprofilerinfo4-getiltonativemapping2-method.md)|Microsoft 中間言語 (MSIL) オフセットから、指定した関数の JIT 再コンパイルバージョンに含まれるコードのネイティブオフセットへのマップを取得します。|  
|[GetObjectSize2 メソッド](icorprofilerinfo4-getobjectsize2-method.md)|指定したオブジェクトのサイズを返します。|  
|[GetReJITIDs メソッド](icorprofilerinfo4-getrejitids-method.md)|割り当てられている指定された関数のすべての JIT 再コンパイルバージョンを識別する Id の配列を返します。|  
|[InitializeCurrentThread メソッド](icorprofilerinfo4-initializecurrentthread-method.md)|デッドロックを回避できるように、同じスレッドで、後続のプロファイラー API 呼び出しの前に現在のスレッドを初期化します。|  
|[RequestReJIT メソッド](icorprofilerinfo4-requestrejit-method.md)|指定された関数のすべてのインスタンスの JIT 再コンパイルを要求します。|  
|[RequestRevert メソッド](icorprofilerinfo4-requestrevert-method.md)|指定された関数のすべてのインスタンスを元のバージョンに戻します。|  
  
## <a name="remarks"></a>Remarks  
 CLR は、`ICorProfilerInfo4` インターフェイスのメソッドを、フリー スレッド モデルを使用して実装します。 各メソッドが、成功または失敗を示す HRESULT を返します。 返される可能性があるリターン コードの一覧については、CorError.h ファイルを参照してください。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー** : CorProf.idl、CorProf.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [プロファイリングのインターフェイス](profiling-interfaces.md)
- [ICorProfilerInfo インターフェイス](icorprofilerinfo-interface.md)
