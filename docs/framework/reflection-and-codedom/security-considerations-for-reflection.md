---
title: リフレクションに関するセキュリティ上の考慮事項
ms.date: 03/30/2017
helpviewer_keywords:
- permissions [.NET Framework], reflection
- MethodInfo parameters
- reflection, security
- partial trust,reflection
- nonpublic members
- reflection,partial trust
- link demands
ms.assetid: 42d9dc2a-8fcc-4ff3-b002-4ff260ef3dc5
ms.openlocfilehash: 1d5289ce15c213024af576c99fe039f5d6c1a247
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73130064"
---
# <a name="security-considerations-for-reflection"></a>リフレクションに関するセキュリティ上の考慮事項

リフレクションを使用すると、型とメンバーに関する情報を取得し、メンバーにアクセスできます (つまり、メソッドやコンストラクターの呼び出し、プロパティ値の取得と設定、イベント ハンドラーの追加と削除などを実行できます)。 リフレクションを使用した型とメンバーに関する情報の取得には、制限がありません。 すべてのコードで、次のタスクを実行するためにリフレクションを使用できます。

- 型とメンバーを列挙し、そのメタデータを確認する。

- アセンブリとモジュールを列挙し、確認する。

これに対して、リフレクションを使用したメンバーへのアクセスには、制限があります。 .NET Framework 4 以降、リフレクションを使用してセキュリティ クリティカルなメンバーにアクセスできるのは、信頼されているコードだけです。 さらに、コンパイル済みコードに直接アクセスできない非パブリック メンバーに、リフレクションを使用してアクセスできるのも信頼されたコードだけです。 最後に、セーフ クリティカルなメンバーにアクセスするリフレクションを使用するコードには、コンパイル済みコードと同様、セーフ クリティカル メンバーが要求するすべてのアクセス許可が必要です。

必要なアクセス許可がある場合、コードはリフレクションを使用して次の種類のアクセスを実行できます。

- セキュリティ クリティカルではないをパブリック メンバーへのアクセス。

- コンパイル済みコードにアクセスできる非パブリック メンバーへのアクセス (そのメンバーがセキュリティ クリティカルでない場合)。 このような非パブリック メンバーの例を次に示します。

  - 呼び出し元コードの基本クラスのプロテクト メンバー。 (リフレクションの場合、これはファミリレベル アクセスと呼ばれます。)

  - 呼び出し元コードのアセンブリの `internal` メンバー (Visual Basic では `Friend` メンバー) (リフレクションの場合、これはアセンブリレベル アクセスと呼ばれます)。

  - 呼び出し元コードを含むクラスの他のインスタンスのプライベート メンバー。

たとえば、サンドボックス化されたアプリケーション ドメインで実行されるコードは、アプリケーション ドメインから追加のアクセス許可が付与されていない限り、この一覧に示したアクセスに限定されます。

.NET Framework 2.0 Service Pack 1 以降では、通常はアクセスできないメンバーにアクセスしようとすると、対象オブジェクトと <xref:System.Security.Permissions.ReflectionPermissionFlag.MemberAccess?displayProperty=nameWithType> フラグが設定された <xref:System.Security.Permissions.ReflectionPermission> の許可セットに対する要求が生成されます。 完全な信頼で実行されているコード (コマンド ラインから起動されるアプリケーションのコードなど) には、必要とされるこれらのアクセス許可が常にあります。 (ただし、後で説明するように、セキュリティ クリティカルなメンバーにアクセスする場合は制限があります)。

