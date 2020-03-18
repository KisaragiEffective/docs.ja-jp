---
title: StrongNameKeyInstall 関数
ms.date: 03/30/2017
api_name:
- StrongNameKeyInstall
api_location:
- mscoree.dll
api_type:
- DLLExport
f1_keywords:
- StrongNameKeyInstall
helpviewer_keywords:
- StrongNameKeyInstall function [.NET Framework strong naming]
ms.assetid: e32fd546-7757-4681-be3d-658e93281e50
topic_type:
- apiref
ms.openlocfilehash: 9e441d4da64e9704fbda2368d2b07289aaea610a
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73125196"
---
# <a name="strongnamekeyinstall-function"></a>StrongNameKeyInstall 関数

公開/秘密キーの組がコンテナーにインポートされます。

この関数は非推奨とされます。 代わりに[ICLRStrongName:: StrongNameKeyInstall](../hosting/iclrstrongname-strongnamekeyinstall-method.md)メソッドを使用してください。

## <a name="syntax"></a>構文

```cpp
BOOLEAN StrongNameKeyInstall (
    [in]  LPCWSTR   wszKeyContainer,
    [in]  BYTE      *pbKeyBlob,
    [in]  ULONG     cbKeyBlob
);
```

## <a name="parameters"></a>パラメーター

`wszKeyContainer`\
からキーコンテナーの名前。 `wszKeyContainer` は空でない文字列である必要があります。

`pbKeyBlob`\
からバイナリキーペア。

`cbKeyBlob`\
から`pbKeyBlob`のサイズ (バイト単位)。

## <a name="return-value"></a>戻り値

正常に完了した場合は `true`。それ以外の場合は、`false`ます。

## <a name="remarks"></a>Remarks

キーコンテナーを削除するには、 [StrongNameKeyDelete](strongnamekeydelete-function.md)関数を使用します。

`StrongNameKeyInstall` 関数が正常に完了しない場合は、 [StrongNameErrorInfo](strongnameerrorinfo-function.md)関数を呼び出して、最後に生成されたエラーを取得します。

## <a name="requirements"></a>［要件］

**:** 「[システム要件](../../get-started/system-requirements.md)」を参照してください。

**ヘッダー:** StrongName

**ライブラリ:** Mscoree.dll にリソースとして含まれています

**.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]

## <a name="see-also"></a>関連項目

- [StrongNameKeyInstall メソッド](../hosting/iclrstrongname-strongnamekeyinstall-method.md)
- [StrongNameKeyDelete メソッド](../hosting/iclrstrongname-strongnamekeydelete-method.md)
- [ICLRStrongName インターフェイス](../hosting/iclrstrongname-interface.md)
