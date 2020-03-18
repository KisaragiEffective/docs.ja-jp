---
title: 複数のプラットフォームを対象とするライブラリのアプリケーション リソース
ms.date: 07/18/2018
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- multiple platforms, resources for
- resources [.NET Framework]
- .NET Framework, resources when targeting multiple platforms
- resources, for multiple platforms
- targeting multiple platforms, resources for
ms.assetid: 72c76f0b-7255-4576-9261-3587f949669c
ms.openlocfilehash: 3bf475117a85c2fced260dcc9460d55cd7007277
ms.sourcegitcommit: 011314e0c8eb4cf4a11d92078f58176c8c3efd2d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/11/2020
ms.locfileid: "77123663"
---
# <a name="app-resources-for-libraries-that-target-multiple-platforms"></a>複数のプラットフォームを対象とするライブラリのアプリケーション リソース
.NET Framework[ポータブルクラスライブラリ](../../../docs/standard/cross-platform/cross-platform-development-with-the-portable-class-library.md)のプロジェクトタイプを使用して、クラスライブラリ内のリソースに複数のプラットフォームからアクセスできるようにすることができます。 このプロジェクトの種類は Visual Studio 2012 で使用でき、.NET Framework クラスライブラリの移植可能なサブセットを対象とします。 ポータブルクラスライブラリを使用すると、デスクトップアプリ、Silverlight アプリ、Windows Phone アプリ、Windows 8.x ストアアプリからライブラリにアクセスできるようになります。

[!INCLUDE[standard](../../../includes/pcl-to-standard.md)]

 ポータブルクラスライブラリプロジェクトでは、アプリケーションで使用可能な <xref:System.Resources> 名前空間の型のサブセットはごく限られていますが、<xref:System.Resources.ResourceManager> クラスを使用してリソースを取得することもできます。 ただし、Visual Studio を使用してアプリケーションを作成する場合は、<xref:System.Resources.ResourceManager> クラス ライブラリを使用するのではなく、Visual Studio によって作成された厳密に型指定されたラッパーを使用する必要があります。

 Visual Studio で厳密に型指定されたラッパーを作成するには、Visual Studio リソースデザイナーでメインリソースファイルの**アクセス修飾子**を**パブリック**に設定します。 これにより、厳密に型指定された ResourceManager ラッパーを含む [resourceFileName].designer.cs または [resourceFileName].designer.vb ファイルが作成されます。 厳密に型指定されたリソースラッパーの使用の詳細については、「 [resgen.exe (リソースファイルジェネレーター)](../../../docs/framework/tools/resgen-exe-resource-file-generator.md) 」トピックの「厳密に型指定されたリソースクラスの生成」セクションを参照してください。

## <a name="resource-manager-in-the-portable-class-library"></a>ポータブルクラスライブラリの Resource Manager
 ポータブルクラスライブラリプロジェクトでは、リソースへのすべてのアクセスは、<xref:System.Resources.ResourceManager> クラスによって処理されます。 <xref:System.Resources.ResourceReader> や <xref:System.Resources.ResourceSet>などの <xref:System.Resources> 名前空間の型は、ポータブルクラスライブラリプロジェクトからアクセスできないため、リソースにアクセスするために使用することはできません。

 ポータブルクラスライブラリプロジェクトには、次の表に示す4つの <xref:System.Resources.ResourceManager> メンバーが含まれています。 これらのコンストラクターおよびメソッドを使用すると、<xref:System.Resources.ResourceManager> オブジェクトをインスタンス化して文字列リソースを取得できます。

|`ResourceManager` のメンバー|[説明]|
|------------------------------|-----------------|
|<xref:System.Resources.ResourceManager.%23ctor%28System.String%2CSystem.Reflection.Assembly%29>|<xref:System.Resources.ResourceManager> インスタンスを作成し、指定されたアセンブリ内にある、名前の指定されたリソース ファイルにアクセスします。|
|<xref:System.Resources.ResourceManager.%23ctor%28System.Type%29>|指定された型に対応する <xref:System.Resources.ResourceManager> インスタンスを作成します。|
|<xref:System.Resources.ResourceManager.GetString%28System.String%29>|現在のカルチャに対する、名前の指定されたリソースを取得します。|
|<xref:System.Resources.ResourceManager.GetString%28System.String%2CSystem.Globalization.CultureInfo%29>|指定されたカルチャに属する、名前の指定されたリソースを取得します。|

 ポータブルクラスライブラリから他の <xref:System.Resources.ResourceManager> メンバーを除外することは、シリアル化されたオブジェクト、文字列以外のデータ、およびイメージをリソースファイルから取得できないことを意味します。 ポータブルクラスライブラリのリソースを使用するには、すべてのオブジェクトデータを文字列形式で格納する必要があります。 たとえば、数値を文字列に変換してリソース ファイルに格納し、これらを取得して、数値データ型の `Parse` メソッドまたは `TryParse` メソッドを使用して数値に戻すことができます。 イメージや他のバイナリ データを文字列形式に変換するには <xref:System.Convert.ToBase64String%2A?displayProperty=nameWithType> メソッドを呼び出し、バイト配列に戻すには <xref:System.Convert.FromBase64String%2A?displayProperty=nameWithType> メソッドを呼び出します。

