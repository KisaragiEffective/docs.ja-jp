---
title: カスタム ログ リスナーの作成
ms.date: 07/20/2015
helpviewer_keywords:
- custom log listeners
- My.Application.Log object, custom log listeners
ms.assetid: 0e019115-4b25-4820-afb1-af8c6e391698
ms.openlocfilehash: 7b611e93119dc66a9404cf271ea201676d7b5318
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "74353622"
---
# <a name="walkthrough-creating-custom-log-listeners-visual-basic"></a>チュートリアル: カスタム ログ リスナーの作成 (Visual Basic)

このチュートリアルでは、カスタム ログ リスナーを作成する方法と、`My.Application.Log` オブジェクトの出力を待機するように構成する方法について説明します。

## <a name="getting-started"></a>作業の開始

ログ リスナーは、<xref:System.Diagnostics.TraceListener> クラスから継承する必要があります。

#### <a name="to-create-the-listener"></a>リスナーを作成するには

- アプリケーションで、`SimpleListener` を継承する <xref:System.Diagnostics.TraceListener> という名前のクラスを作成します。

     [!code-vb[VbVbalrMyApplicationLog#16](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyApplicationLog/VB/Form1.vb#16)]

     <xref:System.Diagnostics.TraceListener.Write%2A> および <xref:System.Diagnostics.TraceListener.WriteLine%2A> メソッド (基底クラスに必須) は、`MsgBox` を呼び出して入力された値を表示します。

     <xref:System.Security.Permissions.HostProtectionAttribute> 属性は、<xref:System.Diagnostics.TraceListener.Write%2A> および <xref:System.Diagnostics.TraceListener.WriteLine%2A> メソッドに適用されます。これは、各メソッドの属性を基底クラスのメソッドに一致させるためです。 <xref:System.Security.Permissions.HostProtectionAttribute> 属性を使用すると、コードを実行するホストは、コードがホスト保護の同期を公開していることを確認できます。

    > [!NOTE]
    > <xref:System.Security.Permissions.HostProtectionAttribute> 属性は、共通言語ランタイムをホストし、ホスト保護を実装している SQL Server などのアンマネージ アプリケーションでのみ有効になります。

`My.Application.Log` でログ リスナーが使用されるようにするには、ログ リスナーを含むアセンブリに厳密な名前を付ける必要があります。

次の手順では、厳密な名前付きのログ リスナー アセンブリを作成するための簡単な手順を示します。 詳しくは、「[厳密な名前付きアセンブリの作成と使用](../../../../standard/assembly/create-use-strong-named.md)」をご覧ください。

#### <a name="to-strongly-name-the-log-listener-assembly"></a>ログ リスナー アセンブリに厳密な名前を付けるには

1. **ソリューション エクスプローラー**でプロジェクトを選択します。 **[プロジェクト]** メニューの **[プロパティ]** をクリックします。

2. **[署名]** タブをクリックします。

3. **[アセンブリの署名]** ボックスを選択します。

4. **[厳密な名前のキー ファイルを選択してください]\< ドロップダウン リストから** [**新規作成>]** を選択します。

     **[厳密な名前キーの作成]** ダイアログ ボックスが開きます。

5. **[キー ファイル名]** ボックスで、キー ファイルの名前を指定します。

6. **[パスワードの入力]** および **[パスワードの確認入力]** ボックスにパスワードを入力します。

7. **[OK]** をクリックします。

8. アプリケーションをリビルドします。

## <a name="adding-the-listener"></a>リスナーの追加

アセンブリに厳密な名前を付けたら、次はリスナーの厳密な名前を確認して、`My.Application.Log` でログ リスナーが使用されるようにする必要があります。

厳密な名前を持つ型の書式は次のとおりです。

\<型名>, \<アセンブリ名>, \<バージョン番号>, \<カルチャ>, \<厳密な名前>

#### <a name="to-determine-the-strong-name-of-the-listener"></a>リスナーの厳密な名前を確認するには

- 次のコードは、厳密に名前指定された `SimpleListener` の型名を確認する方法を示しています。

     [!code-vb[VbVbalrMyApplicationLog#17](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyApplicationLog/VB/Form1.vb#17)]

     型の厳密な名前は、プロジェクトによって変わります。

厳密な名前を使用すると、リスナーを `My.Application.Log` のログ リスナー コレクションに追加できます。

#### <a name="to-add-the-listener-to-myapplicationlog"></a>My.Application.Log にリスナーを追加するには

1. **ソリューション エクスプローラー** で app.config を右クリックし、 **[開く]** を選択します。

     または

     app.config ファイルがある場合は、次の操作を行います。

    1. **[プロジェクト]** メニューの **[新しい項目の追加]** をクリックします。

    2. **[新しい項目の追加]** ダイアログ ボックスで、 **[アプリケーション構成ファイル]** を選択します。

    3. **[追加]** をクリックします。

2. `<listeners>` セクション内にある、 `<source>` 属性が "DefaultSource" の `name` セクションで、 `<sources>` セクションを見つけます。 `<sources>` セクションは、最上位の `<system.diagnostics>` セクション内の `<configuration>` セクションにあります。

3. `<listeners>` セクションに次の要素を追加します。

    ```xml
    <add name="SimpleLog" />
    ```

4. 最上位の `<sharedListeners>` セクション内の `<system.diagnostics>` セクションで、 `<configuration>` セクションを見つけます。

5. その `<sharedListeners>` セクションに次の要素を追加します。

    ```xml
    <add name="SimpleLog" type="SimpleLogStrongName" />
    ```

     `SimpleLogStrongName` の値はリスナーの厳密な名前に置き換えてください。

## <a name="see-also"></a>参照

- <xref:Microsoft.VisualBasic.Logging.Log?displayProperty=nameWithType>
- [アプリケーション ログの使用](../../../../visual-basic/developing-apps/programming/log-info/working-with-application-logs.md)
- [方法 : 例外をログに記録する](../../../../visual-basic/developing-apps/programming/log-info/how-to-log-exceptions.md)
- [方法: ログ メッセージを書き込む](../../../../visual-basic/developing-apps/programming/log-info/how-to-write-log-messages.md)
- [チュートリアル : My.Application.Log による情報の書き込み先の変更](../../../../visual-basic/developing-apps/programming/log-info/walkthrough-changing-where-my-application-log-writes-information.md)
