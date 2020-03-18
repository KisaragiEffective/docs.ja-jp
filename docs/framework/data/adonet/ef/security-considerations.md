---
title: セキュリティに関する注意事項 (Entity Framework)
ms.date: 03/30/2017
ms.assetid: 84758642-9b72-4447-86f9-f831fef46962
ms.openlocfilehash: e2e1fc75049d41b50aa59092fe1aa21e8cdab659
ms.sourcegitcommit: 700ea803fb06c5ce98de017c7f76463ba33ff4a9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/19/2020
ms.locfileid: "77452488"
---
# <a name="security-considerations-entity-framework"></a>セキュリティに関する注意事項 (Entity Framework)
このトピックでは、Entity Framework アプリケーションの開発、配置、および実行に固有のセキュリティの考慮事項について説明します。 また、セキュリティで保護された .NET Framework アプリケーションを作成するための推奨事項に従ってください。 詳細については、[セキュリティの概要](../security-overview.md)に関するページを参照してください。  
  
## <a name="general-security-considerations"></a>セキュリティについての全般的な考慮事項  
 Entity Framework を使用するすべてのアプリケーションには、次のセキュリティの考慮事項が適用されます。  
  
#### <a name="use-only-trusted-data-source-providers"></a>信頼できるデータ ソース プロバイダーのみを使用する  
 データ ソースと通信するためにはプロバイダーで次の処理が行われる必要があります。  
  
- Entity Framework から接続文字列を受け取ります。  
  
- コマンド ツリーをデータ ソースのネイティブ クエリ言語に変換する。  
  
- 結果セットを組み立てて返す。  
  
 ログオン操作の際には、ユーザーのパスワードに基づく情報が、基になるデータ ソースのネットワーク ライブラリを通じてサーバーに渡されます。 悪質なプロバイダーを使用すると、ユーザーの資格情報を盗まれたり、悪質なクエリを生成されたり、結果セットを改ざんされたりする可能性があります。  
  
