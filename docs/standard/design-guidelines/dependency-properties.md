---
title: 依存関係プロパティ
ms.date: 10/22/2008
ms.technology: dotnet-standard
ms.assetid: 212cfb1e-cec4-4047-94a6-47209b387f6f
ms.openlocfilehash: e1372b75cb6501f8bc3fec913364e9d80157ad83
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76741721"
---
# <a name="dependency-properties"></a>依存関係プロパティ
依存関係プロパティ (DP) は、など、型の変数 (field) に格納するのではなく、プロパティストアにその値を格納する通常のプロパティです。

 アタッチされた依存関係プロパティは、静的な Get メソッドと Set メソッドとしてモデル化された依存関係プロパティの一種で、オブジェクトとそのコンテナーの間のリレーションシップ (`Panel` コンテナー上の `Button` オブジェクトの位置など) を示す "プロパティ" を表します。

 スタイル設定、トリガー、データバインディング、アニメーション、動的リソース、継承などの WPF 機能をサポートするためにプロパティが必要な場合は、依存関係プロパティを指定✔️ます。

## <a name="dependency-property-design"></a>依存関係プロパティのデザイン
 依存関係プロパティを実装する場合は、✔️ <xref:System.Windows.DependencyObject>、またはそのサブタイプの1つを継承します。 型は、プロパティストアの非常に効率的な実装を提供し、WPF のデータバインディングを自動的にサポートします。

 ✔️は、依存関係プロパティごとに <xref:System.Windows.DependencyProperty?displayProperty=nameWithType> のインスタンスを格納する通常の CLR プロパティとパブリックの静的読み取り専用フィールドを提供します。

 <xref:System.Windows.DependencyObject.GetValue%2A?displayProperty=nameWithType> と <xref:System.Windows.DependencyObject.SetValue%2A?displayProperty=nameWithType>のインスタンスメソッドを呼び出すことによって、依存関係プロパティを実装✔️ます。

 ✔️プロパティの名前を "suffixing" にして、依存関係プロパティの静的フィールドに名前を付けます。

 コード内で依存関係プロパティの既定値を明示的に設定 ❌代わりに、メタデータに設定します。

 プロパティの既定値を明示的に設定した場合、スタイル設定などのいくつかの暗黙的な方法によってそのプロパティが設定されないようにすることができます。

 ❌ 静的フィールドにアクセスするために、標準コード以外のプロパティアクセサーにコードを配置しないでください。

 スタイル設定などの暗黙的な方法によってプロパティが設定されている場合、そのコードは実行されません。これは、スタイルが静的フィールドを直接使用するためです。

 セキュリティで保護されたデータを格納するために依存関係プロパティを使用しない ❌。 プライベート依存関係プロパティも、パブリックにアクセスできます。

## <a name="attached-dependency-property-design"></a>アタッチされた依存関係プロパティのデザイン
 前のセクションで説明した依存関係プロパティは、宣言する型の組み込みプロパティを表します。たとえば、`Text` プロパティは `TextButton`のプロパティであり、これを宣言します。 特別な種類の依存関係プロパティは、アタッチされる依存関係プロパティです。

 添付プロパティの典型的な例は、<xref:System.Windows.Controls.Grid.Column%2A?displayProperty=nameWithType> プロパティです。 プロパティは、ボタンの (グリッドではなく) 列の位置を表しますが、グリッドにボタンが含まれている場合にのみ関連し、グリッドによってボタンに "アタッチ" されます。

```xaml
<Grid>
    <Grid.ColumnDefinitions>
        <ColumnDefinition />
        <ColumnDefinition />
    </Grid.ColumnDefinitions>

    <Button Grid.Column="0">Click</Button>
    <Button Grid.Column="1">Clack</Button>
</Grid>
```

 添付プロパティの定義は、通常の依存関係プロパティとほとんど同じように見えますが、アクセサーは静的な Get メソッドと Set メソッドによって表される点が異なります。

```csharp
public class Grid {

    public static int GetColumn(DependencyObject obj) {
        return (int)obj.GetValue(ColumnProperty);
    }

    public static void SetColumn(DependencyObject obj, int value) {
        obj.SetValue(ColumnProperty,value);
    }

    public static readonly DependencyProperty ColumnProperty =
        DependencyProperty.RegisterAttached(
            "Column",
            typeof(int),
            typeof(Grid)
    );
}
```

## <a name="dependency-property-validation"></a>依存関係プロパティの検証
 プロパティは、多くの場合、検証と呼ばれるものを実装します。 検証ロジックは、プロパティの値を変更しようとしたときに実行されます。

 残念ながら、依存関係プロパティのアクセサーに任意の検証コードを含めることはできません。 代わりに、プロパティの登録時に依存関係プロパティの検証ロジックを指定する必要があります。

 ❌、プロパティのアクセサーに依存関係プロパティの検証ロジックを配置しません。 代わりに、検証コールバックを `DependencyProperty.Register` メソッドに渡します。

## <a name="dependency-property-change-notifications"></a>依存関係プロパティの変更通知
 ❌ 依存関係プロパティのアクセサーに変更通知ロジックを実装しません。 依存関係プロパティには、変更通知コールバックを <xref:System.Windows.PropertyMetadata>に提供することによって使用する必要がある、組み込みの変更通知機能があります。

## <a name="dependency-property-value-coercion"></a>依存関係プロパティ値の強制変換
 プロパティの強制変換は、プロパティの set アクセス操作子に渡された値が setter によって変更されたときに、プロパティストアが実際に変更される前に行われます。

 ❌ 依存関係プロパティのアクセサーに強制変換ロジックを実装しません。

 依存関係プロパティには組み込みの強制型変換機能があり、強制実行コールバックを `PropertyMetadata`に渡すことによって使用できます。

 *©2005、2009 Microsoft Corporation の部分。すべての権限が予約されています。*

 *2008 年 10 月 22 日に Microsoft Windows Development シリーズの一部として、Addison-Wesley Professional によって発行された、Krzysztof Cwalina および Brad Abrams による「[Framework Design Guidelines: Conventions, Idioms, and Patterns for Reusable .NET Libraries, 2nd Edition](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619)」 (フレームワーク デザイン ガイドライン: 再利用可能な .NET ライブラリの規則、用法、パターン、第 2 版) から Pearson Education, Inc. の許可を得て再印刷されています。*

## <a name="see-also"></a>参照

- [フレームワーク デザインのガイドライン](../../../docs/standard/design-guidelines/index.md)
- [共通デザイン パターン](../../../docs/standard/design-guidelines/common-design-patterns.md)
