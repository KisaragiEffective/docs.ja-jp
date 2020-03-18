---
title: ランタイムがアセンブリを検索する方法
ms.date: 03/30/2017
helpviewer_keywords:
- app.config files, assembly locations
- deploying applications [.NET Framework], assembly locations
- application configuration files, locating assemblies
- .NET Framework application deployment, locating assemblies
- locating assemblies
- assemblies [.NET Framework], location
ms.assetid: 772ac6f4-64d2-4cfb-92fd-58096dcd6c34
ms.openlocfilehash: 13e2661b67ba3b717b8917e80118175acb09e756
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "79181680"
---
# <a name="how-the-runtime-locates-assemblies"></a>ランタイムがアセンブリを検索する方法

.NET Framework アプリケーションを正しく配置するには、アプリケーションを構成するアセンブリを共通言語ランタイムがどのように検索し、バインドするかを理解している必要があります。 既定では、ランタイムはアプリケーションを構成するアセンブリの正しいバージョンをバインドしようとします。 この既定の動作は、構成ファイルの設定によってオーバーライドできます。

共通言語ランタイムは、アセンブリを検索し、アセンブリの参照を解決しようとするときにいくつかの手順を実行します。 それぞれの手順については、後続のセクションで説明します。 ランタイムがアセンブリを検索する方法の説明では、「プローブ」という用語が頻繁に使用されます。プローブとは、アセンブリの名前およびカルチャに基づいてアセンブリを特定するための一連のヒューリスティックです。

> [!NOTE]
> Windows SDK に含まれている[アセンブリ バインディング ログ ビューアー (Fuslogvw.exe)](../tools/fuslogvw-exe-assembly-binding-log-viewer.md) を使用すると、ログ ファイル内のバインディング情報を表示できます。

## <a name="initiating-the-bind"></a>バインドの開始

アセンブリを検索し、バインドするプロセスは、ランタイムが別のアセンブリへの参照を解決しようとしたときに開始します。 この参照は、静的参照または動的参照のいずれかです。 コンパイラは、ビルド時に静的参照をアセンブリ マニフェストのメタデータに記録します。 動的参照は、 <xref:System.Reflection.Assembly.Load%2A?displayProperty=nameWithType>など、各種のメソッド呼び出しの結果として実行時に生成されます。

お勧めするアセンブリの参照方法は、アセンブリ名、バージョン、カルチャ、および公開キー トークン (存在する場合) を含む完全参照を使用することです。 ランタイムはこれらの情報を使用して、このセクションで後述する手順に従ってアセンブリを検索します。 静的アセンブリまたは動的アセンブリのいずれかへの参照に関係なく、ランタイムは同じ解決プロセスを使用します。

アセンブリ名だけを指定するなど、アセンブリについての情報の一部だけを指定した呼び出しメソッドを提供することで、アセンブリを動的に参照することもできます。 その場合は、アセンブリの検索はアプリケーション ディレクトリ内だけでなされ、その他のチェックは実行されません。 <xref:System.Reflection.Assembly.Load%2A?displayProperty=nameWithType> や <xref:System.AppDomain.Load%2A?displayProperty=nameWithType>などのアセンブリの読み込み用のさまざまなメソッドを使用した部分参照を作成します。

最後に、<xref:System.Reflection.Assembly.Load%2A?displayProperty=nameWithType> などのメソッドを使用して動的参照を作成し、情報の一部だけを提供できます。その後で、アプリケーション構成ファイル内の [\<qualifyAssembly>](../configure-apps/file-schema/runtime/qualifyassembly-element.md) 要素を使用してその参照を修飾します。 この要素を使用すると、完全な参照情報 (名前、バージョン、カルチャ、および適用可能な場合は公開キー トークン) をコード内ではなく、アプリケーション構成ファイル内に提供できます。 アプリケーション ディレクトリ外部のアセンブリへの参照を完全に限定する必要がある場合、またはグローバル アセンブリ キャッシュ内のアセンブリを参照するが、完全参照をコード内ではなく構成ファイル内に指定する方が便利な場合は、この手法を使用します。

