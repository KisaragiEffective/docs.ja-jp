---
title: 分離ストレージ
ms.date: 03/30/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- data storage using isolated storage
- stores
- storing data using isolated storage
- isolated storage
- location of isolated storage in file system
- standardizing storage systems
- storing data using isolated storage, when not to use
- code, isolated storage
- isolated storage, options
- data storage using isolated storage, when not to use
- storing data using isolated storage, options
- isolated storage, when not to use
- data storage using isolated storage, options
- isolation
ms.assetid: aff939d7-9e49-46f2-a8cd-938d3020e94e
ms.openlocfilehash: ed784bafda2aed829f2e97d7e7e8b2716c48c7ba
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "75706583"
---
# <a name="isolated-storage"></a>分離ストレージ
<a name="top"></a>デスクトップ アプリでは、分離ストレージは、コードと保存データを関連付ける標準化された方法を定義することにより、分離性と安全性を提供するデータ ストレージ機構です。 標準化には、ほかにも利点があります。 管理者は分離ストレージの操作用にデザインされたツールを使用して、ファイルのストレージ領域の構成、セキュリティ ポリシーの設定、および不要なデータの削除を行うことができます。 分離ストレージを使用すると、ファイル システムの安全な場所を指定するための固有のパスを指定する必要がなくなり、分離ストレージのアクセス権限を持たない他のアプリケーションからデータを保護できます。 アプリケーションのストレージ領域の場所を示すハードコーディングされた情報は不要になります。

