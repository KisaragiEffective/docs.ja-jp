---
title: 例外の作成とスロー - C# プログラミング ガイド
ms.date: 07/20/2015
helpviewer_keywords:
- catching exceptions [C#]
- throwing exceptions [C#]
- exceptions [C#], creating
- exceptions [C#], throwing
ms.assetid: 6bbba495-a115-4c6d-90cc-1f4d7b5f39e2
ms.openlocfilehash: f775a0917560a219f24329adcb1542f605d47dc2
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75712300"
---
# <a name="creating-and-throwing-exceptions-c-programming-guide"></a>例外の作成とスロー (C# プログラミング ガイド)
例外は、プログラムの実行中にエラーが発生したことを示すために使われます。 エラーを説明する例外オブジェクトが作成された後、*throw* キーワードで "[スロー](../../language-reference/keywords/throw.md)" されます。 そのとき、ランタイムは最も互換性のある例外ハンドラーを検索します。  
  
 プログラマは、以下の条件が 1 つでも該当するときは、例外をスローする必要があります。  
  
- メソッドは、定義されている機能を完了できません。  
  
     たとえば、メソッドへのパラメーターに無効な値が設定されている場合などです。  
  
     [!code-csharp[csProgGuideExceptions#12](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideExceptions/CS/Exceptions.cs#12)]  
  
- オブジェクトの状態に基づくと、オブジェクトに対して行われた呼び出しが不適切です。  
  
     たとえば、読み取り専用ファイルに書き込もうとしたような場合です。 オブジェクトの状態により操作が許可されない場合は、<xref:System.InvalidOperationException> のインスタンスまたはこのクラスの派生に基づくオブジェクトをスローします。 <xref:System.InvalidOperationException> オブジェクトをスローするメソッドの例を次に示します。  
  
     [!code-csharp[csProgGuideExceptions#13](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideExceptions/CS/Exceptions.cs#13)]  
  
- メソッドへの引数が原因で例外が発生しました。  
  
     この場合は、元の例外をキャッチして、<xref:System.ArgumentException> のインスタンスを作成する必要があります。 元の例外は、<xref:System.ArgumentException> パラメーターとして <xref:System.Exception.InnerException%2A> のコンストラクターに渡す必要があります。  
  
     [!code-csharp[csProgGuideExceptions#14](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideExceptions/CS/Exceptions.cs#14)]  
  
 例外には、<xref:System.Exception.StackTrace%2A> というプロパティが含まれています。 この文字列には、現在の呼び出し履歴でのメソッドの名前と、各メソッドの例外がスローされたファイル名と行番号が含まれます。 スタック トレースを開始するポイントから例外をスローする必要があるため、共通言語ランタイム (CLR) によって <xref:System.Exception.StackTrace%2A> ステートメントのポイントから `throw` オブジェクトが自動的に作成されます。  
  
 すべての例外には、<xref:System.Exception.Message%2A> というプロパティが含まれています。 例外の原因を説明するには、この文字列を設定する必要があります。 機密性の高い情報はメッセージ テキストに入れないようにする必要があることに注意してください。 <xref:System.Exception.Message%2A> には、<xref:System.ArgumentException> に加え、例外がスローされる原因となる引数の名前に設定される <xref:System.ArgumentException.ParamName%2A> というプロパティが含まれています。 プロパティ セッターの場合、<xref:System.ArgumentException.ParamName%2A> は `value` に設定する必要があります。  
  
 パブリックのプロテクト メンバーは、意図された機能を完了できない場合は常に例外をスローする必要があります。 スローされる例外クラスは、エラー状態に適合する使用可能な例外の中で最も具体的なものである必要があります。 これらの例外はクラスの機能の一部として文書化する必要があり、派生クラスまたは元のクラスの更新では、旧バージョンとの互換性のために同じ動作を維持する必要があります。  
  
## <a name="things-to-avoid-when-throwing-exceptions"></a>例外をスローするときに避ける必要があること  
 次の一覧は、例外をスローするときに避ける必要があることです。  
  
- 通常の実行の一部として、例外を使ってプログラムのフローを変更しないでください。 例外は、エラー状態の報告と処理のためだけに使う必要があります。  
  
- スローする代わりに、戻り値またはパラメーターとして例外を返さないでください。  
  
- 自作のソース コードからは、意図的に <xref:System.Exception?displayProperty=nameWithType>、<xref:System.SystemException?displayProperty=nameWithType>、<xref:System.NullReferenceException?displayProperty=nameWithType>、または <xref:System.IndexOutOfRangeException?displayProperty=nameWithType> をスローしないでください。  
  
- デバッグ モードではスローでき、リリース モードではスローできない例外は、作成しないでください。 開発フェーズ中に実行時エラーを識別するには、代わりにデバッグ アサートを使ってください。  
  
## <a name="defining-exception-classes"></a>例外クラスの定義  
 プログラムでは、<xref:System> 名前空間で事前定義された例外クラスをスローするか (上記の場合を除きます)、<xref:System.Exception> から派生することで独自の例外クラスを作成することができます。 派生クラスでは、少なくとも 4 つのコンストラクターを必ず定義します。パラメータ―なしのコンストラクター、メッセージ プロパティを設定するコンストラクター、<xref:System.Exception.Message%2A> プロパティと <xref:System.Exception.InnerException%2A> プロパティの両方を設定するコンストラクター、 そして 4 番目は例外のシリアル化に使われるコンストラクターです。 新しい例外クラスは、シリアル化可能にする必要があります。 次に例を示します。  
  
 [!code-csharp[csProgGuideExceptions#15](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideExceptions/CS/Exceptions.cs#15)]  
  
 例外クラスへの新しいプロパティの追加は、プロパティによって提供されるデータが例外の解決に役立つ場合にのみ行う必要があります。 派生例外クラスに新しいプロパティを追加する場合は、`ToString()` をオーバーライドして追加情報を返す必要があります。  
  
## <a name="c-language-specification"></a>C# 言語仕様  

詳細については、「[C# 言語仕様](~/_csharplang/spec/exceptions.md)」の[例外](~/_csharplang/spec/statements.md#the-throw-statement)と [throw ステートメント](/dotnet/csharp/language-reference/language-specification/introduction)に関するセクションを参照してください。 言語仕様は、C# の構文と使用法に関する信頼性のある情報源です。
  
## <a name="see-also"></a>参照

- [C# プログラミングガイド](../index.md)
- [例外と例外処理](./index.md)
- [例外階層](../../../standard/exceptions/index.md)
- [例外処理](./exception-handling.md)
