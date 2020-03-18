---
title: .NET Framework アナライザー - .NET
description: .NET Framework Analyzer パッケージの .NET Framework Analyzers を利用し、セキュリティ上のリスクを見つけ、対処する方法について説明します。
author: billwagner
ms.author: wiwagn
ms.date: 01/25/2018
ms.technology: dotnet-standard
ms.openlocfilehash: dd69671e709549fe0ad0f582e4d09b43f7321df2
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "78155998"
---
# <a name="the-net-framework-analyzer"></a>.NET Framework Analyzer

.NET Framework Analyzer を利用し、.NET Framework 基盤のアプリケーション コードに潜在する問題を発見できます。 このアナライザーは潜在的な問題を検出し、改善策を提案します。

コードを書くとき、あるいは CI ビルドの一部として、このアナライザーは Visual Studio で対話的に動作します。 開発のできるだけ早い段階で、プロジェクトにアナライザーを追加してください。 コードに潜む問題を見つけるのが早ければ早いほど、修正が簡単になります。 ただし、開発サイクルのどの段階でも追加できます。 開発者は開発を続行しながら、アナライザーは既存コードの問題を見つけ、新しい問題に関する警告を出します。

## <a name="installing-and-configuring-the-net-framework-analyzer"></a>.NET Framework Analyzer をインストールし、構成する

.NET Framework Analyzer は、それを実行するすべてのプロジェクトで、NuGet パッケージとしてインストールする必要があります。 1 人の開発者がそれをプロジェクトに追加すれば十分です。 アナライザー パッケージはプロジェクト依存関係であり、更新済みのソリューションが与えられると、あらゆる開発者のコンピューターで実行されます。

