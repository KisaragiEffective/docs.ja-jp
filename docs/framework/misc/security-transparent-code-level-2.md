---
title: セキュリティ透過コード、レベル 2
ms.date: 03/30/2017
helpviewer_keywords:
- transparency
- level 2 transparency
- security-transparent code
- security-critical code
ms.assetid: 4d05610a-0da6-4f08-acea-d54c9d6143c0
ms.openlocfilehash: 7ac5660c2c431505f4992f5e687974c2b9d06672
ms.sourcegitcommit: 9c54866bcbdc49dbb981dd55be9bbd0443837aa2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/14/2020
ms.locfileid: "77216994"
---
# <a name="security-transparent-code-level-2"></a>セキュリティ透過コード、レベル 2

[!INCLUDE[net_security_note](../../../includes/net-security-note-md.md)]

レベル2の透過性は .NET Framework 4 で導入されました。 このモデルには、透過的なコード、セキュリティ セーフ クリティカル コード、およびセキュリティ クリティカル コードの 3 つの基本思想があります。

- 透過的なコード (完全に信頼されて実行されているコードを含む) は、他の透過的なコードまたはセキュリティ セーフ クリティカル コードのみを呼び出すことができます。 また、ドメインの部分信頼アクセス許可セット (存在する場合) で許可されるアクションのみを実行できます。 透過的なコードでは、次のアクションは実行できません。

  - 特権の <xref:System.Security.CodeAccessPermission.Assert%2A> または昇格を実行する。

  - アンセーフ コードまたは検証不可能なコードを含める。

  - クリティカル コードを直接呼び出す。

  - ネイティブ コードまたは <xref:System.Security.SuppressUnmanagedCodeSecurityAttribute> 属性を持つコードを呼び出す。

  - <xref:System.Security.Permissions.SecurityAction.LinkDemand> によって保護されているメンバーを呼び出す。

  - クリティカルな型を継承する。

  また、透過的メソッドでは、クリティカルな仮想メソッドをオーバーライドしたりクリティカルなインターフェイス メソッドを実装したりすることはできません。

- セーフ クリティカル コードは完全に信頼されていますが、透過的なコードから呼び出すことができます。 また、完全に信頼されたコードのうち限られた領域のみを公開します。正確性とセキュリティの検証はセーフ クリティカル コード内で実行されます。

- セキュリティ クリティカル コードは任意のコードを呼び出すことができ、完全に信頼されていますが、透過的なコードから呼び出すことはできません。

## <a name="usage-examples-and-behaviors"></a>使用例と動作

.NET Framework 4 つの規則 (レベル2の透過性) を指定するには、アセンブリに次の注釈を使用します。

```csharp
[assembly: SecurityRules(SecurityRuleSet.Level2)]
```

.NET Framework 2.0 のルール (レベル 1 の透過性) にロックするには、次の注釈に使用します。

```csharp
[assembly: SecurityRules(SecurityRuleSet.Level1)]
```

アセンブリに注釈を設定しない場合は、既定で .NET Framework 4 ルールが使用されます。 ただし、推奨されるベストプラクティスは、既定に応じてではなく、<xref:System.Security.SecurityRulesAttribute> 属性を使用することです。

### <a name="assembly-wide-annotation"></a>アセンブリ全体の注釈

アセンブリ レベルでの属性の使用には、次の規則が適用されます。

- 属性なし: 属性を指定しない場合は、ランタイムによってすべてのコードがセキュリティ クリティカルとして解釈されます。ただし、セキュリティ クリティカルであると継承規則に違反する場合 (たとえば、透過的な仮想メソッドまたはインターフェイス メソッドをオーバーライドまたは実装する場合) は除きます。 そのような場合、メソッドはセーフ クリティカルになります。 属性を指定しない場合、透過性規則は共通言語ランタイムによって自動的に判断されます。

- `SecurityTransparent`: すべてのコードが透過的になります。アセンブリ全体で特権の必要な操作や安全でない操作は実行されません。

- `SecurityCritical`: このアセンブリ内の型によって導入されるすべてのコードはクリティカルになり、その他のすべてのコードは透過的になります。 このシナリオは属性を指定しない場合に似ていますが、共通言語ランタイムによって透過性規則が自動的に判断されることはありません。 たとえば、仮想メソッドまたは抽象メソッドをオーバーライドしたり、インターフェイス メソッドを実装したりする場合、既定では、そのメソッドは透過的になります。 そのメソッドには `SecurityCritical` または `SecuritySafeCritical` の注釈を明示的に付ける必要があります。そうしないと、読み込み時に <xref:System.TypeLoadException> がスローされます。 この規則は、基底クラスと派生クラスの両方が同じアセンブリ内にある場合にも適用されます。

