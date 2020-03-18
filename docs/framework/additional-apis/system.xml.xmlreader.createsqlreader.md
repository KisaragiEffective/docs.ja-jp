---
title: XmlReader (System.xml) メソッド (System.xml)
ms.date: 10/17/2019
topic_type:
- apiref
api_name:
- System.Xml.XmlReader.CreateSqlReader
api_location:
- system.xml.dll
api_type:
- Assembly
ms.openlocfilehash: c65ef7c073175488c11c5e912a44d46fd4319209
ms.sourcegitcommit: 9c54866bcbdc49dbb981dd55be9bbd0443837aa2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/14/2020
ms.locfileid: "77215450"
---
# <a name="xmlreadercreatesqlreader-method"></a>XmlReader.CreateSqlReader  メソッド

解析のために指定されたストリーム、設定、およびコンテキスト情報を使用して、新しい <xref:System.Xml.XmlReader> インスタンスを作成します。

```csharp
internal static XmlReader CreateSqlReader(Stream input, 
  XmlReaderSettings settings, XmlParserContext inputContext)
```

## <a name="parameters"></a>パラメーター

- `input` <xref:System.IO.Stream>  
  XML データを格納しているストリーム。

- `settings` <xref:System.Xml.XmlReaderSettings>  
  新しい <xref:System.Xml.XmlReader> インスタンスの設定。 この値には `null` を指定できます。

- `inputContext` <xref:System.Xml.XmlParserContext>  
  XML フラグメントの解析に必要なコンテキスト情報。 この値には `null` を指定できます。

## <a name="returns"></a>戻り値

<xref:System.Xml.XmlReader>  
ストリーム内の XML データの読み取りに使用するオブジェクト。

## <a name="remarks"></a>コメント

> [!WARNING]
> `XmlReader.CreateSqlReader` メソッドは内部であり、コードで直接使用するためのものではありません。
>
> Microsoft では、どのような状況でも、実稼働アプリケーションでこの方法を使用することはサポートしていません。

## <a name="requirements"></a>要件

**名前空間:** <xref:System.Xml>

**アセンブリ:** System.xml

**.NET Framework のバージョン:** 2.0 以降で使用できます。