> [!IMPORTANT]
> 分離ストレージは Windows 8.x Store アプリでは使用できません。 代わりに、Windows ランタイム API に含まれる `Windows.Storage` 名前空間内のアプリケーション データ クラスを使用して、ローカル データとローカル ファイルを格納します。 詳細については、Windows デベロッパー センターの [アプリケーション データ](https://docs.microsoft.com/previous-versions/windows/apps/hh464917(v=win.10)) に関する説明を参照してください。

このトピックには、次のセクションが含まれています。

- [データ コンパートメントとデータ ストア](#data_compartments_and_stores)

- [分離ストレージのクォータ](#quotas)

- [安全なアクセス](#secure_access)

- [許可されるアクセス権限とセキュリティのリスク](#allowed_usage)

- [分離ストレージの場所](#isolated_storage_locations)

- [分離ストレージの作成、列挙、および削除](#isolated_storage_tasks)

- [分離ストレージのシナリオ](#scenarios_for_isolated_storage)

- [関連トピック](#related_topics)

- [参照](#reference)

<a name="data_compartments_and_stores"></a>

## <a name="data-compartments-and-stores"></a>データ コンパートメントとデータ ストア

アプリケーションでファイルにデータを格納する場合は、ファイル名とストレージの場所を慎重に選択して、ストレージの場所が他のアプリケーションに知られたり、無防備に破壊されたりする可能性を最小限に抑える必要があります。 このような問題を管理するための標準的なシステムが適切な場所に用意されていないと、ストレージの問題点を最小限に抑える特別な技術の開発が複雑になり、信頼できる結果が得られません。

分離ストレージを使用すると、データは常にユーザーおよびアセンブリ別に分離されます。 アセンブリの発生元または厳密な名前などの資格情報によって、アセンブリの ID が決定されます。 また、同様の資格情報を使用して、データをアプリケーション ドメイン別に分離することもできます。

分離ストレージを使用している場合、アプリケーションはコードの ID のいくつかの側面 (発行元や署名など) に関連付けられた固有のデータ コンパートメントにデータを保存します。 データ コンパートメントは抽象概念であり、特定の記憶場所ではありません。データ コンパートメントは、データが格納される実際のディレクトリ位置が含まれている、ストアと呼ばれる 1 つ以上の分離ストレージ ファイルで構成されています。 たとえば、アプリケーションがそのアプリケーションに関連付けられたデータ コンパートメントを持ち、ファイル システムのディレクトリがそのアプリケーションのデータを実際に保存するストアを実装します。 ストアには、ユーザー設定情報からアプリケーション状態まで、どのような種類のデータでも保存できます。 開発者にとって、データ コンパートメントの場所は明確ではありません。 通常、ストアはクライアントに常駐しますが、サーバー アプリケーションがユーザーを偽装することによって機能し、分離ストアを使用して情報を格納することもあります。 また、サーバー上の分離ストレージには、情報と共にユーザーのローミング プロファイルを格納できます。これにより、情報はローミング ユーザーと共に移動します。

<a name="quotas"></a>

## <a name="quotas-for-isolated-storage"></a>分離ストレージのクォータ

クォータは、使用できる分離ストレージの量に対する制限です。 クォータには、ファイル領域のバイト数だけでなく、ストア内のディレクトリやその他の情報に関連するオーバーヘッドも含まれます。 分離ストレージは許容クォータを使用します。許容クォータは、 <xref:System.Security.Permissions.IsolatedStoragePermission> オブジェクトを使用して設定されるストレージ制限です。 クォータを超えるデータを書き込もうとすると、 <xref:System.IO.IsolatedStorage.IsolatedStorageException> 例外がスローされます。  セキュリティ ポリシーによって、コードに与えるアクセス許可が決定されます。セキュリティ ポリシーは、.NET Framework 構成ツール (Mscorcfg.msc) を使用して修正できます。 <xref:System.Security.Permissions.IsolatedStoragePermission> を与えられたコードは、 <xref:System.Security.Permissions.IsolatedStoragePermission.UserQuota%2A> プロパティで許容されたストレージの範囲内だけに限定されます。 しかし、別のユーザー ID を提示することにより許容クォータを無視できるため、許容クォータはコードの動作に対する厳格な制限ではなく、動作方法についての指針として使用されます。

ローミング ストアに対しては、クォータは適用されません。 そのため、ローミング ストアを使用するコードに対しては、少し高いレベルのアクセス許可が必要になります。 列挙値 <xref:System.Security.Permissions.IsolatedStorageContainment.AssemblyIsolationByRoamingUser> および <xref:System.Security.Permissions.IsolatedStorageContainment.DomainIsolationByRoamingUser> により、ローミング ユーザーの分離ストレージを使用するためのアクセス許可を指定します。

<a name="secure_access"></a>

## <a name="secure-access"></a>安全なアクセス

分離ストレージを使用すると、完全な権限を与えられていないアプリケーションでも、コンピューターのセキュリティ ポリシーによって制御される方法でデータを格納できます。 分離ストレージは、慎重に実行する必要のあるダウンロードされるコンポーネントに対しては、特に有益です。 標準の I/O 機構を使用してファイル システムにアクセスする場合に、この種のコード アクセス許可がセキュリティ ポリシーによって与えられるのは非常にまれです。 ただし、既定では、ローカル コンピューター、ローカル ネットワーク、またはインターネットから実行するコードには、分離ストレージを使用する権限が与えられています。

管理者は、適切な信頼レベルに基づいて、アプリケーションやユーザーが利用できる分離ストレージの容量を制限できます。 また、管理者は、ユーザーの永続的なデータを完全に削除できます。 分離ストレージを作成したり、分離ストレージにアクセスしたりするには、適切な <xref:System.Security.Permissions.IsolatedStorageFilePermission> アクセス許可を持っている必要があります。

分離ストレージにアクセスするには、必要なすべてのネイティブ プラットフォーム オペレーティング システム権限が必要です。 どのユーザーにファイル システムを使用する権限を与えるかを制御するアクセス制御リスト (ACL: Access Control List) の要件を満たしている必要があります。 .NET Framework のアプリケーションには、分離ストレージにアクセスするためのオペレーティング システム権限が既にあります。ただし、アプリケーションがプラットフォーム固有の偽装を行っていない場合だけです。 偽装している場合は、アプリケーションが偽装しているユーザー ID に対して分離ストレージにアクセスするための適切なオペレーティング システム権限が与えられていることをアプリケーションで確認する責任があります。 偽装によるアクセスは、Web から実行またはダウンロードされたコードで、特定のユーザーに関連付けられたストレージ領域を読み取ったり書き込んだりする場合には都合のよい方法です。

共通言語ランタイムは、 <xref:System.Security.Permissions.IsolatedStorageFilePermission> オブジェクトを使用して分類ストレージへのアクセスを制御します。 各オブジェクトには、次の値を指定するプロパティがあります。

- 許可するアクセス権限。許可するアクセスの種類を指定します。 値は <xref:System.Security.Permissions.IsolatedStorageContainment> 列挙体のメンバーです。 これらの値の詳細については、次のセクションの表を参照してください。

- ストレージ クォータ。前述のセクションを参照してください。

コードが最初にストアを開く場合、ランタイムが <xref:System.Security.Permissions.IsolatedStorageFilePermission> アクセス許可を要求します。 コードに与えられている権限の程度に応じて、このアクセス許可を与えるかどうかが決定されます。 アクセス許可が与えられた場合は、セキュリティ ポリシーとコードからの <xref:System.Security.Permissions.IsolatedStorageFilePermission>の要求によって、許可される使用法とストレージ クォータの値が決定されます。 セキュリティ ポリシーは、.NET Framework 構成ツール (Mscorcfg.msc) を使用して設定します。 それぞれの呼び出し元に最低限適切なアクセス権限が与えられていることを確認するために、コール スタック内のすべての呼び出し元がチェックされます。 ランタイムは、ファイルを保存するストアを開いたコードまたは作成したコードに与えられているクォータもチェックします。 これらの条件を満たしている場合に、アクセス許可が与えられます。 クォータは、ストアにファイルが書き込まれるたびに再チェックされます。

共通言語ランタイムは、適切な <xref:System.Security.Permissions.IsolatedStorageFilePermission> をすべてセキュリティ ポリシーに基づいて与えるため、アプリケーションのコードはアクセス許可を要求する必要はありません。 しかし、 <xref:System.Security.Permissions.IsolatedStorageFilePermission>を含め、アプリケーションに必要な特定のアクセス許可を要求するのにはそれなりの理由があります。

<a name="allowed_usage"></a>

## <a name="allowed-usage-and-security-risks"></a>許可されるアクセス権限とセキュリティのリスク

<xref:System.Security.Permissions.IsolatedStorageFilePermission> で指定されたアクセス権限によって、コードが分離ストレージを成および使用する権限のレベルが決まります。 アクセス許可に指定されているアクセス権限と分離タイプの対応関係をまとめた表を次に示します。また、それぞれのアクセス権限に関係するセキュリティのリスクを要約も示します。

|アクセス権限|分離タイプ|セキュリティのリスク|
|-------------------|---------------------|---------------------|
|<xref:System.Security.Permissions.IsolatedStorageContainment.None>|分離ストレージの使用は不可。|セキュリティのリスクはありません。|
|<xref:System.Security.Permissions.IsolatedStorageContainment.DomainIsolationByUser>|ユーザー、ドメイン、およびアセンブリ別の分離。 各アセンブリがドメイン内にサブストアを 1 つ持っています。 このアクセス許可を使用するストアは、さらに、暗黙にコンピューター別にも分離されます。|このアクセス許可レベルでは、権限なしの承認されない使用に対してリソースが保護されません。ただし、クォータを適用すると、承認されない使用に対する保護を強化できます。 このような認証されない使用は、サービス不能攻撃と呼ばれます。|
|<xref:System.Security.Permissions.IsolatedStorageContainment.DomainIsolationByRoamingUser>|`DomainIsolationByUser` と同じですが、ローミング ユーザー プロファイルが有効で、クォータが適用されていない場合は、ストアは移動先の場所に保存されます。|クォータを使用できないため、ストレージ リソースはサービス不能攻撃に対して、より攻撃されやすくなります。|
|<xref:System.Security.Permissions.IsolatedStorageContainment.AssemblyIsolationByUser>|ユーザーおよびアセンブリ別の分離。 このアクセス許可を使用するストアは、さらに、暗黙にコンピューター別にも分離されます。|このレベルでは、サービス不能攻撃の防止を強化するためにクォータが適用されます。 別のドメインの同じアセンブリがこのストアにアクセスできるので、アプリケーション間で情報がリークする可能性が大きくなります。|
|<xref:System.Security.Permissions.IsolatedStorageContainment.AssemblyIsolationByRoamingUser>|`AssemblyIsolationByUser` と同じですが、ローミング ユーザー プロファイルが有効で、クォータが適用されていない場合は、ストアは移動先の場所に保存されます。|`AssemblyIsolationByUser` と同じですが、クォータが適用されないため、サービス不能攻撃のリスクが大きくなります。|
|<xref:System.Security.Permissions.IsolatedStorageContainment.AdministerIsolatedStorageByUser>|ユーザー別の分離。 通常、管理ツールやデバッグ ツールのみ、このレベルのアクセス許可が使用されます。|このアクセス許可によるアクセスでは、アセンブリの分離とは関係なく、ユーザーの分離ストレージ ファイルやディレクトリはどれも表示または削除できます。 情報のリークとデータ消失のリスクだけでなく、ほかのリスクもあります。|
|<xref:System.Security.Permissions.IsolatedStorageContainment.UnrestrictedIsolatedStorage>|全ユーザー、ドメイン、およびアセンブリ別の分離。 通常、管理ツールやデバッグ ツールのみ、このレベルのアクセス許可が使用されます。|このアクセス許可では、すべてのユーザーのすべての分離ストアがあらゆるリスクにさらされる可能性があります。|

<a name="isolated_storage_locations"></a>

## <a name="isolated-storage-locations"></a>分離ストレージの場所

オペレーティング システムのファイル システムを使用して分離ストレージの変化を検査すると役立つ場合があります。 また、分離ストレージ ファイルの場所を知る必要がある場合もあります。 分離ストレージ ファイルの場所は、オペレーティングによって異なります。 いくつかの一般的なオペレーティング システムで、分離ストレージが作成されるルート位置を次の表に示します。 このルート位置の下の Microsoft\IsolatedStorage のディレクトリを検索してください。 ファイル システムの分離ストレージを表示するには、隠しファイルやフォルダーが表示されるようにフォルダーの設定を変更する必要があります。

|オペレーティング システム|ファイル システム上の位置|
|----------------------|-----------------------------|
|Windows 2000、Windows XP、Windows Server 2003 (Windows NT 4.0 からのアップグレード)|ローミングに対応したストア =<br /><br /> \<SYSTEMROOT>\Profiles\\<user\>\Application Data<br /><br /> ローミングに対応しないストア =<br /><br /> \<SYSTEMROOT>\Profiles\\<user\>\Local Settings\Application Data|
|Windows 2000 - クリーン インストール (および Windows 98 と Windows NT 3.51 からのアップグレード)|ローミングに対応したストア =<br /><br /> \<SYSTEMDRIVE>\Documents and Settings\\<user\>\Application Data<br /><br /> ローミングに対応しないストア =<br /><br /> \<SYSTEMDRIVE>\Documents and Settings\\<user\>\Local Settings\Application Data|
|Windows XP、Windows Server 2003 - クリーン インストール (および Windows 2000 と Windows 98 からのアップグレード)|ローミングに対応したストア =<br /><br /> \<SYSTEMDRIVE>\Documents and Settings\\<user\>\Application Data<br /><br /> ローミングに対応しないストア =<br /><br /> \<SYSTEMDRIVE>\Documents and Settings\\<user\>\Local Settings\Application Data|
|Windows 8、Windows 7、Windows Server 2008、Windows Vista|ローミングに対応したストア =<br /><br /> \<SYSTEMDRIVE>\Users\\<user\>\AppData\Roaming<br /><br /> ローミングに対応しないストア =<br /><br /> \<SYSTEMDRIVE>\Users\\<user\>\AppData\Local|

<a name="isolated_storage_tasks"></a>

## <a name="creating-enumerating-and-deleting-isolated-storage"></a>分離ストレージの作成、列挙、および削除

.NET Framework には、 <xref:System.IO.IsolatedStorage> 名前空間に、分離ストレージを必要とするタスクの実行に役立つ 3 つのクラスが用意されています。

- <xref:System.IO.IsolatedStorage.IsolatedStorageFile>。 <xref:System.IO.IsolatedStorage.IsolatedStorage?displayProperty=nameWithType> の派生クラスであり、格納されたアセンブリおよびアプリケーション ファイルの基本的な管理を提供します。 <xref:System.IO.IsolatedStorage.IsolatedStorageFile> クラスのインスタンスは、ファイル システム内に存在する 1 つのストアを表します。

- <xref:System.IO.IsolatedStorage.IsolatedStorageFileStream> 。 <xref:System.IO.FileStream?displayProperty=nameWithType> の派生クラスであり、ストア内のファイルへのアクセスを提供します。

- <xref:System.IO.IsolatedStorage.IsolatedStorageScope> クラス。適切な分離タイプのストアの作成および選択を可能にする列挙体です。

分離ストレージ クラスを使用すると、分離ストレージの作成、列挙、および削除を行うことができます。 これらの作業を実行するためのメソッドは、 <xref:System.IO.IsolatedStorage.IsolatedStorageFile> オブジェクトを通じて使用できます。 操作によっては、分離ストレージを管理する権限を表す <xref:System.Security.Permissions.IsolatedStorageFilePermission> アクセス許可が必要です。また、ファイルやディレクトリにアクセスするためのオペレーティング システム権限が必要な場合もあります。

分離ストレージに関する一般的なタスクを具体的に説明する一連の例については、「[関連トピック](#related_topics)」に示す「方法」トピックを参照してください。

<a name="scenarios_for_isolated_storage"></a>

## <a name="scenarios-for-isolated-storage"></a>分離ストレージのシナリオ

分離ストレージは、次の 4 つのシナリオを含め、さまざまな状況で役立ちます。

- ダウンロードされたコントロール。 通常の I/O クラスを使用して、インターネットからダウンロードしたマネージド コード コントロールをハード ドライブに書き込むことはできません。ただし、分離ストレージを使用することにより、ユーザーの設定やアプリケーション状態を永続化できます。

- 共有コンポーネント ストレージ。 アプリケーション間で共有されるコンポーネントで分離ストレージを使用することにより、データ ストアへのアクセスを制限できます。

- サーバー ストレージ。 サーバー アプリケーションでは、アプリケーションに対する要求を行う多数のユーザーに個別のストアを提供するために分離ストレージを使用できます。 分離ストレージは常にユーザー別に分離されているため、サーバーは、要求を行うユーザーを偽装する必要があります。 サーバーがユーザーを偽装する場合、データは、プリンシパルの ID (アプリケーションがアプリケーションのユーザーを識別するために使用する ID と同じ ID) に基づいて分離されます。

- ローミング。 アプリケーションでは、分離ストレージをローミング ユーザー プロファイルと共に使用することもできます。 分離ストレージとローミング ユーザー プロファイルを一緒に使用することにより、ユーザーの分離ストアは、プロファイルと共に移動します。

分離ストレージを使用してはならないのは、次のような場合です。

- 暗号化されていないキー、パスワードなど、重要な機密情報の格納。分離ストレージは信頼性の高いコード、アンマネージ コード、またはコンピューターの信頼されるユーザーからは保護されていません。

- コードの格納。

- 管理者が制御する構成設定と配置設定の格納。 ユーザー設定は管理者が制御しないので、構成設定とは見なされません。

多くのアプリケーションでは、データベースを使用してデータを格納し、分離しています。データベースを使用する場合は、データベースの 1 行または複数の行で 1 人の特定のユーザーのストレージを表します。 ユーザー数が少ない場合、データベースを使用するオーバーヘッドが重要である場合、またはデータベース機能が存在しない場合は、データベースの代わりに分離ストレージを使用できます。 また、データベース内の行よりも複雑で柔軟性のあるストレージがアプリケーションに必要な場合でも、分離ストレージによって実行可能な選択肢が用意されます。

<a name="related_topics"></a>

## <a name="related-topics"></a>関連トピック

|タイトル|[説明]|
|-----------|-----------------|
|[分離のタイプ](../../../docs/standard/io/types-of-isolation.md)|さまざまなタイプの分離を説明します。|
|[方法: 分離ストレージでストアを取得する](../../../docs/standard/io/how-to-obtain-stores-for-isolated-storage.md)|<xref:System.IO.IsolatedStorage.IsolatedStorageFile> クラスを使用して、ユーザーおよびアセンブリ別に分離されたストアを取得する例を示します。|
|[方法: 分離ストレージでストアを列挙する](../../../docs/standard/io/how-to-enumerate-stores-for-isolated-storage.md)|<xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetEnumerator%2A?displayProperty=nameWithType> メソッドを使用して、特定のユーザーのすべての分離ストレージのサイズを計算する方法を示します。|
|[方法: 分離ストレージでストアを削除する](../../../docs/standard/io/how-to-delete-stores-in-isolated-storage.md)|<xref:System.IO.IsolatedStorage.IsolatedStorageFile.Remove%2A?displayProperty=nameWithType> メソッドを 2 とおりの方法で使用して、分離ストアを削除する方法を示します。|
|[方法: 分離ストレージの領域不足状態に備える](../../../docs/standard/io/how-to-anticipate-out-of-space-conditions-with-isolated-storage.md)|分離ストアの残りの領域を計測する方法を示します。|
|[方法: 分離ストレージでファイルおよびディレクトリを作成する](../../../docs/standard/io/how-to-create-files-and-directories-in-isolated-storage.md)|分離ストア内にファイルやディレクトリを作成する例を示します。|
|[方法: 分離ストレージ内でファイルおよびディレクトリを検索する](../../../docs/standard/io/how-to-find-existing-files-and-directories-in-isolated-storage.md)|分離ストレージ内のディレクトリ構造やファイルを読み取る方法を示します。|
|[方法: 分離ストレージ内でファイルの読み取りと書き込みを行う](../../../docs/standard/io/how-to-read-and-write-to-files-in-isolated-storage.md)|分離ストレージ ファイルに文字列を書き込み、その文字列を読み取る例を示します。|
|[方法: 分離ストレージでファイルおよびディレクトリを削除する](../../../docs/standard/io/how-to-delete-files-and-directories-in-isolated-storage.md)|分離ストレージのファイルやディレクトリを削除する方法を示します。|
|[ファイルおよびストリーム入出力](../../../docs/standard/io/index.md)|ファイルとデータ ストリームに同期的および非同期的にアクセスする方法を説明します。|

<a name="reference"></a>

## <a name="reference"></a>リファレンス

- <xref:System.IO.IsolatedStorage.IsolatedStorage?displayProperty=nameWithType>

- <xref:System.IO.IsolatedStorage.IsolatedStorageFile?displayProperty=nameWithType>

- <xref:System.IO.IsolatedStorage.IsolatedStorageFileStream?displayProperty=nameWithType>

- <xref:System.IO.IsolatedStorage.IsolatedStorageScope?displayProperty=nameWithType>
