---
title: StrongNameCompareAssemblies 関数
ms.date: 03/30/2017
api_name:
- StrongNameCompareAssemblies
api_location:
- mscoree.dll
api_type:
- DLLExport
f1_keywords:
- StrongNameCompareAssemblies
helpviewer_keywords:
- StrongNameCompareAssemblies function [.NET Framework strong naming]
ms.assetid: 763f2375-efc6-4219-8806-a3b0567ef72b
topic_type:
- apiref
ms.openlocfilehash: adde52dddb63b83dcd7ff10703a43928d9601c92
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73140625"
---
# <a name="strongnamecompareassemblies-function"></a>StrongNameCompareAssemblies 関数
厳密な名前の署名に基づいて 2 つのアセンブリが異なるかどうかが判定されます。  
  
 この関数は非推奨とされます。 代わりに[ICLRStrongName:: StrongNameCompareAssemblies](../hosting/iclrstrongname-strongnamecompareassemblies-method.md)メソッドを使用してください。  
  
## <a name="syntax"></a>構文  
  
```cpp  
BOOLEAN StrongNameCompareAssemblies (  
    [in]  LPCWSTR   wszAssembly1,  
    [in]  LPCWSTR   wszAssembly2,  
    [out] DWORD     *pdwResult  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `wszAssembly1`  
 から最初のアセンブリへのパス。  
  
 `wszAssembly2`  
 から2番目のアセンブリへのパス。  
  
 `pdwResult`  
 入出力次のいずれかの値です。  
  
- `SN_CMP_DIFFERENT` (0)-アセンブリに異なるデータが含まれることを指定します。  
  
- `SN_CMP_IDENTICAL` (1)-署名やチェックサムなど、アセンブリがまったく同じであることを指定します。  
  
- `SN_CMP_SIGONLY` (2)-アセンブリが署名とチェックサムのみで異なることを指定します。  
  
## <a name="return-value"></a>戻り値  
 正常に完了した場合は `true`。それ以外の場合は、`false`ます。  
  
## <a name="requirements"></a>［要件］  
 **:** 「[システム要件](../../get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** StrongName  
  
 **ライブラリ:** Mscoree.dll にリソースとして含まれています  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="remarks"></a>Remarks  
 アセンブリの厳密な名前の署名は、アセンブリのテキスト名、バージョン、カルチャ、および公開キートークンで構成されます。  
  
 `StrongNameCompareAssemblies` 関数が正常に完了しない場合は、 [StrongNameErrorInfo](strongnameerrorinfo-function.md)関数を呼び出して、最後に生成されたエラーを取得します。  
  
## <a name="see-also"></a>関連項目

- [StrongNameCompareAssemblies メソッド](../hosting/iclrstrongname-strongnamecompareassemblies-method.md)
- [ICLRStrongName インターフェイス](../hosting/iclrstrongname-interface.md)
