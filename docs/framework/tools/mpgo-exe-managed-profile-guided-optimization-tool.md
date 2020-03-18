---
title: Mpgo.exe (マネージド プロファイル ガイド付き最適化ツール)
ms.date: 03/30/2017
helpviewer_keywords:
- Mpgo.exe
- training scenarios, generating profiles with
- Managed Profile Guided Optimization Tool
- Ngen.exe
- Ngen.exe, profilers and native images
ms.assetid: f6976502-a000-4fbe-aaf5-a7aab9ce4ec2
ms.openlocfilehash: 0052475697dae2c3ad891db18d300b5ec08a7e62
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "79180349"
---
# <a name="mpgoexe-managed-profile-guided-optimization-tool"></a>Mpgo.exe (マネージド プロファイル ガイド付き最適化ツール)

マネージド プロファイル ガイド付き最適化ツール (Mpgo.exe) は、[ネイティブ イメージ ジェネレーター (Ngen.exe)](ngen-exe-native-image-generator.md) によって作成されたネイティブ イメージ アセンブリを最適化するために一般的なエンド ユーザー シナリオを使用するコマンド ライン ツールです。 このツールによって、プロファイル データを生成するトレーニング シナリオを実行できます。 [ネイティブ イメージ ジェネレーター (Ngen.exe)](ngen-exe-native-image-generator.md) は、このデータを使用して、生成されたネイティブ イメージ アプリケーション アセンブリを最適化します。 トレーニングのシナリオでは、アプリケーションで予期される使用について試行します。 Mpgo.exe は、Visual Studio Ultimate 2012 以降のバージョンで使用できます。 Visual Studio 2013 以降では、Windows 8.x Store アプリを最適化するために Mpgo.exe も使用できます。  
  
ガイド付き最適化のプロファイルは、トレーニング シナリオからデータを収集し、ネイティブ イメージのレイアウトを最適化するためにそのデータを使用することによって、アプリケーションの起動時間、メモリ使用率 (ワーキング セット サイズ)、およびスループットを向上させます。  
  
