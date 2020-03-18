---
title: '例: 動的プログラミングのトラブルシューティング'
ms.date: 03/30/2017
ms.assetid: 42ed860a-a022-4682-8b7f-7c9870784671
ms.openlocfilehash: ff179854066d024a89cb5a84a19d0b9bb054d6e5
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73128437"
---
# <a name="example-troubleshooting-dynamic-programming"></a>例: 動的プログラミングのトラブルシューティング
> [!NOTE]
> このトピックでは、プレリリース ソフトウェアである .NET Native Developer Preview について述べています。 プレビュー版は、[Microsoft Connect Web サイト](https://go.microsoft.com/fwlink/?LinkId=394611)からダウンロードできます (登録が必要です)。  
  
 .NET ネイティブツールチェーンを使用して開発されたアプリでのメタデータ参照の失敗によっては、例外が発生します。  予測できない方法でアプリに出現するものもあります。  次の例は、null オブジェクトの参照により生じるアクセス違反を示しています。  
  
```output
Access violation - code c0000005 (first chance)  
App!$3_App::Core::Util::NavigationArgs.Setup  
App!$3_App::Core::Util::NavigationArgs..ctor  
App!$0_App::Gibbon::Util::DesktopNavigationArgs..ctor  
App!$0_App::ViewModels::DesktopAppVM.NavigateToPage  
App!$3_App::Core::ViewModels::AppViewModel.NavigateToFirstPage  
App!$3_App::Core::ViewModels::AppViewModel::<HandleLaunch>d__a.MoveNext  
App!$43_System::Runtime::CompilerServices::AsyncMethodBuilderCore.CallMoveNext  
App!System::Action.InvokeClosedStaticThunk  
App!System::Action.Invoke  
App!$43_System::Threading::Tasks::AwaitTaskContinuation.InvokeAction  
App!$43_System::Threading::SendOrPostCallback.InvokeOpenStaticThunk  
[snip]  
```  
  
 「[Getting Started](getting-started-with-net-native.md)」(はじめに) の「メタデータの欠落を手動で解決する」セクションで説明されている 3 つの手順からなるアプローチを使用して、この例外をトラブルシューティングしてみましょう。  
  
## <a name="what-was-the-app-doing"></a>アプリが行っていた動作は何か  
 最初に注目するのは、スタックの最下位にある `async` キーワード メカニズムです。  スタックでは元の呼び出しのコンテキストが失われており、別のスレッドで `async` コードを実行しているため、アプリが `async` メソッドで実際に何を行っていたかを決定するのは困難です。 ただし、アプリが最初のページをロードしようとしていたことは推測できます。  `NavigationArgs.Setup` の実装で、次のコードによってアクセス違反が発生しました。  
  
`AppViewModel.Current.LayoutVM.PageMap`  
  
 この例では、`AppViewModel.Current` の `LayoutVM` プロパティは **null** でした。  メタデータの一部が欠落しているため、わずかな動作の違いが生じ、その結果プロパティはアプリで予期されているように設定されるのではなく、初期化されないままになります。  コードの `LayoutVM` が初期化される必要がある位置にブレークポイントを設定すると、状況の把握に役立ちます。  ただし、`LayoutVM` の型は `App.Core.ViewModels.Layout.LayoutApplicationVM` であることに注意してください。  この時点で rd.xml ファイル内に存在するメタデータ ディレクティブは、次のもののみです。  
  
```xml  
<Namespace Name="App.ViewModels" Browse="Required Public" Dynamic="Required Public" />  
```  
  
 失敗の原因として考えられるのは、`App.Core.ViewModels.Layout.LayoutApplicationVM` が別の名前空間にあるために、メタデータが欠落していることです。  
  
 この場合、`App.Core.ViewModels` のランタイム ディレクティブを追加すると、問題が解決します。 根本原因は、**null** を返した <xref:System.Type.GetType%28System.String%29?displayProperty=nameWithType> メソッドへの API 呼び出しで、アプリではクラッシュが発生するまでエラーを出さずにこの問題を無視していました。  
  
 動的プログラミングでは、.NET ネイティブでリフレクション Api を使用する場合は、エラー発生時に例外をスローする <xref:System.Type.GetType%2A?displayProperty=nameWithType> オーバーロードを使用することをお勧めします。  
  
## <a name="is-this-an-isolated-case"></a>特殊なケースかどうか  
 `App.Core.ViewModels` を使用する際に、その他の問題が発生することもあります。  メタデータの欠落例外すべてを特定して修正する意味があるかどうか、または大きい型のクラスにディレクティブを追加して時間を節約するかを決定する必要があります。  この場合は、出力バイナリのサイズ増大が問題になるのでなければ、`dynamic` の `App.Core.ViewModels` メタデータを追加するのが最適な方法でしょう。  
  
## <a name="could-the-code-be-rewritten"></a>コードを書き換えることができるか  
 アプリで `typeof(LayoutApplicationVM)` ではなく `Type.GetType("LayoutApplicationVM")` を使用している場合、ツール チェーンに `browse` メタデータが保存されている可能性があります。  ただし、それでも `invoke` メタデータは作成されておらず、そのため型をインスタンス化するときに [MissingMetadataException](missingmetadataexception-class-net-native.md) 例外が発生する可能性があります。 この例外を回避するには、名前空間または `dynamic` ポリシーを指定する型のランタイム ディレクティブを追加する必要があります。 ランタイム ディレクティブの詳細については、「[ランタイム ディレクティブ (rd.xml) 構成ファイル リファレンス](runtime-directives-rd-xml-configuration-file-reference.md)」を参照してください。  
  
## <a name="see-also"></a>関連項目

- [はじめに](getting-started-with-net-native.md)
- [例: データ バインディング時の例外の処理](example-handling-exceptions-when-binding-data.md)