> [!NOTE]
> このタイプの部分参照は、複数のアプリケーションで共有されるアセンブリに対しては使用しないでください。 構成設定はアセンブリ別ではなくアプリケーション別に適用されるため、このタイプの部分参照を使用する共有アセンブリについては、その共有アセンブリを使用するアプリケーションごとに、そのアプリケーションの構成ファイル内に限定情報を含める必要があります。

ランタイムは、次の手順を使用することによってアセンブリ参照を解決します。

1. 適用可能な構成ファイル (アプリケーション構成ファイル、発行者ポリシー ファイル、マシン構成ファイルなど) をチェックして、[正しいアセンブリ バージョンを決定します。](#step1) 構成ファイルがリモート コンピューターに配置されている場合、ランタイムは最初にアプリケーション構成ファイルを検索し、ダウンロードする必要があります。

2. [以前にアセンブリ名がバインドされているかどうかをチェック](#step2) し、バインドされている場合は、前に読み込んだアセンブリを使用します。 前にアセンブリの読み込み要求が失敗している場合は、アセンブリの読み込みを試みることなく要求が直ちにエラーとなります。

    > [!NOTE]
    > アセンブリ バインディング エラーのキャッシュは、.NET Framework Version 2.0 で新たに追加されました。

3. [グローバル アセンブリ キャッシュをチェックします](#step3)。 そこにアセンブリが見つかった場合は、ランタイムでそのアセンブリが使用されます。

4. 次の手順を使用することによって、[アセンブリのプローブ](#step4) を実行します。

    1. 構成と発行者ポリシーが元の参照に影響しない場合およびバインド要求が <xref:System.Reflection.Assembly.LoadFrom%2A?displayProperty=nameWithType> メソッドを使用することによって作成されている場合、ランタイムは位置ヒントをチェックします。

    2. 構成ファイル内でコードベースが見つかった場合、ランタイムはその場所だけをチェックします。 このプローブが失敗した場合、ランタイムはバインド要求が失敗したと判断し、それ以上のプローブは実行しません。

    3. 「 [手順 4: コードベースまたはプローブによるアセンブリの検索](#step4)」で説明するヒューリスティックを使用してアセンブリをプローブします。 プローブしてもアセンブリが見つからなかった場合、ランタイムは Windows Installer に対してアセンブリを提供するように要求します。 これは、オンデマンド インストール機能として実行されます。

        > [!NOTE]
        > 厳密な名前を持たないアセンブリについては、ランタイムはバージョン チェックをしません。また、厳密な名前を持たないアセンブリについては、グローバル アセンブリ キャッシュ内のチェックも実行しません。

<a name="step1"></a>

## <a name="step-1-examining-the-configuration-files"></a>手順 1: 構成ファイルのチェック

アセンブリ バインディング動作は、次の 3 つの XML ファイルに基づいて、さまざまなレベルで設定できます。

- アプリケーション構成ファイル

- 発行者ポリシー ファイル

- マシン構成ファイル

これらのファイルには、特定のアセンブリについて、バインディング リダイレクト、コードの場所、バインディング モードなどの情報が同じ構文で記述されています。 バインディング プロセスをリダイレクトする各構成ファイルは [\<assemblyBinding> 要素](../configure-apps/file-schema/runtime/assemblybinding-element-for-runtime.md)を含めることができます。 [\<assemblyBinding> 要素](../configure-apps/file-schema/runtime/assemblybinding-element-for-runtime.md)の子要素には [\<dependentAssembly> 要素](../configure-apps/file-schema/runtime/dependentassembly-element.md)が含まれます。 [\<dependentAssembly> 要素](../configure-apps/file-schema/runtime/dependentassembly-element.md)の子は、[\<assemblyIdentity> 要素](/visualstudio/deployment/assemblyidentity-element-clickonce-deployment)、[\<bindingRedirect> 要素](../configure-apps/file-schema/runtime/bindingredirect-element.md)と、[\<codeBase> 要素](../configure-apps/file-schema/runtime/codebase-element.md)が含まれます。

> [!NOTE]
> 構成情報は、この 3 つの構成ファイル内に含まれていますが、すべての要素がすべての構成ファイルで有効なわけではありません。 たとえば、バインディング モードやプライベート パス情報は、アプリケーション構成ファイルだけに含めることができます。 各ファイルに含まれる情報の完全な一覧については、「 [構成ファイルを使用してアプリを構成する方法](../configure-apps/index.md)」を参照してください。

### <a name="application-configuration-file"></a>アプリケーション構成ファイル

最初に、共通言語ランタイムは、アプリケーション構成ファイル内に呼び出し元アセンブリのマニフェストに格納されているバージョン情報をオーバーライドする情報がないかどうかをチェックします。 アプリケーション構成ファイルはアプリケーションと共に配置できますが、アプリケーションの実行にとっては必須ではありません。 通常、このファイルはほとんど瞬時に取得できますが、Internet Explorer Web ベースのシナリオの場合のように、アプリケーション ベースがリモート コンピューターに存在するときは、構成ファイルをダウンロードする必要があります。

クライアント実行可能ファイルの場合、アプリケーション構成ファイルは、アプリケーションの実行可能ファイルと同じディレクトリに配置され、実行可能ファイルと同じ名前に拡張子 .config を付けた基本名が割り当てられます。 たとえば、C:\Program Files\Myapp\Myapp.exe の構成ファイルは、C:\Program Files\Myapp\Myapp.exe.config です。ブラウザー ベースのシナリオでは、HTML ファイルは明示的に構成ファイルを指すために **\<link>** 要素を使用する必要があります。

次のコードは、アプリケーション構成ファイルの簡単な例を示します。 この例では、 <xref:System.Diagnostics.TextWriterTraceListener> を <xref:System.Diagnostics.Debug.Listeners%2A> コレクションに追加し、デバッグ情報のファイルへの記録を有効にします。

```xml
<configuration>
   <system.diagnostics>
      <trace useGlobalLock="false" autoflush="true" indentsize="0">
         <listeners>
            <add name="myListener" type="System.Diagnostics.TextWriterTraceListener, system version=1.0.3300.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" initializeData="c:\myListener.log" />
         </listeners>
      </trace>
   </system.diagnostics>
</configuration>
```

### <a name="publisher-policy-file"></a>発行者ポリシー ファイル

次に、ランタイムは、発行者ポリシー ファイルが存在する場合、このファイルをチェックします。 発行者ポリシー ファイルは、コンポーネントの発行者が共有コンポーネントへの修正または更新として配布します。 それらのファイルには、アセンブリ参照を新しいバージョンに伝える共有コンポーネントの発行者によって発行された互換情報が含まれています。 アプリケーション構成ファイルやマシン構成ファイルとは異なり、発行者ポリシー ファイルは、グローバル アセンブリ キャッシュにインストールする必要のあるその発行者ポリシー ファイルに固有のアセンブリに含める必要があります。

発行者ポリシーの構成ファイルの例を次に示します。

```xml
<configuration>
    <runtime>
        <assemblyBinding xmlns="urn:schemas-microsoft-com:asm.v1">

            <dependentAssembly>
                <assemblyIdentity name="asm6" publicKeyToken="c0305c36380ba429" />
                <bindingRedirect oldVersion="3.0.0.0" newVersion="2.0.0.0"/>
            </dependentAssembly>

        </assemblyBinding>
    </runtime>
</configuration>
```

アセンブリを作成するには、[Al.exe (アセンブリ リンカー)](../tools/al-exe-assembly-linker.md) ツールを使用し、次のようにコマンドを指定します。

```console
Al.exe /link:asm6.exe.config /out:policy.3.0.asm6.dll /keyfile: compatkey.dat /v:3.0.0.0
```

`compatkey.dat` は厳密な名前のキー ファイルです。 このコマンドにより、グローバル アセンブリ キャッシュに配置できる、厳密な名前のアセンブリが作成されます。

> [!NOTE]
> 発行者ポリシーは、同じ共有コンポーネントを使用するすべてのアプリケーションに適用されます。

発行者ポリシー構成ファイルは、アプリケーションからのバージョン情報 (つまり、アセンブリ マニフェストまたはアプリケーション構成ファイルからのバージョン情報) をオーバーライドします。 アプリケーション構成ファイルに、アセンブリ マニフェストに指定されているバージョンをリダイレクトするように指示するステートメントがない場合、発行者ポリシー ファイルは、アセンブリ マニフェストに指定されているバージョンをオーバーライドします。 しかし、アプリケーション構成ファイルにリダイレクト ステートメントがある場合、発行者ポリシーは、マニフェストに指定されているバージョンではなく、アプリケーション構成ファイル内のバージョン情報をオーバーライドします。

発行者ポリシー ファイルを使用するのは、共有コンポーネントが更新され、そのコンポーネントを使用するすべてのアプリケーションが共有コンポーネントの新しいバージョンを選択する必要がある場合です。 アプリケーション構成ファイルにセーフ モードが適用されていない限り、アプリケーション構成ファイル内の設定は、発行者ポリシー ファイル内の設定によってオーバーライドされます。

#### <a name="safe-mode"></a>セーフ モード
通常、発行者ポリシー ファイルは、Service Pack またはプログラム更新の一部として明示的にインストールされます。 アップグレードされた共有コンポーネントに何か問題がある場合は、セーフ モードを使用して発行者ポリシー ファイル内のオーバーライドを無視できます。 セーフ モードは **\<publisherPolicy apply="yes**&#124;**no"/>** 要素によって決まります。この要素は、アプリケーション構成ファイルだけにあります。 この要素は、発行者ポリシーの構成情報をバインディング プロセスから削除するかどうかを指定します。

セーフ モードは、アプリケーション全体に対して設定するか、または選択したアセンブリに対して設定できます。 つまり、アプリケーションを構成するすべてのアセンブリのポリシーをオフに設定するか、または一部のアセンブリについてだけポリシーをオフにし、それ以外はオフにしないように設定できます。 アプリケーションを構成するアセンブリに発行者ポリシーを選択的に適用するには、 **\<publisherPolicy apply\=no/>** を設定し、\<**dependentAssembly**> 要素を使用して適用対象のアセンブリを指定します。 アプリケーションを構成するすべてのアセンブリに発行者ポリシーを適用するには、依存アセンブリの場合は、要素なしの **\<publisherPolicy apply\=no/>** を設定します。 構成の詳細については、「 [構成ファイルを使用してアプリを構成する方法](../configure-apps/index.md)」を参照してください。

### <a name="machine-configuration-file"></a>マシン構成ファイル
3 番目に、ランタイムはマシン構成ファイルをチェックします。 このファイルは、Machine.config という名前で、ローカル コンピューターの、ランタイムがインストールされているルート ディレクトリの Config サブディレクトリにあります。 管理者は、このマシン構成ファイルを使用して、そのコンピューターに固有のアセンブリ バインディング制限を指定できます。 マシン構成ファイル内の設定は、ほかのすべての構成設定に優先します。ただし、これは、他のすべての構成設定をこのマシン構成ファイルに配置する必要があるという意味ではありません。 管理者ポリシー ファイルによって決定されるバージョンは最終的であり、オーバーライドすることはできません。 Machine.config ファイル内に指定されたオーバーライドは、すべてのアプリケーションに作用します。 構成ファイルの詳細については、「 [構成ファイルを使用してアプリを構成する方法](../configure-apps/index.md)」を参照してください。

<a name="step2"></a>

## <a name="step-2-checking-for-previously-referenced-assemblies"></a>手順 2: 前に参照したアセンブリの検索

要求されたアセンブリが以前の呼び出しでも要求されていた場合、共通言語ランタイムは既に読み込まれているアセンブリを使用します。 アプリケーションを構成するアセンブリの名前付けの場合、これが影響を及ぼすこともあります。 アセンブリの名前付けの詳細については、「 [アセンブリ名](../../standard/assembly/names.md)」を参照してください。

アセンブリに対する前の要求が失敗している場合、そのアセンブリに対する後続の要求は、アセンブリの読み込みを試みることなく即座に失敗します。 .NET Framework Version 2.0 からは、アセンブリ バインディング エラーがキャッシュされ、キャッシュされた情報を使用してアセンブリの読み込みを試みるかどうかが判断されるようになりました。

> [!NOTE]
> バインディング エラーをキャッシュしない .NET Framework Version 1.0 および 1.1 の動作に戻すには、構成ファイルに [\<disableCachingBindingFailures> 要素](../configure-apps/file-schema/runtime/disablecachingbindingfailures-element.md)を追加します。

<a name="step3"></a>

## <a name="step-3-checking-the-global-assembly-cache"></a>手順 3: グローバル アセンブリ キャッシュのチェック

厳密な名前の付いたアセンブリの場合、バインディング プロセスは、続いてグローバル アセンブリ キャッシュ内を調べます。 グローバル アセンブリ キャッシュには、コンピューター上の複数のアプリケーションで使用できるアセンブリが格納されています。 グローバル アセンブリ キャッシュに配置するアセンブリには、すべて厳密な名前を付ける必要があります。

<a name="step4"></a>

## <a name="step-4-locating-the-assembly-through-codebases-or-probing"></a>手順 4: コードベースまたはプローブによるアセンブリの検索

共通言語ランタイムは、呼び出し元アセンブリの参照および構成ファイル内の情報を使用して正しいアセンブリ バージョンを決定した後、およびグローバル アセンブリ キャッシュ内をチェック (厳密な名前が付いたアセンブリの場合だけ) した後で、アセンブリの検索を試みます。 アセンブリを検索するプロセスは、次のとおりです。

1. [\<codeBase>](../configure-apps/file-schema/runtime/codebase-element.md) 要素は、アプリケーション構成ファイルに格納されている場合、ランタイムはこの要素に指定されている場所を調べます。 一致するアセンブリが見つかった場合は、そのアセンブリが使用され、プローブは実行されません。 指定されている場所でアセンブリが見つからなかった場合、バインド要求は失敗します。

2. 次に、ランタイムは、後でこのセクションで指定する規則に従って、参照先アセンブリをプローブします。

> [!NOTE]
> アセンブリがディレクトリ内に複数バージョンあり、特定バージョンのアセンブリを参照する場合は、[\<probing>](../configure-apps/file-schema/runtime/probing-element.md) 要素の `privatePath` 属性ではなく [\<codeBase>](../configure-apps/file-schema/runtime/codebase-element.md) 要素を使用する必要があります。 [\<probing>](../configure-apps/file-schema/runtime/probing-element.md) 要素を使用すると、ランタイムは、参照されている単純なアセンブリ名に一致するアセンブリを初めて検出した時点で、それが適切な一致かどうかに関係なくプローブを停止します。 適切な一致である場合は、そのアセンブリが使用されます。 適切な一致でない場合、プローブは停止し、バインディングは失敗します。

### <a name="locating-the-assembly-through-codebases"></a>コードベースによるアセンブリの検索

コードベース情報は、構成ファイルで [\<codeBase>](../configure-apps/file-schema/runtime/codebase-element.md) 要素を使用して指定できます。 コードベースは、ランタイムが参照先アセンブリのプローブを試みる前に、常にチェックされます。 最終バージョン リダイレクトを含む発行者ポリシー ファイルに [\<codeBase>](../configure-apps/file-schema/runtime/codebase-element.md) 要素も含まれている場合、その [\<codeBase>](../configure-apps/file-schema/runtime/codebase-element.md) 要素が使用されます。 たとえば、アプリケーション構成ファイルに [\<codeBase>](../configure-apps/file-schema/runtime/codebase-element.md) 要素が指定されていて、アプリケーション情報をオーバーライドする発行者ポリシー ファイルにも [\<codeBase>](../configure-apps/file-schema/runtime/codebase-element.md) 要素が指定されている場合、発行者ポリシー ファイルの [\<codeBase>](../configure-apps/file-schema/runtime/codebase-element.md) 要素が使用されます。

一致が [\<codeBase>](../configure-apps/file-schema/runtime/codebase-element.md) 要素で指定された場所に見つからなかった場合、バインド要求は失敗し、その後の手順は実行されません。 ランタイムは、アセンブリが呼び出し元アセンブリの検索条件と一致すると判断したときにそのアセンブリを使用します。 [\<codeBase>](../configure-apps/file-schema/runtime/codebase-element.md) の特定の要素に指定されたファイルが読み込まれると、ランタイムは、名前、バージョン、カルチャ、および公開キーが呼び出し元アセンブリの参照と一致するかどうかをチェックします。

> [!NOTE]
> アプリケーションのルート ディレクトリの外部で参照されているアセンブリは、厳密な名前が付けられていなければならず、グローバル アセンブリ キャッシュにインストールするか、または [\<codeBase>](../configure-apps/file-schema/runtime/codebase-element.md) 要素を使用して指定する必要があります。

### <a name="locating-the-assembly-through-probing"></a>プローブによるアセンブリの検索

アプリケーション構成ファイルに [\<codeBase>](../configure-apps/file-schema/runtime/codebase-element.md) 要素がない場合、ランタイムは 4 個の条件を使用してアセンブリをプローブします。

- アプリケーション ベース (アプリケーションが実行されるルート位置)。

- カルチャ (参照先アセンブリのカルチャ属性)。

- 名前 (参照先アセンブリの名前)。

- [\<probing>](../configure-apps/file-schema/runtime/probing-element.md) 要素の `privatePath` 属性 (ルート位置の下にあるサブディレクトリのユーザー定義の一覧)。 この場所は、アプリケーション ドメインの <xref:System.AppDomainSetup.PrivateBinPath?displayProperty=nameWithType> プロパティを使用して、アプリケーション構成ファイルとマネージド コード内に指定できます。 マネージド コード内に指定した場合は、マネージド コード `privatePath` が先にプローブされ、その後でアプリケーション構成ファイルに指定したパスがプローブされます。

#### <a name="probing-the-application-base-and-culture-directories"></a>アプリケーション ベース ディレクトリとカルチャ ディレクトリのプローブ

ランタイムは、常に、アプリケーションのベース (URL またはコンピューター上のアプリケーションのルート ディレクトリのいずれか) からプローブを開始します。 アプリケーション ベースで参照先アセンブリが見つからず、カルチャ情報が提供されていない場合、ランタイムはそのアセンブリ名を持つすべてのサブディレクトリ内を検索します。 プローブされるディレクトリは、次のとおりです。

- [application base] / [assembly name].dll

- [application base] / [assembly name] / [assembly name].dll

参照先アセンブリについてのカルチャ情報が指定されている場合は、次のディレクトリだけがプローブされます。

- [application base] / [culture] / [assembly name].dll

- [application base] / [culture] / [assembly name] / [assembly name].dll

#### <a name="probing-with-the-privatepath-attribute"></a>privatePath 属性を使用したプローブ

参照アセンブリの名前をカルチャ サブディレクトリとサブディレクトリの他に、ランタイムは、[\<probing>](../configure-apps/file-schema/runtime/probing-element.md) 要素の `privatePath` 属性を使用して指定されたディレクトリをプローブします。 `privatePath` 属性を使用して指定するディレクトリは、アプリケーションのルート ディレクトリのサブディレクトリである必要があります。 プローブされるディレクトリは、参照先アセンブリ要求にカルチャ情報が含まれているかどうかによって異なります。

ランタイムは、参照されている単純なアセンブリ名に一致するアセンブリを最初に見つけた時点で、それが適切な一致かどうかに関係なく、プローブを停止します。 適切な一致である場合は、そのアセンブリが使用されます。 適切な一致でない場合、プローブは停止し、バインディングは失敗します。

カルチャが含まれている場合は、次のディレクトリがプローブされます。

- [application base] / [binpath] / [culture] / [assembly name].dll

- [application base] / [binpath] / [culture] / [assembly name] / [assembly name].dll

カルチャ情報が含まれていない場合は、次のディレクトリがプローブされます。

- [application base] / [binpath] / [assembly name].dll

- [application base] / [binpath] / [assembly name] / [assembly name].dll

#### <a name="probing-examples"></a>プローブの例

次の情報が指定されています。

- 参照先アセンブリの名前: myAssembly

- アプリケーション ルート ディレクトリ: `http://www.code.microsoft.com`

- 構成ファイルの [\<probing>](../configure-apps/file-schema/runtime/probing-element.md) 要素による指定: bin

- カルチャ: de

ランタイムは、次の URL をプローブします。

- `http://www.code.microsoft.com/de/myAssembly.dll`

- `http://www.code.microsoft.com/de/myAssembly/myAssembly.dll`

- `http://www.code.microsoft.com/bin/de/myAssembly.dll`

- `http://www.code.microsoft.com/bin/de/myAssembly/myAssembly.dll`

##### <a name="multiple-assemblies-with-the-same-name"></a>同じ名前の複数のアセンブリ

同じ名前を持つ複数のアセンブリを設定する方法を次の例に示します。

```xml
<dependentAssembly>
   <assemblyIdentity name="Server" publicKeyToken="c0305c36380ba429" />
   <codeBase version="1.0.0.0" href="v1/Server.dll" />
   <codeBase version="2.0.0.0" href="v2/Server.dll" />
</dependentAssembly>
```

#### <a name="other-locations-probed"></a>プローブされるその他の場所

現在のバインディング コンテキストを使用して、アセンブリの場所を特定することもできます。 この方法が最もよく使用されるのは、 <xref:System.Reflection.Assembly.LoadFrom%2A?displayProperty=nameWithType> メソッドが使用されている場合と、COM 相互運用のシナリオの場合です。 アセンブリが <xref:System.Reflection.Assembly.LoadFrom%2A> メソッドを使用して別のアセンブリを参照している場合は、呼び出し元アセンブリの場所が、参照先アセンブリを検索する場所についてのヒントと見なされます。 一致するものが見つかった場合は、そのアセンブリが読み込まれます。 一致するものが見つからない場合は、ランタイムは、その検索セマンティクスを続行した後、Windows Installer に問い合わせて、アセンブリを提供するように求めます。 バインド要求と一致するアセンブリが提供されない場合は、例外がスローされます。 この例外は、型を参照していた場合はマネージド コード内の <xref:System.TypeLoadException> であり、読み込もうとするアセンブリが見つからなかった場合は、 <xref:System.IO.FileNotFoundException> です。

たとえば、Assembly1 が Assembly2 を参照し、Assembly1 が `http://www.code.microsoft.com/utils` からダウンロードされていた場合、この場所が、Assembly2.dll を検索する場所についてのヒントと見なされます。 次に、ランタイムは `http://www.code.microsoft.com/utils/Assembly2.dll` と `http://www.code.microsoft.com/utils/Assembly2/Assembly2.dll` のアセンブリをプローブします。 どちらの場所でも Assembly2 が見つからなかった場合は、ランタイムは Windows Installer に問い合わせます。

## <a name="see-also"></a>関連項目

- [アセンブリの読み込みのベスト プラクティス](best-practices-for-assembly-loading.md)
- [配置](index.md)
