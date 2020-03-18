---
title: '方法: 厳密な名前のアセンブリを参照する'
ms.date: 08/20/2019
helpviewer_keywords:
- strong-named assemblies, compile-time references
- compile-time assembly referencing
- assemblies [.NET Framework], strong-named
- assembly binding, strong-named
ms.assetid: 4c6a406a-b5eb-44fa-b4ed-4e95bb95a813
dev_langs:
- csharp
- vb
- cpp
ms.openlocfilehash: adda4ed2ab5c59e3518b8e724044529a79840ad0
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "78156479"
---
# <a name="how-to-reference-a-strong-named-assembly"></a>方法: 厳密な名前のアセンブリを参照する
通常、厳密な名前付きアセンブリ内にある型またはリソースを参照するプロセスは透過的です。 コンパイル時 (事前バインディング) または実行時に参照を作成できます。  
  
コンパイル時の参照は、コンパイルするアセンブリが明示的に別のアセンブリを参照していることをコンパイラに対して指定するときに発生します。 コンパイル時参照を使用する場合、コンパイラは対象の厳密な名前付きアセンブリの公開キーを自動的に取得し、その公開キーをコンパイルされるアセンブリのアセンブリ参照内に自動的に配置します。
  
> [!NOTE]
> 厳密な名前のアセンブリは、他の厳密な名前のアセンブリでのタイプだけを使用できます。 それ以外の場合は、厳密な名前のアセンブリのセキュリティが損なわれます。  
  
## <a name="make-a-compile-time-reference-to-a-strong-named-assembly"></a>厳密な名前付きアセンブリへのコンパイル時参照を作成する  

コマンド プロンプトに次のコマンドを入力します。  

\<*compiler command*>  **/reference:** \<*assembly name*>  

このコマンドで、*compiler command* は、使用している言語のコンパイラ コマンドです。*assembly name* は、参照される厳密な名前付きアセンブリの名前です。 ライブラリ アセンブリを作成するための **/t:library** オプションなどの他のコンパイラ オプションも使用できます。  

*myAssembly.cs* というコード モジュールから厳密な名前付きアセンブリ *myLibAssembly.dll* を参照する *myAssembly.dll* というアセンブリを作成する例を次に示します。  

```cmd
csc /t:library myAssembly.cs /reference:myLibAssembly.dll  
```  

## <a name="make-a-run-time-reference-to-a-strong-named-assembly"></a>厳密な名前付きアセンブリへの実行時参照を作成する  
  
<xref:System.Reflection.Assembly.Load%2A?displayProperty=nameWithType> メソッドまたは <xref:System.Reflection.Assembly.GetType%2A?displayProperty=nameWithType> メソッドを使用するなどの方法で、実行時に厳密な名前付きアセンブリ参照を作成する場合は、参照される厳密な名前付きアセンブリの表示名を使用する必要があります。 表示名の構文は次のとおりです。  

\<*アセンブリ名*> **、** \<*バージョン番号*> **、** \<*カルチャ*> **、** \<*公開キー トークン*>  

次に例を示します。  

```console
myDll, Version=1.1.0.0, Culture=en, PublicKeyToken=03689116d3a4ae33
```  

この例では、`PublicKeyToken` は 16 進形式の公開キートークンです。 カルチャの値がない場合は、`Culture=neutral` を使用します。  

この情報を <xref:System.Reflection.Assembly.Load%2A?displayProperty=nameWithType> メソッドで使用する方法を次の例に示します。  

```cpp
Assembly^ myDll =
    Assembly::Load("myDll, Version=1.0.0.1, Culture=neutral, PublicKeyToken=9b35aa32c18d4fb1");
```

```csharp
Assembly myDll =
    Assembly.Load("myDll, Version=1.0.0.1, Culture=neutral, PublicKeyToken=9b35aa32c18d4fb1");
```

```vb
Dim myDll As Assembly = _
    Assembly.Load("myDll, Version=1.0.0.1, Culture=neutral, PublicKeyToken=9b35aa32c18d4fb1")
```

特定のアセンブリの 16 進形式の公開キーと公開キー トークンは、次の[厳密名 (Sn.exe)](../../framework/tools/sn-exe-strong-name-tool.md) コマンドを使用して出力できます。  

**sn -Tp \<** *アセンブリ* **>**  

公開キーファイルがある場合は、代わりに次のコマンドを使用できます (コマンド ライン オプションの大文字と小文字の違いに注意してください)。  

**sn -tp \<** *公開キー ファイル* **>**  

## <a name="see-also"></a>関連項目

- [厳密な名前付きアセンブリの作成と使用](create-use-strong-named.md)
