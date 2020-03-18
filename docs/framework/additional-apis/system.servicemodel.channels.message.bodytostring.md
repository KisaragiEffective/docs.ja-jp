---
title: Message. BodyToString メソッド (System.servicemodel)
ms.date: 11/01/2019
topic_type:
- apiref
api_name:
- System.ServiceModel.Channels.Message.BodyToString
api_location:
- system.servicemodel.dll
api_type:
- Assembly
ms.openlocfilehash: 9f1f852c0bd82299fd40afe66a5f90cd7c0335cf
ms.sourcegitcommit: 9c54866bcbdc49dbb981dd55be9bbd0443837aa2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/14/2020
ms.locfileid: "77215503"
---
# <a name="messagebodytostring-method"></a>Message. BodyToString メソッド

<xref:System.ServiceModel.Channels.Message.OnBodyToString%2A?displayProperty=nameWithType> メソッドを呼び出すことによって、メッセージ本文を文字列に変換します。

```csharp
internal void BodyToString(XmlDictionaryWriter writer);
```

## <a name="parameters"></a>パラメーター

- `writer` <xref:System.Xml.XmlDictionaryWriter>\
  メッセージ本文を文字列に変換するために使用されるライター。

## <a name="remarks"></a>コメント

> [!WARNING]
> `Message.BodyToString` メソッドは内部であり、コードで直接使用するためのものではありません。
>
> Microsoft では、どのような状況でも、実稼働アプリケーションでこの方法を使用することはサポートしていません。

## <a name="requirements"></a>要件

**名前空間:** <xref:System.ServiceModel.Channels>

**アセンブリ:** System.servicemodel

**.NET Framework のバージョン:** 3.0 以降で使用できます。
