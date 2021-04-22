---
title: Exception.PrepForRemoting メソッド (System)
description: .NET の Exception.PrepForRemoting メソッドについて説明します。 このメソッドを使用すると、クライアントで例外が再スローされる前に、サーバー側のスタック トレースがメッセージに追加されます。
ms.date: 10/08/2019
topic_type:
- apiref
api_name:
- System.Exception.PrepForRemoting
api_location:
- mscorlib.dll
api_type:
- Assembly
ms.openlocfilehash: 9ceb73499ae3bb308975e6db5b961bfe40165ba3
ms.sourcegitcommit: 1c37a894c923bea021a3cc38ce7cba946357bbe1
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/19/2020
ms.locfileid: "85105257"
---
# <a name="exceptionprepforremoting-method"></a>Exception.PrepForRemoting メソッド

クライアント呼び出しサイトで例外が再スローされる前に、サーバー側スタック トレースをメッセージに追加することによって保存します。

```csharp
internal Exception PrepForRemoting();
```

## <a name="returns"></a>戻り値

<xref:System.Exception>  
この <xref:System.Exception> インスタンス。

## <a name="remarks"></a>解説

> [!WARNING]
> `Exception.PrepForRemoting` は内部メソッドであり、コードで直接使用されるものではありません。
>
> Microsoft は、状況に関係なく、運用アプリケーションでのこのメソッドの使用はサポートしていません。

## <a name="requirements"></a>必要条件

**名前空間:** <xref:System>

**アセンブリ:** mscorlib.dll (mscorlib.dll 内)

**.NET Framework のバージョン:** 1.0 以降で使用可能。
