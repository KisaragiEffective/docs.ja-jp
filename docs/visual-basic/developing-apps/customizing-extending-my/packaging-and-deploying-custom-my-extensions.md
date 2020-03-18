---
title: カスタム My 拡張のパッケージ化と配置
ms.date: 08/14/2018
helpviewer_keywords:
- My namespace [Visual Basic], customizing
- My namespace
- My namespace [Visual Basic], extending
ms.assetid: fd89c54b-0290-4c50-95a3-ff17d4487a21
ms.openlocfilehash: a2e2a6705fb3d8d4424d46d96bbf49b41e1414af
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74330257"
---
# <a name="package-and-deploy-custom-my-extensions-visual-basic"></a>カスタムマイ拡張のパッケージ化と配置 (Visual Basic)

Visual Basic では、Visual Studio テンプレートを使用してカスタム `My` 名前空間拡張を簡単にデプロイできます。 `My` の拡張機能が新しいプロジェクトの種類の不可欠な部分であるプロジェクトテンプレートを作成する場合は、テンプレートをエクスポートするときに、カスタムの `My` 拡張機能コードをプロジェクトに含めることができます。 プロジェクトテンプレートのエクスポートの詳細については、「[方法: プロジェクトテンプレートを作成する](/visualstudio/ide/how-to-create-project-templates)」を参照してください。

カスタム `My` 拡張機能が1つのコードファイル内にある場合は、ユーザーが任意の種類の Visual Basic プロジェクトに追加できる項目テンプレートとして、ファイルをエクスポートできます。 次に、項目テンプレートをカスタマイズして、Visual Basic プロジェクトのカスタム `My` 拡張機能の追加機能と動作を有効にすることができます。 次のような機能があります。

- ユーザーが Visual Basic プロジェクトデザイナーの **[マイ拡張**] ページからカスタム `My` 拡張機能を管理できるようにします。

- 指定したアセンブリへの参照がプロジェクトに追加されたときに、カスタム `My` 拡張機能を自動的に追加します。

- **[項目の追加]** ダイアログボックスで `My` 拡張項目テンプレートを非表示にして、プロジェクト項目の一覧に含まれないようにします。

このトピックでは、Visual Basic プロジェクトデザイナーの **[マイ拡張**] ページから管理できる非表示項目テンプレートとして、カスタム `My` 拡張機能をパッケージ化する方法について説明します。 また、指定したアセンブリへの参照がプロジェクトに追加されたときに、カスタム `My` 拡張機能を自動的に追加することもできます。

## <a name="create-a-my-namespace-extension"></a>マイ名前空間拡張を作成する

カスタム `My` 拡張機能の配置パッケージを作成する最初の手順は、拡張機能を1つのコードファイルとして作成することです。 カスタム `My` 拡張機能を作成する方法の詳細とガイダンスについては、「 [Visual Basic での My 名前空間の拡張](../../../visual-basic/developing-apps/customizing-extending-my/extending-the-my-namespace.md)」を参照してください。

## <a name="export-a-my-namespace-extension-as-an-item-template"></a>マイ名前空間拡張を項目テンプレートとしてエクスポートする

`My` 名前空間拡張機能を含むコードファイルを作成した後は、Visual Studio 項目テンプレートとしてコードファイルをエクスポートできます。 ファイルを Visual Studio 項目テンプレートとしてエクスポートする方法については、「[方法: 項目テンプレートを作成](/visualstudio/ide/how-to-create-item-templates)する」を参照してください。

> [!NOTE]
> `My` 名前空間拡張が特定のアセンブリに依存している場合は、そのアセンブリへの参照が追加されるときに、`My` 名前空間拡張機能を自動的にインストールするように項目テンプレートをカスタマイズできます。 そのため、Visual Studio 項目テンプレートとしてコードファイルをエクスポートするときに、そのアセンブリ参照を除外することができます。

## <a name="customize-the-item-template"></a>項目テンプレートをカスタマイズする

項目テンプレートは、Visual Basic プロジェクトデザイナーの **[マイ拡張**] ページから管理できるようになります。 指定したアセンブリへの参照がプロジェクトに追加されたときに、項目テンプレートが自動的に追加されるようにすることもできます。 これらのカスタマイズを有効にするには、CustomData ファイルという名前の新しいファイルをテンプレートに追加し、.vstemplate ファイルの XML に新しい要素を追加します。

### <a name="add-the-customdata-file"></a>CustomData ファイルを追加する

