---
title: インデクサーの使用 - C# プログラミング ガイド
ms.date: 10/03/2018
helpviewer_keywords:
- indexers [C#], about indexers
ms.assetid: df70e1a2-3ce3-4aba-ad80-4b2f3538699f
ms.openlocfilehash: 17162a0dc959a85c03a5cb5757e2b91fe10b0ab3
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "77628164"
---
# <a name="using-indexers-c-programming-guide"></a>インデクサーの使用 (C# プログラミング ガイド)

インデクサーは構文を簡略化します。これを使用すると、[クラス](../../language-reference/keywords/class.md)、[構造体](../../language-reference/builtin-types/struct.md)、または[インターフェイス](../../language-reference/keywords/interface.md)を作成でき、クライアント アプリケーションは配列と同じようにアクセスできます。 インデクサーは、内部コレクションまたは配列をカプセル化することが主な目的である型で最も多く実装されます。 たとえば、24 時間のうちの異なる 10 回の時刻で記録した温度を華氏で表す `TempRecord` クラスがあるとします。 このクラスには、温度値を格納する `temps` 型の配列 `float[]` が含まれています。 このクラスにインデクサーを実装することで、クライアントは、`TempRecord` ではなく `float temp = tr[4]` として `float temp = tr.temps[4]` インスタンスの温度にアクセスできます。 インデクサーはクライアント アプリケーションの構文を簡略化するだけでなく、クラスとその目的を、他の開発者たちにとってわかりやすい、より直感的なものにします。  
  
クラスまたは構造体でインデクサーを宣言するには、次の例のように [this](../../language-reference/keywords/this.md) キーワードを使用します。

```csharp
public int this[int index]    // Indexer declaration  
{  
    // get and set accessors  
}  
```

## <a name="remarks"></a>解説

インデクサーの型とそのパラメーターの型は、少なくとも、インデクサー自体と同程度にアクセス可能である必要があります。 アクセシビリティ レベルの詳細については、「[アクセス修飾子 (C# リファレンス)](../../language-reference/keywords/access-modifiers.md)」を参照してください。  
  
 インターフェイスでインデクサーを使用する方法の詳細については、「[インターフェイスのインデクサー (C# プログラミング ガイド)](./indexers-in-interfaces.md)」を参照してください。  
  
 インデクサーのシグネチャは、その仮パラメーターの数と型で構成されます。 これには、インデクサーの型や仮パラメーターの名前は含まれません。 同じクラス内に複数のインデクサーを宣言する場合は、異なるシグネチャが必要です。  
  
 インデクサーの値は変数として分類されないため、インデクサーの値を [ref](../../language-reference/keywords/ref.md) や [out](../../language-reference/keywords/out-parameter-modifier.md) パラメーターとして渡すことはできません。  
  
 他の言語が使用できる名前をインデクサーに指定するには、次の例のように <xref:System.Runtime.CompilerServices.IndexerNameAttribute?displayProperty=nameWithType> を使用します。  

```csharp
[System.Runtime.CompilerServices.IndexerName("TheItem")]  
public int this[int index]   // Indexer declaration  
{
    // get and set accessors  
}  
```

このインデクサーの名前は `TheItem` になります。 name 属性を指定しないと、`Item` が既定の名前になります。  
  
## <a name="example-1"></a>例 1  
  
次の例は、プライベートな配列フィールド `temps` とインデクサーの宣言方法を示しています。 インデクサーを使用すれば、インスタンス `tempRecord[i]` に直接アクセスできます。 インデクサーを使用しない場合は、配列を [public](../../language-reference/keywords/public.md) メンバーとして宣言し、そのメンバー `tempRecord.temps[i]` に直接アクセスします。  
  
 `Console.Write` ステートメントなどでインデクサーのアクセスが評価されると、[get](../../language-reference/keywords/get.md) アクセサーが呼び出されることに注意してください。 したがって、`get` アクセサーが存在しない場合は、コンパイル時エラーが発生します。  
  
 [!code-csharp[csProgGuideIndexers#1](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideIndexers/CS/Indexers.cs#1)]  
  
## <a name="indexing-using-other-values"></a>他の値を使用したインデックス作成

C# では、インデクサー パラメーター型は整数に制限されません。 たとえば、文字列をインデクサーで使用すると有効な場合があります。 このようなインデクサーは、コレクション内の文字列を検索し、適切な値を返すことによって実装される場合があります。 アクセサーはオーバーロードできるため、文字列と整数のバージョンは共存できます。  
  
## <a name="example-2"></a>例 2  
  
次の例では、曜日を格納するクラスを宣言しています。 `get` アクセサーは、曜日を示す文字列を受け取り、対応する整数を返します。 たとえば、"Sunday" は 0、"Monday" は 1 などと値を返します。  
  
 [!code-csharp[csProgGuideIndexers#2](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideIndexers/CS/Indexers.cs#2)]  
  
## <a name="robust-programming"></a>信頼性の高いプログラミング

 インデクサーのセキュリティと信頼性を改善するには、主に次の 2 つの方法があります。  
  
- クライアント コードが無効なインデックス値を渡しても、それを処理できるように必ずエラー処理戦略を組み込んでください。 このトピックの最初の例の TempRecord クラスには Length プロパティが用意されており、入力がインデクサーに渡される前にクライアント コードで検証できるようになっています。 インデクサー自体にエラー処理コードを配置することもできます。 インデクサーのアクセサー内部でスローされる例外はすべて、ユーザーのために文書化してください。  
  
- [get](../../language-reference/keywords/get.md) および [set](../../language-reference/keywords/set.md) アクセサーのアクセシビリティを設定し、適切な制限を指定します。 これは、`set` アクセサーの場合、特に重要です。 詳細については、「[アクセサーのアクセシビリティの制限](../classes-and-structs/restricting-accessor-accessibility.md)」を参照してください。  
  
## <a name="see-also"></a>参照

- [C# プログラミングガイド](../index.md)
- [インデクサー](./index.md)
- [Properties](../classes-and-structs/properties.md)
