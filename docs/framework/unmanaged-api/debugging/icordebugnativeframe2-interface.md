---
title: ICorDebugNativeFrame2 インターフェイス
ms.date: 03/30/2017
api_name:
- ICorDebugNativeFrame2
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugNativeFrame2
helpviewer_keywords:
- ICorDebugNativeFrame2 interface [.NET Framework debugging]
ms.assetid: 52a80838-af36-4399-bc97-d8a4c6d76df2
topic_type:
- apiref
ms.openlocfilehash: 22a3f39bc1f9b4e6cad1db4fd0a6480b7c04e8fa
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76792751"
---
# <a name="icordebugnativeframe2-interface"></a>ICorDebugNativeFrame2 インターフェイス
子と親のフレームの関係をテストするメソッドを提供します。  
  
## <a name="methods"></a>メソッド  
  
|メソッド|説明|  
|------------|-----------------|  
|[IsChild メソッド](icordebugnativeframe2-ischild-method.md)|現在のフレームが子フレームであるかどうかを判断します。|  
|[IsMatchingParentFrame メソッド](icordebugnativeframe2-ismatchingparentframe-method.md)|指定したフレームが現在のフレームの親であるかどうかを判断します。|  
|[GetStackParameterSize メソッド](icordebugnativeframe2-getstackparametersize-method.md)|X86 オペレーティングシステムのスタックのパラメーターの累積サイズを返します。|  
  
## <a name="remarks"></a>コメント  
 このインターフェイスは、"" の "テキスト" インターフェイスを論理的に拡張します。  
  
> [!NOTE]
> このインターフェイスは、コンピューター間またはプロセス間でのリモート呼び出しをサポートしていません。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [デバッグ インターフェイス](debugging-interfaces.md)
- [デバッグ](index.md)
