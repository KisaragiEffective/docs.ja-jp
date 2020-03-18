---
title: アセンブリへの遅延署名
ms.date: 08/19/2019
helpviewer_keywords:
- deferring assembly signing
- signing assemblies
- assemblies [.NET Framework], signing
- strong-named assemblies, delaying assembly signing
- partial assembly signing
ms.assetid: 9d300e17-5bf1-4360-97da-2aa55efd9070
dev_langs:
- csharp
- vb
- cpp
ms.openlocfilehash: 113df1ad3fc3ac1e27ebfef572494c1f15a3dbb5
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73733161"
---
# <a name="delay-sign-an-assembly"></a>アセンブリへの遅延署名

組織には、開発者が日常的にアクセスできない厳重に保護されたキーのペアがある場合があります。 公開キーは広く使用可能ですが、秘密キーへのアクセスは少数のユーザーに限定されます。 厳密な名前のアセンブリを開発すると、厳密な名前のターゲット アセンブリを参照する各アセンブリに、そのターゲット アセンブリに厳密な名前を指定するために使用する公開キーのトークンが含まれています。 この場合、開発プロセスで、公開キーを使用できる必要があります。

ビルド時に遅延署名または部分署名を使用することで、厳密な名前の署名のためにポータブル実行可能 (PE) ファイルに領域を確保し、少し後の段階、通常はアセンブリが出荷される直前まで、実際の署名を遅らせることができます。

アセンブリへの遅延署名を行うには、次のようにします。

1. 最終的な署名を行う組織のキー ペアの公開キー部分を取得します。 通常、このキーは、Windows SDK により提供される[厳密名ツール (Sn.exe)](../../framework/tools/sn-exe-strong-name-tool.md) を使って作成できる *.snk* ファイルの形式です。

2. <xref:System.Reflection> から次の 2 つのカスタム属性を含むアセンブリのソース コードに注釈を付けます。

   - 公開キーをパラメーターとして含むファイルの名前を、そのコンストラクターに渡す <xref:System.Reflection.AssemblyKeyFileAttribute>。

   - その遅延署名がパラメーターとして **true** をそのコンストラクターに渡すことで使用されていることを示す <xref:System.Reflection.AssemblyDelaySignAttribute>。

   次に例を示します。

   ```cpp
   [assembly:AssemblyKeyFileAttribute("myKey.snk")];
   [assembly:AssemblyDelaySignAttribute(true)];
   ```

   ```csharp
   [assembly:AssemblyKeyFileAttribute("myKey.snk")]
   [assembly:AssemblyDelaySignAttribute(true)]
   ```

   ```vb
   <Assembly:AssemblyKeyFileAttribute("myKey.snk")>
   <Assembly:AssemblyDelaySignAttribute(True)>
   ```

3. コンパイラは、アセンブリ マニフェストに公開キーを挿入し、完全に厳密な名前の署名のために PE ファイルの領域を確保します。 このアセンブリを参照する他のアセンブリがキーを取得して自身のアセンブリ参照に格納できるように、アセンブリのビルド中に実際の公開キーが格納されている必要があります。

4. アセンブリには有効な厳密な名前の署名がないため、その署名の検証をオフにする必要があります。 これは、厳密な名前ツールで **–Vr** オプションを使用して行うことができます。

     次の例では、*myAssembly.dll* と呼ばれるアセンブリの検証をオフにしています。

   ```console
   sn –Vr myAssembly.dll
   ```

   厳密な名前ツールを実行できない Advanced RISC Machine (ARM) マイクロプロセッサなどのプラットフォームで検証をオフにするには、 **–Vk** オプションを使用してレジストリ ファイルを作成します。 検証をオフにするコンピューターのレジストリにこのレジストリ ファイルをインポートします。 次の例では、`myAssembly.dll` にレジストリ ファイルを作成しています。

   ```console
   sn –Vk myRegFile.reg myAssembly.dll
   ```

   **–Vr** または **–Vk** のどちらかのオプションでは、テスト キー署名のための *.snk* ファイルを任意で含めることができます。

   > [!WARNING]
   > セキュリティに関しては、厳格な名前に依存しないでください。 厳格な名前は、一意の ID を提供するだけです。

   > [!NOTE]
   > 64 ビット コンピューターの Visual Studio を使用した開発中に、遅延署名を使用して、**Any CPU** のアセンブリをコンパイルする場合は、 **-Vr** オプションを 2 回適用しなければならない場合があります。 (Visual Studio では、**Any CPU** は**プラットフォーム ターゲット**のビルド プロパティの値です。コマンド ラインからコンパイルする場合は、これが既定です。)コマンド ラインまたはファイル エクスプローラーからアプリケーションを実行する場合は、64 ビット バージョンの [Sn.exe (厳密な名前ツール)](../../framework/tools/sn-exe-strong-name-tool.md) を使用して、アセンブリに **-Vr** オプションを適用します。 設計時に Visual Studio にアセンブリを読み込む (たとえば、アプリケーションの他のアセンブリで使用されているコンポーネントがアセンブリに含まれている) 場合は、32 ビット バージョンの厳密な名前ツールを使用してください。 これは、Just-In-Time (JIT) コンパイラが、コマンド ラインからアセンブリを実行する場合には 64 ビットのネイティブ コードに、設計時の環境にアセンブリを読み込む場合には 32 ビットのネイティブ コードにアセンブリをコンパイルするためです。

5. 後ほど (通常は出荷の直前)、厳密な名前ツールで **–R** オプションを使用して実際に厳密な名前の署名を行うため、組織の署名機関にアセンブリを送信します。

   キー ペア *sgKey.snk* を使用して厳密な名前で *myAssembly.dll* というアセンブリに署名する例を、次に示します。

   ```console
   sn -R myAssembly.dll sgKey.snk
   ```

## <a name="see-also"></a>関連項目

- [アセンブリを作成する](create.md)
- [方法: 公開キーと秘密キーのキー ペアを作成する](create-public-private-key-pair.md)
- [Sn.exe (厳密名ツール)](../../framework/tools/sn-exe-strong-name-tool.md)
