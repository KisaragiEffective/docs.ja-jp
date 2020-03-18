---
title: 配列のトラブルシューティング
ms.date: 07/20/2015
helpviewer_keywords:
- troubleshooting arrays
- arrays [Visual Basic], initialization errors
- troubleshooting Visual Basic, arrays
- arrays [Visual Basic], compilation errors
- arrays [Visual Basic], declaration errors
- arrays [Visual Basic], troubleshooting
ms.assetid: f4e971c7-c0a4-4ed7-a77a-8d71039f266f
ms.openlocfilehash: 3c50c68c2a39aa04cff2dd43b5dfde709aec290f
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74349065"
---
# <a name="troubleshooting-arrays-visual-basic"></a>配列のトラブルシューティング (Visual Basic)
このページでは、配列を使用するときに発生する可能性のある一般的な問題をいくつか示します。  
  
## <a name="compilation-errors-declaring-and-initializing-an-array"></a>配列の宣言と初期化に関するコンパイルエラー  
 コンパイルエラーは、配列の宣言、作成、および初期化に関する規則の誤解から生じる可能性があります。 エラーの最も一般的な原因は次のとおりです。  
  
- 配列変数宣言で次元の長さを指定した後に、[New 演算子](../../../../visual-basic/language-reference/operators/new-operator.md) 句を指定します。 次のコード行は、この型の無効な宣言を示しています。  
  
     `Dim INVALIDsingleDimByteArray(2) As Byte = New Byte()`  
  
     `Dim INVALIDtwoDimShortArray(1, 1) As Short = New Short(,)`  
  
     `Dim INVALIDjaggedByteArray(1)() As Byte = New Byte()()`  
  
- ジャグ配列の最上位レベル配列を超える次元の長さを指定します。 次のコード行は、この型の無効な宣言を示しています。  
  
     `Dim INVALIDjaggedByteArray(1)(1) As Byte`  
  
- 要素の値を指定するときに、`New` キーワードを省略します。 次のコード行は、この型の無効な宣言を示しています。  
  
     `Dim INVALIDoneDimShortArray() As Short = Short() {0, 1, 2, 3}`  
  
- 中かっこ (`{}`) のない `New` 句を指定します。 次のコード行は、この型の無効な宣言を示しています。  
  
     `Dim INVALIDsingleDimByteArray() As Byte = New Byte()`  
  
     `Dim INVALIDsingleDimByteArray() As Byte = New Byte(2)`  
  
     `Dim INVALIDtwoDimShortArray(,) As Short = New Short(,)`  
  
     `Dim INVALIDtwoDimShortArray(,) As Short = New Short(1, 1)`  
  
## <a name="accessing-an-array-out-of-bounds"></a>配列へのアクセス (範囲外)  
 配列を初期化するプロセスでは、各次元に上限と下限が割り当てられます。 配列の要素へのすべてのアクセスでは、各次元に有効なインデックス (添字) を指定する必要があります。 インデックスが下限を下回るか上限を超えている場合、<xref:System.IndexOutOfRangeException> 例外が発生します。 コンパイラはこのようなエラーを検出できないため、実行時にエラーが発生します。  
  
### <a name="determining-bounds"></a>範囲の決定  
 別のコンポーネントがコードに配列を渡す場合 (たとえば、プロシージャの引数として)、その配列のサイズや次元の長さはわかりません。 すべての要素にアクセスする前に、配列の各次元の上限を常に決定する必要があります。 配列が Visual Basic `New` 句以外の何らかの方法で作成されている場合、下限は0以外の値になる可能性があり、下限が下限であるかどうかを判断するのは安全です。  
  
### <a name="specifying-the-dimension"></a>ディメンションの指定  
 多次元配列の範囲を決定するときは、ディメンションをどのように指定するかを考慮してください。 <xref:System.Array.GetLowerBound%2A> メソッドと <xref:System.Array.GetUpperBound%2A> メソッドの `dimension` パラメーターは0から始まるものですが、Visual Basic <xref:Microsoft.VisualBasic.Information.LBound%2A> 関数と <xref:Microsoft.VisualBasic.Information.UBound%2A> 関数の `Rank` パラメーターは1から始まるものです。  
  
## <a name="see-also"></a>参照

- [配列](../../../../visual-basic/programming-guide/language-features/arrays/index.md)
- [方法: Visual Basic で配列変数を初期化する](../../../../visual-basic/programming-guide/language-features/arrays/how-to-initialize-an-array-variable.md)
