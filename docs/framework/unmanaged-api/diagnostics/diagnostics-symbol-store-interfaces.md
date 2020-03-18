---
title: シンボル ストア診断インターフェイス
ms.date: 03/30/2017
helpviewer_keywords:
- unmanaged interfaces [.NET Framework], debugging
- diagnostics symbol store interfaces [.NET Framework]
- interfaces [.NET Framework], diagnostics symbol store
- unmanaged interfaces [.NET Framework], diagnostics symbol store
- debugging interfaces [.NET Framework]
- interfaces [.NET Framework debugging]
ms.assetid: f96987d5-e6a5-478b-ac5e-302e16545cce
ms.openlocfilehash: bdb691570a9a2bf7bd2bb21af500b06c10b0bc53
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74448527"
---
# <a name="diagnostics-symbol-store-interfaces"></a>シンボル ストア診断インターフェイス
このトピックでは、コンパイラがデバッガーで使用するシンボル情報を生成できるようにするアンマネージインターフェイスについて説明します。  
  
## <a name="in-this-section"></a>このセクションの内容  
 [IBindingDisplay インターフェイス](../../../../docs/framework/unmanaged-api/diagnostics/ibindingdisplay-interface.md)  
 実行中のアプリケーションに関する現在のバインド情報を表示するメソッドを提供します。  
  
 [IDebugAutoAttach インターフェイス](../../../../docs/framework/unmanaged-api/diagnostics/idebugautoattach-interface.md)  
 サーバーによって呼び出されるデバッガーの自動アタッチのインターフェイスを定義します。  
  
 [INotifyConnection2 インターフェイス](../../../../docs/framework/unmanaged-api/diagnostics/inotifyconnection2-interface.md)  
 接続通知ソースを登録および登録解除するためのメソッドを宣言します。  
  
 [INotifySink2 インターフェイス](../../../../docs/framework/unmanaged-api/diagnostics/inotifysink2-interface.md)  
 シンク通知のメソッドを宣言します。  
  
 [INotifySource2 インターフェイス](../../../../docs/framework/unmanaged-api/diagnostics/inotifysource2-interface.md)  
 通知フィルターを設定するためのメソッドを宣言します。  
  
 [ISymENCUnmanagedMethod インターフェイス](../../../../docs/framework/unmanaged-api/diagnostics/isymencunmanagedmethod-interface.md)  
 エディットコンティニュ機能に関する情報を提供します。  
  
 [ISymUnmanagedAsyncMethod インターフェイス](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedasyncmethod-interface.md)  
 このインターフェイスは、 [ISymUnmanagedAsyncMethodPropertiesWriter インターフェイス](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedasyncmethodpropertieswriter-interface.md)への読み取り補数です。  
  
 [ISymUnmanagedAsyncMethodPropertiesWriter インターフェイス](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedasyncmethodpropertieswriter-interface.md)  
 メソッドシンボルごとにオプションの非同期メソッド情報を定義できます。 は、開いているメソッド (つまり、 [Openmethod メソッド](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedwriter-openmethod-method.md)と[closemethod メソッド](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedwriter-closemethod-method.md)の呼び出しの間) でを使用する必要があります。  
  
 [ISymUnmanagedBinder インターフェイス](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedbinder-interface.md)  
 アンマネージコードのシンボルバインダーを表します。  
  
 [ISymUnmanagedBinder2 インターフェイス](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedbinder2-interface.md)  
 アンマネージコードのシンボルバインダーを表し、`ISymUnmanagedBinder` インターフェイスを拡張します。  
  
 [ISymUnmanagedBinder3 インターフェイス](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedbinder3-interface.md)  
 アンマネージコードのシンボルバインダーを表し、`ISymUnmanagedBinder` インターフェイスを拡張します。  
  
 [ISymUnmanagedConstant インターフェイス](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedconstant-interface.md)  
 アンマネージ定数へのアクセスを提供します。  
  
 [ISymUnmanagedDispose インターフェイス](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanageddispose-interface.md)  
 アンマネージリソースを破棄します。  
  
 [ISymUnmanagedDocument インターフェイス](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanageddocument-interface.md)  
 シンボル ストアによって参照されるドキュメントを表します。  
  
 [ISymUnmanagedDocumentWriter インターフェイス](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanageddocumentwriter-interface.md)  
 シンボル ストアで参照されるドキュメントに書き込むためのメソッドを提供します。  
  
 [ISymUnmanagedENCUpdate インターフェイス](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedencupdate-interface.md)  
 エディットコンティニュ機能のメソッドを提供します。  
  
 [ISymUnmanagedMethod インターフェイス](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedmethod-interface.md)  
 シンボルストア内のメソッドを表します。  
  
 [ISymUnmanagedNamespace インターフェイス](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagednamespace-interface.md)  
 名前空間を表します。  
  
 [ISymUnmanagedReader インターフェイス](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedreader-interface.md)  
 シンボルストア内のドキュメント、メソッド、および変数へのアクセスを提供するシンボルリーダーを表します。  
  
 [ISymUnmanagedReader2 インターフェイス](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedreader2-interface.md)  
 メソッドトークンと編集およびコピーバージョン番号を指定して、シンボルリーダーメソッドを取得します。  
  
 [ISymUnmanagedReaderSymbolSearchInfo インターフェイス](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedreadersymbolsearchinfo-interface.md)  
 シンボル検索情報を取得するメソッドを提供します。  
  
 [ISymUnmanagedScope インターフェイス](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedscope-interface.md)  
 メソッド内の構文のスコープを表します。  
  
 [ISymUnmanagedScope2 インターフェイス](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedscope2-interface.md)  
 メソッド内の構文のスコープを表し、スコープ内で定義された定数に関する情報を取得するメソッドを使用して `ISymUnmanagedScope` インターフェイスを拡張します。「」を参照してください。  
  
 [ISymUnmanagedSourceServerModule インターフェイス](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedsourceservermodule-interface.md)  
 モジュールのソースサーバーデータを提供します。  
  
 [ISymUnmanagedSymbolSearchInfo インターフェイス](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedsymbolsearchinfo-interface.md)  
 検索パスに関する情報を取得するメソッドを提供します。  
  
 [ISymUnmanagedVariable インターフェイス](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedvariable-interface.md)  
 パラメーター、ローカル変数、フィールドなどの変数を表します。  
  
 [ISymUnmanagedWriter インターフェイス](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedwriter-interface.md)  
 シンボルライターを表し、ドキュメント、シーケンスポイント、構文スコープ、および変数を定義するメソッドを提供します。  
  
 [ISymUnmanagedWriter2 インターフェイス](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedwriter2-interface.md)  
 シンボルライターを表し、ドキュメント、シーケンスポイント、構文スコープ、および変数を定義するメソッドを提供します。 `ISymUnmanagedWriter` インターフェイスを拡張します。  
  
 [ISymUnmanagedWriter3 インターフェイス](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedwriter3-interface.md)  
 シンボルライターを表し、ドキュメント、シーケンスポイント、構文スコープ、および変数を定義するメソッドを提供します。 `ISymUnmanagedWriter` インターフェイスを拡張します。  
  
 [ISymUnmanagedWriter4 インターフェイス](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedwriter4-interface.md)  
 ISymUnmanagedWriter4 インターフェイス。  
  
 [ISymUnmanagedWriter5 インターフェイス](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedwriter5-interface.md)  
 ISymUnmanagedWriter5 インターフェイス。  
  
## <a name="related-sections"></a>関連セクション  
 [シンボル ストア診断列挙型](../../../../docs/framework/unmanaged-api/diagnostics/diagnostics-symbol-store-enumerations.md)  
  
 [シンボル ストア診断構造体](../../../../docs/framework/unmanaged-api/diagnostics/diagnostics-symbol-store-structures.md)  
  
 [デバッグ](../../../../docs/framework/unmanaged-api/debugging/index.md)
