---
title: ISymUnmanagedReader インターフェイス
ms.date: 03/30/2017
api_name:
- ISymUnmanagedReader
api_location:
- diasymreader.dll
api_type:
- COM
f1_keywords:
- ISymUnmanagedReader
helpviewer_keywords:
- ISymUnmanagedReader interface [.NET Framework debugging]
ms.assetid: aa3cc15d-058e-4e6f-b03e-39569646ba47
topic_type:
- apiref
ms.openlocfilehash: 7ae978f5d2c9f7e90f4549c15a3935b441eabf04
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74434010"
---
# <a name="isymunmanagedreader-interface"></a>ISymUnmanagedReader インターフェイス
シンボルストア内のドキュメント、メソッド、および変数へのアクセスを提供するシンボルリーダーを表します。  
  
## <a name="methods"></a>メソッド  
  
|メソッド|説明|  
|------------|-----------------|  
|[GetDocument メソッド](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedreader-getdocument-method.md)|ドキュメントを検索します。|  
|[GetDocuments メソッド](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedreader-getdocuments-method.md)|シンボルストアに定義されているすべてのドキュメントの配列を返します。|  
|[GetDocumentVersion メソッド](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedreader-getdocumentversion-method.md)|指定したドキュメントの指定したバージョンを取得します。|  
|[GetGlobalVariables メソッド](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedreader-getglobalvariables-method.md)|すべてのグローバル変数を返します。|  
|[GetMethod メソッド](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedreader-getmethod-method.md)|メソッドトークンを指定して、シンボルリーダーメソッドを取得します。|  
|[GetMethodByVersion メソッド](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedreader-getmethodbyversion-method.md)|メソッドトークンと編集およびコピーバージョン番号を指定して、シンボルリーダーメソッドを取得します。|  
|[GetMethodFromDocumentPosition メソッド](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedreader-getmethodfromdocumentposition-method.md)|ドキュメント内の指定された位置にあるブレークポイントを含むメソッドを返します。|  
|[GetMethodsFromDocumentPosition メソッド](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedreader-getmethodsfromdocumentposition-method.md)|メソッドの配列を返します。各メソッドには、ドキュメント内の指定された位置にあるブレークポイントが含まれています。|  
|[GetMethodVersion メソッド](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedreader-getmethodversion-method.md)|メソッドのバージョンを取得します。|  
|[GetNamespaces メソッド](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedreader-getnamespaces-method.md)|このシンボルストア内のグローバルスコープで定義されている名前空間を取得します。|  
|[GetSymAttribute メソッド](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedreader-getsymattribute-method.md)|名前に基づいてカスタム属性を取得します。|  
|[GetSymbolStoreFileName メソッド](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedreader-getsymbolstorefilename-method.md)|シンボルストアのディスク上のファイル名を提供します。|  
|[GetUserEntryPoint メソッド](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedreader-getuserentrypoint-method.md)|モジュールのユーザーエントリポイントとして指定されたメソッド (存在する場合) を返します。|  
|[GetVariables メソッド](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedreader-getvariables-method.md)|親と名前を指定して、非ローカル変数を返します。|  
|[Initialize メソッド](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedreader-initialize-method.md)|このリーダーが関連付けられるメタデータインポーターインターフェイスと、モジュールのファイル名を使用して、シンボルリーダーを初期化します。|  
|[ReplaceSymbolStore メソッド](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedreader-replacesymbolstore-method.md)|既存のシンボル ストアをデルタ シンボル ストアで置き換えます。|  
|[UpdateSymbolStore メソッド](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedreader-updatesymbolstore-method.md)|既存のシンボル ストアをデルタ シンボル ストアで更新します。|  
  
## <a name="requirements"></a>要件  
 **ヘッダー:** CorSym .idl、CorSym .h  
  
## <a name="see-also"></a>参照

- [シンボル ストア診断インターフェイス](../../../../docs/framework/unmanaged-api/diagnostics/diagnostics-symbol-store-interfaces.md)
- [ISymUnmanagedReader2 インターフェイス](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedreader2-interface.md)