#### <a name="encrypt-your-connection-to-protect-sensitive-data"></a>接続を暗号化して機密データを保護する  
 Entity Framework は、データの暗号化を直接処理しません。 ユーザーがパブリック ネットワーク経由でデータにアクセスする場合は、セキュリティを強化するためにアプリケーションでデータ ソースへの暗号化接続を確立する必要があります。 詳細については、データ ソースのセキュリティ関連のドキュメントを参照してください。 SQL Server データソースについては、「 [SQL Server への接続の暗号化](https://docs.microsoft.com/previous-versions/sql/sql-server-2008-r2/ms189067(v=sql.105))」を参照してください。  
  
#### <a name="secure-the-connection-string"></a>接続文字列を保護する  
 アプリケーションのセキュリティを実現するうえで、データ ソースへのアクセスを保護することは、最も重要な目標の 1 つです。 保護されていない接続文字列や適切に作成されていない接続文字列は脆弱性を招く原因になります。 接続情報をテキスト形式で保存したり、メモリ内に保持したりすると、システム全体のセキュリティが損なわれる可能性があります。 接続文字列を保護するための推奨事項を以下に示します。  
  
- SQL Server データ ソースで Windows 認証を使用する。  
  
     Windows 認証を使用して SQL Server データ ソースに接続する場合、接続文字列にログオンやパスワードの情報が含まれません。  
  
- 保護構成を使用して構成ファイル セクションを暗号化する。  
  
     ASP.NET には、保護構成と呼ばれる、構成ファイルの機密情報を暗号化するための機能が用意されています。 保護構成は、主に ASP.NET を想定して設計されたものですが、Windows アプリケーションの構成ファイル セクションを暗号化する目的でも使用できます。 保護された新しい構成機能の詳細については、「[保護された構成を使用した構成情報の暗号化](https://docs.microsoft.com/previous-versions/aspnet/53tyfkaw(v=vs.100))」を参照してください。  
  
- セキュリティで保護された構成ファイルに接続文字列を格納する。  
  
     接続文字列はソース コードに埋め込まないようにしてください。 接続文字列は構成ファイルに保存でき、そうすることで、アプリケーションのコードに接続文字列を組み込むことを避けられます。 Entity Data Model ウィザードでは、接続文字列が既定でアプリケーション構成ファイルに保存されます。 不正なアクセスが行われないようにそのファイルをセキュリティで保護してください。  
  
- 接続を動的に作成する場合は接続文字列ビルダーを使用する。  
  
     接続文字列を実行時に作成する必要がある場合は <xref:System.Data.EntityClient.EntityConnectionStringBuilder> クラスを使用します。 この文字列ビルダー クラスは、入力情報を検証して無効な入力情報をエスケープする処理により、接続文字列インジェクション攻撃の防止に役立ちます。 詳細については、「[方法: EntityConnection 接続文字列を作成](how-to-build-an-entityconnection-connection-string.md)する」を参照してください。 また、適切な文字列ビルダークラスを使用して、Entity Framework 接続文字列の一部であるデータソース接続文字列を作成します。 ADO.NET プロバイダーの接続文字列ビルダーの詳細については、「[接続文字列ビルダー](../connection-string-builders.md)」を参照してください。  
  
 詳細については、「[接続情報の保護](../protecting-connection-information.md)」を参照してください。  
  
#### <a name="do-not-expose-an-entityconnection-to-untrusted-users"></a>信頼できないユーザーに EntityConnection を公開しない  
 <xref:System.Data.EntityClient.EntityConnection> オブジェクトは、基になる接続の接続文字列を公開します。 <xref:System.Data.EntityClient.EntityConnection> オブジェクトにアクセスできるユーザーは、基になる接続の <xref:System.Data.ConnectionState> を変更することもできます。 <xref:System.Data.EntityClient.EntityConnection> クラスはスレッド セーフではありません。  
  
#### <a name="do-not-pass-connections-outside-the-security-context"></a>接続をセキュリティ コンテキストの外で渡さない  
 接続が確立された後にその接続をセキュリティ コンテキストの外で渡さないでください。 たとえば、接続を開くためのアクセス許可を持つスレッドが、接続をグローバルな場所に格納することは避けてください。 接続がグローバルな場所にあると、アクセス許可を明示的に与えられていない他の悪質なスレッドであっても、この開いている接続を使用することが可能になってしまいます。  
  
#### <a name="be-aware-that-logon-information-and-passwords-may-be-visible-in-a-memory-dump"></a>メモリ ダンプからログオン情報やパスワードが漏洩する可能性がある  
 データ ソースのログオンとパスワードの情報が接続文字列で渡されると、その情報はガベージ コレクションが行われるまでメモリに保持されます。 その結果、パスワードの文字列がいつメモリからなくなるのか判定できなくなります。 アプリケーションがクラッシュした場合、重要なセキュリティ情報がメモリ ダンプ ファイルに含まれている可能性があります。メモリ ダンプ ファイルは、アプリケーションを実行しているユーザーと、コンピューターに対する管理アクセス権を持つすべてのユーザーが表示できます。 Microsoft SQL Server への接続には Windows 認証を使用してください。  
  
#### <a name="grant-users-only-the-necessary-permissions-in-the-data-source"></a>データ ソースでは必要なアクセス許可のみをユーザーに与える  
 データ ソースの管理者は、必要なアクセス許可のみをユーザーに与えるようにしてください。 [!INCLUDE[esql](../../../../../includes/esql-md.md)] では、データを変更する DML ステートメント (INSERT、UPDATE、DELETE など) はサポートされていませんが、データ ソースへの接続にアクセスすることはできます。 悪質なユーザーによって、その接続を使用してデータ ソースのネイティブ言語で DML ステートメントを実行される可能性があります。  
  
#### <a name="run-applications-with-the-minimum-permissions"></a>最小限のアクセス許可でアプリケーションを実行する  
 マネージアプリケーションを完全信頼のアクセス許可で実行できるようにすると、.NET Framework によって、コンピューターへのアプリケーションのアクセスが制限されることはありません。 これは、システム全体を危険にさらすセキュリティの脆弱性の原因になります。 .NET Framework でコードアクセスセキュリティおよびその他のセキュリティメカニズムを使用するには、部分信頼アクセス許可を使用してアプリケーションを実行し、アプリケーションを機能させるために必要な最小限のアクセス許可セットを使用してアプリケーションを実行する必要があります。 次のコードアクセス許可は、Entity Framework アプリケーションで必要な最小限のアクセス許可です。  
  
- <xref:System.Security.Permissions.FileIOPermission>: <xref:System.Security.Permissions.FileIOPermissionAccess.Write> (指定されたメタデータ ファイルを開くため) または <xref:System.Security.Permissions.FileIOPermissionAccess.PathDiscovery> (メタデータ ファイルのディレクトリを検索するため)。  
  
- <xref:System.Security.Permissions.ReflectionPermission>: <xref:System.Security.Permissions.ReflectionPermissionFlag.RestrictedMemberAccess> (LINQ to Entities クエリをサポートするため)。  
  
- <xref:System.Transactions.DistributedTransactionPermission>: <xref:System.Security.Permissions.PermissionState.Unrestricted> (<xref:System.Transactions> の <xref:System.Transactions.Transaction> に参加するため)。  
  
- <xref:System.Security.Permissions.SecurityPermission>: <xref:System.Security.Permissions.SecurityPermissionFlag.SerializationFormatter> (<xref:System.Runtime.Serialization.ISerializable> インターフェイスを使用して例外をシリアル化するため)。  
  
- データベース接続を開いて、データベースに対してコマンドを実行する権限 (SQL Server データベースの <xref:System.Data.SqlClient.SqlClientPermission> など)。  
  
 詳細については、「 [Code Access Security and ADO.NET](../code-access-security.md)」を参照してください。  
  
#### <a name="do-not-install-untrusted-applications"></a>信頼できないアプリケーションをインストールしない  
 Entity Framework では、セキュリティアクセス許可は適用されません。ユーザーが指定したデータオブジェクトコードは、信頼されているかどうかに関係なく、処理中に呼び出されます。 データ ストアとアプリケーションでクライアントの認証および承認が行われるようにしてください。  
  
#### <a name="restrict-access-to-all-configuration-files"></a>すべての構成ファイルへのアクセスを制限する  
 管理者は、enterprisesec、config、machine.config、アプリケーション構成ファイル \<*アプリケーション*> など、アプリケーションの構成を指定するすべてのファイルへの書き込みアクセスを制限する必要があります。  
  
 プロバイダーの不変名は、app.config で変更できます。クライアントアプリケーションは、厳密な名前を使用して、標準プロバイダーファクトリモデルを通じて基になるプロバイダーにアクセスする必要があります。  
  
#### <a name="restrict-permissions-to-the-model-and-mapping-files"></a>モデル ファイルとマッピング ファイルへのアクセス許可を制限する  
 管理者は、モデル ファイルとマッピング ファイル (.edmx、.csdl、.ssdl、および .msl) への書き込みアクセスを、モデルやマッピングを変更するユーザーのみに制限する必要があります。 Entity Framework には、実行時にのみこれらのファイルに対する読み取りアクセス権が必要です。 また、管理者は、Entity Data Model ツールによって生成されるオブジェクトレイヤーおよびプリコンパイル済みビューソースコードファイルへのアクセスを制限する必要があります。  
  
## <a name="security-considerations-for-queries"></a>クエリのセキュリティに関する注意点  
 概念モデルのクエリを実行する際は、セキュリティに関して次の点に注意する必要があります。 これらの注意点は、EntityClient を使用する [!INCLUDE[esql](../../../../../includes/esql-md.md)] クエリと、LINQ、[!INCLUDE[esql](../../../../../includes/esql-md.md)]、およびクエリ ビルダー メソッドを使用するオブジェクト クエリに当てはまります。  
  
#### <a name="prevent-sql-injection-attacks"></a>SQL インジェクション攻撃を防止する  
 アプリケーションによっては (ユーザーや他の外部エージェントからの) 外部入力を受け取り、その入力内容に応じた処理を実行する場合があります。 直接、間接を問わず、ユーザーまたは外部エージェントから受け取った入力には、対象言語の構文を巧みに利用して不正なアクションを実行する内容が含まれている可能性があります。 ターゲット言語が Transact-sql などの構造化照会言語 (SQL) の場合、この操作は SQL インジェクション攻撃と呼ばれます。 悪質なユーザーによって、クエリに直接コマンドを挿入してデータベースのテーブルを削除されたり、サービス拒否の状態を引き起こされたり、本来実行されるべき操作を改変されたりする可能性があります。  
  
- [!INCLUDE[esql](../../../../../includes/esql-md.md)] インジェクション攻撃:  
  
     [!INCLUDE[esql](../../../../../includes/esql-md.md)] では、クエリ述語やパラメーター名で使用される値に悪質な入力を渡すことによって SQL インジェクション攻撃が行われる可能性があります。 SQL インジェクションが行われないようにするため、ユーザー入力を [!INCLUDE[esql](../../../../../includes/esql-md.md)] コマンド テキストと組み合わせないようにしてください。  
  
     [!INCLUDE[esql](../../../../../includes/esql-md.md)] クエリでは、リテラルを渡すことのできる場所であればどこででもパラメーターを渡すことができます。 外部エージェントから受け取ったリテラルを直接クエリに挿入することは避け、パラメーター化クエリを使用するようにしてください。 また、[クエリビルダーメソッド](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/bb896238(v=vs.100))を使用して Entity SQL を安全に構築することも検討してください。  
  
- LINQ to Entities インジェクション攻撃:  
  
     LINQ to Entities ではクエリの構成が可能ですが、オブジェクトモデルの API を使用して実行されます。 [!INCLUDE[esql](../../../../../includes/esql-md.md)] クエリとは異なり、LINQ to Entities クエリは文字列操作や連結を使用して構成されず、従来の SQL インジェクション攻撃の影響を受けにくくなります。  
  
#### <a name="prevent-very-large-result-sets"></a>非常に大きな結果セットを使用しないようにする  
 非常に大きな結果セットを使用すると、消費されるリソースが結果セットのサイズに比例して増加する操作を実行する場合にクライアント システムがシャットダウンする可能性があります。 次のような状況では、予想外に大きな結果セットが生成されることがあります。  
  
- 適切なフィルター条件が含まれていない、大きなデータベースに対するクエリ。  
  
- サーバーで Cartesian Join を作成するクエリ。  
  
- 入れ子になった [!INCLUDE[esql](../../../../../includes/esql-md.md)] クエリ。  
  
 ユーザー入力を受け取るときには、その入力によって結果セットがシステムで処理しきれないほど大きくならないことを確認する必要があります。 また、[!INCLUDE[esql](../../../../../includes/esql-md.md)] の LINQ to Entities または[limit](./language-reference/limit-entity-sql.md)演算子で <xref:System.Linq.Queryable.Take%2A> メソッドを使用して、結果セットのサイズを制限することもできます。  
  
#### <a name="avoid-returning-iqueryable-results-when-exposing-methods-to-potentially-untrusted-callers"></a>信頼できない可能性のある呼び出し元にメソッドを公開するときに IQueryable 結果を返さないようにする  
 次の理由で、信頼できない可能性のある呼び出し元に公開されたメソッドから <xref:System.Linq.IQueryable%601> 型を返さないようにします。  
  
- <xref:System.Linq.IQueryable%601> 型を公開するクエリのコンシューマーが、セキュリティ保護されたデータを公開したり結果セットのサイズを増やしたりする結果に関するメソッドを呼び出す可能性があるため。 たとえば、次のようなメソッド シグネチャについて考えてみます。  

    ```csharp
    public IQueryable<Customer> GetCustomer(int customerId)
    ```

    このクエリのコンシューマーは返された `.Include("Orders")` の `IQueryable<Customer>` を呼び出し、クエリが公開を意図していないデータを取得する可能性があります。 これを回避するには、メソッドの戻り値の型を <xref:System.Collections.Generic.IEnumerable%601> に変更し、結果を具体化するメソッド (`.ToList()` など) を呼び出します。  
  
- 結果が反復されると <xref:System.Linq.IQueryable%601> クエリが実行されるため、<xref:System.Linq.IQueryable%601> 型を公開するクエリのコンシューマーがスローされた例外をキャッチする可能性があります。 例外にコンシューマーを対象としていない情報が含まれている場合があります。  
  
## <a name="security-considerations-for-entities"></a>エンティティのセキュリティに関する注意点  
 エンティティ型を生成したり操作したりする際には次の点に注意する必要があります。  
  
#### <a name="do-not-share-an-objectcontext-across-application-domains"></a>ObjectContext を複数のアプリケーション ドメインで共有しない  
 <xref:System.Data.Objects.ObjectContext> を複数のアプリケーション ドメインで共有すると接続文字列の情報が漏洩する可能性があります。 代わりに、シリアル化したオブジェクトやオブジェクト グラフをもう一方のアプリケーション ドメインに転送して、そのアプリケーション ドメインでそれらのオブジェクトを <xref:System.Data.Objects.ObjectContext> にアタッチするようにしてください。 詳細については、「[オブジェクトのシリアル](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/bb738446(v=vs.100))化」を参照してください。  
  
#### <a name="prevent-type-safety-violations"></a>型の安全性違反を防止する  
 タイプセーフに違反した場合、Entity Framework は、オブジェクト内のデータの整合性を保証できません。 型の安全性違反は、信頼できないアプリケーションを完全信頼のコード アクセス セキュリティで実行できるようにすると発生する可能性があります。  
  
#### <a name="handle-exceptions"></a>例外を処理します。  
 try/catch ブロック内の <xref:System.Data.Objects.ObjectContext> のメソッドとプロパティにアクセスします。 例外をキャッチすると、未処理の例外によって <xref:System.Data.Objects.ObjectStateManager> のエントリまたはモデル情報 (テーブル名など) がアプリケーションのユーザーに公開されることはありません。  
  
## <a name="security-considerations-for-aspnet-applications"></a>ASP.NET アプリケーションのセキュリティに関する注意点  

ASP.NET アプリケーションでパスを操作するときは、次の点を考慮する必要があります。  
  
#### <a name="verify-whether-your-host-performs-path-checks"></a>ホストでパスがチェックされるかどうかを確認する  
 `|DataDirectory|` (パイプ記号で囲まれた) の置換文字列が使用される場合、ADO.NET は解決されたパスがサポートされていることを確認します。 たとえば、`DataDirectory` の後に ".." を使用することはできません。 ASP.NET をホストするプロセスによって、Web アプリケーションのルート操作 (`~`) を解決するための同じチェックが実行されます。 IIS ではこのチェックが行われますが、IIS 以外のホストでは、解決されたパスがサポートされているかどうかが確認されない可能性があります。 Entity Framework アプリケーションを展開するホストの動作を把握しておく必要があります。  
  
#### <a name="do-not-make-assumptions-about-resolved-path-names"></a>解決されるパス名を想定しない  
 ルート演算子 (`~`) と `DataDirectory` 置換文字列を解決する値は、アプリケーションの実行時には一定のままである必要がありますが、Entity Framework では、ホストがこれらの値を変更できないように制限していません。  
  
#### <a name="verify-the-path-length-before-deployment"></a>配置の前にパスの長さを確認する  
 Entity Framework アプリケーションを展開する前に、ルート演算子 (~) と `DataDirectory` の置換文字列の値が、オペレーティングシステムのパスの長さの制限を超えていないことを確認する必要があります。 ADO.NET データプロバイダーでは、パスの長さが有効な制限内であることは保証されません。  
  
## <a name="security-considerations-for-adonet-metadata"></a>ADO.NET メタデータのセキュリティに関する注意点  
 モデル ファイルとマッピング ファイルの生成と操作を行う際は、セキュリティに関して次の点に注意する必要があります。  
  
#### <a name="do-not-expose-sensitive-information-through-logging"></a>機密情報がログによって公開されないようにする。  
ADO.NET metadata service コンポーネントでは、プライベート情報はログに記録されません。 アクセス制限のために返すことのできない結果があった場合は、データベース管理システムやファイル システムで例外を生成する代わりに 0 個の結果を返す必要があります。例外には機密情報が含まれる可能性があります。  
  
#### <a name="do-not-accept-metadataworkspace-objects-from-untrusted-sources"></a>信頼されていないソースから MetadataWorkspace オブジェクトを受け取らない  
 信頼されていないソースから <xref:System.Data.Metadata.Edm.MetadataWorkspace> クラスのインスタンスをアプリケーションで受け取らないようにしてください。 代わりに、それらのソースから明示的にワークスペースを作成および設定する必要があります。  
  
## <a name="see-also"></a>参照

- [ADO.NET アプリケーションのセキュリティ保護](../securing-ado-net-applications.md)
- [配置に関する注意事項](deployment-considerations.md)
- [移行に関する注意事項](migration-considerations.md)
