---
title: -link (C# コンパイラ オプション)
ms.date: 07/20/2015
helpviewer_keywords:
- /l compiler option [C#]
- /link compiler option [C#]
- -l compiler option [C#]
- EmbedInteropTypes
- l compiler option [C#]
- embed interop types [COM Interop]
- -link compiler option [C#]
- link compiler option [C#]
ms.assetid: 00da70c6-9ea1-43c2-86f2-aa7f26c03475
ms.openlocfilehash: 4c96f7be5ac500886ea036c93b4651fa814ee58a
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "70970102"
---
# <a name="-link-c-compiler-options"></a>-link (C# コンパイラ オプション)
指定したアセンブリ内の COM 型情報を、現在のコンパイル対象のプロジェクトで使用できるようにします。  
  
## <a name="syntax"></a>構文  
  
```console  
-link:fileList  
// -or-  
-l:fileList  
```  
  
## <a name="arguments"></a>引数  
 `fileList`  
 必須です。 アセンブリ ファイル名のコンマ区切りリスト。 ファイル名に空白が含まれている場合は、名前を二重引用符で囲みます。  
  
## <a name="remarks"></a>解説  
 `-link` オプションを使用すると、埋め込み型情報を含むアプリケーションを配置できます。 その後、このアプリケーションは、埋め込み型情報を実装する、ランタイム アセンブリ内の型を使用できます。その際、ランタイム アセンブリへの参照は必要ありません。 ランタイム アセンブリのさまざまなバージョンが公開されている場合、埋め込み型情報を含むアプリケーションは、再コンパイルする必要なく、各種バージョンで動作できます。 例については、「[チュートリアル: マネージド アセンブリからの型の埋め込み](../../../standard/assembly/embed-types-visual-studio.md)」を参照してください。  
  
 `-link` オプションの使用は、COM 相互運用を使用している場合に特に便利です。 COM 型を埋め込むことができるため、アプリケーションは、ターゲット コンピューター上にプライマリ相互運用機能アセンブリ (PIA) を必要としなくなります。 `-link` オプションを使用すると、コンパイラによって、COM 型情報は、参照先の相互運用アセンブリから結果としてコンパイルされるコードに埋め込まれます。 COM 型は、CLSID (GUID) 値によって識別されます。 その結果、同じ CLSID 値の同じ COM 型がインストールされているターゲット コンピューターでアプリケーションを実行できます。 Microsoft Office を自動化するアプリケーションが良い例です。 Office のようなアプリケーションは、通常、さまざまなバージョン間で同じ CLSID 値を保持するため、.NET Framework 4 以降がターゲット コンピューターにインストールされていて、参照先の COM 型に含まれているメソッド、プロパティ、またはイベントがアプリケーションで使用される限りは、そのアプリケーションで参照先の COM 型を使用できます。  
  
 `-link` オプションで埋め込まれるのは、インターフェイス、構造体、デリゲートのみです。 COM クラスの埋め込みはサポートされていません。  
  
> [!NOTE]
> コードで埋め込み COM 型のインスタンスを作成する際は、適切なインターフェイスを使用してインスタンスを作成する必要があります。 コクラスを使用して埋め込み COM 型のインスタンスを作成しようとすると、エラーが発生します。  
  
 Visual Studio で `-link` オプションを設定するには、アセンブリ参照を追加し、`Embed Interop Types` プロパティを **true** に設定します。 `Embed Interop Types` プロパティの既定値は **false** です。  
  
 別の COM アセンブリ (アセンブリ B) を参照する COM アセンブリ (アセンブリ A) にリンクする場合、次のいずれかが当てはまるときは、アセンブリ B にもリンクする必要があります。  
  
- アセンブリ A の型がアセンブリ B の型から継承されているか、アセンブリ B のインターフェイスを実装する。  
  
- アセンブリ B の戻り値の型またはパラメーターの型を使用するフィールド、プロパティ、イベント、またはメソッドが呼び出される。  
  
 [-reference](./reference-compiler-option.md) コンパイラ オプションと同様、`-link` コンパイラ オプションでは、使用頻度の高い .NET Framework アセンブリを参照する Csc.rsp 応答ファイルを使用します。 コンパイラで Csc.rsp ファイルを使用しない場合は、[-noconfig](./noconfig-compiler-option.md) コンパイラ オプションを使用してください。  
  
 `-link` の省略形は `-l` です。  
  
## <a name="generics-and-embedded-types"></a>ジェネリック型と埋め込み型  
 以降のセクションでは、相互運用機能型を埋め込むアプリケーションでジェネリック型を使用する際の制限事項について説明します。  
  
### <a name="generic-interfaces"></a>ジェネリック インターフェイス  
 相互運用アセンブリから埋め込まれるジェネリック インターフェイスを使用することはできません。 次の例を参照してください。  
  
 [!code-csharp[VbLinkCompilerCS#1](~/samples/snippets/csharp/VS_Snippets_VBCSharp/vblinkcompilercs/cs/program.cs#1)]  
  
### <a name="types-that-have-generic-parameters"></a>ジェネリック パラメーターを含む型  
 型が相互運用アセンブリから埋め込まれているジェネリック パラメーターを含む型は、外部アセンブリからの型である場合に使用できません。 この制限はインターフェイスには当てはまりません。 たとえば、<xref:Microsoft.Office.Interop.Excel.Range> アセンブリで定義されている <xref:Microsoft.Office.Interop.Excel> インターフェイスについて考えます。 ライブラリによって <xref:Microsoft.Office.Interop.Excel> アセンブリから相互運用型が埋め込まれ、型が <xref:Microsoft.Office.Interop.Excel.Range> インターフェイスであるパラメーターを含むジェネリック型を返すメソッドが公開される場合、次のコード例に示すように、そのメソッドはジェネリック インターフェイスを返す必要があります。  
  
 [!code-csharp[VbLinkCompilerCS#2](~/samples/snippets/csharp/VS_Snippets_VBCSharp/vblinkcompilercs/cs/utility.cs#2)]  
[!code-csharp[VbLinkCompilerCS#3](~/samples/snippets/csharp/VS_Snippets_VBCSharp/vblinkcompilercs/cs/utility.cs#3)]  
[!code-csharp[VbLinkCompilerCS#4](~/samples/snippets/csharp/VS_Snippets_VBCSharp/vblinkcompilercs/cs/utility.cs#4)]  
  
 次の例では、クライアント コードで、<xref:System.Collections.IList> ジェネリック インターフェイスを返すメソッドをエラーなしで呼び出すことができます。  
  
 [!code-csharp[VbLinkCompilerCS#5](~/samples/snippets/csharp/VS_Snippets_VBCSharp/vblinkcompilercs/cs/program.cs#5)]  
  
## <a name="example"></a>例  
 次のコードは、ソース ファイル `OfficeApp.cs` と、`COMData1.dll` および `COMData2.dll` からの参照アセンブリをコンパイルして、`OfficeApp.exe` を生成します。  
  
```csharp  
csc -link:COMData1.dll,COMData2.dll -out:OfficeApp.exe OfficeApp.cs  
```  
  
## <a name="see-also"></a>参照

- [C# コンパイラ オプション](./index.md)
- [チュートリアル: マネージド アセンブリからの型の埋め込み](../../../standard/assembly/embed-types-visual-studio.md)
- [-reference (C# コンパイラ オプション)](./reference-compiler-option.md)
- [-noconfig (C# コンパイラ オプション)](./noconfig-compiler-option.md)
- [csc.exe を使用したコマンド ラインからのビルド](./command-line-building-with-csc-exe.md)
- [相互運用性の概要](../../programming-guide/interop/interoperability-overview.md)