CustomData ファイルは、というファイル名拡張子を持つテキストファイルです。CustomData (ファイル名は、テンプレートにとって意味のある任意の値に設定できます)。また、XML が含まれています。 CustomData ファイルの XML は、ユーザーが Visual Basic プロジェクトデザイナーの **[マイ拡張**] ページを使用するときに、`My` 拡張機能を含めるように Visual Basic に指示します。 必要に応じて、CustomData ファイルの XML に <`AssemblyFullName>` 属性を追加することもできます。 これにより、特定のアセンブリへの参照がプロジェクトに追加されたときに、カスタム `My` 拡張機能を自動的にインストールするように Visual Basic に指示します。 任意のテキストエディターまたは XML エディターを使用して CustomData ファイルを作成し、項目テンプレートの圧縮フォルダー (.zip ファイル) に追加することができます。

たとえば、次の XML は、CustomData ファイルの内容を示しています。このファイルは、プロジェクトに追加されると、プロジェクト Visual Basic の [My Extensions] フォルダーにテンプレートアイテムが追加されます。

```xml
<VBMyExtensionTemplate
    ID="Microsoft.VisualBasic.Samples.MyExtensions.MyPrinterInfo"
    Version="1.0.0.0"
    AssemblyFullName="Microsoft.VisualBasic.PowerPacks.vs"
/>
```

CustomData ファイルには、次の表に示す属性を持つ <`VBMyExtensionTemplate>` 要素が含まれています。

|属性|説明|
|---|---|
|`ID`|必須。 拡張機能の一意の識別子。 この ID を持つ拡張機能が既にプロジェクトに追加されている場合、ユーザーは再度追加するように求められません。|
|`Version`|必須。 項目テンプレートのバージョン番号。|
|`AssemblyFullName`|省略可。 アセンブリ名。 このアセンブリへの参照がプロジェクトに追加されると、ユーザーは、この項目テンプレートから `My` 拡張機能を追加するように求められます。|

### <a name="add-the-customdatasignature-element-to-the-vstemplate-file"></a>\<CustomDataSignature > 要素を .vstemplate ファイルに追加します。

Visual Studio 項目テンプレートを `My` 名前空間拡張として識別するには、項目テンプレートの .vstemplate ファイルも変更する必要があります。 `<TemplateData>` 要素に `<CustomDataSignature>` 要素を追加する必要があります。 `<CustomDataSignature>` 要素には、次の例に示すように、テキスト `Microsoft.VisualBasic.MyExtension`が含まれている必要があります。

```xml
<CustomDataSignature>Microsoft.VisualBasic.MyExtension</CustomDataSignature>
```

圧縮フォルダー (.zip ファイル) 内のファイルを直接変更することはできません。 圧縮フォルダーから .vstemplate ファイルをコピーして変更し、圧縮フォルダー内の .vstemplate ファイルを更新したコピーで置き換える必要があります。

次の例は、`<CustomDataSignature>` 要素が追加された .vstemplate ファイルの内容を示しています。

```xml
<VSTemplate Version="2.0.0" xmlns="http://schemas.microsoft.com/developer/vstemplate/2005" Type="Item">
  <TemplateData>
    <DefaultName>MyCustomExtensionModule.vb</DefaultName>
    <Name>MyPrinterInfo</Name>
    <Description>Custom My Extensions Item Template</Description>
    <ProjectType>VisualBasic</ProjectType>
    <SortOrder>10</SortOrder>
    <Icon>__TemplateIcon.ico</Icon>
    <CustomDataSignature      >Microsoft.VisualBasic.MyExtension</CustomDataSignature>
  </TemplateData>
  <TemplateContent>
    <References />
    <ProjectItem SubType="Code"
                 TargetFileName="$fileinputname$.vb"
                 ReplaceParameters="true"
     >MyCustomExtensionModule.vb</ProjectItem>
  </TemplateContent>
</VSTemplate>
```

## <a name="install-the-template"></a>テンプレートをインストールする

テンプレートをインストールするには、圧縮フォルダー ( *.zip*ファイル) を Visual Basic 項目テンプレートフォルダーにコピーします。 既定では、ユーザー項目テンプレートは *%USERPROFILE%\Documents\Visual Studio \<Version\>\Templates\ItemTemplates\Visual Basic*にあります。 または、テンプレートを Visual Studio インストーラー ( *.vsi*) ファイルとして発行することもできます。

## <a name="see-also"></a>参照

- [Visual Basic における My 名前空間の拡張](../../../visual-basic/developing-apps/customizing-extending-my/extending-the-my-namespace.md)
- [Visual Basic アプリケーション モデルの拡張](../../../visual-basic/developing-apps/customizing-extending-my/extending-the-visual-basic-application-model.md)
- [My で利用可能なオブジェクトのカスタマイズ](../../../visual-basic/developing-apps/customizing-extending-my/customizing-which-objects-are-available-in-my.md)
- [[マイ拡張] ページ (プロジェクト デザイナー)](/visualstudio/ide/reference/my-extensions-page-project-designer-visual-basic)
