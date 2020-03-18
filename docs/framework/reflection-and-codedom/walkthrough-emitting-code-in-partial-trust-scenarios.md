---
title: 'チュートリアル: 部分信頼シナリオにおけるコード出力'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- reflection emit, anonymously hosted dynamic methods
- partial trust, reflection
- partial trust, emitting dynamic methods
- reflection emit, partial trust scenarios
- anonymously hosted dynamic methods [.NET Framework]
- emitting dynamic assemblies,partial trust scenarios
- reflection emit, dynamic methods
- dynamic methods
ms.assetid: c45be261-2a9d-4c4e-9bd6-27f0931b7d25
ms.openlocfilehash: fd420c9754494b95c55df403edec87743572db03
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73129992"
---
# <a name="walkthrough-emitting-code-in-partial-trust-scenarios"></a>チュートリアル: 部分信頼シナリオにおけるコード出力

リフレクション出力は、完全信頼または部分信頼において同じ API セットを使用しますが、部分的に信頼されるコードでは実行する機能によって特定のアクセス許可が必要になります。 リフレクション出力には、匿名でホストされる動的メソッドという機能があります。この機能は、透過的セキュリティ アセンブリによって部分信頼で使用されます。

> [!NOTE]
> .NET Framework 3.5 以前では、コード出力を行うには <xref:System.Security.Permissions.ReflectionPermission> に <xref:System.Security.Permissions.ReflectionPermissionFlag.ReflectionEmit?displayProperty=nameWithType> フラグを指定する必要がありました。 このアクセス許可は、既定で `FullTrust` および `Intranet` の名前付きアクセス許可セットには含まれますが、`Internet` アクセス許可セットには含まれません。 したがって、ライブラリを部分信頼で使用するには、<xref:System.Security.SecurityCriticalAttribute> 属性を設定し、<xref:System.Security.Permissions.ReflectionPermissionFlag.ReflectionEmit> に対して <xref:System.Security.PermissionSet.Assert%2A> メソッドを実行する必要がありました。 このようなライブラリでは、コーディング エラーがあるとセキュリティ ホールが発生するおそれがあるため、セキュリティを慎重にレビューする必要があります。 コードの生成は本質的に特権を必要とする操作ではないため、.NET Framework 3.5 はセキュリティ確認要求を発行せずに部分信頼シナリオでコードを出力できます。 これは、生成されたコードには、コードを出力したアセンブリと同等以下のアクセス許可しかないことを意味します。 これにより、コードを出力するライブラリは透過的セキュリティになるため、<xref:System.Security.Permissions.ReflectionPermissionFlag.ReflectionEmit> を要求する必要がなくなります。つまり、安全なライブラリを記述するためにセキュリティを入念に確認する必要がなくなります。

このチュートリアルでは、次の作業について説明します。