中間言語 (IL) アセンブリの起動時間および作業セット サイズでパフォーマンスの問題が見つかった場合、Just-In-Time (JIT) コンパイルのコストを回避し、コード共有を容易にするために、最初に Ngen.exe を使用することをお勧めします。 さらなる向上が必要な場合は、アプリケーションを最適化するために Mpgo.exe を使用できます。 パフォーマンスの向上を評価するためのベースラインとして、最適化されていないネイティブ イメージ アセンブリからのパフォーマンス データを使用できます。 Mpgo.exe を使用することにより、コールド スタートの時間を短縮し、作業セットのサイズを縮小できます。 Mpgo.exe は、最適化されたネイティブ イメージ アセンブリを作成するために Ngen.exe で使用される IL アセンブリに情報を追加します。 詳しくは、.NET ブログ エントリの「[Improving Launch Performance for your Desktop Applications](https://devblogs.microsoft.com/dotnet/improving-launch-performance-for-your-desktop-applications/)」 (デスクトップ アプリケーションの起動時のパフォーマンスの向上) をご覧ください。  
  
このツールは、Visual Studio と共に自動的にインストールされます。 このツールを実行するには、管理者の資格情報で Visual Studio 用開発者コマンド プロンプト (または、Windows 7 の Visual Studio コマンド プロンプト) を使用し、コマンド プロンプトで次のように入力します。 詳細については、「[Visual Studio 用開発者コマンド プロンプト](developer-command-prompt-for-vs.md)」を参照してください。  
  
デスクトップ アプリの場合:  
  
```console  
mpgo –Scenario <command> [-Import <directory>] –AssemblyList <assembly1>  <assembly2> ... -OutDir <directory> [options]  
```  
  
Windows 8.x Store アプリの場合:  
  
## <a name="syntax"></a>構文  
  
```console  
mpgo –Scenario <packageName> -AppID <appId> -Timeout <seconds>  
```  
  
## <a name="parameters"></a>パラメーター  
 Mpgo.exe では、どの引数も大文字と小文字が区別されません。 コマンドには、ダッシュがプレフィックスとして付けられます。  
  
> [!NOTE]
> 必要なコマンドとして `–Scenario` または `–Import` のいずれかを使用できますが、両方は使用できません。 `–Reset` オプションを指定する場合、必須パラメーターのいずれも使用されません。

|必須パラメーター|説明|
|------------------------|-----------------|
|`-Scenario` \<*command*><br /><br /> または<br /><br /> `-Scenario` \<*packageName*><br /><br /> \- または -<br /><br /> `-Import` \<*directory*>|デスクトップ アプリでは、コマンド ライン引数も含めて、最適化するアプリケーションを実行するためのコマンド指定のために `–Scenario` を使用します。 スペースを含むパスを指定する場合は、*command* を 3 組の二重引用符で囲みます (例: `mpgo.exe -scenario """C:\My App\myapp.exe""" -assemblylist """C:\My App\myapp.exe""" -outdir "C:\optimized files"`)。 二重引用符は使用しません。*command* にスペースが含まれる場合に、正しく機能しません。<br /><br /> \- または -<br /><br /> Windows 8.x Store アプリでは、プロファイル情報を生成するパッケージを指定するために `–Scenario` を使用します。 完全なパッケージ名ではなく、パッケージの表示名またはファミリ名を指定した場合、Mpgo.exe では単一の一致のみが存在するときに指定の名前に一致するパッケージが選択されます。 指定の名前に複数のパッケージが一致する場合、Mpgo.exe ではパッケージを選択するように求めるプロンプトが表示されます。<br /><br /> または<br /><br /> `-Import` でアセンブリを最適化するために、以前に最適化されたアセンブリからの最適化データを使用する旨を指定するには `-AssemblyList` を使用します。 *directory* では、以前に最適化されたファイルを含めるディレクトリを指定します。 `–AssemblyList` または `–AssemblyListFile` で指定するアセンブリは、インポートされたファイルのデータを使用して最適化されるアセンブリの新しいバージョンです。 古いバージョンのアセンブリからの最適化データを使用することによって、シナリオを再実行しなくてもアセンブリの新しいバージョンを最適化できます。  ただし、インポートしたターゲット アセンブリのコードが大きく異なる場合は、最適化データは非効率的になります。 `–AssemblyList` または `–AssemblyListFile` で指定するアセンブリ名は、`–Import`*directory* によって指定するディレクトリに存在する必要があります。 スペースを含むパスを指定する場合は、*directory* を 3 組の二重引用符で囲みます。<br /><br /> `–Scenario` または `–Import` のいずれかを指定する必要がありますが、両方のパラメーターを指定する必要はありません。|
|`-OutDir` \<*directory*>|最適化されたアセンブリを配置するディレクトリ。 出力ディレクトリ フォルダーにアセンブリが既に存在する場合、新しいコピーが作成され、その名前にインデックス番号が付加されます (例: *assemblyname*-1.exe)。 スペースを含むパスを指定する場合は、*directory* を二重引用符で囲みます。|
|`-AssemblyList` \<*assembly1 assembly2 ...* ><br /><br /> または<br /><br /> `-AssemblyListFile` \<*file*>|プロファイル情報を収集するスペースで区切った (.exe ファイルおよび .dll ファイルを含む) アセンブリのリスト。 `C:\Dir\*.dll` または `*.dll` を指定して、指定した作業ディレクトリまたは現在の作業ディレクトリのアセンブリをすべて選択できます。 詳細については、次の「解説」を参照してください。<br /><br /> または<br /><br /> 行ごとに 1 つのアセンブリを記述した、プロファイル情報を収集するアセンブリのリストを含むテキスト ファイル。 アセンブリ名の先頭がハイフン (-) である場合は、アセンブリ ファイル リストを使用するか、またはアセンブリ名を変更します。|
|`-AppID` \<*appId*>|指定したパッケージのアプリケーション ID。 ワイルドカード (\*) を使用した場合、Mpgo.exe はパッケージ内の AppID の列挙を試み、失敗した場合は \<*package_family_name*>!App にフォール バックします。 感嘆符 (!) のプレフィックスを付けた文字列を指定した場合、Mpgo.exe ではパッケージ ファミリ名と指定した引数が連結されます。|
|`-Timeout` \<*seconds*>|Windows 8.x Store アプリが終了するまでに実行を許可される時間。|

|省略可能なパラメーター|説明|
|------------------------|-----------------|
|`-64bit`|64 ビット システムのアセンブリをインストルメント化します。  アセンブリ自体が 64 ビットとして宣言していても、このパラメーターを 64 ビット アセンブリ用に指定する必要があります。|
|`-ExeConfig` \<*filename*>|シナリオがバージョンおよびローダーの情報を指定するために使用する構成ファイルを指定します。|
|`-f`|バイナリ アセンブリには、署名済みであってもプロファイル データを強制的に含めます。  アセンブリが署名されている場合、再署名が必要です。再署名しない場合は、アセンブリの読み込みおよび実行が失敗します。|
|`-Reset`|中止されたプロファイル セッションがアセンブリに影響しないことを確認するために環境をリセットして、終了します。 環境は、プロファイル セッションの前後に既定でリセットされます。|
|`-Timeout` \<*time in seconds*>|プロファイルの継続期間を秒単位で指定します。 計測した GUI アプリケーションの起動時間よりも少し大きい値を使用します。 タイムアウト期間の終了時に、アプリケーションは実行を続行しますが、プロファイル データが記録されます。 このオプションを設定しない場合、プロファイリングはアプリケーションのシャットダウンまで続行され、その時点までデータが記録されます。|
|`-LeaveNativeImages`|インストルメント化されたネイティブ イメージはシナリオの実行後に削除されないことを指定します。 このオプションは、主に、実行中のシナリオ用に指定したアプリケーションを取得するときに使用されます。 これにより、Mpgo.exe のその後の実行に対してネイティブ イメージを再生成できなくなります。 このオプションを指定した場合、アプリケーションの実行の完了後に、キャッシュ内に孤立したネイティブ イメージが存在する可能性があります。 この場合、同じシナリオおよびアセンブリ リストと共に Mpgo.exe を実行し、`–RemoveNativeImages` パラメーターを使用することによって、こうしたネイティブ イメージを削除します。|
|`-RemoveNativeImages`|`–LeaveNativeImages` が指定された実行からクリーンアップします。 `-RemoveNativeImages` を指定すると、Mpgo.exe は `-64bit` と `–AssemblyList` を除く引数を無視して、すべてのインストルメントされたネイティブ イメージを削除すると終了します。|

## <a name="remarks"></a>Remarks
 `–AssemblyList` と `- AssemblyListFile` の両方をコマンド行で複数回、使用できます。

 アセンブリを指定する際に完全パス名を指定しないと、Mpgo.exe は現在のディレクトリ内を調べます。 正しくないパスを指定した場合、Mpgo.exe がエラー メッセージを表示しますが、他のアセンブリに対するデータの生成は続行されます。 トレーニング シナリオの実行中に読み込まれていないアセンブリを指定すると、トレーニングのデータがそのアセンブリに対して生成されません。

 リストのアセンブリがグローバル アセンブリ キャッシュ内にある場合、そのアセンブリはプロファイル情報を含めるように更新されません。  プロファイル情報を収集するには、グローバル アセンブリ キャッシュから削除します。

 プリコンパイルされたネイティブ イメージの利点は一般的に、実行時の JIT コンパイルが大幅に除外される場合に限り得られるので、大規模なマネージド アプリケーションでのみ、Ngen.exe および Mpgo.exe を使用することをお勧めします。 作業セットを集中的に使用しない "Hello World" スタイルのアプリケーションに対して Mpgo.exe を実行すると、利点は得られず、Mpgo.exe でプロファイル データの収集に失敗することすらあります。

> [!NOTE]
> Ngen.exe および Mpgo.exe は、ASP.NET アプリケーションおよび Windows Communication Foundation (WCF) サービスでは推奨されません。  
  
## <a name="to-use-mpgoexe"></a>Mpgo.exe を使用するには  
  
1. Visual Studio Ultimate 2012 およびアプリケーションがインストールされたコンピューターを使用します。  
  
2. 管理者として必要なパラメーターと共に Mpgo.exe を実行します。  サンプル コマンドについては、次のセクションを参照してください。  
  
     最適化された中間言語 (IL) アセンブリが `–OutDir` パラメーターによって指定したフォルダー内に作成されます (この例では、`C:\Optimized` フォルダー)。  
  
3. `–OutDir` によって指定したディレクトリからのプロファイル情報を含む新しい IL アセンブリにより、Ngen.exe で使用する IL アセンブリを置き換えます。  
  
4. (Mpgo.exe によって提供されるイメージを使用した) アプリケーション セットアップでは、最適化されたネイティブなイメージがインストールされます。  
  
## <a name="suggested-workflow"></a>推奨されるワークフロー  
  
1. `–Scenario` パラメーターと共に Mpgo.exe を使用することにより、一連の最適化された IL アセンブリを作成します。  
  
2. ソース管理に最適化された IL アセンブリをチェックインします。  
  
3. ビルド プロセスで、Ngen.exe に渡す最適化された IL イメージを生成するために、ビルド後のステップとして `–Import` パラメーターと共に Mpgo.exe を呼び出します。  
  
 このプロセスでは、すべてのアセンブリが最適化データを持つことが保証されます。 更新および最適化されたアセンブリのチェックインをより頻繁に行う場合 (ステップ 1 および 2)、製品開発全体でパフォーマンス値はより一貫したものになります。  
  
## <a name="using-mpgoexe-from-visual-studio"></a>Visual Studio の Mpgo.exe の使用  
 Visual Studio から次の制限で Mpgo.exe を実行できます ([方法: ビルド イベントを指定する (C#)](/visualstudio/ide/how-to-specify-build-events-csharp) という記事を参照してください)。  
  
- 既定では Visual Studio マクロも末尾でスラッシュを使用するので、末尾にスラッシュを含む引用符付きのパスを使用することはできません。 (たとえば、`–OutDir "C:\Output Folder\"` は無効です。)この制限を回避するために、末尾のスラッシュをエスケープできます。 (たとえば、代わりに `-OutDir "$(OutDir)\"` を使用します。)  
  
- 既定では、Mpgo.exe は Visual Studio のビルド パス上にありません。 Visual Studio にこのパスを追加するか、Mpgo のコマンド ラインで完全パスを指定する必要があります。 Visual Studio でビルド後イベントに `–Scenario` または `–Import` パラメーターのいずれかを使用できます。 ただし、通常のプロセスでは、Visual Studio 用開発者コマンド プロンプトから `–Scenario` を一度使用し、その後 `–Import` を使用し、各ビルド後に最適化されたアセンブリを更新します (たとえば、`"C:\Program Files\Microsoft Visual Studio 11.0\Team Tools\Performance Tools\mpgo.exe" -import "$(OutDir)tmp" -assemblylist "$(TargetPath)" -outdir "$(OutDir)\"`)。  
  
<a name="samples"></a>
## <a name="examples"></a>使用例  
 Visual Studio 用開発者コマンド プロンプトから次の Mpgo.exe コマンドにより税処理アプリケーションを最適化します。  
  
```console  
mpgo –scenario "C:\MyApp\MyTax.exe /params par" –AssemblyList Mytax.dll MyTaxUtil2011.dll –OutDir C:\Optimized –TimeOut 15  
```  
  
 次の Mpgo.exe コマンドはサウンド アプリケーションを最適化します。  
  
```console  
mpgo –scenario "C:\MyApp\wav2wma.exe –input song1.wav –output song1.wma" –AssemblyList transcode.dll –OutDir C:\Optimized –TimeOut 15  
```  
  
 次の Mpgo.exe コマンドは、新しいバージョンのアセンブリを最適化するために、以前に最適化されたアセンブリからのデータを使用します。  
  
```console  
mpgo.exe -import "C:\Optimized" -assemblylist "C:\MyApp\MyTax.dll" "C:\MyApp\MyTaxUtil2011.dll" -outdir C:\ReOptimized  
```  
  
## <a name="see-also"></a>関連項目

- [Ngen.exe (ネイティブ イメージ ジェネレーター)](ngen-exe-native-image-generator.md)
- [Visual Studio 用開発者コマンド プロンプト](developer-command-prompt-for-vs.md)
- [デスクトップ アプリケーションの起動時のパフォーマンスの向上](https://devblogs.microsoft.com/dotnet/improving-launch-performance-for-your-desktop-applications/)
- [.NET 4.5 のパフォーマンスの向上の概要](https://docs.microsoft.com/archive/msdn-magazine/2012/april/clr-an-overview-of-performance-improvements-in-net-4-5)