必要に応じて、サンドボックス化されたアプリケーション ドメインから、<xref:System.Security.Permissions.ReflectionPermissionFlag.MemberAccess?displayProperty=nameWithType> フラグが設定された <xref:System.Security.Permissions.ReflectionPermission> を付与できます。これについては、後半の「[通常はアクセスできないメンバーへのアクセス](#accessingNormallyInaccessible)」で説明します。

<a name="accessingSecurityCritical"></a>

## <a name="accessing-security-critical-members"></a>セキュリティ クリティカルなメンバーへのアクセス

メンバーは、<xref:System.Security.SecurityCriticalAttribute> が指定されている場合、<xref:System.Security.SecurityCriticalAttribute> が指定されている型に属する場合、またはセキュリティ クリティカルなアセンブリ内にある場合は、セキュリティ クリティカルです。 .NET Framework 4 以降では、セキュリティ クリティカルなメンバーにアクセスする場合の規則は次のとおりです。

- 透過的なコードでは、コードが完全に信頼されている場合でも、リフレクションを使用してセキュリティ クリティカルなメンバーにアクセスすることはできません。 <xref:System.MethodAccessException>、<xref:System.FieldAccessException>、または <xref:System.TypeAccessException> がスローされます。

- 部分信頼で実行されているコードは透過的として扱われます。

これらの規則は、セキュリティ クリティカルなメンバーにコンパイルされたコードから直接アクセスする場合でも、リフレクションを使用してアクセスする場合でも同じです。

コマンド ラインから実行されるアプリケーション コードは完全信頼で実行されます。 この場合、透過的とマークされていない限り、リフレクションを使用してセキュリティ クリティカルなメンバーにアクセスできます。 同じコードが部分信頼で実行される場合 (サンドボックス化されたアプリケーション ドメイン内など)、セキュリティ クリティカル コードにアクセスできるかどうかは、アセンブリの信頼レベルによって決まります。アセンブリが厳密な名前を持ち、グローバル アセンブリ キャッシュにインストールされている場合は、信頼されたアセンブリとして、セキュリティ クリティカルなメンバーを呼び出すことができます。 信頼されない場合は、透過的とマークされていなくても透過的として扱われ、セキュリティ クリティカルなメンバーにはアクセスできません。

.NET Framework 4 のセキュリティ モデルの詳細については、「[.NET Framework におけるセキュリティの変更点](../security/security-changes.md)」を参照してください。

## <a name="reflection-and-transparency"></a>リフレクションと透過

.NET Framework 4 以降では、型またはメンバーの透過度は、アセンブリの信頼レベルやアプリケーション ドメインの信頼レベルなど、複数の要素に基づいて共通言語ランタイムによって決定されます。 リフレクションには、型の透過度を確認できる <xref:System.Type.IsSecurityCritical%2A>、<xref:System.Type.IsSecuritySafeCritical%2A>、<xref:System.Type.IsSecurityTransparent%2A> の各プロパティが用意されています。 これらのプロパティの有効な組み合わせを、次の表に示します。

|セキュリティ レベル|IsSecurityCritical|IsSecuritySafeCritical|IsSecurityTransparent|
|--------------------|------------------------|----------------------------|---------------------------|
|Critical|`true`|`false`|`false`|
|セーフ クリティカル|`true`|`true`|`false`|
|透明|`false`|`false`|`true`|

これらのプロパティを使用する方が、アセンブリとその型のセキュリティの注釈を調べたり、現在の信頼レベルを確認したり、ランタイムの規則を複製したりするより、はるかに簡単です。 たとえば、同じ型でも、コマンド ラインから実行される場合はセキュリティ クリティカルとなり、サンドボックス化されたアプリケーション ドメインで実行される場合は透過的セキュリティとなります。

<xref:System.Reflection.MethodBase>、<xref:System.Reflection.FieldInfo>、<xref:System.Reflection.Emit.TypeBuilder>、<xref:System.Reflection.Emit.MethodBuilder>、<xref:System.Reflection.Emit.DynamicMethod> の各クラスには、同様のプロパティがあります (プロパティ アクセサーに適用されるプロパティの場合など、その他のリフレクションとリフレクション出力の抽象クラスでは、関連付けられたメソッドにセキュリティ属性が適用されます)。

<a name="accessingNormallyInaccessible"></a>

## <a name="accessing-members-that-are-normally-inaccessible"></a>通常はアクセスできないメンバーへのアクセス

リフレクションを使用して、共通言語ランタイムのアクセシビリティ規則によりアクセスできないメンバーを呼び出すには、次の 2 つのアクセス許可のどちらかをコードに許可する必要があります。

- コードから非パブリック メンバーを呼び出せるようにするには、<xref:System.Security.Permissions.ReflectionPermissionFlag.MemberAccess?displayProperty=nameWithType> フラグが設定された <xref:System.Security.Permissions.ReflectionPermission> をコードに与える必要があります。

  > [!NOTE]
  > 既定で、インターネットを起源とするコードにこのアクセス許可を与えることは、セキュリティ ポリシーによって禁じられています。 インターネットを起源とするコードには、このアクセス許可を絶対に与えないでください。

- 呼び出されるメンバーが格納されているアセンブリの許可セットが、呼び出し元のコードが格納されているアセンブリの許可セットと同じか、またはそのサブセットである場合に限り、コードから非パブリック メンバーを呼び出せるようにするには、<xref:System.Security.Permissions.ReflectionPermissionFlag.RestrictedMemberAccess?displayProperty=nameWithType> フラグが設定された <xref:System.Security.Permissions.ReflectionPermission> をコードに与える必要があります。

たとえば、アプリケーション ドメインにインターネット アクセス許可を付与すると共に、<xref:System.Security.Permissions.ReflectionPermissionFlag.RestrictedMemberAccess?displayProperty=nameWithType> に <xref:System.Security.Permissions.ReflectionPermission> フラグを指定するとします。次に、A と B の 2 つのアセンブリを使用して、インターネット アプリケーションを実行します。

- アセンブリ B の許可セットには、アセンブリ A に付与されていないアクセス許可が含まれていないため、アセンブリ A でリフレクションを使用してアセンブリ B のプライベート メンバーにアクセスできます。

- アセンブリ A では、リフレクションを使用して mscorlib.dll などの .NET Framework アセンブリのプライベート メンバーにアクセスすることはできません。これは、mscorlib.dll が完全に信頼されており、アセンブリ A に付与されていないアクセス許可を持つためです。実行時にコード アクセス セキュリティによりスタックが走査されると、<xref:System.MemberAccessException> がスローされます。

## <a name="serialization"></a>シリアル化

シリアル化の目的で、<xref:System.Security.Permissions.SecurityPermission> に <xref:System.Security.Permissions.SecurityPermissionAttribute.SerializationFormatter%2A?displayProperty=nameWithType> フラグを指定することで、アクセシビリティに関係なく、シリアル化が可能な型のメンバーを取得および設定できるようになります。 このアクセス許可があると、コードで、インスタンスのプライベート状態を探索したり、変更したりできます。 型に対して適切なアクセス許可が与えられていると同時に、メタデータ内でシリアル化可能として[マークされている](../../standard/attributes/applying-attributes.md)必要があります。

## <a name="parameters-of-type-methodinfo"></a>MethodInfo 型のパラメーター

特に、信頼されているコードには、<xref:System.Reflection.MethodInfo> パラメーターを受け取るパブリック メンバーを記述しないでください。 このようなメンバーは、悪意のあるコードの攻撃を受けやすい場合があります。 たとえば、信頼性の高いコード内で、<xref:System.Reflection.MethodInfo> パラメーターを受け取るパブリック メンバーの場合を考えてみましょう。 このパブリック メンバーが、渡されたパラメーターに対して間接的に <xref:System.Reflection.MethodBase.Invoke%2A> メソッドを呼び出すとします。 パブリック メンバーが必要なアクセス許可を確認しない場合、セキュリティ システムは呼び出し元が大きな権限を持っていると判断するため、その <xref:System.Reflection.MethodBase.Invoke%2A> メソッド呼び出しは常に成功します。 悪意のあるコードがそのメソッドを直接呼び出すアクセス許可を持っていなくても、パブリック メンバーを呼び出すことによって間接的にこのメソッドを呼び出すことができます。

## <a name="version-information"></a>バージョン情報

- .NET Framework 4 以降では、透過的なコードからリフレクションを使用してセキュリティ クリティカルなメンバーにアクセスすることはできません。

- <xref:System.Security.Permissions.ReflectionPermissionFlag.RestrictedMemberAccess?displayProperty=nameWithType> フラグは、.NET Framework 2.0 Service Pack 1 で導入されています。 以前のバージョンの .NET Framework では、コードから非パブリック メンバーにアクセスするためにリフレクションを使用するには <xref:System.Security.Permissions.ReflectionPermissionFlag.MemberAccess?displayProperty=nameWithType> フラグを指定する必要がありました。 このアクセス許可は、部分的に信頼されるコードには絶対に付与しないでください。

- .NET Framework 2.0 以降、リフレクションを使用して非パブリックな型とメンバーに関する情報を取得する場合、アクセス許可が不要になりました。 以前のバージョンでは、<xref:System.Security.Permissions.ReflectionPermission> に <xref:System.Security.Permissions.ReflectionPermissionFlag.TypeInformation?displayProperty=nameWithType> フラグを指定する必要があります。

## <a name="see-also"></a>関連項目

- <xref:System.Security.Permissions.ReflectionPermissionFlag>
- <xref:System.Security.Permissions.ReflectionPermission>
- <xref:System.Security.Permissions.SecurityPermission>
- [セキュリティの変更](../security/security-changes.md)
- [コード アクセス セキュリティ](../misc/code-access-security.md)
- [リフレクション出力のセキュリティ関連事項](security-issues-in-reflection-emit.md)
- [型情報の表示](viewing-type-information.md)
- [属性の適用](../../standard/attributes/applying-attributes.md)
- [カスタム属性へのアクセス](accessing-custom-attributes.md)
