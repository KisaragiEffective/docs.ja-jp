---
title: ICorDebugMetaDataLocator::GetMetaData メソッド
ms.date: 03/30/2017
api_name:
- ICorDebugMetaDataLocator.GetMetaData
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugMetaDataLocator::GetMetaData
helpviewer_keywords:
- ICorDebugMetaDataLocator::GetMetaData method [.NET Framework debugging]
- GetMetaData method, ICorDebugMetaDataLocator interface [.NET Framework debugging]
ms.assetid: f9b0ff22-54db-45eb-9cc3-508000a3141d
topic_type:
- apiref
ms.openlocfilehash: 43f3c1dd866b98bff51b375a11e28727e41d3ead
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76793051"
---
# <a name="icordebugmetadatalocatorgetmetadata-method"></a>ICorDebugMetaDataLocator::GetMetaData メソッド
デバッガーが要求した操作を完了するために必要となるメタデータが含まれているモジュールの完全パスを返すように、デバッガーに求めます。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT GetMetaData(  
      [in] LPCWSTR wszImagePath,  
      [in] DWORD   dwImageTimeStamp,  
      [in] DWORD   dwImageSize,  
      [in] ULONG32 cchPathBuffer,  
      [out] ULONG32 * pcchPathBuffer,  
      [out, size_is(cchPathBuffer), length_is(*pcchPathBuffer)]  
               WCHAR wszPathBuffer[]  
      );  
```  
  
## <a name="parameters"></a>パラメーター  
 `wszImagePath`  
 [in] ファイルの完全パスを表す null で終わる文字列。 完全なパスが使用できない場合は、ファイルの名前と拡張子 (ファイル*名*)。*拡張機能*)。  
  
 `dwImageTimeStamp`  
 [in] イメージの PE ファイル ヘッダーのタイムスタンプ。 このパラメーターは、シンボルサーバー ([Symsrv](/windows/desktop/debug/using-symsrv)) の検索に使用される可能性があります。  
  
 `dwImageSize`  
 [in] PE ファイル ヘッダーのイメージ サイズ。 このパラメーターは、SymSrv の検索に使用される可能性があります。  
  
 `cchPathBuffer`  
 [in] `wszPathBuffer` の文字数。  
  
 `pcchPathBuffer`  
 [out] `wszPathBuffer` に書き込まれる `WCHAR` の数。  
  
 メソッドが E_NOT_SUFFICIENT_BUFFER を返す場合は、パスを格納するために必要な `WCHAR` の数。  
  
 `wszPathBuffer`  
 [out] 要求されたメタデータを格納するファイルの完全パスが、デバッガーによりコピーされるバッファーへのポインター。  
  
 [Coropenflags](../../../../docs/framework/unmanaged-api/metadata/coropenflags-enumeration.md)列挙の `ofReadOnly` フラグは、このファイル内のメタデータへの読み取り専用アクセスを要求するために使用されます。  
  
## <a name="return-value"></a>戻り値  
 このメソッドは、次の特定の HRESULT と、メソッドの失敗を示す HRESULT エラーも返します。 これ以外のエラー HRESULT はすべて、ファイルを取得できないことを示します。  
  
|HRESULT|説明|  
|-------------|-----------------|  
|S_OK|メソッドは正常に終了しました。 `wszPathBuffer` にはファイルの完全パスが含まれます。また終端は null です。|  
|E_NOT_SUFFICIENT_BUFFER|`wszPathBuffer` の現在のサイズが十分ではないため、完全パスを保持できません。 この場合、`pcchPathBuffer` に必要な `WCHAR` の数 (終端の null 文字も含む) が格納され、要求されたバッファー サイズで `GetMetaData` がもう一度呼び出されます。|  
  
## <a name="remarks"></a>コメント  
 `wszImagePath` にダンプのモジュールの完全パスが格納されている場合は、ダンプが収集されたコンピューターからのパスを示しています。 この場所にはファイルが存在しない、または同じ名前の正しくないファイルがパス上に格納されている可能性があります。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ICorDebugThread4 インターフェイス](icordebugthread4-interface.md)
- [デバッグ インターフェイス](debugging-interfaces.md)
- [デバッグ](index.md)
