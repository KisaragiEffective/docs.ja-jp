---
title: プラットフォーム呼び出し (P/Invoke)
description: .NET で P/Invoke を介してネイティブ関数を呼び出す方法について説明します。
ms.date: 01/18/2019
ms.openlocfilehash: fa8b43edfba50fbc620f257c4e7caf1673f83235
ms.sourcegitcommit: 5f236cd78cf09593c8945a7d753e0850e96a0b80
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2020
ms.locfileid: "75706306"
---
# <a name="platform-invoke-pinvoke"></a>プラットフォーム呼び出し (P/Invoke)

P/Invoke は、アンマネージド ライブラリ内の構造体、コールバック、および関数をマネージド コードからアクセスできるようにするテクノロジです。 P/Invoke API のほとんどは、`System` と `System.Runtime.InteropServices` の 2 つの名前空間に含まれます。 これら 2 つの名前空間を使用すると、ネイティブ コンポーネントと通信する方法を記述するツールを利用できます。

最も一般的な例から始めましょう。これはマネージド コードでアンマネージド 関数を呼び出します。 コマンドライン アプリケーションからメッセージ ボックスを表示してみましょう。

[!code-csharp[MessageBox](~/samples/snippets/standard/interop/pinvoke/messagebox.cs)]

上の例は単純ですが、マネージド コードからアンマネージド 関数を呼び出すために必要なことを示しています。 この例の手順を説明します。

- 1 行目は、必要なすべての項目を保持する名前空間 `System.Runtime.InteropServices` のステートメントの使用を示しています。
- 7 行目で `DllImport` 属性を導入しています。 この属性は、ランタイムにアンマネージ DLL を読み込むように伝えるため、きわめて重要です。 渡された文字列は、ターゲット関数が入っている DLL です。 さらに、文字列のマーシャリングに使用する[文字セット](./charset.md)を指定します。 最後に、この関数が [SetLastError](/windows/desktop/api/errhandlingapi/nf-errhandlingapi-setlasterror) を呼び出し、ランタイムでそのエラー コードをキャプチャしてユーザーが <xref:System.Runtime.InteropServices.Marshal.GetLastWin32Error?displayProperty=nameWithType> を介して取得できるように指定します。
- 8 行目は、P/Invoke 作業の最も重要な箇所です。 ここでは、アンマネージドと**正確に同じシグネチャ**を持つマネージド メソッドを定義しています。 宣言には新しいキーワード `extern` があることがわかります。これはランタイムに、これが外部メソッドであり、それを呼び出したときに、ランタイムが `DllImport` 属性に指定された DLL からそれを見つける必要があることを伝えます。

例の残りの部分は、その他のマネージド メソッドと同じように、メソッドを呼び出しているだけです。

サンプルは、macOS の場合も似ています。 macOS ではダイナミック ライブラリの名前付けのスキームが異なるため、`DllImport` 属性内のライブラリの名前を変更する必要があります。 次のサンプルでは、`getpid(2)` 関数を使用して、アプリケーションのプロセス ID を取得し、それをコンソールに出力しています。

[!code-csharp[getpid macOS](~/samples/snippets/standard/interop/pinvoke/getpid-macos.cs)]

これは Linux でも同様です。 `getpid(2)` は標準的な [POSIX](https://en.wikipedia.org/wiki/POSIX) システム コールであるため、関数名が同じです。

[!code-csharp[getpid Linux](~/samples/snippets/standard/interop/pinvoke/getpid-linux.cs)]

## <a name="invoking-managed-code-from-unmanaged-code"></a>アンマネージド コードからのマネージド コードの呼び出し

ランタイムは通信が双方向にフローする通信を許可しているため、関数ポインターを使用して、ネイティブ関数からマネージド コードをコール バックすることができます。 マネージド コードで、関数ポインターに最も近いものが、**デリゲート**であるため、これをネイティブ コードからマネージド コードへのコールバックを許可するために使います。

この機能を使用する方法は、前述のマネージドからネイティブへのプロセスに似ています。 指定されたコールバックに対し、ユーザーがシグネチャと一致するデリゲートを定義し、それを外部メソッドに渡します。 ランタイムは、他のすべてのことを処理します。

[!code-csharp[EnumWindows](~/samples/snippets/standard/interop/pinvoke/enumwindows.cs)]

例の手順に従う前に、使用する必要があるアンマネージ関数のシグネチャについて見直しておくのはよいことです。 すべてのウィンドウを列挙するために呼び出す関数は、次のシグネチャを持ちます。`BOOL EnumWindows (WNDENUMPROC lpEnumFunc, LPARAM lParam);`

最初のパラメーターでは、コールバックです。 上記のコールバックのシグネチャは次のとおりです。`BOOL CALLBACK EnumWindowsProc (HWND hwnd, LPARAM lParam);`

次の例を確認してみましょう。

- 例の 9 行目では、アンマネージ コードからのコールバックのシグネチャと一致するデリゲートを定義しています。 マネージド コードで `IntPtr` を使用して、LPARAM および HWND 型を表す方法に注意してください。
- 13 行目と 14 行目では、user32.dll ライブラリから `EnumWindows` 関数を導入しています。
- 17 - 20 行目は、デリゲートを実装しています。 この簡単な例では、ハンドルだけコンソールに出力します。
- 最後に、24 行目で、外部メソッドを呼び出し、デリゲートを渡しています。

Linux と macOS の例を、以下に示します。 それらの場合、`libc` C ライブラリに見つかる `ftw` 関数を使用します。 この関数は、ディレクトリ階層をスキャンするために使用し、そのパラメーターの 1 つとして、関数へのポインターを受け取ります。 上記のメソッドのシグネチャは次のとおりです。`int (*fn) (const char *fpath, const struct stat *sb, int typeflag)`

[!code-csharp[ftw Linux](~/samples/snippets/standard/interop/pinvoke/ftw-linux.cs)]

macOS の例では、同じ関数を使用していますが、唯一の違いは `DllImport` 属性への引数です。macOS は `libc` を別の場所で保持しているためです。

[!code-csharp[ftw macOS](~/samples/snippets/standard/interop/pinvoke/ftw-macos.cs)]

前述の例のどちらも、パラメーターに依存し、どちらの場合もパラメーターは、マネージド型として指定されています。 ランタイムは、"正しいこと" を実行し、これらを他方の側で同等のものに処理します。 型をネイティブ コードにマーシャリングする方法については、「[Type marshalling (型のマーシャリング)](type-marshaling.md)」を参照してください。

## <a name="more-resources"></a>その他のリソース

- [PInvoke.net wiki](https://www.pinvoke.net/) は、一般的な Windows API とそれらを呼び出す方法に関する情報を記載した優れた Wiki です。
- [C++/CLI での P/Invoke](/cpp/dotnet/native-and-dotnet-interoperability)
- [P/invoke に関する Mono のドキュメント](https://www.mono-project.com/docs/advanced/pinvoke/)
