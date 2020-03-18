---
title: '方法: プライマリ相互運用機能アセンブリを登録する'
ms.date: 03/30/2017
helpviewer_keywords:
- registering primary interop assemblies
- primary interop assemblies, registering
ms.assetid: 4b2fcf8a-429d-43ce-8334-e026040be8bb
ms.openlocfilehash: c799e4ead2932f1c376a57488df30390ad48b90f
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73107680"
---
# <a name="how-to-register-primary-interop-assemblies"></a>方法: プライマリ相互運用機能アセンブリを登録する

クラスは、COM 相互運用でのみマーシャリングすることができ、常にインターフェイスとしてマーシャリングされます。 クラスをマーシャリングするために使用されるインターフェイスが、クラス インターフェイスと呼ばれる場合があります。 クラス インターフェイスを任意のインターフェイスでオーバーライドする方法の詳細については、「[COM 呼び出し可能ラッパー](../../standard/native-interop/com-callable-wrapper.md)」を参照してください。

 .NET Framework アプリケーションから COM の型を使用するすべての開発者は相互運用機能アセンブリを生成できますが、そのようにすると問題が発生します。 開発者が COM タイプ ライブラリをインポートして署名するたびに、その開発者は、別の開発者によってインポートされて署名されたものとは互換性のない一意の型のセットを作成することになります。 この型の非互換性の問題を解決する方法は、各開発者が、ベンダーによって提供されて署名されたプライマリ相互運用機能アセンブリを取得することです。

 サードパーティの COM 型を他のアプリケーションに公開する予定の場合は、それが定義するタイプ ライブラリと同じ発行元によって提供されるプライマリ相互運用機能アセンブリを常に使用してください。 保証された型の互換性を提供することに加えて、多くの場合、プライマリ相互運用機能アセンブリは相互運用性を強化するためにベンダーによってカスタマイズされます。

 サードパーティの COM 型を公開する予定がない場合でも、プライマリ相互運用機能アセンブリを使用することで、COM コンポーネントとの相互運用のタスクを容易に実行できます。 ただし、この方法では、プライマリ相互運用機能アセンブリで定義された型に対してベンダーが加える可能性のある変更から隔離されることはありません。 使用するアプリケーションでそのような隔離が必要な場合は、プライマリ相互運用機能アセンブリを使用する代わりに独自の相互運用アセンブリを生成してください。

 取得したすべてのプライマリ相互運用機能アセンブリを開発コンピューターに登録してからでなければ、Visual Studio でそれらを参照できません。 Visual Studio は、初めて COM タイプ ライブラリから型を参照するときに、プライマリ相互運用機能アセンブリを検索して使用します。 Visual Studio は、タイプ ライブラリに関連付けられているプライマリ相互運用機能アセンブリが見つからない場合、それを取得するように求めるプロンプトを出すか、または相互運用機能アセンブリを代わりに作成することを申し出ます。 同じように、[タイプ ライブラリ インポーター (Tlbimp.exe)](../tools/tlbimp-exe-type-library-importer.md) も、レジストリを使用してプライマリ相互運用機能アセンブリを見つけます。

 Visual Studio を使用する予定がない限り、プライマリ相互運用機能アセンブリを登録する必要はありませんが、登録には次の 2 つの利点があります。

- 登録されているプライマリ相互運用機能アセンブリは、元のタイプ ライブラリのレジストリ キーの下で明白にマークされます。 登録は、コンピューター上のプライマリ相互運用機能アセンブリを検索するための最善の方法です。

- 将来のいつか、Visual Studio を使用して登録されていないプライマリ相互運用機能アセンブリのある型を参照する場合に、間違えて新しい相互運用機能アセンブリを生成して使用してしまうことを回避できます。

[アセンブリ登録ツール (Regasm.exe)](../tools/regasm-exe-assembly-registration-tool.md) を使用して、プライマリ相互運用機能アセンブリを登録します。

## <a name="to-register-a-primary-interop-assembly"></a>プライマリ相互運用機能アセンブリを登録する方法

1. コマンド プロンプトで、次のコマンドを入力します。

     **regasm** *assemblyname*

     このコマンドで、*assemblyname* は登録されているアセンブリのファイル名です。 Regasm.exe は、元のタイプ ライブラリと同じレジストリ キーの下に、プライマリ相互運用機能アセンブリの項目を追加します。

## <a name="example"></a>例
 次の例は、`CompanyA.UtilLib.dll` プライマリ相互運用機能アセンブリを登録します。

```console
regasm CompanyA.UtilLib.dll
```

## <a name="see-also"></a>関連項目

- [プライマリ相互運用機能アセンブリを使ったプログラミング](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/baxfadst(v=vs.100))
- [プライマリ相互運用機能アセンブリの検索](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/y06sxw56(v=vs.100))
- [プライマリ相互運用機能アセンブリの再配布](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/w0dt2w20(v=vs.100))
