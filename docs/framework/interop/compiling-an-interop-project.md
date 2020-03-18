---
title: 相互運用プロジェクトのコンパイル
ms.date: 03/30/2017
helpviewer_keywords:
- interoperation with unmanaged code, compiling
- COM interop, compiling
- exposing COM components to .NET Framework
- compiling interop projects
- interoperation with unmanaged code, exposing COM components
- COM interop, exposing COM components
ms.assetid: 6fcf6588-5e25-41af-b4ae-780974f2c3df
ms.openlocfilehash: 32102910ae674a97e941e1346a1898585f503527
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73123684"
---
# <a name="compiling-an-interop-project"></a>相互運用プロジェクトのコンパイル

インポートされた COM 型を含む 1 つ以上のアセンブリを参照する COM 相互運用プロジェクトは、他のマネージド プロジェクトと同じようにコンパイルされます。 相互運用機能アセンブリは、Visual Studio などの開発環境で参照できます。コマンド ライン コンパイラを使用して参照することもできます。 どちらの場合でも、正常にコンパイルするには、相互運用機能アセンブリを、その他のプロジェクト ファイルと同じディレクトリに配置する必要があります。

 相互運用機能アセンブリを参照する方法には、次の 2 つがあります。

- 埋め込まれた相互運用機能型: .NET Framework 4 と Visual Studio 2010 以降では、相互運用機能アセンブリから実行可能ファイルに型情報を埋め込むようにコンパイラに指示できます。 この手法を使用することをお勧めします。

- 相互運用機能アセンブリの配置: 相互運用機能アセンブリへの標準の参照を作成できます。 この場合、アプリケーションで相互運用機能アセンブリを配置する必要があります。

 この 2 つの手法の違いの詳細については、「[Using COM Types in Managed Code](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/3y76b69k(v=vs.100))」(マネージド コードでの COM 型の使用) を参照してください。

 Visual Studio での相互運用機能型の埋め込みについては、 [「チュートリアル: Visual studio におけるマネージアセンブリからの型の埋め込み](../../standard/assembly/embed-types-visual-studio.md)」で説明されています。

 コマンドラインコンパイラを使用して相互運用機能アセンブリを参照し、実行可能ファイルに型情報を埋め込むには、 [-link (C#コンパイラオプション)](../../csharp/language-reference/compiler-options/link-compiler-option.md)または[-link (Visual Basic)](../../visual-basic/reference/command-line-compiler/link.md)コンパイラスイッチを使用して、相互運用機能アセンブリの名前を指定します。

> [!NOTE]
> Visual C++ アプリケーションは型情報を埋め込むことはできませんが、型情報を埋め込むことができるアプリケーションまたはアドインと相互運用できます。

 配置時にプライマリ相互運用機能アセンブリを含むアプリケーションをコンパイルするには、 **/reference** コンパイラ スイッチを使用して、相互運用機能アセンブリの名前を指定します。

## <a name="see-also"></a>関連項目

- [.NET Framework への COM コンポーネントの公開](exposing-com-components.md)
- [言語への非依存性、および言語非依存コンポーネント](../../standard/language-independence-and-language-independent-components.md)
- [マネージド コードでの COM 型の使用](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/3y76b69k(v=vs.100))
- [チュートリアル: マネージド アセンブリからの型の埋め込み (Visual Studio)](../../standard/assembly/embed-types-visual-studio.md)
- [タイプ ライブラリのアセンブリとしてのインポート](importing-a-type-library-as-an-assembly.md)