.NET Framework Analyzer は [Microsoft.NetFramework.Analyzers](https://www.nuget.org/packages/Microsoft.NetFramework.Analyzers/) NuGet パッケージで配布されます。 このパッケージは、.NET Framework に固有のアナライザーのみ提供します。これにはセキュリティ アナライザーが含まれています。 多くの場合、[Microsoft.CodeAnalysis.FxCopAnalyzers](https://www.nuget.org/packages/Microsoft.CodeAnalysis.FxCopAnalyzers) NuGet パッケージが必要になります。
FxCopAnalyzers 集合パッケージには、Framework.Analyzers パッケージに含まれているすべてのフレームワーク アナライザーと次のアナライザーが含まれています。

- [Microsoft.CodeQuality.Analyzers](https://www.nuget.org/packages/Microsoft.CodeQuality.Analyzers): .NET Standard API の一般的なガイダンスを提供します。
- [Microsoft.NetCore.Analyzers](https://www.nuget.org/packages/Microsoft.NetCore.Analyzers): アナライザー固有の .NET Core API を提供します。
- [Text.Analyzers](https://www.nuget.org/packages/Text.Analyzers): コメントなど、コードとして含まれているテキストのガイダンスを提供します。

これをインストールするには、プロジェクトを右クリックし、"依存関係の管理" を選択します。
NuGet エクスプローラーから、"NetFramework Analyzer" を探します。あるいは、"Fx Cop Analyzer" がよければそれを探します。 ソリューションのすべてのプロジェクトで安定した最新のバージョンをインストールします。

## <a name="using-the-net-framework-analyzer"></a>.NET Framework Analyzer を使用する

NuGet パッケージがインストールされたら、ソリューションをビルドします。 アナライザーは、コードベースで見つかった問題を報告します。 問題は Visual Studio の [エラー一覧] ウィンドウに警告として表示されます。次の画像をご覧ください。

![フレームワーク アナライザーによって報告される問題](./media/framework-analyzers-2.png)

コードを記述しているとき、コードに潜在的な問題があれば、その部分に波線が表示されます。
問題の上にカーソルを合わせると、問題の詳細と考えられる修正案が表示されます。次の画像をご覧ください。

![フレームワーク アナライザーが見つけた問題の対話型報告](./media/framework-analyzers-1.png)

アナライザーはソリューションのコードを調べ、問題があれば警告の一覧を出します。

### <a name="ca1058-types-should-not-extend-certain-base-types"></a>CA1058: 型は、一定の基本型を拡張することはできません

.NET Framework には、直接の派生元にしてはいけない型がいくつかあります。

**カテゴリ:** デザイン

**重要度:** 警告

追加情報: [CA1058: 型は、一定の基本型を拡張することはできません](/visualstudio/code-quality/ca1058-types-should-not-extend-certain-base-types)

### <a name="ca2153-do-not-catch-corrupted-state-exceptions"></a>CA2153: 破損状態例外はキャッチしないでください

破損状態例外をキャッチすると、エラー (アクセス違反など) が隠され、結果的に実行状態に一貫性がなくなり、攻撃者にとってシステムが攻撃しやすくなります。 その代わりに、より具体的な例外型セットをキャッチして処理するか、例外を再スローします。

**カテゴリ:** セキュリティ

**重要度:** 警告

追加情報: [## CA2153: 破損状態例外はキャッチしないでください](/visualstudio/code-quality/ca2153-avoid-handling-corrupted-state-exceptions)

### <a name="ca2229-implement-serialization-constructors"></a>CA2229: シリアル化コンストラクターを実装します

アナライザーは、<xref:System.Runtime.Serialization.ISerializable> インターフェイスを実装する型を作成すると、この警告を出します。ただし、必須のシリアル化コンストラクターを定義しません。 この規則違反を修正するには、シリアル化コンストラクターを実装します。 シールされたクラスの場合、コンストラクターをプライベートにするか、プロテクトにします。 シリアル化コンストラクターには次のシグネチャがあります。

```csharp
public class MyItemType
{
    // The special constructor is used to deserialize values.
    public MyItemType(SerializationInfo info, StreamingContext context)
    {
        // implementation removed.
    }
}
```

**カテゴリ:** 使用

**重要度:** 警告

追加情報: [CA2229: シリアル化コンストラクターを実装します](/visualstudio/code-quality/ca2229-implement-serialization-constructors)

### <a name="ca2235-mark-all-non-serializable-fields"></a>CA2235: すべてのシリアル化不可能なフィールドを設定します

シリアル化できない型のインスタンス フィールドが、シリアル化できる型で宣言されています。 この警告を解決するには、そのフィールドを <xref:System.NonSerializedAttribute> で明示的にマークする必要があります。

**カテゴリ:** 使用

**重要度:** 警告

追加情報: [CA2235: すべてのシリアル化不可能なフィールドを設定します](/visualstudio/code-quality/ca2235-mark-all-non-serializable-fields)

### <a name="ca2237-mark-iserializable-types-with-serializable"></a>CA2237: ISerializable 型を Serializable に設定します

型が共通言語ランタイムでシリアル化できると認識されるようにするには、型を <xref:System.SerializableAttribute> 属性でマークする必要があります。型が <xref:System.Runtime.Serialization.ISerializable> インターフェイスの実装を通じてカスタムのシリアル化ルーチンを使用している場合でも、マークする必要があります。

**カテゴリ:** 使用

**重要度:** 警告

追加情報: [CA2237: ISerializable 型を Serializable に設定します](/visualstudio/code-quality/ca2237-mark-iserializable-types-with-serializableattribute)

### <a name="ca3075-insecure-dtd-processing-in-xml"></a>CA3075: XML の安全ではない DTD の処理

安全ではない <xref:System.Xml.XmlReaderSettings.DtdProcessing%2A> インスタンスを使用する場合、または外部エンティティ ソースを参照する場合、パーサーは信頼されていない入力を受け入れ、攻撃者に機密情報を漏えいしてしまう可能性があります。  

**カテゴリ:** セキュリティ

**重要度:** 警告

追加情報: [A3075: XML の安全ではない DTD 処理](/visualstudio/code-quality/ca2237-mark-iserializable-types-with-serializableattribute)

### <a name="ca5350-do-not-use-weak-cryptographic-algorithms"></a>CA5350: 脆弱な暗号アルゴリズムを使用しないでください

時間の経過と共に攻撃が高度になり、暗号アルゴリズムが弱体化します。 この暗号アルゴリズムの型と用途によっては、暗号の力がさらに弱まり、攻撃者が暗号化メッセージを読んだり、改ざんしたり、電子署名を偽造したり、ハッシュしたコンテンツを改ざんしたり、その他の面でこのアルゴリズムを基礎とする暗号システムを攻撃したりすることを許します。 暗号化には、AES アルゴリズムを利用します (AES-256、AES-192、AES-128 が許容されます)。キーの長さは 128 ビット以上にします。 ハッシュには、SHA-2 512、SHA-2 384、SHA-2 256 などの SHA-2 群のハッシュ関数を利用します。

**カテゴリ:** セキュリティ

**重要度:** 警告

追加情報: [CA5350: 脆弱な暗号アルゴリズムを使用しないでください](/visualstudio/code-quality/ca5350-do-not-use-weak-cryptographic-algorithms)

### <a name="ca5351-do-not-use-broken-cryptographic-algorithms"></a>CA5351 破られた暗号アルゴリズムを使用しないでください

コンピューターを利用してこのアルゴリズムを突破する攻撃が存在します。 攻撃者は、本来守られるはずの暗号を突破できます。 この暗号アルゴリズムの型と用途によっては、攻撃者が暗号化メッセージを読んだり、改ざんしたり、電子署名を偽造したり、ハッシュしたコンテンツを改ざんしたり、その他の面でこのアルゴリズムを基礎とする暗号システムを攻撃したりすることを許します。 暗号化には、AES アルゴリズムを利用します (AES-256、AES-192、AES-128 が許容されます)。キーの長さは 128 ビット以上にします。 ハッシュには、SHA512、SHA384、SHA256 などの SHA-2 群のハッシュ関数を利用します。 デジタル署名には、RSA か ECDSA を利用します。RSA の場合、キーの長さを 2048 ビット以上にします。ECDSA の場合、キーの長さを 256 ビット以上にします。

**カテゴリ:** セキュリティ

**重要度:** 警告

追加情報: [CA5351: 破られた暗号アルゴリズムを使用しないでください](/visualstudio/code-quality/ca5351)
