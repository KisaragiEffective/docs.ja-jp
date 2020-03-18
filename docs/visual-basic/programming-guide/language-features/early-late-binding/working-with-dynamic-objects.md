---
title: 動的オブジェクトの使用
ms.date: 07/20/2015
helpviewer_keywords:
- dynamic objects [Visual Basic]
ms.assetid: bdee2a00-07ff-46f9-86dd-fdac9b99cc97
ms.openlocfilehash: 20d007fb48e1db352bab6d8e25d2e60e02554732
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74345175"
---
# <a name="working-with-dynamic-objects-visual-basic"></a>動的オブジェクトの使用 (Visual Basic)
動的オブジェクトを使用すると、`Object` 型以外の別の方法で、実行時にオブジェクトへの遅延バインドを行うことができます。 動的オブジェクトは、<xref:System.Dynamic> 名前空間で定義されている動的インターフェイスを使用して、実行時にプロパティやメソッドなどのメンバーを公開します。 <xref:System.Dynamic> 名前空間のクラスを使用すると、静的な型または形式に一致しないデータ構造体を操作するオブジェクトを作成できます。 IronPython や IronRuby などの動的言語で定義されている動的オブジェクトを使用することもできます。 動的オブジェクトを作成したり、動的言語で定義された動的オブジェクトを使用したりする方法を示す例については、「[チュートリアル: 動的オブジェクトの作成と使用](../../../../csharp/programming-guide/types/walkthrough-creating-and-using-dynamic-objects.md)」、「<xref:System.Dynamic.DynamicObject>」、または「<xref:System.Dynamic.ExpandoObject>」を参照してください。  
  
 Visual Basic は、<xref:System.Dynamic.IDynamicMetaObjectProvider> インターフェイスを使用して、動的言語ランタイムおよび動的言語 (IronPython や IronRuby など) からオブジェクトにバインドします。 `IDynamicMetaObjectProvider` インターフェイスを実装するクラスの例として、<xref:System.Dynamic.DynamicObject> クラスと <xref:System.Dynamic.ExpandoObject> クラスがあります。  
  
 `IDynamicMetaObjectProvider` インターフェイスを実装するオブジェクトに対して遅延バインディング呼び出しが行われた場合、Visual Basic はそのインターフェイスを使用して動的オブジェクトにバインドされます。 `IDynamicMetaObjectProvider` インターフェイスを実装していないオブジェクトに対して遅延バインディング呼び出しが行われた場合、または `IDynamicMetaObjectProvider` インターフェイスへの呼び出しが失敗した場合、Visual Basic Visual Basic ランタイムの遅延バインディング機能を使用してオブジェクトにバインドされます。  
  
## <a name="see-also"></a>参照

- <xref:System.Dynamic.DynamicObject>
- <xref:System.Dynamic.ExpandoObject>
- [チュートリアル: 動的オブジェクトの作成と使用](../../../../csharp/programming-guide/types/walkthrough-creating-and-using-dynamic-objects.md)
- [事前バインディングと遅延バインディング](../../../../visual-basic/programming-guide/language-features/early-late-binding/index.md)
