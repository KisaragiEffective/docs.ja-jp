---
title: '方法: 発行者ポリシーを作成する'
description: アセンブリ ベンダーが、.NET でアップグレードされたアセンブリを使用して発行者ポリシー ファイルを作成し、アプリケーションで新しいバージョンを使用する必要があることを規定する方法について説明します。
ms.date: 03/30/2017
helpviewer_keywords:
- publisher policy assembly
- publisher policy files
- GAC (global assembly cache), publisher policy assembly
- global assembly cache, publisher policy assembly
ms.assetid: 8046bc5d-2fa9-4277-8a5e-6dcc96c281d9
ms.openlocfilehash: 23e9d8144ec5742e0371d566b7af59dc9dd30c9b
ms.sourcegitcommit: 1c37a894c923bea021a3cc38ce7cba946357bbe1
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/19/2020
ms.locfileid: "85105404"
---
# <a name="how-to-create-a-publisher-policy"></a>方法: 発行者ポリシーを作成する

アセンブリのベンダーは、アップグレードされたアセンブリに発行者ポリシー ファイルを含めることにより、より新しいバージョンのアセンブリをアプリケーションで使用する必要があることを説明できます。 発行者ポリシー ファイルは、アセンブリのリダイレクトとコード ベースの設定を指定するものであり、アプリケーション構成ファイルと同じ形式を使用します。 発行者ポリシー ファイルは、アセンブリにコンパイルされて、グローバル アセンブリ キャッシュに配置されます。

発行者ポリシーを作成するには、3 つのステップが必要です。

1. 発行者ポリシー ファイルを作成します。

2. 発行者ポリシー アセンブリを作成します。

3. 発行者ポリシー アセンブリをグローバル アセンブリ キャッシュに追加します。

発行元ポリシーのスキーマについては、「[アセンブリ バージョンのリダイレクト](redirect-assembly-versions.md)」を参照してください。 次の例では、`myAssembly` の 1 つのバージョンを別のものにリダイレクトする発行者ポリシー ファイルを示します。

```xml
<configuration>
   <runtime>
      <assemblyBinding xmlns="urn:schemas-microsoft-com:asm.v1">
       <dependentAssembly>
         <assemblyIdentity name="myAssembly"
                           publicKeyToken="32ab4ba45e0a69a1"
                           culture="en-us" />
         <!-- Redirecting to version 2.0.0.0 of the assembly. -->
         <bindingRedirect oldVersion="1.0.0.0"
                          newVersion="2.0.0.0"/>
       </dependentAssembly>
      </assemblyBinding>
   </runtime>
</configuration>
```

コード ベースを指定する方法については、「[アセンブリの場所の指定](specify-assembly-location.md)」を参照してください。

## <a name="creating-the-publisher-policy-assembly"></a>発行者ポリシー アセンブリの作成

発行者ポリシー アセンブリを作成するには、[アセンブリ リンカー (Al.exe)](../tools/al-exe-assembly-linker.md) を使用します。

#### <a name="to-create-a-publisher-policy-assembly"></a>発行者ポリシー アセンブリを作成するには

コマンド プロンプトに、次のコマンドを入力します。

```console
al /link:publisherPolicyFile /out:publisherPolicyAssemblyFile /keyfile:keyPairFile /platform:processorArchitecture
```

このコマンドの説明:

- `publisherPolicyFile` 引数は、発行者ポリシー ファイルの名前です。

- `publisherPolicyAssemblyFile` 引数は、このコマンドから生成される発行者ポリシー アセンブリの名前です。 アセンブリ ファイル名は、次の形式に従う必要があります。

  "policy.majorNumber.minorNumber.mainAssemblyName.dll"

- `keyPairFile` 引数は、キー ペアが格納されているファイルの名前です。 アセンブリと発行者ポリシー アセンブリには、同じキー ペアで署名する必要があります。

- `processorArchitecture` 引数は、プロセッサ固有のアセンブリの対象となるプラットフォームを示します。

  > [!NOTE]
  > 特定のプロセッサ アーキテクチャを対象にする機能は、.NET Framework 2.0 以降で使用できます。

特定のプロセッサ アーキテクチャを対象にする機能は、.NET Framework 2.0 以降で使用できます。 次のコマンドを実行すると、`pub.config` という発行者ポリシー ファイルから `policy.1.0.myAssembly` という発行者ポリシー アセンブリが作成され、`sgKey.snk` ファイルのキー ペアを使用してアセンブリに厳密な名前が割り当てられて、アセンブリの対象が x86 プロセッサ アーキテクチャであることが指定されます。

```console
al /link:pub.config /out:policy.1.0.myAssembly.dll /keyfile:sgKey.snk /platform:x86
```

発行者ポリシー アセンブリは、適用対象のアセンブリのプロセッサ アーキテクチャと一致している必要があります。 したがって、アセンブリの <xref:System.Reflection.ProcessorArchitecture.MSIL> の値が <xref:System.Reflection.AssemblyName.ProcessorArchitecture%2A> の場合は、そのアセンブリの発行者ポリシー アセンブリを `/platform:anycpu` で作成する必要があります。 プロセッサ固有のアセンブリごとに、個別の発行者ポリシー アセンブリを提供する必要があります。

このルールの結果として、アセンブリのプロセッサ アーキテクチャを変更するには、バージョン番号のメジャーまたはマイナー コンポーネントを変更して、正しいプロセッサ アーキテクチャの新しい発行者ポリシー アセンブリを提供できるようにする必要があります。 アセンブリが異なるプロセッサ アーキテクチャになった場合、古い発行者ポリシー アセンブリではアセンブリを処理できません。

もう 1 つの影響として、バージョン 2.0 のリンカーを使用して、以前のバージョンの .NET Framework を使用してコンパイルされたアセンブリ用の発行者ポリシー アセンブリを作成することはできません。これは、常にプロセッサ アーキテクチャが指定されるためです。

## <a name="adding-the-publisher-policy-assembly-to-the-global-assembly-cache"></a>グローバル アセンブリ キャッシュへの発行者ポリシー アセンブリの追加

グローバル アセンブリ キャッシュに発行者ポリシー アセンブリを追加するには、[グローバル アセンブリ キャッシュ ツール (Gacutil.exe)](../tools/gacutil-exe-gac-tool.md) を使用します。

### <a name="to-add-the-publisher-policy-assembly-to-the-global-assembly-cache"></a>発行者ポリシー アセンブリをグローバル アセンブリ キャッシュに追加するには

コマンド プロンプトに、次のコマンドを入力します。

```console
gacutil /i publisherPolicyAssemblyFile
```

次のコマンドを実行すると、`policy.1.0.myAssembly.dll` がグローバル アセンブリ キャッシュに追加されます。

```console
gacutil /i policy.1.0.myAssembly.dll
```

> [!IMPORTANT]
> `/link` 引数で指定されている元の発行者ポリシー ファイルがアセンブリと同じディレクトリにない場合、発行者ポリシー アセンブリをグローバル アセンブリ キャッシュに追加することはできません。

## <a name="see-also"></a>関連項目

- [アセンブリを使用したプログラミング](../../standard/assembly/index.md)
- [ランタイムがアセンブリを検索する方法](../deployment/how-the-runtime-locates-assemblies.md)
- [構成ファイルを使用してアプリを構成する方法](index.md)
- [ランタイム設定スキーマ](./file-schema/runtime/index.md)
- [構成ファイル スキーマ](./file-schema/index.md)
- [アセンブリ バージョンのリダイレクト](redirect-assembly-versions.md)
