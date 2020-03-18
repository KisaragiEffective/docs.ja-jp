---
title: '方法 : トレースとデバッグを指定して条件付きコンパイルを実行する'
ms.date: 03/30/2017
helpviewer_keywords:
- trace compiler options
- trace statements
- compiling source code, trace statements
- tracing [.NET Framework], enabling or disabling
- tracing [.NET Framework], compiling conditionally
- TRACE directive
- conditional compilation, tracing code
ms.assetid: 56d051c3-012c-42c1-9a58-7270edc624aa
ms.openlocfilehash: 2c3ec54535319f4c7507563a5976038ca40d20aa
ms.sourcegitcommit: 9c54866bcbdc49dbb981dd55be9bbd0443837aa2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/14/2020
ms.locfileid: "77217450"
---
# <a name="how-to-compile-conditionally-with-trace-and-debug"></a>方法 : トレースとデバッグを指定して条件付きコンパイルを実行する
開発時にアプリケーションをデバッグするときは、トレース出力とデバッグ出力の両方が Visual Studio の [出力] ウィンドウに表示されます。 ただし、配置されるアプリケーションにトレース機能を組み込むには、**TRACE** コンパイラ ディレクティブを有効にして、インストルメント化されたアプリケーションをコンパイルする必要があります。 これにより、コンパイルされたアプリケーションのリリース バージョンに、トレース コードが組み込まれます。 **TRACE** ディレクティブを有効にしないと、コンパイル時にすべてのトレース コードが無視され、配置する実行可能コードに含まれなくなります。  
  
 トレース用のメソッドとデバッグ用のメソッドにはどちらも、関連付けられた条件属性があります。 たとえば、トレースの条件属性が **true** の場合は、すべてのトレース ステートメントがアセンブリ (コンパイル済みの .exe ファイルや .dll ファイル) 内に組み込まれます。また、**Trace** 条件属性が **false** の場合、トレース ステートメントは組み込まれません。  
  
 ビルドでは、**Trace** 条件属性または **Debug** 条件属性をオンにすることも、両方をオンにすることも、あるいは両方をオフにすることもできます。 したがって、**Debug** のみをオン、**Trace** のみをオン、両方ともオン、両方ともオフという、4 種類のビルド方法が存在します。 実際の配置用のリリース ビルドでは両方ともオフにする場合もありますが、大半のデバッグ ビルドでは両方ともオンにします。  
  
 アプリケーションのコンパイラ設定は、次に示すいくつかの方法で指定できます。  
  
- プロパティ ページ  
  
- コマンド ライン  
  
- **#CONST** (Visual Basic の場合) および **#define** (C# の場合)  
  
### <a name="to-change-compile-settings-from-the-property-pages-dialog-box"></a>プロパティ ページのダイアログ ボックスでコンパイル設定を変更するには  
  
1. **ソリューション エクスプローラー**で、プロジェクト ノードを右クリックします。  
  
2. ショートカット メニューの **[プロパティ]** を選択します。  
  
    - Visual Basic では、プロパティ ページの左ペインで **[コンパイル]** タブをクリックし、 **[詳細コンパイル オプション]** ボタンをクリックして **[コンパイラの詳細設定]** ダイアログ ボックスを表示します。 有効にするコンパイラ設定のチェック ボックスをオンにします。 無効にする設定のチェック ボックスをオフにします。  
  
    - C# では、プロパティ ページの左ペインで **[ビルド]** タブをクリックし、有効にするコンパイラ設定のチェック ボックスをオンにします。 無効にする設定のチェック ボックスをオフにします。  
  
### <a name="to-compile-instrumented-code-using-the-command-line"></a>インストルメント化されたコードをコマンド ラインを使用してコンパイルするには  
  
