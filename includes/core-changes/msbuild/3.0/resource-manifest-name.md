---
ms.openlocfilehash: 16ee73bfc0ab33b04ea3e2fa6d0eec521a9b8634
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "78968182"
---
### <a name="resource-manifest-file-names"></a>リソース マニフェストのファイル名

.NET Core 3.0 以降、生成されるリソース マニフェストのファイル名が変更されています。

#### <a name="version-introduced"></a>導入されたバージョン

3.0

#### <a name="change-description"></a>変更の説明

.NET Core 3.0 より前は、[DependentUpon](/visualstudio/msbuild/common-msbuild-project-items#compile) メタデータが MSBuild プロジェクト ファイルのリソース ( *.resx*) ファイルに対して設定された場合、生成されるマニフェスト名は *Namespace.Classname.resources* でした。 [DependentUpon](/visualstudio/msbuild/common-msbuild-project-items#compile) が設定されなかった場合、生成されるマニフェスト名は *Namespace.Classname.FolderPathRelativeToRoot.Culture.resources* でした。

.NET Core 3.0 以降、たとえば Windows フォーム アプリで *.resx* ファイルが同じ名前のソース ファイルと併置される場合、リソース マニフェスト名は、ソース ファイル内の最初の型のフル ネームから生成されます。 たとえば、*Type.cs* が *Type.resx* と併置される場合、生成されるマニフェスト名は *Namespace.Classname.resources* になります。 ただし、 *.resx* ファイルの `EmbeddedResource` プロパティの属性を変更した場合、生成されるマニフェスト ファイル名は異なる場合があります。

- `EmbeddedResource` プロパティの `LogicalName` 属性が設定されている場合は、その値がリソース マニフェストのファイル名として使用されます。

  次に例を示します。

  ```xml
  <EmbeddedResource Include="X.resx" LogicalName="SomeName.resources" />
  -or-
  <EmbeddedResource Include="X.fr-FR.resx" LogicalName="SomeName.resources" />
  ```

  **生成されるリソース マニフェストのファイル名**:*SomeName.resources* ( *.resx* ファイル名、カルチャ、またはその他のメタデータには関係しません)。

- `LogicalName` は設定されていないが、`EmbeddedResource` プロパティの `ManifestResourceName` 属性が設定されている場合は、その値とファイル拡張子 *.resources* の組み合わせがリソース マニフェストのファイル名として使用されます。

  次に例を示します。

  ```xml
  <EmbeddedResource Include="X.resx" ManifestResourceName="SomeName" />
  -or-
  <EmbeddedResource Include="X.fr-FR.resx" ManifestResourceName="SomeName.fr-FR" />
  ```

  **生成されるリソース マニフェストのファイル名**:*SomeName.resources* または *SomeName.fr-FR.resources*。

- 前の規則が適用されず、`EmbeddedResource` 要素の `DependentUpon` 属性がソース ファイルに設定されている場合、ソース ファイル内の最初のクラスの型名がリソース マニフェストのファイル名の中で使用されます。 具体的には、生成されるファイル名は *Namespace.Classname\[.Culture].resources* になります。

  次に例を示します。

  ```xml
  <EmbeddedResource Include="X.resx" DependentUpon="MyTypes.cs">
  -or-
  <EmbeddedResource Include="X.fr-FR.resx" DependentUpon="MyTypes.cs">
  ```

  **生成されるリソース マニフェストのファイル名**:*Namespace.Classname.resources* または *Namespace.Classname.fr-FR.resources* (この `Namespace.Classname` が *MyTypes.cs* 内の最初のクラスの名前です)。

- 前の規則が適用されず、`EmbeddedResourceUseDependentUponConvention` が `true` であり (.NET Core の既定値)、同じ基本ファイル名を持つ *.resx* ファイルと併置されるソース ファイルが存在する場合、 *.resx* ファイルは一致するソース ファイルに依存し、生成される名前は前の規則と同じになります。 これは、.NET Core プロジェクトの "既定の設定" 規則です。
  
  次に例を示します。
  
  *MyTypes.cs* ファイル、*MyTypes.resx* ファイル、*MyTypes.fr-FR.resx* ファイルが同じフォルダーに存在します。
  
  **生成されるリソース マニフェストのファイル名**:*Namespace.Classname.resources* または *Namespace.Classname.fr-FR.resources* (この `Namespace.Classname` が *MyTypes.cs* 内の最初のクラスの名前です)。

- 上記の規則のいずれも当てはまらない場合、生成されるリソース マニフェストのファイル名は *RootNamespace.RelativePathWithDotsForSlashes.\[Culture.]resources* になります。 相対パスは、`EmbeddedResource` 要素の `Link` 属性が設定されていれば、その値になります。 それ以外の場合、相対パスは、`EmbeddedResource` 要素の `Identity` 属性の値になります。 Visual Studio では、これはプロジェクト ルートからソリューション エクスプローラー内のファイルへのパスです。

#### <a name="recommended-action"></a>推奨アクション

生成されるマニフェスト名に満足できない場合は、以下を実行できます。

- 前述の名前付け規則のいずれかに従って、リソース ファイルのメタデータを変更します。

- プロジェクト ファイルで `EmbeddedResourceUseDependentUponConvention` を `false` に設定して、新しい規則全体が無効になるようにします。

   ```xml
   <EmbeddedResourceUseDependentUponConvention>false</EmbeddedResourceUseDependentUponConvention>
   ```

   > [!NOTE]
   > `LogicalName` または `ManifestResourceName` 属性が存在する場合は、生成されるファイル名でそれらの値が引き続き使用されます。

#### <a name="category"></a>カテゴリ

MSBuild

#### <a name="affected-apis"></a>影響を受ける API

N/A
