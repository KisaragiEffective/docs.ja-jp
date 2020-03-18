---
title: コード アクセス セキュリティ ポリシーの互換性と移行
ms.date: 03/30/2017
helpviewer_keywords:
- policy migration, compatibility
- CLR policy migration
ms.assetid: 19cb4d39-e38a-4262-b507-458915303115
ms.openlocfilehash: 949739b3336a9182eef583cc405e60e09d7ec09d
ms.sourcegitcommit: 9c54866bcbdc49dbb981dd55be9bbd0443837aa2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/14/2020
ms.locfileid: "77217160"
---
# <a name="code-access-security-policy-compatibility-and-migration"></a>コード アクセス セキュリティ ポリシーの互換性と移行

[!INCLUDE[net_security_note](../../../includes/net-security-note-md.md)]

コードアクセスセキュリティ (CAS) のポリシー部分は、.NET Framework 4 では廃止されました。 その結果、互換性のために残されているポリシーの型とメンバーを明示的または[暗黙的](#implicit_use)に (他の型とメンバーを介して)[明示的](#explicit_use)に呼び出す場合は、コンパイルの警告とランタイム例外が発生する可能性があります。

以下のいずれかの方法で警告やエラーを回避できます。

- 互換性のために残されている呼び出しの .NET Framework 4 の置換に[移行](#migration)しています。

   \- - または -

- [\<NetFx40_LegacySecurityPolicy > configuration 要素](../configure-apps/file-schema/runtime/netfx40-legacysecuritypolicy-element.md)を使用して、従来の CAS ポリシーの動作を選択します。

このトピックには、次のセクションが含まれます。

- [明示的な使用](#explicit_use)

- [暗黙の使用](#implicit_use)

- [エラーと警告](#errors_and_warnings)

- [移行: 互換性のために残されている呼び出しの置換](#migration)

- [互換性: CAS ポリシーレガシオプションの使用](#compatibility)

<a name="explicit_use"></a>

## <a name="explicit-use"></a>明示的な使用

直接セキュリティ ポリシーを操作するメンバー、または CAS ポリシーをサンドボックス化することが必要なメンバーは廃止され、既定ではエラーを発生するようになりました。

次に例を示します。

- <xref:System.AppDomain.SetAppDomainPolicy%2A?displayProperty=nameWithType>

- <xref:System.Security.HostSecurityManager.DomainPolicy%2A?displayProperty=nameWithType>

- <xref:System.Security.Policy.PolicyLevel.CreateAppDomainLevel%2A?displayProperty=nameWithType>

- <xref:System.Security.SecurityManager.LoadPolicyLevelFromString%2A?displayProperty=nameWithType>

- <xref:System.Security.SecurityManager.LoadPolicyLevelFromFile%2A?displayProperty=nameWithType>

- <xref:System.Security.SecurityManager.ResolvePolicy%2A?displayProperty=nameWithType>

- <xref:System.Security.SecurityManager.ResolveSystemPolicy%2A?displayProperty=nameWithType>

- <xref:System.Security.SecurityManager.ResolvePolicyGroups%2A?displayProperty=nameWithType>

- <xref:System.Security.SecurityManager.PolicyHierarchy%2A?displayProperty=nameWithType>

- <xref:System.Security.SecurityManager.SavePolicy%2A?displayProperty=nameWithType>

<a name="implicit_use"></a>

## <a name="implicit-use"></a>暗黙的な使用

アセンブリの読み込みオーバーロードの中にはエラーを生成するものがあります。CAS ポリシーを暗黙的に使用することが原因です。 これらのオーバーロードはCAS ポリシーを解決するために <xref:System.Security.Policy.Evidence> パラメーターを取り、アセンブリにアクセス許可セットを提供します。

次に例をいくつか示します。 パラメーターとして <xref:System.Security.Policy.Evidence> を取るオーバーロードが廃止されました。

- <xref:System.Activator.CreateInstanceFrom%2A?displayProperty=nameWithType>

- <xref:System.AppDomain.CreateInstanceFrom%2A?displayProperty=nameWithType>

- <xref:System.AppDomain.CreateInstanceAndUnwrap%2A?displayProperty=nameWithType>

- <xref:System.AppDomain.DefineDynamicAssembly%2A?displayProperty=nameWithType>

- <xref:System.AppDomain.ExecuteAssemblyByName%2A?displayProperty=nameWithType>

- <xref:System.AppDomain.Load%2A?displayProperty=nameWithType>

- <xref:System.Reflection.Assembly.LoadFrom%2A?displayProperty=nameWithType>

- <xref:System.Reflection.Assembly.Load%2A?displayProperty=nameWithType>

- <xref:System.Reflection.Assembly.LoadFrom%2A?displayProperty=nameWithType>

<a name="errors_and_warnings"></a>

## <a name="errors-and-warnings"></a>エラーおよび警告

廃止された種類とメンバーが使用されると、以下のエラー メッセージが生成されます。 <xref:System.Security.Policy.Evidence?displayProperty=nameWithType> 型自体は廃止されていないことに注意してください。

コンパイル時の警告:

`warning CS0618: '<API Name>' is obsolete: 'This method is obsolete and will be removed in a future release of the .NET Framework. Please use <suggested alternate API>. See <link> for more information.'`

実行時の例外:

<xref:System.NotSupportedException>: `This method uses CAS policy, which has been obsoleted by the .NET Framework. In order to enable CAS policy for compatibility reasons, please use the <NetFx40_LegacySecurityPolicy> configuration switch. Please see <link> for more information.`

<a name="migration"></a>

## <a name="migration-replacement-for-obsolete-calls"></a>移行: 廃止された呼び出しの代替方法

### <a name="determining-an-assemblys-trust-level"></a>アセンブリの信頼レベルの判別

CAS ポリシーは多くの場合、アセンブリ、アプリケーション ドメインのアクセス許可セット、または信頼レベルを判断するために使用されます。 .NET Framework 4 では、セキュリティポリシーを解決する必要がない次の便利なプロパティが公開されています。

- <xref:System.Reflection.Assembly.PermissionSet%2A?displayProperty=nameWithType>

- <xref:System.Reflection.Assembly.IsFullyTrusted%2A?displayProperty=nameWithType>

- <xref:System.AppDomain.PermissionSet%2A?displayProperty=nameWithType>

- <xref:System.AppDomain.IsFullyTrusted%2A?displayProperty=nameWithType>

### <a name="application-domain-sandboxing"></a>アプリケーション ドメインのサンドボックス化

通常 <xref:System.AppDomain.SetAppDomainPolicy%2A?displayProperty=nameWithType> メソッドは、アプリケーション ドメイン内のアセンブリをサンド ボックス化するために使用します。 .NET Framework 4 は、この目的で <xref:System.Security.Policy.PolicyLevel> を使用する必要がないメンバーを公開します。 詳細については、「[方法: サンドボックスで部分信頼コードを実行する](how-to-run-partially-trusted-code-in-a-sandbox.md)」を参照してください。

### <a name="determining-a-safe-or-reasonable-permission-set-for-partially-trusted-code"></a>部分的に信頼できるコードに対する安全なまたは適切なアクセス許可セットの決定

多くの場合ホストでは、ホストされているコードをサンドボックス化するための適切なアクセス許可を判別する必要があります。 .NET Framework 4 より前は、CAS ポリシーは、<xref:System.Security.SecurityManager.ResolvePolicy%2A?displayProperty=nameWithType> 方法でこれを行う方法を提供していました。 代替として、.NET Framework 4 には、指定された証拠の安全な標準のアクセス許可セットを返す <xref:System.Security.SecurityManager.GetStandardSandbox%2A?displayProperty=nameWithType> メソッドが用意されています。

### <a name="non-sandboxing-scenarios-overloads-for-assembly-loads"></a>サンドボックス化以外のシナリオ: アセンブリ読み込みのオーバーロード

アセンブリ読み込みオーバーロードを使用する目的は、アセンブリのサンドボックス化の代わりに、アセンブリ読み込みオーバーロードでないと使用できないパラメーターを指定することです。 .NET Framework 4 以降では、パラメーターとして <xref:System.Security.Policy.Evidence?displayProperty=nameWithType> オブジェクトを必要としないアセンブリ読み込みオーバーロード (<xref:System.AppDomain.ExecuteAssembly%28System.String%2CSystem.String%5B%5D%2CSystem.Byte%5B%5D%2CSystem.Configuration.Assemblies.AssemblyHashAlgorithm%29?displayProperty=nameWithType>など) を使用して、このシナリオを有効にします。

アセンブリをサンドボックス化する場合には、<xref:System.AppDomain.CreateDomain%28System.String%2CSystem.Security.Policy.Evidence%2CSystem.AppDomainSetup%2CSystem.Security.PermissionSet%2CSystem.Security.Policy.StrongName%5B%5D%29?displayProperty=nameWithType> オーバー ロードを使用します。

<a name="compatibility"></a>

## <a name="compatibility-using-the-cas-policy-legacy-option"></a>互換性: CAS ポリシーのレガシー オプションを使用する

[\<NetFx40_LegacySecurityPolicy > configuration 要素](../configure-apps/file-schema/runtime/netfx40-legacysecuritypolicy-element.md)を使用して、プロセスまたはライブラリが従来の CAS ポリシーを使用するように指定できます。 この要素を有効にすると、ポリシーと証拠のオーバーロードは、framework の以前のバージョンの場合と同様に動作します。

> [!NOTE]
> CAS ポリシーの動作は特定のランタイム バージョンに固有なので、そのランタイム バージョンの CAS ポリシーを変更しても、別のバージョンの CAS ポリシーには影響しません。

```xml
<configuration>
   <runtime>
      <NetFx40_LegacySecurityPolicy enabled="true"/>
   </runtime>
</configuration>
```

## <a name="see-also"></a>参照

- [方法 : サンドボックスで部分信頼コードを実行する](how-to-run-partially-trusted-code-in-a-sandbox.md)
- [安全なコーディングのガイドライン](../../standard/security/secure-coding-guidelines.md)
