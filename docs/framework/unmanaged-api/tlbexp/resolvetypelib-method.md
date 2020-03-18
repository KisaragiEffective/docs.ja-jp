---
title: ResolveTypeLib メソッド
ms.date: 03/30/2017
api_name:
- ResolveTypeLib
api_location:
- tlbref.dll
f1_keywords:
- ResolveTypeLib
helpviewer_keywords:
- ITypeLibResolver::ResolveTypeLib method [.NET Framework]
- ResolveTypeLib method [.NET Framework]
ms.assetid: 95d2aa0d-8eeb-4a9f-a216-5249f7e2c167
topic_type:
- apiref
ms.openlocfilehash: f0f6fe321f4d38129b6d70ce94a7ea8de8fff6c8
ms.sourcegitcommit: 7e2128d4a4c45b4274bea3b8e5760d4694569ca1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/14/2020
ms.locfileid: "75935669"
---
# <a name="resolvetypelib-method"></a>ResolveTypeLib メソッド
完全修飾パスを返すことにより、タイプライブラリの簡易名を解決します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT ResolveTypeLib(  
    [in]  BSTR      bstrSimpleName,  
    [in]  GUID      tlbid,  
    [in]  LCID      lcid,  
    [in]  USHORT    wMajorVersion,  
    [in]  USHORT    wMinorVersion,  
    [in]  SYSKIND   syskind,  
    [out] BSTR     *pbstrResolvedTlbName);  
```  
  
## <a name="parameters"></a>パラメーター  
 `bstrSimpleName`  
 からタイプライブラリの簡易名を格納している[BSTR](https://docs.microsoft.com/previous-versions/windows/desktop/automat/bstr) 。  
  
 `tlbid`  
 からレジストリのタイプライブラリに割り当てられている GUID。  
  
 `lcid`  
 からタイプライブラリのローカライズ ID。  
  
 `wMajorVersion`  
 からタイプライブラリのメジャーバージョン番号。 たとえば、バージョン*x.y*の場合、メジャーバージョン番号は*x*になります。  
  
 `wMinorVersion`  
 からタイプライブラリのマイナーバージョン番号。 たとえば、バージョン*x.y*の場合、マイナーバージョン番号は*y*になります。  
  
 `syskind`  
 からオペレーティング環境を識別する[SYSKIND](/windows/win32/api/oaidl/ne-oaidl-syskind)フラグ。 共通値は SYS_WIN32 と SYS_WIN64 です。  
  
 `pbstrResolvedTlbName`  
 入出力`bstrSimpleName` パラメーターに指定されたタイプライブラリの完全パスを格納する[BSTR](https://docs.microsoft.com/previous-versions/windows/desktop/automat/bstr)へのポインター。  
  
## <a name="remarks"></a>Remarks  
 `ResolveTypeLib` メソッドは、 [tlbexp.exe (タイプライブラリエクスポーター)](../../tools/tlbexp-exe-type-library-exporter.md)の処理中に[LoadTypeLibWithResolver 関数](loadtypelibwithresolver-function.md)によって呼び出されます。  
  
 このインターフェイスのカスタム実装では、`bstrSimpleName`パラメーターに指定されたタイプライブラリの完全パスを含む [BSTR](https://docs.microsoft.com/previous-versions/windows/desktop/automat/bstr) を返す必要があります。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Tlf .idl, Tl. h  
  
 **ライブラリ:** Tlf .lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [Tlbexp ヘルパー関数](index.md)
- [LoadTypeLibEx](https://docs.microsoft.com/previous-versions/windows/desktop/api/oleauto/nf-oleauto-loadtypelibex)
