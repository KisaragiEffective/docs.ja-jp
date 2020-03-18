---
title: ICorDebugModule2::ApplyChanges メソッド
ms.date: 03/30/2017
api_name:
- ICorDebugModule2.ApplyChanges
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugModule2::ApplyChanges
helpviewer_keywords:
- ApplyChanges method [.NET Framework debugging]
- ICorDebugModule2::ApplyChanges method [.NET Framework debugging]
ms.assetid: 96fa3406-6a6f-41a1-88c6-d9bc5d1a16d1
topic_type:
- apiref
ms.openlocfilehash: c324019e1e62701f4f2aaba1c00948b292ba6847
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73127912"
---
# <a name="icordebugmodule2applychanges-method"></a>ICorDebugModule2::ApplyChanges メソッド
メタデータの変更と、Microsoft 中間言語 (MSIL) コードの変更を実行中のプロセスに適用します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT ApplyChanges (  
    [in] ULONG                       cbMetadata,  
    [in, size_is(cbMetadata)] BYTE   pbMetadata[],  
    [in] ULONG                       cbIL,  
    [in, size_is(cbIL)] BYTE         pbIL[]  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `cbMetadata`  
 からデルタメタデータのサイズ (バイト単位)。  
  
 `pbMetadata`  
 からデルタメタデータを格納しているバッファー。 バッファーのアドレスは、 [IMetaDataEmit2:: SaveDeltaToMemory](../../../../docs/framework/unmanaged-api/metadata/imetadataemit2-savedeltatomemory-method.md)メソッドから返されます。  
  
 メタデータ内の相対仮想アドレス (RVAs) は、MSIL コードの先頭に対して相対的に指定する必要があります。  
  
 `cbIL`  
 からデルタ MSIL コードのサイズ (バイト単位)。  
  
 `pbIL`  
 から更新された MSIL コードを格納しているバッファー。  
  
## <a name="remarks"></a>Remarks  
 `pbMetadata` パラメーターは、( [IMetaDataEmit2:: SaveDeltaToMemory](../../../../docs/framework/unmanaged-api/metadata/imetadataemit2-savedeltatomemory-method.md)によって出力される) 特別なデルタメタデータ形式です。 `pbMetadata` は、前のメタデータをベースとして受け取り、そのベースに適用する個々の変更について説明します。  
  
 これに対して、`pbIL[`] パラメーターには、更新されたメソッドの新しい MSIL が含まれます。これは、そのメソッドの前の MSIL を完全に置き換えることを目的としています。  
  
 デルタ MSIL とメタデータがデバッガーのメモリ内に作成されると、デバッガーは `ApplyChanges` を呼び出して、変更を共通言語ランタイム (CLR) に送信します。 ランタイムはそのメタデータテーブルを更新し、新しい MSIL をプロセスに配置して、新しい MSIL の just-in-time (JIT) コンパイルを設定します。 変更が適用されると、デバッガーは[IMetaDataEmit2:: ResetENCLog](../../../../docs/framework/unmanaged-api/metadata/imetadataemit2-resetenclog-method.md)を呼び出して、次の編集セッションの準備を行う必要があります。 デバッガーはプロセスを続行できます。  
  
 デバッガーは、デルタメタデータを持つモジュールで `ApplyChanges` を呼び出すたびに、変更を出力するために使用されるコピーを除き、そのモジュールのメタデータのすべてのコピーに対して同じデルタメタデータを使用して[IMetaDataEmit:: ApplyEditAndContinue](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-applyeditandcontinue-method.md)を呼び出す必要があります。 メタデータのコピーが実際のメタデータと同期しなくなる場合、デバッガーは常にそのコピーを破棄して新しいコピーを取得できます。  
  
 `ApplyChanges` メソッドが失敗した場合、デバッグセッションは無効な状態にあり、再起動する必要があります。  
  
## <a name="requirements"></a>［要件］  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]
