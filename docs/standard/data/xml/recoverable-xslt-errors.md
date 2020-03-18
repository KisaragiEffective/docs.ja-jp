---
title: XSLT エラーの解決
ms.date: 03/30/2017
ms.technology: dotnet-standard
ms.assetid: 484929b0-fefb-4629-87ee-ebdde70ff1f8
ms.openlocfilehash: e3ff86cc80887d14fdffe50f256409cb70ff2d88
ms.sourcegitcommit: 5f236cd78cf09593c8945a7d753e0850e96a0b80
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2020
ms.locfileid: "75710376"
---
# <a name="recoverable-xslt-errors"></a>XSLT エラーの解決
W3C 勧告『XSL Transformations (XSLT) Version 1.0』には、対処方法を実装者が決定できる事項があります。 このような事項は、随意動作と見なされています。 たとえば、XSLT 1.0 Recommendation は、セクション 7.3「Creating Processing Instructions」で、`xsl:processing-instruction` の内容をインスタンス化したときに、テキスト ノード以外のノードが作成されるのはエラーであるとしています。 いくつかの問題に関しては、プロセッサがエラー状態から回復するときにどのような対処をするべきかを、XSLT 1.0 Recommendation が規定しています。 セクション 7.3 に記述されている問題に関しては、W3C では、作成されたノードとその内容を無視することで、このエラーから回復できるとしています。  
  
## <a name="discretionary-behaviors"></a>随意動作  
 XSLT 1.0 Recommendation で許可されている随意動作と <xref:System.Xml.Xsl.XslCompiledTransform> クラスによるこれらの動作の処理方法を次の表に示します。  
  
- 復元は、<xref:System.Xml.Xsl.XslCompiledTransform> クラスがこのエラーから回復することを示しています。 <xref:System.Xml.Xsl.XsltArgumentList.XsltMessageEncountered?displayProperty=nameWithType> イベントを使用すると、XSLT プロセッサのすべてのイベントを通知できます。  
  
- エラーは、この状態に対する例外が発生したことを示します。  
  
- 詳細については、[W3C の XSL Transformations (XSLT) Version 1.0 勧告](https://www.w3.org/TR/xslt)に関するページと [W3C の「XSL Transformations (XSLT) Version 1.0 Specification Errata」(XSL Transformations (XSLT) Version 1.0 仕様の正誤表)](https://www.w3.org/1999/11/REC-xslt-19991116-errata/) を参照してください。  
  
|XSLT の状態|セクション|XslCompiledTransform の動作|  
|--------------------|-------------|-----------------------------------|  
|テキスト ノードが `xsl:strip-space` と `xsl:preserve-space` の両方に適合している。|3.4|回復|  
|ソース ノードが複数のテンプレート規則に適合している。|5.5|回復|  
|1 つの名前空間 URI が同じインポート優先順位を持つ複数の名前空間 URI のエイリアスとして宣言されている。|7.1.1|回復|  
|属性値から生成された `name` と `xsl:attribute` 内の `xsl:element` 属性が QName ではない。|7.1.2, 7.1.3|エラー*|  
|同じインポートおよび展開名を持つ 2 つの属性セットに共通の属性が含まれており、これらの属性セット以外に、同じ名前で優先順位の高い共通属性を含む属性セットがない。|7.1.4|回復|  
|要素に子が既に追加されているにもかかわらず、その要素に属性が追加される。|7.1.3|エラー*|  
|'xmlns' という名前を持つ属性が作成される。|7.1.3|エラー*|  
|要素ではないノードに属性が追加される。|7.1.3|エラー*|  
|`xsl:attribute` 属性の内容をインスタンス化する際にテキスト ノード以外のノードが作成される。|7.1.3|エラー*|  
|`name` の `xsl:processing-instruction` 属性が NCName と処理命令ターゲットのどちらも生成しない。|7.3|エラー*|  
|`xsl:processing-instruction` の内容をインスタンス化すると、テキスト ノード以外のノードが作成される。|7.3|エラー*|  
|`xsl:processing-instruction` の内容をインスタンス化した結果に文字列 "?&gt;" が含まれている。|7.3|回復|  
|`xsl:processing-instruction` の内容をインスタンス化した結果に文字列 "--" が含まれているか、または "-" で終了している。|7.4|回復|  
|`xsl:comment` の内容をインスタンス化した結果、テキスト ノード以外のノードが作成される。|7.4|エラー*|  
|変数バインディング要素内のテンプレートが属性ノードまたは名前空間ノードを返す。|11.2|エラー*|  
|document 関数に渡された URI からのリソースの取得時にエラーが発生する。|12.1|エラー|  
|document 関数内の URI 参照にフラグメント ID が含まれており、その ID を処理するとエラーが発生する。|12.1|復元*|  
|同じ名前とさまざまな値を持つ複数の属性があり、これらの属性が同じインポート優先順位を持つ `xsl:output` の名前付き cdata-section 要素にならない。|16|回復|  
|プロセッサが `xsl:output` の encoding 属性のエンコーディングをサポートしない。|16.1|回復|  
|テキスト ノードの出力エスケープが無効で、そのテキスト ノードが結果ツリーのテキスト ノード以外のもののために使用されている。|16.4|復元*|  
|出力エスケープが有効に設定されているテキスト ノードが結果ツリー フラグメントに含まれている場合に、そのフラグメントが数値や文字列に変換される。|16.4|復元*|  
|XSLT プロセッサが出力に使用しているエンコーディングで表すことができない文字に対して、出力エスケープが無効に設定される。|16.4|復元*|  
|要素に子または属性が追加された後で、その要素に名前空間ノードが追加される。|errata 25|エラー*|  
|`value` の `xsl:number` 属性が NAN、無限、または 0.5 未満である。|errata 24|回復|  
|document 関数への 2 番目の引数ノード セットが空であり、URI 参照が相対 URI 参照である。|errata 14|回復|  
  
 <sup>*</sup> この動作は、<xref:System.Xml.Xsl.XslTransform> クラスの動作とは異なります。 詳細については、「[XslTransform クラスの随意動作の実装](../../../../docs/standard/data/xml/implementation-of-discretionary-behaviors-in-the-xsltransform-class.md)」を参照してください。  
  
## <a name="see-also"></a>関連項目

- [XSLT 変換](../../../../docs/standard/data/xml/xslt-transformations.md)