- `AllowPartiallyTrustedCallers` (レベル 2 のみ): すべてのコードが既定で透過的になります。 ただし、個々の型やメンバーに他の属性を設定することもできます。

次の表は、レベル2のアセンブリレベルの動作とレベル1を比較したものです。

|Assembly 属性|[レベル 2]|[レベル 1]|
|------------------------|-------------|-------------|
|部分的に信頼されたアセンブリで属性なし|型およびメンバーは既定で透過的になりますが、セキュリティ クリティカルまたはセキュリティ セーフ クリティカルにすることもできます。|すべての型およびメンバーは透過的です。|
|属性なし|属性を指定しない場合、透過性規則は共通言語ランタイムによって自動的に判断されます。 すべての型およびメンバーはセキュリティ クリティカルになります (ただし、セキュリティ クリティカルになると継承ルールに違反する場所を除く)。|完全に信頼されたアセンブリ (グローバル アセンブリ キャッシュ内に存在するか、`AppDomain` で完全に信頼と指定されている) では、すべての型は透過的であり、すべてのメンバーはセキュリティ セーフ クリティカルです。|
|`SecurityTransparent`|すべての型およびメンバーは透過的です。|すべての型およびメンバーは透過的です。|
|`SecurityCritical(SecurityCriticalScope.Everything)`|該当しない。|すべての型およびメンバーはセキュリティ クリティカルです。|
|`SecurityCritical`|このアセンブリ内の型によって導入されるすべてのコードはクリティカルになり、その他のすべてのコードは透過的になります。 仮想メソッドまたは抽象メソッドをオーバーライドしたり、インターフェイス メソッドを実装したりする場合は、メソッドに `SecurityCritical` または `SecuritySafeCritical` の注釈を明示的に付ける必要があります。|すべてのコードは既定で透過的になります。 ただし、個々の型やメンバーに他の属性を設定することもできます。|

### <a name="type-and-member-annotation"></a>型およびメンバーの注釈

型に適用されるセキュリティ属性は、その型によって導入されるメンバーにも適用されます。 ただし、基底クラスまたはインターフェイスの実装の仮想オーバーライドや抽象オーバーライドには適用されません。 型およびメンバーのレベルでの属性の使用には、次の規則が適用されます。

- `SecurityCritical`: 型またはメンバーはクリティカルで、完全に信頼されているコードからのみ呼び出すことができます。 セキュリティ クリティカルな型で導入されるメソッドはクリティカルです。

    > [!IMPORTANT]
    > 基底クラスまたはインターフェイスで導入されてセキュリティ クリティカルなクラスでオーバーライドまたは実装される仮想メソッドおよび抽象メソッドは、既定では透過的です。 これらのメソッドは、`SecuritySafeCritical` または `SecurityCritical` のいずれかとして指定する必要があります。

- `SecuritySafeCritical`: 型またはメンバーはセーフ クリティカルです。 ただし、型またはメンバーは透過的な (部分的に信頼された) コードから呼び出すことができ、機能的に他のクリティカル コードと同等になります。 コードはセキュリティ監査を受ける必要があります。

## <a name="override-patterns"></a>オーバーライドのパターン

レベル 2 の透過性で使用できるメソッドのオーバーライドを次の表に示します。

|基底クラスの仮想/インターフェイス メンバー|オーバーライド/インターフェイス|
|------------------------------------|-------------------------|
|`Transparent`|`Transparent`|
|`Transparent`|`SafeCritical`|
|`SafeCritical`|`Transparent`|
|`SafeCritical`|`SafeCritical`|
|`Critical`|`Critical`|

## <a name="inheritance-rules"></a>継承規則

このセクションでは、アクセスおよび機能に基づいて、次の順序が `Transparent`、`Critical`、および `SafeCritical` のコードに割り当てられています。

`Transparent` < `SafeCritical` < `Critical`

- 型の規則: 左から右に向かってアクセス制限がより厳しくなります。 派生型は、基本型と少なくとも同じ程度に制限する必要があります。

- メソッドの規則。基底メソッドのアクセシビリティを派生メソッドで変更することはできません。 既定の動作では、注釈のない派生メソッドはすべて `Transparent` になります。 クリティカルな型の派生型では、オーバーライドしたメソッドに `SecurityCritical` の注釈が明示的に付けられていない場合に、例外がスローされます。

次の表に、使用できる型の継承パターンを示します。

|[基底クラス]|派生クラスで可能なレベル|
|----------------|--------------------------|
|`Transparent`|`Transparent`|
|`Transparent`|`SafeCritical`|
|`Transparent`|`Critical`|
|`SafeCritical`|`SafeCritical`|
|`SafeCritical`|`Critical`|
|`Critical`|`Critical`|