## <a name="the-portable-class-library-and-windows-store-apps"></a>ポータブルクラスライブラリと Windows ストアアプリ
 ポータブルクラスライブラリプロジェクトでは、リソースが .resx ファイルに格納されます。このファイルは .resources ファイルにコンパイルされ、コンパイル時にメインアセンブリまたはサテライトアセンブリに埋め込まれます。 一方、Windows 8.x ストアアプリでは、リソースを resw ファイルに格納する必要があります。これは、1つのパッケージリソースインデックス (PRI) ファイルにコンパイルされます。 ただし、互換性のないファイル形式にかかわらず、ポータブルクラスライブラリは Windows 8.x ストアアプリで動作します。

 Windows 8.x ストアアプリからクラスライブラリを使用するには、Windows ストアアプリプロジェクトでそのクラスライブラリへの参照を追加します。 Visual Studio は、アセンブリから. resw ファイルにリソースを透過的に抽出し、それを使用して Windows ランタイムがリソースを抽出できる PRI ファイルを生成します。 実行時には、Windows ランタイムはポータブルクラスライブラリのコードを実行しますが、このクラスは、PRI ファイルからポータブルクラスライブラリのリソースを取得します。

 ポータブルクラスライブラリプロジェクトにローカライズされたリソースが含まれている場合は、デスクトップアプリのライブラリの場合と同様に、ハブアンドスポークモデルを使用してそれらをデプロイします。 メインのリソースファイルと、Windows 8.x ストアアプリのローカライズされたリソースファイルを使用するには、メインアセンブリへの参照を追加します。 コンパイル時に、Visual Studio はメイン リソース ファイルとローカライズされているリソース ファイルからリソースを個別の .resw ファイルに抽出します。 次に、実行時に Windows ランタイムがアクセスする単一の PRI ファイルに、resw ファイルをコンパイルします。

<a name="NonLoc"></a>
## <a name="example-non-localized-portable-class-library"></a>例: ローカライズされていないポータブルクラスライブラリ
 次の単純なローカライズされていないポータブルクラスライブラリの例では、リソースを使用して列の名前を格納し、表形式データ用に予約する文字数を決定します。 この例では LibResources.resx という名前のファイルを使用して、次の表に示す文字列リソースを格納します。

|リソース名|リソースの値|
|-------------------|--------------------|
|Born|Birthdate|
|BornLength|12|
|Hired|Hire Date|
|HiredLength|12|
|id|id|
|ID.Length|12|
|Name|Name|
|NameLength|25|
|タイトル|従業員データベース|

 次のコードは、ファイルの**アクセス修飾子**が**Public**に変更されたときに Visual Studio によって生成される `resources` という名前のリソースマネージャーラッパーを使用する `UILibrary` クラスを定義します。 UILibrary クラスは必要に応じて文字列データを解析します。 。 このクラスは `MyCompany.Employees` 名前空間にあることに注意してください。

 [!code-csharp[Conceptual.Resources.Portable#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.resources.portable/cs/uilibrary.cs#1)]
 [!code-vb[Conceptual.Resources.Portable#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.resources.portable/vb/uilibrary.vb#1)]

 次のコードは、`UILibrary` クラスとそのリソースをコンソールモード アプリケーションからアクセスする方法を示します。 コンソールアプリプロジェクトに追加するには、.dll への参照が必要です。

 [!code-csharp[Conceptual.Resources.Portable#2](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.resources.portable/cs/program.cs#2)]
 [!code-vb[Conceptual.Resources.Portable#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.resources.portable/vb/module1.vb#2)]

 次のコードは、`UILibrary` クラスとそのリソースを Windows 8.x ストアアプリからアクセスする方法を示しています。 Windows ストアアプリプロジェクトに追加するには、.dll への参照が必要です。

 [!code-csharp[Conceptual.Resources.PortableMetro#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.resources.portablemetro/cs/blankpage.xaml.cs#1)]

## <a name="example-localized-portable-class-library"></a>例: ローカライズされたポータブルクラスライブラリ
 次のローカライズされたポータブルクラスライブラリの例には、フランス語 (フランス) と英語 (米国) のカルチャのリソースが含まれています。 英語 (米国) カルチャは、アプリの既定のカルチャです。そのリソースについては、前の[セクション](../../../docs/standard/cross-platform/app-resources-for-libraries-that-target-multiple-platforms.md#NonLoc)の表を参照してください。 フランス語 (フランス) のカルチャのリソース ファイルは LibResources.fr-FR.resx という名前で、以下の表に示す文字列リソースで構成されています。 `UILibrary` クラスのソース コードは前のセクションに示すものと同じです。

|リソース名|リソースの値|
|-------------------|--------------------|
|Born|Date de naissance|
|BornLength|20|
|Hired|Date embauché|
|HiredLength|16|
|id|id|
|Name|Nom|
|タイトル|Base de données des employés|

 次のコードは、`UILibrary` クラスとそのリソースをコンソールモード アプリケーションからアクセスする方法を示します。 コンソールアプリプロジェクトに追加するには、.dll への参照が必要です。

 [!code-csharp[Conceptual.Resources.Portable#3](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.resources.portable/cs/program2.cs#3)]
 [!code-vb[Conceptual.Resources.Portable#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.resources.portable/vb/module2.vb#3)]

 次のコードは、`UILibrary` クラスとそのリソースを Windows 8.x ストアアプリからアクセスする方法を示しています。 Windows ストアアプリプロジェクトに追加するには、.dll への参照が必要です。 静的な `ApplicationLanguages.PrimaryLanguageOverride` プロパティを使用してアプリケーションの優先言語をフランス語に設定します。

 [!code-csharp[Conceptual.Resources.PortableMetroLoc#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.resources.portablemetroloc/cs/blankpage.xaml.cs#1)]
 [!code-vb[Conceptual.Resources.PortableMetroLoc#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.resources.portablemetroloc/vb/blankpage.xaml.vb#1)]  
  
## <a name="see-also"></a>参照

- <xref:System.Resources.ResourceManager>
- [デスクトップ アプリケーションのリソース](../../../docs/framework/resources/index.md)
- [リソースのパッケージ化と配置](../../../docs/framework/resources/packaging-and-deploying-resources-in-desktop-apps.md)
