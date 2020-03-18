---
title: .NET Framework ツール
ms.date: 03/30/2017
helpviewer_keywords:
- command line, .NET Framework tools
- .NET Framework, tools
- tools [.NET Framework]
- running .NET Framework tools
ms.assetid: a2ca532d-91f7-426a-9303-417c2ee1247c
ms.openlocfilehash: 4503e2cff18f4aa901d20c76cfe4076a7fed3600
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "75715775"
---
# <a name="net-framework-tools"></a>.NET Framework ツール

.NET Framework ツールを使用すると、.NET Framework に対応したアプリケーションやコンポーネントを簡単に作成、配置、および管理できます。

ここで説明する .NET Framework ツールの大半は、Visual Studio のインストール時に自動的にインストールされます。 Visual Studio は、[Visual Studio のダウンロード](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) ページからダウンロードできます。

アセンブリ キャッシュ ビューアー (*Shfusion.dll*) を除き、これらのツールはすべてコマンド ラインから実行できます。 エクスプローラーから *Shfusion.dll* にアクセスする必要があります。
  
コマンド ライン ツールの最適な実行方法は、Visual Studio 用開発者コマンド プロンプトを使用することです。 これらのユーティリティを使用すると、インストール フォルダーに移動することなくツールを簡単に実行できます。 詳細については、「[コマンド プロンプト](developer-command-prompt-for-vs.md)」を参照してください。

> [!NOTE]
> ツールの中には、32 ビット コンピューターまたは 64 ビット コンピューターに固有のものもあります。 該当するコンピューターに適切なバージョンのツールを実行します。

## <a name="in-this-section"></a>このセクションの内容

- [Al.exe (アセンブリ リンカー)](al-exe-assembly-linker.md)  
モジュール ファイルまたはリソース ファイルから、アセンブリ マニフェストを含むファイルを生成します。

- [Aximp.exe (Windows フォーム ActiveX コントロール インポーター)](aximp-exe-windows-forms-activex-control-importer.md)  
ActiveX コントロール用の COM タイプ ライブラリに属する型定義を Windows フォーム コントロールに変換します。

- [Caspol.exe (コード アクセス セキュリティ ポリシー ツール)](caspol-exe-code-access-security-policy-tool.md)  
コンピューター ポリシー レベル、ユーザー ポリシー レベル、およびエンタープライズ ポリシー レベルのセキュリティ ポリシーを表示および構成できます。 .NET Framework 4 以降では、[\<legacyCasPolicy> 要素](../configure-apps/file-schema/runtime/netfx40-legacysecuritypolicy-element.md)が `true` に設定されていない限り、このツールがコード アクセス セキュリティ (CAS) ポリシーに影響を与えることはありません。 詳細については、「[セキュリティの変更点](../security/security-changes.md)」を参照してください。

