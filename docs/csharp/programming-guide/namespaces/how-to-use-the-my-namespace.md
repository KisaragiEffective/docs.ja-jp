---
title: My 名前空間を使用する方法 - C# プログラミング ガイド
ms.date: 07/20/2015
helpviewer_keywords:
- C# language, My namespace access
ms.assetid: e7152414-0ea5-4c8e-bf02-c8d5bbe45ff4
ms.openlocfilehash: 063b46a32ced859c6c86e40c4a6b9870c3decd24
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75700420"
---
# <a name="how-to-use-the-my-namespace-c-programming-guide"></a>My 名前空間を使用する方法 (C# プログラミング ガイド)
<xref:Microsoft.VisualBasic.MyServices> 名前空間 (Visual Basic では `My`) を使用すると、いくつもの .NET Framework クラスに簡単かつ直感的にアクセスでき、コンピューター、アプリケーション、設定、リソースなどと対話するコードを記述できます。 `MyServices` 名前空間は、もともとは Visual Basic で使用するものとして設計されましたが、C# アプリケーションでも使用できます。  
  
 Visual Basic で `MyServices` 名前空間を使用する方法の詳細については、[My を使用した開発](../../../visual-basic/developing-apps/development-with-my/index.md) に関するページを参照してください。  
  
## <a name="adding-a-reference"></a>参照の追加  
 `MyServices` クラスをソリューションで使用する前に、Visual Basic ライブラリへの参照を追加する必要があります。  
  
#### <a name="to-add-a-reference-to-the-visual-basic-library"></a>Visual Basic ライブラリへの参照を追加するには  
  
1. **ソリューション エクスプローラー**で、 **[参照設定]** ノードを右クリックし、 **[参照の追加]** をクリックします。  
  
2. **[参照設定]** ダイアログ ボックスが表示されたら、一覧を下にスクロールし、Microsoft.VisualBasic.dll を選択します。  
  
     プログラムの先頭の `using` セクションに次の行を追加することもできます。  
  
     [!code-csharp[csProgGuideNamespaces#18](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideNamespaces/CS/Namespaces3.cs#18)]  
  
## <a name="example"></a>例  
 次の例では、`MyServices` 名前空間に含まれているさまざまな静的メソッドを呼び出します。 このコードをコンパイルするには、Microsoft.VisualBasic.DLL への参照をプロジェクトに追加する必要があります。  
  
 [!code-csharp[csProgGuideNamespaces#19](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideNamespaces/CS/Namespaces3.cs#19)]  
  
 `MyServices` 名前空間のクラスの中には C# アプリケーションから呼び出すことができないクラスもあります。たとえば、<xref:Microsoft.VisualBasic.MyServices.FileSystemProxy> クラスは、C# と互換性がありません。 そのような場合は、同様に VisualBasic.dll に含まれている <xref:Microsoft.VisualBasic.FileIO.FileSystem> を構成する静的メソッドを代わりに使用できます。 このようなメソッドを使用してディレクトリを複製する方法を次に示します。  
  
 [!code-csharp[csProgGuideNamespaces#20](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideNamespaces/CS/Namespaces3.cs#20)]  
  
## <a name="see-also"></a>参照

- [C# プログラミングガイド](../index.md)
- [名前空間](./index.md)
- [名前空間の使用](./using-namespaces.md)
