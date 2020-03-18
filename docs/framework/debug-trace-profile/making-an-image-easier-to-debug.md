---
title: .NET でのデバッグが容易になるようにイメージを作成する
description: IDE スイッチとコマンドラインオプションを使用して簡単にデバッグできるように実行可能イメージを構成する方法について説明します。
ms.date: 08/30/2018
helpviewer_keywords:
- images [.NET Framework], debugging
- executable image for debugging
- debugging [.NET Framework], executable images for
ms.assetid: 7d90ea7a-150f-4f97-98a7-f9c26541b9a3
ms.openlocfilehash: 44d512a8ebec0e21e33f51c07428331e5e22b7bf
ms.sourcegitcommit: 9c54866bcbdc49dbb981dd55be9bbd0443837aa2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/14/2020
ms.locfileid: "77217342"
---
# <a name="making-an-image-easier-to-debug-in-net"></a>.NET でのデバッグが容易になるようにイメージを作成する

アンマネージ コードをコンパイルするときは、IDE スイッチまたはコマンド ライン オプションを使用して、デバッグ用の実行可能イメージを構成できます。 たとえば、Visual C++ で /**Zi** コマンド ライン オプションを使用すると、デバッグ シンボル ファイル (拡張子 .pdb) が生成されます。 同様に、/**Od** コマンド ライン オプションを使用すると、コンパイラは最適化処理を無効にします。 結果として得られるコードはより遅くなりますが、デバッグが簡単になります。

マネージ コードを .NET Framework をコンパイルするときに、Visual C、Visual Basic、および C# などのコンパイラは、Microsoft intermediate language (MSIL) にそのソース プログラムをコンパイルします。 MSIL は、実行直前に JIT コンパイルされ、ネイティブマシンコードに変換されます。 アンマネージ コードでは、IDE スイッチまたはコマンド ライン オプションを設定することにより、デバッグ用の実行可能イメージを構成できます。 また、ほぼ同じ方法で、デバッグ用に JIT コンパイルを構成することもできます。

このような目的で JIT を構成する場合、次の 2 つの要素が関係してきます。

- 追跡情報を生成するように JIT コンパイラに要求できます。 追跡情報を生成すると、デバッガーは、MSIL のチェインをマシン語コードの対応部分に対応させたり、ローカル変数と関数の引数が格納される位置を追跡したりできるようになります。 .NET Framework バージョン2.0 以降では、JIT コンパイラは常に追跡情報を生成するため、要求する必要はありません。

- JIT コンパイラに対して、結果として得られるマシンコードを最適化しないように要求できます。

通常、MSIL を生成するコンパイラは、指定した IDE スイッチまたはコマンドラインオプション (/**Od**など) に基づいて、これらの JIT コンパイラオプションを適切に設定します。

場合によっては、JIT コンパイラが生成するマシン語コードをデバッグしやすくするために、JIT コンパイラの動作の変更が必要となることがあります。 たとえば、製品版またはコントロールを最適化するために JIT 追跡情報の生成が必要となる場合などです。 このような場合は、初期化 (.ini) ファイルを使用します。

たとえば、デバッグするアセンブリが*myapp*という名前の場合、myapp という名前のテキストファイルを作成します。このフォルダーに*は、次*の3つの行が含ま*れてい*ます。

```ini
[.NET Framework Debugging Control]
GenerateTrackingInfo=1
AllowOptimize=0
```

各オプションの値として、0 または 1 を設定できます。記述しなかったオプションの値は、既定値の 0 に設定されます。 `GenerateTrackingInfo` を 1 に設定し、`AllowOptimize` を 0 に設定すると、デバッグが最も簡単になります。

.NET Framework バージョン2.0 以降では、`GenerateTrackingInfo`の値に関係なく、JIT コンパイラは常に追跡情報を生成します。ただし、`AllowOptimize` の値は依然として効果があります。 [Ngen.exe (Native Image Generator)](../tools/ngen-exe-native-image-generator.md) を使用して最適化を行わずにネイティブ イメージをプリコンパイルする場合は、`AllowOptimize=0` が記述された .ini ファイルが、Ngen.exe 実行時にターゲット フォルダー内に存在している必要があります。 最適化せずにアセンブリをプリコンパイルした場合は、ngen.exe を再実行してから、最適化されたコードをプリコンパイルする前に、Ngen.exe **/uninstall**オプションを使用してプリコンパイル済みコードを削除する必要があります。 .Ini ファイルがフォルダー内に存在しない場合、既定では、Ngen.exe はコードを最適化されたものとしてプリコンパイルします。

アセンブリの設定は、<xref:System.Diagnostics.DebuggableAttribute?displayProperty=nameWithType> によって制御されます。 **DebuggableAttribute**には、JIT コンパイラが追跡情報を最適化するか、生成するかを制御する2つのフィールドが含まれています。 .NET Framework バージョン2.0 以降では、JIT コンパイラは常に追跡情報を生成します。

リテールビルドの場合、コンパイラは**DebuggableAttribute**を設定しません。 既定では、JIT コンパイラによって最高のパフォーマンスが生成され、コンピューターコードのデバッグが最も困難になります。 JIT 追跡を有効にすると、パフォーマンスが多少低下します。最適化処理を無効にすると、パフォーマンスは大幅に低下します。

**DebuggableAttribute** は、アセンブリに含まれる個々のモジュールではなく、アセンブリ全体に適用されます。 そのため開発ツールでは、アセンブリが既に作成されている場合は、カスタム属性をアセンブリのメタデータ トークン、または **System.Runtime.CompilerServices.AssemblyAttributesGoHere** というクラスに追加できる必要があります。 次に、ALink ツールは、各モジュールからこれらの**DebuggableAttribute**属性を、その一部になるアセンブリに昇格させます。 競合がある場合、ALink 操作は失敗します。

> [!NOTE]
> .NET Framework Version 1.0 では、 **/clr** および **/Zi** コンパイラ オプションを指定すると、Microsoft Visual C++ コンパイラによって **DebuggableAttribute** が追加されます。 .NET Framework のバージョン1.1 では、 **DebuggableAttribute**をコードに手動で追加するか、 **/assemblydebug**リンカーオプションを使用する必要があります。

## <a name="see-also"></a>参照

- [デバッグ、トレース、およびプロファイリング](index.md)
- [JIT アタッチ デバッグの有効化](enabling-jit-attach-debugging.md)
- [プロファイルの有効化](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/s5ec0es1(v=vs.100))
