---
title: LoadTypeLibWithResolver 関数
ms.date: 03/30/2017
api_name:
- LoadTypeLibWithResolver
api_location:
- TlbRef.dll
api_type:
- DLLExport
f1_keywords:
- LoadTypeLibWithResolver
helpviewer_keywords:
- LoadTypeLibWithResolver function [.NET Framework]
ms.assetid: 7123a89b-eb9b-463a-a552-a081e33b0a3a
topic_type:
- apiref
ms.openlocfilehash: adbb5eca3b7ffa36d0c963d0dacc3b2afdb664d4
ms.sourcegitcommit: 7e2128d4a4c45b4274bea3b8e5760d4694569ca1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/14/2020
ms.locfileid: "75935555"
---
# <a name="loadtypelibwithresolver-function"></a>LoadTypeLibWithResolver 関数
タイプライブラリを読み込み、指定された[ITypeLibResolver インターフェイス](itypelibresolver-interface.md)を使用して、内部で参照されているタイプライブラリを解決します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT LoadTypeLibWithResolver(  
    [in]  LPCOLESTR           szFile,  
    [in]  REGKIND             regkind,  
    [in]  ITypeLibResolver   *pTlbResolver,  
    [out] ITypeLib          **pptlib);  
```  
  
## <a name="parameters"></a>パラメーター  
 `szFile`  
 からタイプライブラリのファイルパス。  
  
 `regkind`  
 からタイプライブラリの登録方法を制御する[Regkind 列挙](/windows/win32/api/oleauto/ne-oleauto-regkind)フラグ。 指定できる値は次のとおりです。  
  
- `REGKIND_DEFAULT`: 既定の登録動作を使用します。  
  
- `REGKIND_REGISTER`: このタイプライブラリを登録します。  
  
- `REGKIND_NONE`: このタイプライブラリを登録しません。  
  
 `pTlbResolver`  
 から[ITypeLibResolver インターフェイス](itypelibresolver-interface.md)の実装へのポインター。  
  
 `pptlib`  
 入出力読み込まれているタイプライブラリへの参照。  
  
## <a name="return-value"></a>戻り値  
 次の表に示す HRESULT 値の1つ。  
  
|戻り値|説明|  
|------------------|-------------|  
|`S_OK`|成功。|  
|`E_OUTOFMEMORY`|メモリ不足です。|  
|`E_POINTER`|1つ以上のポインターが無効です。|  
|`E_INVALIDARG`|1 つ以上の引数が無効です。|  
|`TYPE_E_IOERROR`|関数はファイルに書き込めませんでした。|  
|`TYPE_E_REGISTRYACCESS`|システム登録データベースを開けませんでした。|  
|`TYPE_E_INVALIDSTATE`|タイプライブラリを開けませんでした。|  
|`TYPE_E_CANTLOADLIBRARY`|タイプライブラリまたは DLL を読み込めませんでした。|  
  
## <a name="remarks"></a>Remarks  
 [Tlbexp.exe (タイプライブラリエクスポーター)](../../tools/tlbexp-exe-type-library-exporter.md)は、アセンブリからタイプライブラリへの変換プロセス中に `LoadTypeLibWithResolver` 関数を呼び出します。  
  
 この関数は、レジストリへの最小限のアクセスで、指定されたタイプライブラリを読み込みます。 次に、関数は、内部的に参照されるタイプライブラリのタイプライブラリを調べます。このタイプライブラリはそれぞれ、親タイプライブラリに読み込まれ、追加される必要があります。  
  
 参照されるタイプライブラリを読み込む前に、その参照ファイルのパスを完全なファイルパスに解決する必要があります。 これは、`pTlbResolver` パラメーターで渡される[ITypeLibResolver インターフェイス](itypelibresolver-interface.md)によって提供される[resolvetypelib メソッド](resolvetypelib-method.md)を使用して実現されます。  
  
 参照されているタイプライブラリの完全なファイルパスがわかっている場合、`LoadTypeLibWithResolver` 関数は、参照されるタイプライブラリを読み込み、親タイプライブラリに追加して、結合されたマスタータイプライブラリを作成します。  
  
 関数は、内部的に参照されているすべてのタイプライブラリを解決して読み込み、`pptlib` パラメーターでマスター解決済みタイプライブラリへの参照を返します。  
  
 `LoadTypeLibWithResolver` 関数は、通常、 [tlbexp.exe (タイプライブラリエクスポーター)](../../tools/tlbexp-exe-type-library-exporter.md)によって呼び出されます。これは、独自の内部[ITypeLibResolver インターフェイス](itypelibresolver-interface.md)実装を `pTlbResolver` パラメーターに提供します。  
  
 `LoadTypeLibWithResolver` を直接呼び出す場合は、独自の[ITypeLibResolver インターフェイス](itypelibresolver-interface.md)実装を指定する必要があります。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Tlf .h  
  
 **ライブラリ:** Tlf .lib  
  
 **.NET Framework バージョン:** 3.5、3.0、2.0  
  
## <a name="see-also"></a>関連項目

- [Tlbexp ヘルパー関数](index.md)
- [LoadTypeLibEx 関数](https://docs.microsoft.com/previous-versions/windows/desktop/api/oleauto/nf-oleauto-loadtypelibex)