1. コマンド ラインで、条件付きコンパイラ スイッチを設定します。 コンパイラにより、トレース コードまたはデバッグ コードが実行可能ファイルに組み込まれます。  
  
     たとえば、コマンド ラインで次のコンパイラ命令を入力すると、コンパイルされた実行可能ファイルにトレース コードが組み込まれます。  
  
     Visual Basic の場合: **vbc.exe-r:System.dll-MyApplication トレース = TRUE-d: DEBUG = FALSE**  
  
     C#: **Csc-r:System.dll-d-d-d DEBUG = FALSE MyApplication.cs**  
  
    > [!TIP]
    > 複数のアプリケーション ファイルをコンパイルするには、各ファイル名の間にスペースを 1 つ挿入します。たとえば、「**MyApplication1.vb MyApplication2.vb MyApplication3.vb**」または「**MyApplication1.cs MyApplication2.cs MyApplication3.cs**」とします。  
  
     上記の例で使用した条件付きコンパイルのディレクティブの意味は次のとおりです。  
  
    |Directive|意味|  
    |---------------|-------------|  
    |`vbc`|Visual Basic コンパイラ|  
    |`csc`|C# コンパイラ|  
    |`-r:`|外部アセンブリ (EXE または DLL) を参照します。|  
    |`-d:`|条件付きコンパイル シンボルを定義します。|  
  
    > [!NOTE]
    > TRACE または DEBUG は大文字で入力する必要があります。 条件付きコンパイル コマンドの詳細情報を参照するには、コマンド プロンプトに `vbc /?` (Visual Basic の場合) または `csc /?` (c# の場合) と入力します。 詳細については、「[コマンド ラインからのビルド](../../csharp/language-reference/compiler-options/how-to-set-environment-variables-for-the-visual-studio-command-line.md)」(C# の場合) または「[コマンド ライン コンパイラの起動](../../visual-basic/reference/command-line-compiler/how-to-invoke-the-command-line-compiler.md)」(Visual Basic の場合) を参照してください。  
  
### <a name="to-perform-conditional-compilation-using-const-or-define"></a>#CONST または #define を使用して条件付きコンパイルを実行するには  
  
1. ソース コード ファイルの先頭に、使用するプログラミング言語に該当するステートメントを入力します。  
  
    |言語|明細書|結果|  
    |--------------|---------------|------------|  
    |**Visual Basic**|**#CONST TRACE = true**|トレースを有効にします。|  
    ||**#CONST TRACE = false**|トレースを無効にします。|  
    ||**#CONST DEBUG = true**|デバッグを有効にします。|  
    ||**#CONST DEBUG = false**|デバッグを無効にします。|  
    |**C#**|**#define TRACE**|トレースを有効にします。|  
    ||**#undef TRACE**|トレースを無効にします。|  
    ||**#define DEBUG**|デバッグを有効にします。|  
    ||**#undef DEBUG**|デバッグを無効にします。|  
  
### <a name="to-disable-tracing-or-debugging"></a>トレースまたはデバッグを無効にするには  
  
ソース コードからコンパイラ ディレクティブを削除します。  
  
\- - または -  
  
コンパイラ ディレクティブをコメント アウトします。  
  
> [!NOTE]
> コンパイルの準備ができたら、 **[ビルド]** メニューの **[ビルド]** を選択できます。または、条件付きコンパイル シンボルを定義するための「**d:** 」を入力せずにコマンド ライン メソッドを使用することもできます。  
  
## <a name="see-also"></a>参照

- [アプリケーションのトレースとインストルメント](tracing-and-instrumenting-applications.md)
- [方法 : トレース スイッチを作成、初期化、および構成する](how-to-create-initialize-and-configure-trace-switches.md)
- [トレース スイッチ](trace-switches.md)
- [トレース リスナー](trace-listeners.md)
- [方法 : アプリケーション コードにトレース ステートメントを追加する](how-to-add-trace-statements-to-application-code.md)
- [Visual Studio のコマンドラインのための環境変数を設定する方法](../../csharp/language-reference/compiler-options/how-to-set-environment-variables-for-the-visual-studio-command-line.md)
- [方法 : コマンド ライン コンパイラを起動する](../../visual-basic/reference/command-line-compiler/how-to-invoke-the-command-line-compiler.md)
