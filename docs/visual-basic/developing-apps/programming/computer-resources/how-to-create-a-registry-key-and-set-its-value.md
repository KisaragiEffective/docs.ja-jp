---
title: '方法: レジストリ キーを作成し、その値を設定する'
ms.date: 07/20/2015
f1_keywords:
- RegistryKey.CreateSubKey
- RegistryKey.SetValue
helpviewer_keywords:
- registry keys [Visual Basic], creating
- registry [Visual Basic], adding values
- registry [Visual Basic], adding keys
- registry keys [Visual Basic], setting values
- examples [Visual Basic], registry
ms.assetid: d3e40f74-c283-480c-ab18-e5e9052cd814
ms.openlocfilehash: 459c4b3f971009ee4b6b669c55bc058db0826595
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "74349201"
---
# <a name="how-to-create-a-registry-key-and-set-its-value-in-visual-basic"></a>方法: レジストリ キーを作成し、その値を設定する (Visual Basic)

レジストリ キーを作成するには、`CreateSubKey` オブジェクトの `My.Computer.Registry` メソッドを使用します。

## <a name="procedure"></a>プロシージャ

### <a name="to-create-a-registry-key"></a>レジストリ キーを作成するには

- `CreateSubKey` メソッドを使用し、キーを入れるハイブとキー名を指定します。 パラメーター `Subkey` では、大文字と小文字は区別されません。 この例では、HKEY_CURRENT_USER の下にレジストリ キー `MyTestKey` を作成します。

    [!code-vb[VbResourceTasks#17](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbResourceTasks/VB/Class1.vb#17)]

### <a name="to-create-a-registry-key-and-set-a-value-in-it"></a>レジストリ キーを作成し、それに値を設定するには

1. `CreateSubkey` メソッドを使用し、キーを入れるハイブとキー名を指定します。 この例では、HKEY_CURRENT_USER の下にレジストリ キー `MyTestKey` を作成します。

    [!code-vb[VbResourceTasks#17](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbResourceTasks/VB/Class1.vb#17)]

2. 値は、`SetValue` メソッドを使用して設定します。 この例では、文字列値 "MyTestKeyValue" を "This is a test value" に設定します。

    [!code-vb[VbResourceTasks#14](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbResourceTasks/VB/Class1.vb#14)]

## <a name="example"></a>例

この例では、HKEY_CURRENT_USER の下にレジストリ キー `MyTestKey` を作成し、文字列値 `MyTestKeyValue` を `This is a test value` に設定します。

[!code-vb[VbResourceTasks#15](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbResourceTasks/VB/Class1.vb#15)]

## <a name="robust-programming"></a>信頼性の高いプログラミング

レジストリの構造を調べて、キーの適切な場所を見つけることができます。 たとえば、現在のユーザーの HKEY_CURRENT_USER\Software キーを開き、会社名のキーを作成できます。 その後で、会社名のキーにレジストリ値を追加します。

Web アプリケーションからレジストリを読み取る際、現在のユーザーは Web アプリケーションに実装されている認証と偽装によります。

ローカル コンピューター (<xref:Microsoft.Win32.Registry.CurrentUser>) よりもユーザー フォルダー (<xref:Microsoft.Win32.Registry.LocalMachine>) にデータを書き込む方が安全です。

レジストリの値を作成するときは、その値が既存の値である場合の処理を決めておく必要があります。 悪意のあるユーザーによって作成された別のプロセスが既に値を作成し、アクセス権を持っている可能性があります。 レジストリ値にデータを設定すると、そのデータを他のプロセスから利用できるようになります。 これを回避するには、<xref:Microsoft.Win32.RegistryKey.GetValue%2A> メソッドを使います。 このメソッドは、キーがまだ存在しない場合、`Nothing` を返します。

レジストリ キーがアクセス制御リスト (ACL: Access Control List) によって保護されていても、パスワードなど他人に知られたくないデータをプレーン テキストでレジストリに格納するのは危険です。

次の条件を満たす場合は、例外が発生する可能性があります。

- キーの名前が `Nothing` である場合 (<xref:System.ArgumentNullException>)。

- レジストリ キーを作成するためのアクセス許可がユーザーにない場合 (<xref:System.Security.SecurityException>)。

- キー名が 255 文字の制限を超えている場合 (<xref:System.ArgumentException>)。

- キーが閉じている場合 (<xref:System.IO.IOException>)。

- レジストリ キーが読み取り専用の場合 (<xref:System.UnauthorizedAccessException>)。

## <a name="net-framework-security"></a>.NET Framework のセキュリティ

このプロセスを実行するには、アセンブリに対して <xref:System.Security.Permissions.RegistryPermission> クラスで特権レベルが許可されている必要があります。 部分的に信頼されたコンテキストで実行している場合、プロセスは、特権がないために例外をスローする可能性があります。 同様に、ユーザーには、設定に対する作成や書き込みを行うための適切な ACL が必要です。 たとえば、コード アクセス セキュリティのアクセス許可を持つローカル アプリケーションには、オペレーティング システムのアクセス許可がない可能性があります。 詳しくは、「[コード アクセス セキュリティの基礎](../../../../framework/misc/code-access-security-basics.md)」をご覧ください。

## <a name="see-also"></a>参照

- <xref:Microsoft.VisualBasic.MyServices.RegistryProxy>
- <xref:Microsoft.VisualBasic.MyServices.RegistryProxy.CurrentUser%2A>
- <xref:Microsoft.Win32.RegistryKey.CreateSubKey%2A>
- [レジストリからの読み取りとレジストリへの書き込み](../../../../visual-basic/developing-apps/programming/computer-resources/reading-from-and-writing-to-the-registry.md)
- [コード アクセス セキュリティの基礎](../../../../framework/misc/code-access-security-basics.md)