次の表に、使用できない型の継承パターンを示します。

|[基底クラス]|派生クラスで不可のレベル|
|----------------|-----------------------------|
|`SafeCritical`|`Transparent`|
|`Critical`|`Transparent`|
|`Critical`|`SafeCritical`|

次の表に、使用できるメソッドの継承パターンを示します。

|基底メソッド|派生メソッドで可能なレベル|
|-----------------|---------------------------|
|`Transparent`|`Transparent`|
|`Transparent`|`SafeCritical`|
|`SafeCritical`|`Transparent`|
|`SafeCritical`|`SafeCritical`|
|`Critical`|`Critical`|

次の表に、使用できないメソッドの継承パターンを示します。

|基底メソッド|派生メソッドで不可のレベル|
|-----------------|------------------------------|
|`Transparent`|`Critical`|
|`SafeCritical`|`Critical`|
|`Critical`|`Transparent`|
|`Critical`|`SafeCritical`|

> [!NOTE]
> これらの継承規則は、レベル 2 の型およびメンバーに適用されます。 レベル 1 アセンブリ内の型は、レベル 2 のセキュリティ クリティカルな型およびメンバーから継承できます。 したがって、レベル 2 の型およびメンバーでは、レベル 1 の継承先に対して個別の継承確認要求が必要です。

## <a name="additional-information-and-rules"></a>追加情報と規則

### <a name="linkdemand-support"></a>LinkDemand のサポート

レベル 2 の透過性モデルでは、<xref:System.Security.Permissions.SecurityAction.LinkDemand> は <xref:System.Security.SecurityCriticalAttribute> 属性に置き換えられます。 レガシ (レベル 1) コードでは、<xref:System.Security.Permissions.SecurityAction.LinkDemand> は自動的 <xref:System.Security.Permissions.SecurityAction.Demand> として扱われます。

### <a name="reflection"></a>リフレクション

クリティカル メソッドを呼び出したりクリティカル フィールドを読み取ったりすると、完全信頼の確認要求がトリガーされます (プライベート メソッドまたはプライベート フィールドを呼び出す場合と同様)。 したがって、完全に信頼されているコードではクリティカル メソッドを呼び出すことができるのに対し、部分的に信頼されたコードでは呼び出すことができません。

<xref:System.Reflection> 名前空間に追加された `SecurityCritical`、`SecuritySafeCritical`、および `SecurityTransparent` の各プロパティを使用すると、型、メソッド、またはフィールドが <xref:System.Type.IsSecurityCritical%2A>、<xref:System.Reflection.MethodBase.IsSecuritySafeCritical%2A>、または <xref:System.Reflection.MethodBase.IsSecurityTransparent%2A> のどれであるかを判断できます。 属性の存在を確認する代わりに、リフレクションを使用して透過性を判断するには、これらのプロパティを使用します。 透過性の規則は複雑なので、属性のチェックでは不十分なことがあります。

> [!NOTE]
> `SafeCritical` メソッドは、<xref:System.Type.IsSecurityCritical%2A> と <xref:System.Reflection.MethodBase.IsSecuritySafeCritical%2A>の両方に対して `true` を返します。これは、`SafeCritical` は実際には重要です (クリティカルコードと同じ機能を持ちますが、透過的なコードから呼び出すことができます)。

動的メソッドは、関連付けられているモジュールの透過性を継承します。型の透過性は継承しません (型に関連付けられている場合)。

### <a name="skip-verification-in-full-trust"></a>完全な信頼での検証のスキップ

完全に信頼されている透過的なアセンブリの検証をスキップするには、<xref:System.Security.SecurityRulesAttribute.SkipVerificationInFullTrust%2A> 属性の `true` プロパティを <xref:System.Security.SecurityRulesAttribute> に設定します。

`[assembly: SecurityRules(SecurityRuleSet.Level2, SkipVerificationInFullTrust = true)]`

<xref:System.Security.SecurityRulesAttribute.SkipVerificationInFullTrust%2A> プロパティは既定では `false` であるため、検証をスキップするにはこのプロパティを `true` に設定する必要があります。 これをするのは、最適化のためだけにしてください。 [PEVerify ツール](../tools/peverify-exe-peverify-tool.md)の `transparent` オプションを使用して、アセンブリ内の透過的なコードが検証可能であることを確認する必要があります。

## <a name="see-also"></a>参照

- [透過的セキュリティコード、レベル1](security-transparent-code-level-1.md)
- [セキュリティの変更](../security/security-changes.md)
