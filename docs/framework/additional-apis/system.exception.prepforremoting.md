---
title: PrepForRemoting メソッド (System)
ms.date: 10/08/2019
topic_type:
- apiref
api_name:
- System.Exception.PrepForRemoting
api_location:
- mscorlib.dll
api_type:
- Assembly
ms.openlocfilehash: ce1c24578690a1643b7f5af0e44eaae95ed7b0a2
ms.sourcegitcommit: 9c54866bcbdc49dbb981dd55be9bbd0443837aa2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/14/2020
ms.locfileid: "77214895"
---
# <a name="exceptionprepforremoting-method"></a>Exception.PrepForRemoting メソッド

クライアント呼び出しサイトで例外が再スローされる前に、サーバー側スタックトレースをメッセージに追加して保存します。

```csharp
internal Exception PrepForRemoting();
```

## <a name="returns"></a>戻り値

<xref:System.Exception>  
この <xref:System.Exception> インスタンス。

## <a name="remarks"></a>コメント

> [!WARNING]
> `Exception.PrepForRemoting` メソッドは内部であり、コードで直接使用するためのものではありません。
>
> Microsoft では、どのような状況でも、実稼働アプリケーションでこの方法を使用することはサポートしていません。

## <a name="requirements"></a>要件

**名前空間:** <xref:System>

**アセンブリ:** mscorlib.dll (mscorlib.dll 内)

**.NET Framework のバージョン:** 1.0 以降で使用できます。
