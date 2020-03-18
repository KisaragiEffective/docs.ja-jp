---
title: moduloObjectHashcode MDA
ms.date: 03/30/2017
helpviewer_keywords:
- managed debugging assistants (MDAs), hashcode modulus
- Modulo object hash code
- moduloObjectHashcode MDA
- hashcode modulus
- MDAs (managed debugging assistants), hashcode modulus
- GetHashCode method
- modulus of hashcodes
ms.assetid: b45366ff-2a7a-4b8e-ab01-537b72e9de68
ms.openlocfilehash: 65bbdfec2d7050d1b474a8186a9ea6e9bb93bd9e
ms.sourcegitcommit: 9c54866bcbdc49dbb981dd55be9bbd0443837aa2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/14/2020
ms.locfileid: "77216177"
---
# <a name="moduloobjecthashcode-mda"></a>moduloObjectHashcode MDA
`moduloObjectHashcode` マネージド デバッグ アシスタント (MDA) は、<xref:System.Object> メソッドによって返されるハッシュ コードに対してモジュロ演算を実行するように、<xref:System.Object.GetHashCode%2A> クラスの動作を変更します。 この MDA の既定の係数は 1 であり、これにより <xref:System.Object.GetHashCode%2A> はすべてのオブジェクトに対して 0 を返すようになります。  
  
## <a name="symptoms"></a>現象  
 新しいバージョンの共通言語ランタイム (CLR) に移行すると、プログラムが正しく動作しなくなります。  
  
- プログラムは、<xref:System.Collections.Hashtable> から正しくないオブジェクトを取得します。  
  
- <xref:System.Collections.Hashtable> からの列挙の順序に、プログラムが動作しなくなる変更があります。  
  
- 以前は等しかった 2 つのオブジェクトが、等しくなくなっています。  
  
- 以前は等しくなかった 2 つのオブジェクトが、等しくなっています。  
  
## <a name="cause"></a>原因  
 <xref:System.Collections.Hashtable> へのキーに対するクラスの <xref:System.Object.Equals%2A> メソッドの実装は、<xref:System.Collections.Hashtable> メソッドの呼び出しの結果を比較することによりオブジェクトの等価性をテストするため、プログラムが <xref:System.Object.GetHashCode%2A> から正しくないオブジェクトを取得している可能性があります。 それぞれのフィールドの値が異なる場合であっても、2 つのオブジェクトのハッシュ コードが同じになる場合があるので、オブジェクトの等価性のテストにハッシュ コードを使うことはできません。 実際にはまれなことですが、ハッシュ コードの衝突が発生します。 このことによる <xref:System.Collections.Hashtable> のルックアップへの影響としては、等しくない 2 つのキーが等しいものと見なされ、正しくないオブジェクトが <xref:System.Collections.Hashtable> から返されます。 パフォーマンス上の理由から、<xref:System.Object.GetHashCode%2A> の実装はランタイム バージョンによって変更される場合があり、あるバージョンでは発生しない競合が後のバージョンでは発生する可能性があります。 ハッシュ コードが競合するときのバグがコードにあるかどうかをテストするには、この MDA を有効にします。 この MDA を有効にすると、<xref:System.Object.GetHashCode%2A> メソッドは 0 を返すようになり、結果としてすべてのハッシュ コードが競合します。 この MDA を有効にすることによるプログラムへの唯一の影響は、プログラムの実行が遅くなることです。  
  
 キー変更のハッシュ コードの計算に使われるアルゴリズムがランタイムのあるバージョンから別のバージョンに変更された場合、<xref:System.Collections.Hashtable> からの列挙の順序が変わる可能性があります。 ハッシュ テーブルからのキーまたは値の列挙順序にプログラムが依存しているかどうかは、この MDA を有効にすることでテストできます。  
  
## <a name="resolution"></a>解決策  
 オブジェクト ID の代わりに、ハッシュ コードを使わないでください。 ハッシュ コードを比較しないように、<xref:System.Object.Equals%2A?displayProperty=nameWithType> メソッドのオーバーライドを実装します。  
  
 ハッシュ テーブル内のキーまたは値の列挙の順序への依存関係を作成しないでください。  
  
## <a name="effect-on-the-runtime"></a>ランタイムへの影響  
 この MDA を有効にすると、アプリケーションの速度が低下します。 この MDA は、返されたハッシュ コードを取得し、代わりに係数で除算したときの余りを返します。  
  
## <a name="output"></a>出力  
 この MDA に出力はありません。  
  
## <a name="configuration"></a>構成  
 `modulus` 属性では、ハッシュ コードで使う係数を指定します。 既定値は 1 です。  
  
```xml  
<mdaConfig>  
  <assistants>  
    <moduloObjectHashcode modulus="1" />  
  </assistants>  
</mdaConfig>  
```  
  
## <a name="see-also"></a>参照

- <xref:System.Object.GetHashCode%2A?displayProperty=nameWithType>
- <xref:System.Object.Equals%2A?displayProperty=nameWithType>
- [マネージド デバッグ アシスタントによるエラーの診断](diagnosing-errors-with-managed-debugging-assistants.md)
