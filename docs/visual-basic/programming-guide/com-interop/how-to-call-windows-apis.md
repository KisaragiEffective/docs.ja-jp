---
title: '方法: Windows API を呼び出す'
ms.date: 07/20/2015
helpviewer_keywords:
- API calls [Visual Basic]
- Windows API, calling
- API calls [Visual Basic], platform invoke
- calls [Visual Basic], stored procedures
ms.assetid: 27d75f0a-54ab-4ee1-b91d-43513a19b12d
ms.openlocfilehash: 9de9f0fbcca291af0b6aadfd8e3fe7033708fbc6
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75347542"
---
# <a name="how-to-call-windows-apis-visual-basic"></a>方法: Windows API を呼び出す (Visual Basic)
この例は user32.dll 内の `MessageBox` 関数の定義と呼び出しを行い、そして文字列を渡しています。  
  
## <a name="example"></a>使用例  
 [!code-vb[VbVbalrInterop#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrInterop/VB/Class1.vb#1)]  
  
## <a name="compile-the-code"></a>コードのコンパイル  
 この例で必要な要素は次のとおりです。  
  
- <xref:System> 名前空間への参照  
  
## <a name="robust-programming"></a>堅牢性の高いプログラミング  
 次の条件を満たす場合は、例外が発生する可能性があります。  
  
- メソッドが静的でない、または抽象メソッドである、または以前に定義されているメソッドの場合。 親の型がインターフェイスである、または*name*や*dllName*の長さが 0 の場合。 (<xref:System.ArgumentException>)  
  
- *name*または*dllName*が`Nothing`の場合。 (<xref:System.ArgumentNullException>)  
  
- 含んでいる型が `CreateType` を使用して以前に作成されています。 (<xref:System.InvalidOperationException>)  
  
## <a name="see-also"></a>関連項目

- [プラットフォーム呼び出しの詳細](../../../framework/interop/consuming-unmanaged-dll-functions.md#a-closer-look-at-platform-invoke)
- [プラットフォーム呼び出しの例](../../../framework/interop/platform-invoke-examples.md)
- [アンマネージ DLL 関数の処理](../../../framework/interop/consuming-unmanaged-dll-functions.md)
- [リフレクション出力によるメソッドの定義](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/w63y4d4f(v=vs.100))
- [チュートリアル : Windows API の呼び出し](../../../visual-basic/programming-guide/com-interop/walkthrough-calling-windows-apis.md)
- [COM 相互運用](../../../visual-basic/programming-guide/com-interop/index.md)
