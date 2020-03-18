---
title: MemoryStream (System.IO) メソッド ()
ms.date: 11/19/2019
topic_type:
- apiref
api_name:
- System.IO.MemoryStream.InternalGetOriginAndLength
api_location:
- mscorlib.dll
api_type:
- Assembly
ms.openlocfilehash: d82b5080e9fbd5fc6603f1cddae996c75a06d3a3
ms.sourcegitcommit: 9c54866bcbdc49dbb981dd55be9bbd0443837aa2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/14/2020
ms.locfileid: "77215461"
---
# <a name="memorystreaminternalgetoriginandlength-method"></a>MemoryStream および Length メソッド

メモリストリームの原点と長さの内部値を取得します。

```csharp
internal void InternalGetOriginAndLength(out int origin, out int length)
```

## <a name="parameters"></a>パラメーター

- `origin` <xref:System.Int32>\
  このメソッドから制御が戻るときに、新しい <xref:System.IO.MemoryStream> オブジェクトを作成するときに指定されたバイト配列のオフセット。 <xref:System.IO.MemoryStream>によってバイト配列が作成された場合は0を格納します。

- `length` <xref:System.Int32>\
  このメソッドから制御が戻るときに、メモリストリーム内のバイト数。

## <a name="remarks"></a>コメント

> [!WARNING]
> `MemoryStream.InternalGetOriginAndLength` メソッドは内部であり、コードで直接使用するためのものではありません。
>
> Microsoft では、どのような状況でも、実稼働アプリケーションでこの方法を使用することはサポートしていません。

## <a name="requirements"></a>要件

**名前空間:** <xref:System.IO>

**アセンブリ:** mscorlib.dll (mscorlib.dll 内)

**.NET Framework のバージョン:** 2.0 以降で使用できます。