- [部分的に信頼されたコードのテスト用に単純なサンドボックスを設定する](#Setting_up)

  > [!IMPORTANT]
  > これは、部分信頼でコードを試す簡単な方法です。 実際には信頼されていない場所から取得したコードを実行する場合は、「[How to: Run Partially Trusted Code in a Sandbox](../misc/how-to-run-partially-trusted-code-in-a-sandbox.md)」 (方法: サンドボックスで部分信頼コードを実行する) を参照してください。

- [部分的に信頼されたアプリケーション ドメインでコードを実行する](#Running_code)

- [匿名でホストされる動的メソッドを使用して部分信頼でコードを出力し実行する](#Using_methods)

部分信頼シナリオでのコード出力の詳細については、「[リフレクション出力のセキュリティ関連事項](security-issues-in-reflection-emit.md)」を参照してください。

ここで説明する手順に示すコードの完全な一覧については、このチュートリアルの最後の「[例](#Example)」を参照してください。

<a name="Setting_up"></a>

## <a name="setting-up-partially-trusted-locations"></a>部分的に信頼された場所を設定する

次の 2 つの手順では、部分信頼でテストできるコードの場所を設定する方法を説明します。

- 最初の手順では、サンドボックス化されたアプリケーション ドメインを作成する方法について説明します。このドメインでは、インターネット アクセス許可がコードに付与されます。

- 2 番目の手順では、部分的に信頼されたアプリケーション ドメインに対し、<xref:System.Security.Permissions.ReflectionPermission> フラグを設定して <xref:System.Security.Permissions.ReflectionPermissionFlag.RestrictedMemberAccess?displayProperty=nameWithType> を追加する方法を説明します。これにより、信頼レベルが同等以下のアセンブリでプライベート データにアクセスできるようになります。

### <a name="creating-sandboxed-application-domains"></a>サンドボックス化されたアプリケーション ドメインを作成する

部分信頼でアセンブリを実行するアプリケーション ドメインを作成するには、<xref:System.AppDomain.CreateDomain%28System.String%2CSystem.Security.Policy.Evidence%2CSystem.AppDomainSetup%2CSystem.Security.PermissionSet%2CSystem.Security.Policy.StrongName%5B%5D%29?displayProperty=nameWithType> メソッド オーバーロードを使用して、アセンブリに付与するアクセス許可セットを指定する必要があります。 許可セットを指定するための最も簡単な方法として、セキュリティ ポリシーから名前付きアクセス許可セットを取得する方法があります。

次の手順では、部分信頼でコードを実行するサンドボックス化されたアプリケーション ドメインを作成し、出力されるコードが、パブリック型のパブリック メンバーにのみアクセスできるというシナリオをテストします。 後続の手順では、<xref:System.Security.Permissions.ReflectionPermissionFlag.RestrictedMemberAccess> を追加する方法を説明し、出力されるコードが、同等以下のアクセス許可を付与されたアセンブリ内にある非パブリックな型とメンバーにアクセスできるというシナリオをテストします。

#### <a name="to-create-an-application-domain-with-partial-trust"></a>部分信頼でアプリケーション ドメインを作成するには

1. サンドボックス化されたアプリケーション ドメイン内のアセンブリに付与するアクセス許可セットを作成します。 ここでは、インターネット ゾーンのアクセス許可セットを使用します。

    [!code-csharp[HowToEmitCodeInPartialTrust#2](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEmitCodeInPartialTrust/cs/source.cs#2)]
    [!code-vb[HowToEmitCodeInPartialTrust#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEmitCodeInPartialTrust/vb/source.vb#2)]

2. アプリケーション ドメインをアプリケーション パスで初期化するため、<xref:System.AppDomainSetup> オブジェクトを作成します。

    > [!IMPORTANT]
    > 単純にするため、このコード例では現在のフォルダーを使用します。 実際にインターネットから取得されたコードを実行する場合は、信頼できないコード用の別のフォルダーを使用してください。詳細については、「[How to: Run Partially Trusted Code in a Sandbox](../misc/how-to-run-partially-trusted-code-in-a-sandbox.md)」(方法: サンドボックスで部分信頼コードを実行する) を参照してください。

    [!code-csharp[HowToEmitCodeInPartialTrust#3](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEmitCodeInPartialTrust/cs/source.cs#3)]
    [!code-vb[HowToEmitCodeInPartialTrust#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEmitCodeInPartialTrust/vb/source.vb#3)]

3. アプリケーション ドメインで実行されるすべてのアセンブリについて、アプリケーション ドメイン設定情報、および許可セットを指定して、アプリケーション ドメインを作成します。

    [!code-csharp[HowToEmitCodeInPartialTrust#5](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEmitCodeInPartialTrust/cs/source.cs#5)]
    [!code-vb[HowToEmitCodeInPartialTrust#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEmitCodeInPartialTrust/vb/source.vb#5)]

    <xref:System.AppDomain.CreateDomain%28System.String%2CSystem.Security.Policy.Evidence%2CSystem.AppDomainSetup%2CSystem.Security.PermissionSet%2CSystem.Security.Policy.StrongName%5B%5D%29?displayProperty=nameWithType> メソッド オーバーロードの最後のパラメーターを使用すると、アプリケーション ドメインの許可セットではなく、完全な信頼が付与されたアセンブリのセットを指定できます。 .NET Framework アセンブリはグローバル アセンブリ キャッシュにあるため、アプリケーションで使用するそれらのアセンブリを指定する必要はありません。 グローバル アセンブリ キャッシュにあるアセンブリは、常に完全に信頼されます。 このパラメーターを使用して、グローバル アセンブリ キャッシュには存在しない厳密名のアセンブリを指定できます。

### <a name="adding-restrictedmemberaccess-to-sandboxed-domains"></a>サンドボックス化されたドメインに RestrictedMemberAccess を追加する

ホスト アプリケーションは、匿名でホストされる動的メソッドが、コードを出力するアセンブリの信頼レベルと同等以下の信頼レベルが設定されたアセンブリ内のプライベート データにアクセスすることを許可できます。 この制限付き機能を有効にして Just-In-Time (JIT) 参照範囲チェックをスキップするため、ホスト アプリケーションは、許可セットに対し、<xref:System.Security.Permissions.ReflectionPermission> (RMA) フラグを設定した <xref:System.Security.Permissions.ReflectionPermissionFlag.RestrictedMemberAccess?displayProperty=nameWithType> オブジェクトを追加します。

たとえば、ホストはインターネット アプリケーションに対し、RMA を設定した Internet アクセス許可を付与し、インターネット アプリケーションが独自のアセンブリ内にあるプライベート データにアクセスするコードを出力できるようにすることが可能です。 このアクセスは、信頼レベルが同等以下であるアセンブリに限定されるため、インターネット アプリケーションは、.NET Framework アセンブリのような完全に信頼されたアセンブリのメンバーにはアクセスできません。

> [!NOTE]
> 特権の昇格を回避するため、匿名でホストされる動的メソッドの作成時にはアセンブリ出力の履歴情報が含まれます。 メソッドの呼び出し時に履歴情報がチェックされるため、 完全に信頼されたコードから呼び出された、匿名でホストされる動的メソッドは、依然として出力アセンブリの信頼レベルに制限されます。

#### <a name="to-create-an-application-domain-with-partial-trust-plus-rma"></a>RMA を設定した部分信頼でアプリケーション ドメインを作成するには

1. <xref:System.Security.Permissions.ReflectionPermission> (RMA) フラグを指定した新しい <xref:System.Security.Permissions.ReflectionPermissionFlag.RestrictedMemberAccess> オブジェクトを作成し、<xref:System.Security.PermissionSet.SetPermission%2A?displayProperty=nameWithType> メソッドを使用して、許可セットにアクセス許可を追加します。

    [!code-csharp[HowToEmitCodeInPartialTrust#7](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEmitCodeInPartialTrust/cs/source.cs#7)]
    [!code-vb[HowToEmitCodeInPartialTrust#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEmitCodeInPartialTrust/vb/source.vb#7)]

    許可セットにアクセス許可が含まれていない場合、<xref:System.Security.PermissionSet.AddPermission%2A> メソッドによって、許可セットにアクセス許可が追加されます。 許可セットにアクセス許可が含まれている場合は、既存のアクセス許可に指定フラグが追加されます。

    > [!NOTE]
    > RMA は、匿名でホストされる動的メソッドの 1 つの機能です。 通常の動的メソッドが JIT 参照範囲チェックをスキップした場合、出力コードには完全信頼が必要です。

2. アプリケーション ドメイン設定情報、および許可セットを指定して、アプリケーション ドメインを作成します。

    [!code-csharp[HowToEmitCodeInPartialTrust#8](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEmitCodeInPartialTrust/cs/source.cs#8)]
    [!code-vb[HowToEmitCodeInPartialTrust#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEmitCodeInPartialTrust/vb/source.vb#8)]

<a name="Running_code"></a>

## <a name="running-code-in-sandboxed-application-domains"></a>サンドボックス化されたアプリケーション ドメインでコードを実行する

次の手順では、アプリケーション ドメインで実行可能なメソッドを使用してクラスを定義する方法、そのドメインでクラスのインスタンスを作成する方法、そのメソッドを実行する方法について説明します。

#### <a name="to-define-and-execute-a-method-in-an-application-domain"></a>アプリケーション ドメインでメソッドを定義して実行するには

1. <xref:System.MarshalByRefObject> から派生するクラスを定義します。 これにより、他のアプリケーション ドメインにクラスのインスタンスを作成し、アプリケーション ドメインの境界を越えてメソッドを呼び出すことができます。 この例のクラスの名前は `Worker` です。

    [!code-csharp[HowToEmitCodeInPartialTrust#10](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEmitCodeInPartialTrust/cs/source.cs#10)]
    [!code-vb[HowToEmitCodeInPartialTrust#10](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEmitCodeInPartialTrust/vb/source.vb#10)]

2. 実行するコードを含むパブリック メソッドを定義します。 この例では、コードは単純な動的メソッドを出力し、メソッドを実行するためのデリゲートを作成して、デリゲートを呼び出します。

    [!code-csharp[HowToEmitCodeInPartialTrust#11](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEmitCodeInPartialTrust/cs/source.cs#11)]
    [!code-vb[HowToEmitCodeInPartialTrust#11](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEmitCodeInPartialTrust/vb/source.vb#11)]

3. メイン プログラムで、アセンブリの表示名を取得します。 この名前は、サンドボックス化されたアプリケーション ドメインで `Worker` クラスのインスタンスを作成するときに使用します。

    [!code-csharp[HowToEmitCodeInPartialTrust#14](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEmitCodeInPartialTrust/cs/source.cs#14)]
    [!code-vb[HowToEmitCodeInPartialTrust#14](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEmitCodeInPartialTrust/vb/source.vb#14)]

4. メイン プログラムでは、このチュートリアルの[最初の手順](#Setting_up)で説明したとおり、サンドボックス化されたアプリケーション ドメインを作成します。 `Internet` メソッドはパブリック メソッドのみを使用するため、`SimpleEmitDemo` アクセス許可セットに任意のアクセス許可を追加しないでください。

5. メイン プログラムで、サンドボックス化されたアプリケーション ドメインに `Worker` クラスのインスタンスを作成します。

    [!code-csharp[HowToEmitCodeInPartialTrust#12](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEmitCodeInPartialTrust/cs/source.cs#12)]
    [!code-vb[HowToEmitCodeInPartialTrust#12](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEmitCodeInPartialTrust/vb/source.vb#12)]

    <xref:System.AppDomain.CreateInstanceAndUnwrap%2A> メソッドにより、対象のアプリケーション ドメイン内にオブジェクトが作成され、オブジェクトのプロパティおよびメソッドの呼び出しに使用できるプロキシが返されます。

    > [!NOTE]
    > Visual Studio でこのコードを使用する場合は、名前空間を含むようにクラスの名前を変更する必要があります。 既定では、名前空間がプロジェクト名になります。 たとえば、プロジェクト名が "PartialTrust" である場合、クラス名は "PartialTrust.Worker" にする必要があります。

6. `SimpleEmitDemo` メソッドを呼び出すコードを追加します。 呼び出しはアプリケーションのドメイン境界を越えてマーシャリングされ、コードはサンドボックス化されたアプリケーション ドメインで実行されます。

    [!code-csharp[HowToEmitCodeInPartialTrust#13](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEmitCodeInPartialTrust/cs/source.cs#13)]
    [!code-vb[HowToEmitCodeInPartialTrust#13](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEmitCodeInPartialTrust/vb/source.vb#13)]

<a name="Using_methods"></a>

## <a name="using-anonymously-hosted-dynamic-methods"></a>匿名でホストされる動的メソッドを使用する

匿名でホストされる動的メソッドは、システムが提供する透過的なアセンブリに関連付けられます。 そのため、これらのメッソドに含まれるコードは透過的になります。 それに対し、通常の動的メソッドは、直接指定されるか関連付けられた型から推論されるかに関係なく、既存のモジュールに関連付ける必要があり、そのモジュールからセキュリティ レベルを引き継ぎます。

> [!NOTE]
> 次の手順では、匿名のホストを提供するアセンブリに動的メソッドを関連付ける唯一の方法であるコンストラクターの使用法について説明します。 匿名のホスト アセンブリでモジュールを明示的に指定することはできません。

通常の動的メソッドは、関連付けられているモジュールの内部メンバーまたは関連付けられている型のプライベート メンバーにアクセスできます。 匿名でホストされる動的メソッドは他のコードから分離されているため、プライベート データにアクセスすることはできません。 ただし、JIT 参照範囲チェックをスキップしてプライベート データにアクセスするための、制限付き機能が設定されています。 この機能が設定されるのは、コードを出力するアセンブリの信頼レベルと同等以下の信頼レベルが設定されたアセンブリに限定されます。

特権の昇格を回避するため、匿名でホストされる動的メソッドの作成時にはアセンブリ出力の履歴情報が含まれます。 メソッドの呼び出し時に履歴情報がチェックされるため、 完全に信頼されたコードから呼び出された、匿名でホストされる動的メソッドは、依然として出力アセンブリの信頼レベルに制限されます。

#### <a name="to-use-anonymously-hosted-dynamic-methods"></a>匿名でホストされる動的メソッドを使用するには

- 関連付けられたモジュールや型を指定しないコンストラクターを使用して、匿名でホストされる動的メソッドを作成します。

  [!code-csharp[HowToEmitCodeInPartialTrust#15](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEmitCodeInPartialTrust/cs/source.cs#15)]
  [!code-vb[HowToEmitCodeInPartialTrust#15](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEmitCodeInPartialTrust/vb/source.vb#15)]

  匿名でホストされる動的メソッドが使用するのがパブリック型とパブリック メソッドのみである場合、メンバー アクセスを制限する必要はなく、JIT 参照範囲チェックをスキップする必要もありません。

  動的メソッドの出力に必要なアクセス許可はありませんが、出力コードには、そのコードが使用する型やメソッドに応じたアクセス許可が必要です。 たとえば、ファイルにアクセスするメソッドを呼び出す出力コードには、<xref:System.Security.Permissions.FileIOPermission> が必要です。 信頼レベルに該当のアクセス許可が含まれていないと、出力コードの実行時にセキュリティ例外がスローされます。 ここでは、<xref:System.Console.WriteLine%2A?displayProperty=nameWithType> メソッドのみを使用する動的メソッドを出力するコードを示します。 このコードは、部分的に信頼された場所から実行できます。

- または、JIT 参照範囲チェックをスキップするために機能を制限した、匿名でホストされる動的メソッドを作成します。これを行うには、<xref:System.Reflection.Emit.DynamicMethod.%23ctor%28System.String%2CSystem.Type%2CSystem.Type%5B%5D%2CSystem.Boolean%29> コンストラクターを使用して、`true` パラメーターに `restrictedSkipVisibility` を指定します。

  [!code-csharp[HowToEmitCodeInPartialTrust#16](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEmitCodeInPartialTrust/cs/source.cs#16)]
  [!code-vb[HowToEmitCodeInPartialTrust#16](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEmitCodeInPartialTrust/vb/source.vb#16)]

  ここでの制限は、匿名でホストされる動的メソッドがアクセスできるプライベート データは、出力アセンブリの信頼レベルと同等以下の信頼レベルが設定されたアセンブリ内にあるプライベート データに限定されることを意味します。 たとえば、動的メソッドをインターネット信頼で実行している場合、同様にインターネット信頼で実行している他のアセンブリ内にあるプライベート データにはアクセスできますが、.NET Framework アセンブリのプライベート データにはアクセスできません。 .NET Framework アセンブリは、グローバル アセンブリ キャッシュにインストールされているもので、常に完全に信頼されます。

  匿名でホストされる動的メソッドは、ホスト アプリケーションが <xref:System.Security.Permissions.ReflectionPermission> フラグが設定された <xref:System.Security.Permissions.ReflectionPermissionFlag.RestrictedMemberAccess?displayProperty=nameWithType> を付与している場合のみ、JIT 参照範囲チェックをスキップして制限付き機能を使用できます。 このアクセス許可は、メソッドの呼び出し時に要求されます。

  > [!NOTE]
  > 出力アセンブリの呼び出し履歴情報は、動的メソッドの作成時に含まれます。 したがって、この要求は、メソッドを呼び出すアセンブリではなく、出力アセンブリのアクセス許可に対して発行されます。 これにより、昇格されたアクセス権で出力コードが実行されることを回避します。

  制限付きメンバー アクセスの使用と制約については、このチュートリアルの最後の「[完全なコード例](#Example)」に示します。 `Worker` クラスには、参照範囲チェックをスキップするための制限付き機能の有無に関係なく匿名でホストされる動的メソッドを作成できるメソッドが含まれます。例では、異なる信頼レベルを設定したアプリケーション ドメインでこのメソッドを実行した結果も示します。

  > [!NOTE]
  > 参照範囲チェックをスキップするための制限付き機能は、匿名でホストされる動的メソッドの 1 つの機能です。 通常の動的メソッドが JIT 参照範囲チェックをスキップした場合、完全信頼が与えられている必要があります。

<a name="Example"></a>

## <a name="example"></a>例

### <a name="description"></a>説明

<xref:System.Security.Permissions.ReflectionPermissionFlag.RestrictedMemberAccess> フラグを使用して、匿名でホストされる動的メソッドが JIT の参照範囲チェックをスキップできるようにするコード例を次に示します。ただし、対象のメンバーの信頼レベルは、コードを出力するアセンブリ以下であることを前提とします。

この例では、アプリケーション ドメインの境界を越えてマーシャリングできる `Worker` クラスを定義します。 クラスには、動的メソッドを出力して実行する、`AccessPrivateMethod` メソッド オーバーロードが 2 つあります。 最初のオーバーロードは、`PrivateMethod` クラスのプライベート `Worker` メソッドを呼び出す動的メソッドを出力します。これは、JIT の参照範囲チェックを実行するかどうかにかかわらず、動的メソッドを出力できます。 2 番目のオーバーロードは、`internal` クラスの `Friend` プロパティ (Visual Basic では <xref:System.String> プロパティ) にアクセスする動的メソッドを出力します。

この例では、ヘルパー メソッドを使用して、`Internet` アクセス許可に制限される許可セットを作成します。次に、<xref:System.AppDomain.CreateDomain%28System.String%2CSystem.Security.Policy.Evidence%2CSystem.AppDomainSetup%2CSystem.Security.PermissionSet%2CSystem.Security.Policy.StrongName%5B%5D%29?displayProperty=nameWithType> メソッド オーバーロードを使用してアプリケーション ドメインを作成し、この許可セットを使用するドメインで実行するすべてのコードを指定します。 アプリケーション ドメインに `Worker` クラスのインスタンスを作成し、`AccessPrivateMethod` メソッドを 2 回実行します。

- `AccessPrivateMethod` メソッドを初めて実行すると、JIT の参照範囲チェックが強制的に実行されます。 動的メソッドは、JIT の参照範囲チェックによってプライベート メソッドにアクセスできなくなるため、呼び出し時に失敗します。

- `AccessPrivateMethod` メソッドの 2 回目の実行では、JIT 参照範囲チェックをスキップします。 `Internet` 許可セットでは、参照範囲チェックをスキップするのに十分なアクセス許可が付与されていないため、動的メソッドはコンパイル時に失敗します。

この例では、<xref:System.Security.Permissions.ReflectionPermission> を指定した <xref:System.Security.Permissions.ReflectionPermissionFlag.RestrictedMemberAccess?displayProperty=nameWithType> を許可セットに追加します。 次に、2 番目のドメインを作成し、新しい許可セットにアクセス許可が付与されるドメインで実行するすべてのコードを指定します。 新しいアプリケーション ドメインに `Worker` クラスのインスタンスを作成し、`AccessPrivateMethod` メソッドの両方のオーバーロードを実行します。

- `AccessPrivateMethod` メソッドの最初のオーバーロードが実行され、JIT の参照範囲チェックがスキップされます。 コードを出力するアセンブリがプライベート メソッドを含むアセンブリと同じであるため、動的メソッドのコンパイルと実行は成功します。 したがって、信頼レベルは同等です。 `Worker` クラスを含むアプリケーションに複数のアセンブリが存在する場合、これらは同じ信頼レベルにあるため、どのアセンブリでも同じプロセスが成功します。

- `AccessPrivateMethod` メソッドの 2 番目のオーバーロードが実行され、JIT の参照範囲チェックは再度スキップされます。 <xref:System.String> クラスの `internal` `FirstChar` プロパティへのアクセスを試行するため、ここでは、動的メソッドはコンパイル時に失敗します。 <xref:System.String> クラスを含むアセンブリは完全に信頼されます。 したがって、コードを出力するアセンブリよりも信頼レベルが高くなります。

この比較により、<xref:System.Security.Permissions.ReflectionPermissionFlag.RestrictedMemberAccess?displayProperty=nameWithType> によって、信頼されたコードのセキュリティを損なうことなく、部分的に信頼されたコードが、部分的に信頼された他のコードの参照範囲チェックをスキップできるようにする方法を示します。

### <a name="code"></a>コード

[!code-csharp[HowToEmitCodeInPartialTrust#1](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEmitCodeInPartialTrust/cs/source.cs#1)]
[!code-vb[HowToEmitCodeInPartialTrust#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEmitCodeInPartialTrust/vb/source.vb#1)]

## <a name="compiling-the-code"></a>コードのコンパイル

- このコード例を Visual Studio でビルドする場合は、クラスを <xref:System.AppDomain.CreateInstanceAndUnwrap%2A> メソッドに渡すときに、名前空間を含むようにクラスの名前を変更する必要があります。 既定では、名前空間がプロジェクト名になります。 たとえば、プロジェクト名が "PartialTrust" である場合、クラス名は "PartialTrust.Worker" にする必要があります。

## <a name="see-also"></a>関連項目

- [リフレクション出力のセキュリティ関連事項](security-issues-in-reflection-emit.md)
- [方法 : サンドボックスで部分信頼コードを実行する](../misc/how-to-run-partially-trusted-code-in-a-sandbox.md)
