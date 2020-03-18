---
title: -reference (C# コンパイラ オプション)
ms.date: 07/20/2015
f1_keywords:
- /reference
helpviewer_keywords:
- /r compiler option [C#]
- reference compiler option [C#]
- r compiler option [C#]
- /reference compiler option [C#]
- -r compiler option [C#]
- metadata import [C#]
- public type information [C#]
- -reference compiler option [C#]
ms.assetid: 8d13e5b0-abf6-4c46-bf71-2daf2cd0a6c4
ms.openlocfilehash: 3e6a999d528be111ba2b92886f4e6e3ebf185d5c
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79173667"
---
# <a name="-reference-c-compiler-options"></a>-reference (C# コンパイラ オプション)
**-reference** オプションを指定すると、コンパイラは指定されたファイルの [public](../keywords/public.md) 型の情報を現在のプロジェクトにインポートし、指定されたアセンブリ ファイルからメタデータを参照できるようにします。  
  
## <a name="syntax"></a>構文  
  
```console  
-reference:[alias=]filename  
-reference:filename  
```  
  
## <a name="arguments"></a>引数  
 `filename`  
 アセンブリ マニフェストを含むファイルの名前。 複数のファイルをインポートするには、ファイルごとに個別に **-reference** オプションを指定します。  
  
 `alias`  
 アセンブリ内のすべての名前空間を格納するルート名前空間を表す有効な C# 識別子。  
  
## <a name="remarks"></a>解説  
 複数のファイルからインポートするには、ファイルごとに **-reference** オプションを指定します。  
  
 インポートするファイルは、マニフェストが含まれている必要があります。出力ファイルは、[-target:module](./target-compiler-option.md) 以外のいずれかの [-target](./target-module-compiler-option.md) オプションでコンパイルされている必要があります。  
  
 **-r** は **-reference** の省略形です。  
  
 アセンブリ マニフェストを含まない出力ファイルからメタデータをインポートするには、[-addmodule](./addmodule-compiler-option.md) を使います。  
  
 あるアセンブリ (アセンブリ A) を参照していて、そのアセンブリが別のアセンブリ (アセンブリ B) を参照している場合は、以下の条件が当てはまる場合はアセンブリ B を参照する必要があります。  
  
- アセンブリ A から使う型がアセンブリ B の型から継承されているか、アセンブリ B からインターフェイスを実装している。  
  
- アセンブリ B の戻り値の型またはパラメーターの型を使用するフィールド、プロパティ、イベント、またはメソッドを呼び出す。  
  
 アセンブリ参照が存在するディレクトリを指定するには、[-lib](./lib-compiler-option.md) を使います。 **-lib** のトピックでは、コンパイラがアセンブリを検索するディレクトリについても説明されています。  
  
 モジュールではなくアセンブリ内の型をコンパイラで認識するには、型の解決を強制する必要があります。そのためには、型のインスタンスを定義します。 アセンブリの型名を解決する方法は他にもあります。たとえば、アセンブリの型を継承すると、コンパイラで型名が認識されます。  
  
 1 つのアセンブリ内から、同じコンポーネントの 2 つの異なるバージョンを参照することが必要になる場合があります。 そのためには、ファイルごとに **-reference** スイッチの alias サブオブションを使って、2 つのファイルを区別します。 この別名は、コンポーネント名の修飾子として使われ、いずれかのファイルのコンポーネントに解決されます。  
  
 既定では、よく使われる .NET Framework アセンブリを参照する csc 応答 (.rsp) ファイルが使われます。 コンパイラが csc.rsp を使わないようにする場合は、[-noconfig](./noconfig-compiler-option.md) を指定します。  
  
> [!NOTE]
> Visual Studio では、 **[参照の追加]** ダイアログ ボックスを使います。 詳細については、「 [How to: Add or Remove References By Using the Reference Manager](/visualstudio/ide/how-to-add-or-remove-references-by-using-the-reference-manager)」を参照してください。 `-reference` を使った場合と **[参照の追加]** ダイアログ ボックスを使った場合で、参照の追加の動作が同じになるようにするには、追加するアセンブリの **[相互運用型の埋め込み]** プロパティを **[False]** に設定します。 このプロパティの既定値は **[True]** です。  
  
## <a name="example"></a>例  
 この例では、[extern alias](../keywords/extern-alias.md) 機能を使う方法を示します。  
  
 ソース ファイルをコンパイルし、`grid.dll` と `grid20.dll` からメタデータをインポートします。これらは事前にコンパイルしておきます。 2 つの DLL には同じコンポーネントの異なるバージョンが含まれており、ソース ファイルをコンパイルするには 2 つの **-reference** と別名オプションを使います。 オプションは次のようになります。  

```console
-reference:GridV1=grid.dll -reference:GridV2=grid20.dll  
```
  
 これは、外部別名 `GridV1` と `GridV2` を設定します。プログラムではこれらを `extern` ステートメントで使います。  
  
```csharp  
extern alias GridV1;  
extern alias GridV2;  
// Using statements go here.  
```  
  
 このようにすると、コントロール名にプレフィックス `grid.dll` を付けることで、`GridV1` のグリッド コントロールを参照できます。次に示すのはその例です。  
  
```csharp  
GridV1::Grid  
```  
  
 さらに、コントロール名にプレフィックス `grid20.dll` を付けることで、`GridV2` のグリッド コントロールを参照できます。次に示すのはその例です。  
  
```csharp  
GridV2::Grid
```  
  
## <a name="see-also"></a>参照

- [C# コンパイラ オプション](./index.md)
- [プロジェクトおよびソリューションのプロパティの管理](/visualstudio/ide/managing-project-and-solution-properties)
