---
title: リフレクション出力のセキュリティ関連事項
ms.date: 03/30/2017
helpviewer_keywords:
- partially trusted code
- emitting dynamic assemblies, security
- reflection emit, security
- reflection emit, partial trust scenarios
- partial trust,emitting dynamic methods
- anonymously hosted dynamic methods [.NET Framework]
- emitting dynamic assemblies,partial trust scenarios
- dynamic assemblies, security
ms.assetid: 0f8bf8fa-b993-478f-87ab-1a1a7976d298
ms.openlocfilehash: f04b40edde0755315f3b4fd4284fc7c804a54313
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73130041"
---
# <a name="security-issues-in-reflection-emit"></a>リフレクション出力のセキュリティ関連事項
.NET Framework には、Microsoft Intermediate Language (MSIL) を出力する方法が 3 種類ありますが、それぞれに固有のセキュリティ問題があります。  
  
- [動的アセンブリ](#Dynamic_Assemblies)  
  
- [匿名でホストされる動的メソッド](#Anonymously_Hosted_Dynamic_Methods)  
  
- [既存のアセンブリに関連付けられている動的メソッド](#Dynamic_Methods_Associated_with_Existing_Assemblies)  
  
 動的コードの生成方法に関係なく、生成済みコードを実行するには、生成済みコードで使用される型やメソッドに必要なすべてのアクセス許可が必要です。  
  
> [!NOTE]
> コードでのリフレクションとコードの出力に必要なアクセス許可は、.NET Framework のリリースによって異なります。 このトピックの「[バージョン情報](#Version_Information)」をご覧ください。  
  
<a name="Dynamic_Assemblies"></a>   
## <a name="dynamic-assemblies"></a>動的アセンブリ  
 動的アセンブリを作成するには、<xref:System.AppDomain.DefineDynamicAssembly%2A?displayProperty=nameWithType> メソッドのオーバーロードを使用します。 コンピューター全体のセキュリティ ポリシーが削除されたため、このメソッドのほとんどのオーバーロードは .NET Framework 4 では推奨されていません。 (「[セキュリティの変更](../security/security-changes.md)」を参照してください)。残りのオーバーロードは、信頼レベルに関係なく、任意のコードで実行できます。 これらのオーバーロードは 2 つのグループに分けられます。1 つは、動的アセンブリの作成時に適用する属性の一覧を指定するグループで、もう 1 つは属性の一覧を指定しないグループです。 アセンブリの透過性モデルを指定しない場合は、アセンブリの作成時に <xref:System.Security.SecurityRulesAttribute> 属性を適用することによって、出力アセンブリから透過性モデルが継承されます。  
  
> [!NOTE]
> <xref:System.Reflection.Emit.AssemblyBuilder.SetCustomAttribute%2A> メソッドを使用すると、動的アセンブリの作成後に適用する属性は、そのアセンブリがディスクに保存され、メモリに再び読み込まれるまでは有効になりません。  
  
 動的アセンブリのコードからは、参照できる型と他のアセンブリのメンバーのみにアクセスできます。  
  
> [!NOTE]
> 動的アセンブリでは、動的メソッドが非パブリックな型とメンバーにアクセスすることを許可する <xref:System.Security.Permissions.ReflectionPermissionFlag.MemberAccess?displayProperty=nameWithType> フラグと <xref:System.Security.Permissions.ReflectionPermissionFlag.RestrictedMemberAccess?displayProperty=nameWithType> フラグが使用されません。  
  
 一時動的アセンブリはメモリ内に作成され、ディスクに保存されることがないため、ファイルへのアクセス許可は不要です。 動的アセンブリをディスクに保存するには、<xref:System.Security.Permissions.FileIOPermission> に適切なフラグを指定する必要があります。  
  
### <a name="generating-dynamic-assemblies-from-partially-trusted-code"></a>部分信頼コードからの動的アセンブリの生成  
 インターネット アクセス許可が設定されたアセンブリで一時動的アセンブリを生成し、そのコードを実行するための条件を考えてみましょう。  
  
- 動的アセンブリで使用されるのが、パブリック型と他のアセンブリのメンバーのみである。  
  
- その型およびメンバーで必要なアクセス許可が、部分信頼のアセンブリの許可セットに含まれている。  
  
- アセンブリがディスクに保存されていない。  
  
- デバッグ シンボルが生成されていない。 (`Internet` および `LocalIntranet` のアクセス許可セットに必要なアクセス許可が指定されていない)  
  
<a name="Anonymously_Hosted_Dynamic_Methods"></a>   
## <a name="anonymously-hosted-dynamic-methods"></a>匿名でホストされる動的メソッド  
 関連付けられる型やモジュールを指定しない 2 つの <xref:System.Reflection.Emit.DynamicMethod> コンストラクター (<xref:System.Reflection.Emit.DynamicMethod.%23ctor%28System.String%2CSystem.Type%2CSystem.Type%5B%5D%29> および <xref:System.Reflection.Emit.DynamicMethod.%23ctor%28System.String%2CSystem.Type%2CSystem.Type%5B%5D%2CSystem.Boolean%29>) を使用して、匿名でホストされる動的メソッドを作成します。 このコンストラクターにより、システムで指定された完全に信頼される透過的セキュリティ アセンブリに動的メソッドが置かれます。 このコンストラクターの使用や動的メソッドのコード出力のために、特別なアクセス許可は必要ありません。  
  
 ただし、匿名でホストされる動的メソッドが作成されるとき、コール スタックがキャプチャされます。 メソッドの作成時に、キャプチャされたコール スタックに対してセキュリティ要求がなされます。  
  
> [!NOTE]
> 概念的には、メソッドの作成中に要求がなされます。 つまり、MSIL 命令が生成されるたびに要求を実行できます。 現在の実装では、<xref:System.Reflection.Emit.DynamicMethod.CreateDelegate%2A?displayProperty=nameWithType> メソッドが呼び出されたとき (<xref:System.Reflection.Emit.DynamicMethod.CreateDelegate%2A> を呼び出さずにこのメソッドが呼び出される場合)、または Just-In-Time (JIT) コンパイラが呼び出されたときに、すべての要求がなされます。  
  
 アプリケーション ドメインで許可されている場合、匿名でホストされる動的メソッドでは、JIT の参照範囲チェックを省略できますが、制限があります。匿名でホストされる動的メソッドによりアクセスされる非パブリックの型とメンバーがアセンブリに含まれ、そのアセンブリの許可セットが、出力元のコール スタックの許可セットに等しいか、または、そのサブセットであることが必要です。 アプリケーション ドメインで <xref:System.Security.Permissions.ReflectionPermission> に <xref:System.Security.Permissions.ReflectionPermissionFlag.RestrictedMemberAccess?displayProperty=nameWithType> フラグが指定されている場合は、この制限付き機能を使って JIT 参照範囲チェックを省略できます。  
  
- メソッドでパブリックな型とメンバーのみを使用する場合は、作成中にアクセス許可は不要です。  
  
- JIT 参照範囲チェックを省略するように指定した場合、メソッドの作成時になされる要求には <xref:System.Security.Permissions.ReflectionPermissionFlag.RestrictedMemberAccess?displayProperty=nameWithType> フラグが指定された <xref:System.Security.Permissions.ReflectionPermission> と、アクセスされる非パブリック メンバーを含むアセンブリの許可セットが含まれます。  
  
 非パブリック メンバーの許可セットが考慮されるため、<xref:System.Security.Permissions.ReflectionPermissionFlag.RestrictedMemberAccess?displayProperty=nameWithType> によって許可された部分信頼コードでは、信頼されるアセンブリの非パブリック メンバーを実行しても特権を昇格することができません。  
  
 他の出力済みコードと同じように、動的メソッドを実行するには、動的メソッドで使用されるメソッドで必要なアクセス許可がすべて必要になります。  
  
 匿名でホストされる動的メソッドをホストするシステム アセンブリでは、<xref:System.Security.SecurityRuleSet.Level1?displayProperty=nameWithType> 透過性モデルが使用されます。これは、.NET Framework 4 より前の .NET Framework で使用されていた透過性モデルです。  
  
 詳細については、<xref:System.Reflection.Emit.DynamicMethod> クラスを参照してください。  
  
### <a name="generating-anonymously-hosted-dynamic-methods-from-partially-trusted-code"></a>部分的に信頼されるコードによる匿名でホストされる動的メソッドの生成  
 インターネット アクセス許可が設定されたアセンブリで、匿名でホストされる動的メソッドを生成し、それを実行するための条件を考えてみましょう。  
  
- 動的メソッドで使用されるのが、パブリックな型とメンバーのみである。 許可セットに <xref:System.Security.Permissions.ReflectionPermissionFlag.RestrictedMemberAccess?displayProperty=nameWithType> が含まれる場合、許可セットが出力元アセンブリの許可セットと同じか、またはそのサブセットであるアセンブリの非パブリックの型とメンバーを使用できます。  
  
- 動的メソッドで使用されるすべての型およびメンバーで必要なアクセス許可が、部分信頼のアセンブリの許可セットに含まれている。  
  
> [!NOTE]
> 動的メソッドではデバッグ シンボルがサポートされていません。  
  
<a name="Dynamic_Methods_Associated_with_Existing_Assemblies"></a>   
## <a name="dynamic-methods-associated-with-existing-assemblies"></a>既存のアセンブリに関連付けられている動的メソッド  
 動的メソッドに既存アセンブリの型またはモジュールを関連付けるには、関連付ける型またはモジュールを指定する <xref:System.Reflection.Emit.DynamicMethod> コンストラクターのどれかを使用します。 動的メソッドに既存の型またはモジュールを関連付けると、動的メソッドが非パブリックの型とメンバーにアクセスできるようになるため、これらのコンストラクターを呼び出すにはアクセス許可が必要です。  
  
- 型に関連付けられる動的メソッドは、その型のすべてのメンバー (プライベート メンバーを含む) にアクセスでき、関連付けられた型が含まれるアセンブリ内部のすべての型およびメンバーにアクセスできます。  
  
- モジュールに関連付けられる動的メソッドは、モジュールのすべての `internal` 型およびメンバー (Visual Basic では `Friend`、共通言語ランタイムのメタデータでは `assembly`) にアクセスできます。  
  
 さらに、JIT コンパイラの参照範囲チェックを省略する機能を指定するコンストラクターを使用できます。 これを使用した場合、アクセス レベルに関係なく、動的メソッドからすべてのアセンブリのすべての型およびメンバーにアクセスできます。  
  
 コンストラクターで必要なアクセス許可は、動的メソッドに与えるアクセス許可のレベルに応じて決まります。  
  
- メソッドでパブリックな型とメンバーのみを使用し、それに独自の型や独自のモジュールを関連付ける場合は、アクセス許可が不要です。  
  
- JIT 参照範囲チェックを省略するように指定する場合、コンストラクターでは <xref:System.Security.Permissions.ReflectionPermission> に <xref:System.Security.Permissions.ReflectionPermissionFlag.MemberAccess?displayProperty=nameWithType> フラグを指定する必要があります。  
  
- 動的メソッドに別の型を関連付ける場合 (独自に作成したアセンブリ内の別の型でも)、コンストラクターでは <xref:System.Security.Permissions.ReflectionPermission> に <xref:System.Security.Permissions.ReflectionPermissionFlag.MemberAccess?displayProperty=nameWithType> フラグを指定し、<xref:System.Security.Permissions.SecurityPermission> に <xref:System.Security.Permissions.SecurityPermissionFlag.ControlEvidence?displayProperty=nameWithType> フラグを指定する必要があります。  
  
- 動的メソッドに別のアセンブリの型またはモジュールを関連付ける場合、コンストラクターでは 2 つの操作が必要です。それは、<xref:System.Security.Permissions.ReflectionPermission> に <xref:System.Security.Permissions.ReflectionPermissionFlag.RestrictedMemberAccess?displayProperty=nameWithType> フラグを指定することと、他方のモジュールを含むアセンブリの許可セットを用意することです。 つまり、対象のモジュールの許可セットにあるすべてのアクセス許可に加えて、<xref:System.Security.Permissions.ReflectionPermissionFlag.RestrictedMemberAccess?displayProperty=nameWithType> がコール スタックに含まれる必要があります。  
  
    > [!NOTE]
    > 下位互換性を確保するために、対象の許可セットと <xref:System.Security.Permissions.ReflectionPermissionFlag.RestrictedMemberAccess?displayProperty=nameWithType> の両方の要件が満たされなかった場合、コンストラクターでは <xref:System.Security.Permissions.SecurityPermission> に <xref:System.Security.Permissions.SecurityPermissionFlag.ControlEvidence?displayProperty=nameWithType> フラグを指定する必要があります。  
  
 この一覧の項目は出力アセンブリの許可セットの観点から書かれていますが、要求はコール スタック全体 (アプリケーション ドメイン境界を含む) に対してなされることに注意してください。  
  
 詳細については、<xref:System.Reflection.Emit.DynamicMethod> クラスを参照してください。  
  
### <a name="generating-dynamic-methods-from-partially-trusted-code"></a>部分信頼コードからの動的メソッドの生成  
  
> [!NOTE]
> 部分的に信頼されるコードから動的メソッドを生成する方法として推奨されるのは、[匿名でホストされる動的メソッドを使用する方法](#Anonymously_Hosted_Dynamic_Methods)です。  
  
 インターネット アクセス許可が設定されたアセンブリで、動的メソッドを生成し、それを実行するための条件を考えてみましょう。  
  
- 動的メソッドにそれを出力するモジュールまたは型が関連付けられているか、その許可セットに <xref:System.Security.Permissions.ReflectionPermissionFlag.RestrictedMemberAccess?displayProperty=nameWithType> が含まれている。さらに、関連付けられているモジュールが含まれているアセンブリの許可セットが出力アセンブリの許可セットと等しいか、そのサブセットである。  
  
- 動的メソッドで使用されるのが、パブリックな型とメンバーのみである。 許可セットに <xref:System.Security.Permissions.ReflectionPermissionFlag.RestrictedMemberAccess?displayProperty=nameWithType> が含まれ、関連付けられているモジュールが含まれているアセンブリの許可セットが出力アセンブリの許可セットと等しいか、そのサブセットである場合、その関連モジュールの中で `internal` と指定されている (Visual Basic では `Friend`、共通言語ランタイム メタデータでは `assembly`) 型とメンバーを使用できます。  
  
- 動的メソッドで使用されるすべての型およびメンバーで必要なアクセス許可が、部分信頼のアセンブリの許可セットに含まれている。  
  
- 動的メソッドで JIT 参照範囲チェックを省略しない。  
  
> [!NOTE]
> 動的メソッドではデバッグ シンボルがサポートされていません。  
  
<a name="Version_Information"></a>   
## <a name="version-information"></a>バージョン情報  
 .NET Framework 4 以降では、コンピューター全体のセキュリティ ポリシーが削除され、セキュリティ透過性が既定の適用機構になりました。 「[セキュリティの変更](../security/security-changes.md)」をご覧ください。  
  
 .NET Framework 2.0 Service Pack 1 以降では、動的アセンブリと動的メソッドを出力するときに、<xref:System.Security.Permissions.ReflectionPermission> に <xref:System.Security.Permissions.ReflectionPermissionFlag.ReflectionEmit?displayProperty=nameWithType> フラグを指定する必要がなくなりました。 このフラグは、それ以前のすべてのバージョンの .NET Framework では必要となります。  
  
> [!NOTE]
> `FullTrust` および `LocalIntranet` の名前付きアクセス許可セットでは、既定で <xref:System.Security.Permissions.ReflectionPermission> に <xref:System.Security.Permissions.ReflectionPermissionFlag.ReflectionEmit?displayProperty=nameWithType> フラグが指定されますが、`Internet` アクセス許可セットでは指定されません。 そのため、以前のバージョンの .NET Framework では、<xref:System.Security.Permissions.ReflectionPermissionFlag.ReflectionEmit> に対して <xref:System.Security.PermissionSet.Assert%2A> を実行する場合にのみ、インターネット アクセス許可でライブラリを使用できます。 このようなライブラリでは、コーディング エラーがあるとセキュリティ ホールが発生するおそれがあるため、セキュリティを慎重にレビューする必要があります。 コードの生成は本質的に特権を必要とする操作ではないため、.NET Framework 2.0 SP1 はセキュリティ確認要求を発行せずに部分信頼シナリオでコードを出力できます。 これは、生成されたコードには、コードを出力したアセンブリと同等以下のアクセス許可しかないことを意味します。 これにより、コードを出力するライブラリは透過的セキュリティになるため、<xref:System.Security.Permissions.ReflectionPermissionFlag.ReflectionEmit> を要求する必要がなくなります。そのため、安全なライブラリを簡単に作成できるようになります。  
  
 さらに、.NET Framework 2.0 SP1 では部分的に信頼される動的メソッドの非パブリックの型およびメンバーにアクセスできるように、<xref:System.Security.Permissions.ReflectionPermissionFlag.RestrictedMemberAccess?displayProperty=nameWithType> フラグが導入されています。 以前のバージョンの .NET Framework では、動的メソッドで非パブリックの型およびメンバーにアクセスするには <xref:System.Security.Permissions.ReflectionPermissionFlag.MemberAccess?displayProperty=nameWithType> フラグが必要でした。これは、部分信頼のコードには付与されないアクセス許可です。  
  
 最終的に、.NET Framework 2.0 SP1 では、匿名でホストされるメソッドが導入されました。  
  
### <a name="obtaining-information-on-types-and-members"></a>型およびメンバーの情報の取得  
 .NET Framework 2.0 以降から、非パブリックな型とメンバーに関する情報を取得する際にアクセス許可は不要になりました。 動的メソッドを出力するために必要な情報を取得するには、リフレクションを使用します。 たとえば、<xref:System.Reflection.MethodInfo> オブジェクトを使用してメソッド呼び出しを出力します。 以前のバージョンの .NET Framework では、<xref:System.Security.Permissions.ReflectionPermission> に <xref:System.Security.Permissions.ReflectionPermissionFlag.TypeInformation?displayProperty=nameWithType> フラグを指定する必要があります。 詳しくは、「[リフレクションに関するセキュリティ上の考慮事項](security-considerations-for-reflection.md)」をご覧ください。  
  
## <a name="see-also"></a>関連項目

- [リフレクションに関するセキュリティ上の考慮事項](security-considerations-for-reflection.md)
- [動的メソッドおよびアセンブリの出力](emitting-dynamic-methods-and-assemblies.md)
