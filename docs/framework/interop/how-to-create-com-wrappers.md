---
title: '方法: COM ラッパーを作成する'
ms.date: 03/30/2017
helpviewer_keywords:
- COM,wrappers creating
- COM,wrappers Visual Studio
ms.assetid: bdf89bea-1623-45ee-a57b-cf7c90395efa
ms.openlocfilehash: 623df8aa86d25d9a57d3039bee01b0ee39d402a8
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73123939"
---
# <a name="how-to-create-com-wrappers"></a>方法: COM ラッパーを作成する

Visual Studio 2005 の機能または .NET Framework のツールである Tlbimp.exe と Regasm.exe を使用して、COM (コンポーネント オブジェクト モデル) ラッパーを作成することができます。 どちらの方法でも以下の 2 種類の COM ラッパーが作成されます。

- タイプ ライブラリからの[ランタイム呼び出し可能ラッパー](../../standard/native-interop/runtime-callable-wrapper.md)。マネージド コードで COM オブジェクトを実行します。

- 必要なレジストリ設定を含む [COM 呼び出し可能ラッパー](../../standard/native-interop/com-callable-wrapper.md)。ネイティブ アプリケーションでマネージド オブジェクトを実行します。

Visual Studio 2005 では、プロジェクトに参照として COM ラッパーを追加できます。

## <a name="wrap-com-objects-in-a-managed-application"></a>マネージド アプリケーションに COM オブジェクトをラップする

### <a name="to-create-a-runtime-callable-wrapper-using-visual-studio"></a>Visual Studio を使用してランタイム呼び出し可能ラッパーを作成するには

1. マネージド アプリケーションのプロジェクトを開きます。

2. **[プロジェクト]** メニューの **[すべてのファイルを表示]** をクリックします。

3. **[プロジェクト]** メニューの **[参照の追加]** をクリックします。

4. [参照の追加] ダイアログ ボックスで、 **[COM]** タブをクリックし、使用するコンポーネントを選択して **[OK]** をクリックします。

     **ソリューション エクスプローラー**で、COM コンポーネントがプロジェクトの [参照設定] フォルダーに追加されていることを確認します。

これで、COM オブジェクトにアクセスするためのコードを作成できます。 まず、Visual Basic の場合は `Imports` ステートメント、C# の場合は `Using` ステートメントなど、オブジェクトの宣言から始めます。

> [!NOTE]
> Microsoft Office コンポーネントをプログラミングする場合は、最初に Microsoft ダウンロード センターから [Microsoft Office プライマリ相互運用機能アセンブリ](https://go.microsoft.com/fwlink/?LinkId=50479) (PIA) をインストールします。 手順 4 で、必要な Office 製品で使用可能な最新バージョンのオブジェクト ライブラリ (**Microsoft Word 11.0 Object Library** など) を選択します。  
  
### <a name="to-create-a-runtime-callable-wrapper-using-net-framework-tools"></a>.NET Framework ツールを使用してランタイム呼び出し可能ラッパーを作成するには  
  
- [Tlbimp.exe (タイプ ライブラリ インポーター)](../tools/tlbimp-exe-type-library-importer.md) ツールを実行します。  
  
 このツールは、元のタイプ ライブラリで定義された型のランタイム メタデータを含むアセンブリを作成します。  
  
## <a name="wrap-managed-objects-in-a-native-application"></a>ネイティブ アプリケーションにマネージド オブジェクトをラップする  
  
### <a name="to-create-a-com-callable-wrapper-using-visual-studio"></a>Visual Studio を使用して COM 呼び出し可能ラッパーを作成するには  
  
1. ネイティブ コードで実行するマネージド クラス用のクラス ライブラリ プロジェクトを作成します。 このクラスには、パラメーターなしのコンストラクターが含まれている必要があります。  
  
     AssemblyInfo ファイルで、アセンブリの 4 つの部分で構成される完全なバージョン番号があることを確認します。 この番号は、Windows レジストリでバージョンを管理するために必要となります。 バージョン番号の詳細については、「[アセンブリのバージョン管理](../../standard/assembly/versioning.md)」を参照してください。  
  
2. **[プロジェクト]** メニューの **[プロパティ]** をクリックします。  
  
3. **[コンパイル]** タブをクリックします。  
  
4. **[COM の相互運用機能に登録]** チェック ボックスをオンにします。  
  
 プロジェクトをビルドすると、アセンブリが COM 相互運用機能に対して自動的に登録されます。 Visual Studio 2005 でネイティブ アプリケーションをビルドする場合は、 **[プロジェクト]** メニューの **[参照の追加]** をクリックすることで、アセンブリを使用できます。  
  
### <a name="to-create-a-com-callable-wrapper-using-net-framework-tools"></a>.NET Framework ツールを使用して COM 呼び出し可能ラッパーを作成するには  
  
[Regasm.exe (アセンブリ登録ツール)](../tools/regasm-exe-assembly-registration-tool.md) を実行します。  
  
このツールはアセンブリ メタデータを読み取り、必要なエントリをレジストリに追加します。 その結果、COM クライアントで .NET Framework クラスを透過的に作成できるようになります。 アセンブリは、ネイティブ COM クラスの場合と同じように使用することができます。  
  
任意のディレクトリにあるアセンブリで Regasm.exe を実行してから、[Gacutil.exe (グローバル アセンブリ キャッシュ ツール)](../tools/gacutil-exe-gac-tool.md) を実行して、そのアセンブリをグローバル アセンブリ キャッシュに移動することができます。 アセンブリを移動しても場所のレジストリ エントリが無効になることはありません。これは、アセンブリが他の場所で見つからない場合に常にグローバル アセンブリ キャッシュが調べられるからです。  
  
## <a name="see-also"></a>関連項目

- [ランタイム呼び出し可能ラッパー](../../standard/native-interop/runtime-callable-wrapper.md)
- [COM 呼び出し可能ラッパー](../../standard/native-interop/com-callable-wrapper.md)
