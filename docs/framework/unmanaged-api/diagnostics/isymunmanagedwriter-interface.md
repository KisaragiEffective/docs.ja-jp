---
title: ISymUnmanagedWriter インターフェイス
ms.date: 03/30/2017
api_name:
- ISymUnmanagedWriter
api_location:
- diasymreader.dll
api_type:
- COM
f1_keywords:
- ISymUnmanagedWriter
helpviewer_keywords:
- ISymUnmanagedWriter interface [.NET Framework debugging]
ms.assetid: 7d6733ec-f081-4166-bc17-de09e16dc304
topic_type:
- apiref
ms.openlocfilehash: fc19ee25e903046daef376e4297c8feb3d01ad47
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74427937"
---
# <a name="isymunmanagedwriter-interface"></a>ISymUnmanagedWriter インターフェイス
シンボルライターを表し、ドキュメント、シーケンスポイント、構文スコープ、および変数を定義するメソッドを提供します。  
  
## <a name="methods"></a>メソッド  
  
|メソッド|説明|  
|------------|-----------------|  
|[Abort メソッド](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedwriter-abort-method.md)|シンボルストアにシンボルをコミットせずにシンボルライターを閉じます。|  
|[Close メソッド](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedwriter-close-method.md)|シンボルをシンボルストアにコミットした後、シンボルライターを閉じます。|  
|[CloseMethod メソッド](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedwriter-closemethod-method.md)|現在のメソッドを閉じます。 メソッドを閉じると、それ以上シンボルを定義することはできません。|  
|[CloseNamespace メソッド](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedwriter-closenamespace-method.md)|最近開いた名前空間を閉じます。|  
|[CloseScope メソッド](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedwriter-closescope-method.md)|現在の構文のスコープを閉じます。|  
|[DefineConstant メソッド](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedwriter-defineconstant-method.md)|定数値の名前を定義します。|  
|[DefineDocument メソッド](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedwriter-definedocument-method.md)|ソース ドキュメントを定義します。|  
|[DefineField メソッド](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedwriter-definefield-method.md)|メソッド内にない単一の変数を定義します。|  
|[DefineGlobalVariable メソッド](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedwriter-defineglobalvariable-method.md)|1つのグローバル変数を定義します。|  
|[DefineLocalVariable メソッド](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedwriter-definelocalvariable-method.md)|現在の構文のスコープの変数を 1 つ定義します。|  
|[DefineParameter メソッド](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedwriter-defineparameter-method.md)|現在のメソッドの1つのパラメーターを定義します。|  
|[DefineSequencePoints メソッド](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedwriter-definesequencepoints-method.md)|現在のメソッド内のシーケンス ポイントのグループを定義します。|  
|[GetDebugInfo メソッド](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedwriter-getdebuginfo-method.md)|コンパイラがポータブル実行可能 (PE) ファイルヘッダーにデバッグディレクトリエントリを書き込むために必要な情報を返します。|  
|[Initialize メソッド](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedwriter-initialize-method.md)|このライターが関連付けられるメタデータエミッタインターフェイスを設定し、デバッグシンボルの書き込み先となる出力ファイル名を設定します。|  
|[Initialize2 メソッド](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedwriter-initialize2-method.md)|このライターが関連付けられるメタデータエミッタインターフェイスを設定し、デバッグシンボルの書き込み先となる出力ファイル名を設定し、プログラムデータベース (PDB) ファイルの最終的な場所を設定します。|  
|[OpenMethod メソッド](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedwriter-openmethod-method.md)|シンボル情報を出力するメソッドを開きます。|  
|[OpenNamespace メソッド](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedwriter-opennamespace-method.md)|新しい名前空間を開きます。|  
|[OpenScope メソッド](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedwriter-openscope-method.md)|現在のメソッドの構文の新しいスコープを開きます。|  
|[RemapToken メソッド](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedwriter-remaptoken-method.md)|メタデータが生成されたときにメタデータトークンが再マップされたことをシンボルライターに通知します。|  
|[SetMethodSourceRange メソッド](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedwriter-setmethodsourcerange-method.md)|ソースファイル内のメソッドの実際の開始と終了を指定します。|  
|[SetScopeRange メソッド](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedwriter-setscoperange-method.md)|指定した構文のスコープのオフセット範囲を定義します。|  
|[SetSymAttribute メソッド](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedwriter-setsymattribute-method.md)|名前に基づいてカスタム属性を定義します。|  
|[SetUserEntryPoint メソッド](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedwriter-setuserentrypoint-method.md)|このモジュールのエントリポイントであるユーザー定義メソッドを指定します。|  
|[UsingNamespace メソッド](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedwriter-usingnamespace-method.md)|現在開いている構文のスコープ内で、指定された完全修飾名前空間名が使用されていることを指定します。|  
  
## <a name="requirements"></a>要件  
 **ヘッダー:** CorSym .idl、CorSym .h  
  
## <a name="see-also"></a>参照

- [シンボル ストア診断インターフェイス](../../../../docs/framework/unmanaged-api/diagnostics/diagnostics-symbol-store-interfaces.md)
- [ISymUnmanagedWriter2 インターフェイス](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedwriter2-interface.md)
- [ISymUnmanagedWriter3 インターフェイス](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedwriter3-interface.md)
