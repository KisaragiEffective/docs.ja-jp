---
title: WriteStartHeaders メソッド (System.servicemodel. Channels)
ms.date: 11/01/2019
topic_type:
- apiref
api_name:
- System.ServiceModel.Channels.Message.WriteStartHeaders
api_location:
- system.servicemodel.dll
api_type:
- Assembly
ms.openlocfilehash: c826e6a3b976e5705e9815586441e8a25b64f76e
ms.sourcegitcommit: 9c54866bcbdc49dbb981dd55be9bbd0443837aa2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/14/2020
ms.locfileid: "77214865"
---
# <a name="messagewritestartheaders-method"></a>WriteStartHeaders メソッド

<xref:System.ServiceModel.Channels.Message.OnWriteStartHeaders%2A?displayProperty=nameWithType> メソッドを呼び出すことによって、開始ヘッダーを XML ファイルに書き込みます。

```csharp
internal void WriteStartHeaders(XmlDictionaryWriter writer)
```

## <a name="parameters"></a>パラメーター

- `writer` <xref:System.Xml.XmlDictionaryWriter>\
  開始ヘッダーを XML ファイルに書き込むために使用するライター。

## <a name="remarks"></a>コメント

> [!WARNING]
> `Message.WriteStartHeaders` メソッドは内部であり、コードで直接使用するためのものではありません。
>
> Microsoft では、どのような状況でも、実稼働アプリケーションでこの方法を使用することはサポートしていません。

## <a name="requirements"></a>要件

**名前空間:** <xref:System.ServiceModel.Channels>

**アセンブリ:** System.servicemodel

**.NET Framework のバージョン:** 3.0 以降で使用できます。