- [Cert2spc.exe (ソフトウェア発行元証明書テスト ツール)](cert2spc-exe-software-publisher-certificate-test-tool.md)  
1 つ以上の X.509 証明書からソフトウェア発行元証明書 (SPC: Software Publisher's Certificate) を作成します。 このツールはテスト専用です。

- [Certmgr.exe (証明書マネージャー ツール)](certmgr-exe-certificate-manager-tool.md)  
証明書、証明書信頼リスト (CTL: Certificate Trust Lists)、および証明書失効リスト (CRL: Certificate Revocation Lists) を管理します。

- [Clrver.exe (CLR バージョン ツール)](clrver-exe-clr-version-tool.md)  
コンピューターにインストールされている共通言語ランタイム (CLR) のすべてのバージョンを報告します。

- [Visual Studio 用開発者コマンド プロンプト](developer-command-prompt-for-vs.md)  
.NET Framework ツールをより簡単に使用できるようになります。 それは、特定の環境変数を自動的に設定するコマンド プロンプトです。

- [CorFlags.exe (CorFlags 変換ツール)](corflags-exe-corflags-conversion-tool.md)  
移植可能な実行可能 (PE) イメージのヘッダー内の CorFlags セクションを構成します。

- [Fuslogvw.exe (アセンブリ バインディング ログ ビューアー)](fuslogvw-exe-assembly-binding-log-viewer.md)  
アセンブリ バインドに関する情報を表示します。これは、.NET Framework が実行時にアセンブリを見つけられない原因を診断する場合に役立ちます。

- [Gacutil.exe (グローバル アセンブリ キャッシュ ツール)](gacutil-exe-gac-tool.md)  
グローバル アセンブリ キャッシュおよびダウンロード キャッシュの内容の表示と操作を行います。

- [Ilasm.exe (IL アセンブラー)](ilasm-exe-il-assembler.md)  
中間言語 (IL) から移植可能な実行可能 (PE) ファイルを生成します。 生成された実行可能ファイルを実行すると、IL が期待どおりに動作するかどうかを確認できます。

- [Ildasm.exe (IL 逆アセンブラー)](ildasm-exe-il-disassembler.md)  
中間言語 (IL) コードを含む移植可能な実行可能 (PE) ファイルを受け取り、IL アセンブラー (Ilasm.exe) への入力として使用できるテキスト ファイルを作成します。

- [Installutil.exe (インストーラー ツール)](installutil-exe-installer-tool.md)  
指定したアセンブリのインストーラー コンポーネントを実行することによって、サーバー リソースのインストールとアンインストールを実行できます (<xref:System.Configuration.Install> 名前空間のクラスと連携して動作します)。

- [Lc.exe (ライセンス コンパイラ)](lc-exe-license-compiler.md)  
ライセンス情報を含むテキスト ファイルを読み込んで、 *.licenses* ファイルを生成します。これは、共通言語ランタイムの実行可能ファイルにリソースとして埋め込むことができます。

- [Mage.exe (マニフェストの生成および編集ツール)](mage-exe-manifest-generation-and-editing-tool.md)  
アプリケーション マニフェストと配置マニフェストの作成、編集、および署名を行います。 *Mage.exe* はコマンド ライン ツールであるため、バッチ スクリプトから実行したり、ASP.NET アプリケーションなどの他の Windows ベースのアプリケーションから実行したりできます。

- [MageUI.exe (マニフェスト生成および編集ツールのグラフィカル クライアント)](mageui-exe-manifest-generation-and-editing-tool-graphical-client.md)  
コマンド ライン ツールの Mage.exe と同じ機能をサポートしますが、Windows ベースのユーザー インターフェイス (UI) を使用します。 コマンド ライン ツールの *Mage.exe* と同じ機能がサポートされますが、Windows ベースのユーザー インターフェイス (UI) が使用されます。

- [MDbg.exe (.NET Framework コマンド ライン デバッガー)](mdbg-exe.md)  
.NET Framework 共通言語ランタイムを対象としたプログラムに含まれるバグの発見と修正について、ツールの販売元とアプリケーション開発者を支援するツールです。 このツールは、ランタイムのデバッグ API を使用してデバッグ サービスを提供します。

- [Mgmtclassgen.exe (厳密型クラス ジェネレーター)](mgmtclassgen-exe.md)  
指定した WMI (Windows Management Instrumentation) クラスに対して、事前バインディングされたマネージド クラスを生成できます。

- [Mpgo.exe (マネージド プロファイル ガイド付き最適化ツール)](mpgo-exe-managed-profile-guided-optimization-tool.md)  
一般的なエンド ユーザーのシナリオを使用してネイティブ イメージのアセンブリを調整できるようにします。 Mpgo.exe により、アプリケーション開発者によって選択されたトレーニング シナリオを使用したネイティブ イメージ アプリケーション アセンブリ (.NET Framework アセンブリではない) のプロファイル データの生成と消費が可能になります。

- [Ngen.exe (ネイティブ イメージ ジェネレーター)](ngen-exe-native-image-generator.md)  
ネイティブ イメージ (コンパイルされたプロセッサ固有のマシン語コードを格納しているファイル) を使用することで、マネージド アプリケーションのパフォーマンスを改善します。 ランタイムは、Just-In-Time (JIT) コンパイラを使用してオリジナルのアセンブリをコンパイルする代わりに、キャッシュにあるネイティブ イメージを使用できます。

- [Peverify.exe (PEVerify ツール)](peverify-exe-peverify-tool.md)  
Microsoft Intermediate Language (MSIL) コードと関連メタデータがタイプ セーフ要件を満たしているかどうかを検証する場合に役立ちます。

- [Regasm.exe (アセンブリ登録ツール)](regasm-exe-assembly-registration-tool.md)  
アセンブリ内のメタデータを読み取り、必要なエントリをレジストリに追加します。 これにより、COM クライアントが .NET Framework クラスとして表示されるようになります。

- [Regsvcs.exe (.NET サービス インストール ツール)](regsvcs-exe-net-services-installation-tool.md)  
アセンブリを読み込んで登録し、タイプ ライブラリを生成して指定された COM+ Version 1.0 アプリケーションにインストールし、プログラムによってクラスに追加されたサービスを構成します。

- [Resgen.exe (リソース ファイル ジェネレーター)](resgen-exe-resource-file-generator.md)  
テキスト ファイル ( *.txt* または *.restext*) と XML ベースのリソース形式ファイル ( *.resx*) を、共通言語ランタイムのバイナリ ファイル ( *.resources*) に変換します。これは、ランタイム バイナリ実行可能ファイルに埋め込んだり、サテライト アセンブリにコンパイルしたりできます。

- [SecAnnotate.exe (.NET Security Annotator ツール)](secannotate-exe-net-security-annotator-tool.md)  
アセンブリの `SecurityCritical` 部分と `SecuritySafeCritical` 部分を識別します。

- [SignTool.exe (署名ツール)](signtool-exe.md)  
ファイルにデジタル署名を添付し、ファイルの署名を検証し、ファイルにタイム スタンプを付けます。

- [Sn.exe (厳密名ツール)](sn-exe-strong-name-tool.md)  
厳密な名前を持つアセンブリを作成するときに役立ちます。 このツールには、キーの管理、署名の生成、署名の検査に関する各オプションが用意されています。

- [SOS.dll (SOS デバッガー拡張)](sos-dll-sos-debugging-extension.md)  
内部の共通言語ランタイム環境に関する情報を提供して、WinDbg.exe デバッガーおよび Visual Studio におけるマネージド プログラムのデバッグを支援します。

- [SqlMetal.exe (コード生成ツール)](sqlmetal-exe-code-generation-tool.md)  
.NET Framework の LINQ to SQL コンポーネント用のコードとマッピングを生成します。

- [Storeadm.exe (分離ストレージ ツール)](storeadm-exe-isolated-storage-tool.md)  
分離ストレージを管理します。ユーザーのストアの一覧表示や削除を行うためのオプションが用意されています。

- [Tlbexp.exe (タイプ ライブラリ エクスポーター)](tlbexp-exe-type-library-exporter.md)  
共通言語ランタイム アセンブリで定義されている型を記述するタイプ ライブラリを生成します。

- [Tlbimp.exe (タイプ ライブラリ インポーター)](tlbimp-exe-type-library-importer.md)  
COM タイプ ライブラリ内の型定義を、共通言語ランタイム アセンブリで等価な定義に変換します。

- [Winmdexp.exe (Windows ランタイム メタデータのエクスポート ツール)](winmdexp-exe-windows-runtime-metadata-export-tool.md)  
*.winmdobj* ファイルとしてコンパイルされた .NET Framework アセンブリを Windows ランタイム コンポーネントにエクスポートします。このコンポーネントは、Windows ランタイム メタデータと実装に関する情報の両方を含む *.winmd* ファイルとしてパッケージされます。

- [Winres.exe (Windows フォーム リソース エディター)](winres-exe-windows-forms-resource-editor.md)  
Windows フォームで使用されるユーザー インターフェイス (UI) リソース ( *.resx* ファイルまたは *.resources* ファイル) をローカライズするのに役立ちます。 文字列を翻訳した後、ローカライズされた文字列に合わせて、コントロールのサイズを変更したり、コントロールを移動したり、非表示にしたりできます。

## <a name="related-sections"></a>関連項目

- [WPF ツール](https://docs.microsoft.com/previous-versions/ms742404(v=vs.110))  
isXPS 適合性ツール (isXPS.exe) およびパフォーマンス プロファイリング ツールなどのツールを含みます。

- [Windows Communication Foundation ツール](../wcf/tools.md)  
WCF (Windows Communication Foundation) アプリケーションの作成、配置、および管理を効率化するツールを含みます。
