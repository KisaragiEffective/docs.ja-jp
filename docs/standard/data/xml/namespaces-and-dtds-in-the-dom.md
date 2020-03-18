---
title: DOM における名前空間と DTD
ms.date: 03/30/2017
ms.technology: dotnet-standard
ms.assetid: 1e9b55c4-76ad-4f54-8d96-7ce4b4cf1e05
ms.openlocfilehash: 22762e3a7003d9b28a53c7b500829aaa41924c6d
ms.sourcegitcommit: 5f236cd78cf09593c8945a7d753e0850e96a0b80
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2020
ms.locfileid: "75710597"
---
# <a name="namespaces-and-dtds-in-the-dom"></a>DOM における名前空間と DTD
ドキュメント型定義 (DTD) によって名前空間のサポートは複雑になります。 たとえば、次の XML では、既定の属性の名前にコロンが含まれています。  
  
```xml  
<!ATTLIST item x:id CDATA #IMPLIED>  
```  
  
 この構造が許容される場合は、次のように解決される可能性があります。  
  
- `x:` は名前空間プレフィックスとして扱われますが、このプレフィックスは `xmlns:x` という名前空間宣言によって解決されなければならないため、名前空間宣言を DTD のどこかに記述する必要があります。 このプレフィックスをインスタンス ドキュメント内の別の対象に割り当てるとエラーになります。  
  
- `x:` は名前空間プレフィックスとして扱われますが、このプレフィックスは常にインスタンス要素のコンテキストで解決されます。 つまり、`item` 要素が現れる名前空間スコープに応じて、プレフィックスが別々の名前空間 URI (Uniform Resource Identifier) に割り当てられる可能性があります。 これは上記の解決よりも予測しやすい動作ですが、既定の属性を実体化する必要があるため、別の複雑な問題が発生します。  
  
- コロンは DTD 内にあるために無視され、属性の名前はプレフィックスと名前空間 URI のない `x:y` になります。  
  
- 既定の属性にコロンが含まれているため、DTD 内の名前でコロンを使用することはサポートされていないことを示す例外がスローされます。 これは予測可能な動作ですが、W3C (World Wide Web Consortium) が公開している DTD の多くは読み込めないことになります。  
  
- ユーザーが DTD 検証を要求すると、ドキュメント全体に対する名前空間のサポートがオフになります。 これによって W3C DTD の読み込みが可能になり、結果は予測可能な動作になります。  
  
 Microsoft .NET Framework の XML には、W3C との互換性を最大限に保つための第 2 のオプションが実装されています。  
  
## <a name="see-also"></a>関連項目

- [XML ドキュメント オブジェクト モデル (DOM)](../../../../docs/standard/data/xml/xml-document-object-model-dom.md)
