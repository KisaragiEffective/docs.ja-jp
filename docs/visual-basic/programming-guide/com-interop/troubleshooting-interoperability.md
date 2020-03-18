---
title: 相互運用性のトラブルシューティング
ms.date: 07/20/2015
helpviewer_keywords:
- interop, deploying assemblies
- assemblies [Visual Basic]
- interop, installing assemblies that share components
- COM objects, troubleshooting
- interop, sharing components
- troubleshooting interoperability [Visual Basic]
- interoperability, troubleshooting
- COM interop [Visual Basic], troubleshooting
- assemblies [Visual Basic], deploying
- troubleshooting Visual Basic, interoperability
- interop assemblies
- interoperability, sharing components
- shared components, using with assemblies
ms.assetid: b324cc1e-b03c-4f39-aea6-6a6d5bfd0e37
ms.openlocfilehash: 344c180cf0b9426898e17b45db768a337fd45beb
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74338688"
---
# <a name="troubleshooting-interoperability-visual-basic"></a>相互運用性のトラブルシューティング (Visual Basic)
.NET Framework の COM とマネージコードの間で相互運用を行う場合は、次の一般的な問題の1つ以上が発生する可能性があります。  
  
## <a name="vbconinteroperabilitymarshalinganchor1"></a>相互運用マーシャリング  
 場合によっては、.NET Framework の一部ではないデータ型の使用が必要になることがあります。 相互運用機能アセンブリは COM オブジェクトのほとんどの作業を処理しますが、マネージオブジェクトが COM に公開されるときに使用されるデータ型の制御が必要になる場合があります。 たとえば、クラスライブラリ内の構造体は、Visual Basic 6.0 以前のバージョンで作成された COM オブジェクトに送信される文字列に `BStr` アンマネージ型を指定する必要があります。 このような場合は、<xref:System.Runtime.InteropServices.MarshalAsAttribute> 属性を使用して、マネージ型をアンマネージ型として公開することができます。  
  
## <a name="vbconinteroperabilitymarshalinganchor2"></a>アンマネージコードへの固定長文字列のエクスポート  
 Visual Basic 6.0 以前のバージョンでは、文字列は、null 終端文字のないバイトのシーケンスとして COM オブジェクトにエクスポートされます。 他の言語との互換性のために、Visual Basic .NET には文字列をエクスポートするときに終了文字が含まれます。 この非互換性に対処する最善の方法は、`Byte` または `Char`の配列として終了文字を持たない文字列をエクスポートすることです。  
  
## <a name="vbconinteroperabilitymarshalinganchor3"></a>継承階層のエクスポート  
 COM オブジェクトとして公開されると、マネージクラスの階層はフラット化されます。 たとえば、メンバーを持つ基本クラスを定義した後、COM オブジェクトとして公開されている派生クラスで基底クラスを継承した場合、COM オブジェクトの派生クラスを使用するクライアントは、継承されたメンバーを使用できません。 基底クラスのメンバーは、COM オブジェクトから基底クラスのインスタンスとしてのみアクセスできます。その後、基底クラスも COM オブジェクトとして作成されます。  
  
## <a name="overloaded-methods"></a>オーバーロードされたメソッド  
 オーバーロードされたメソッドは Visual Basic で作成できますが、COM ではサポートされていません。 オーバーロードされたメソッドを含むクラスが COM オブジェクトとして公開されると、オーバーロードされたメソッドの新しいメソッド名が生成されます。  
  
 たとえば、`Synch` メソッドの2つのオーバーロードを持つクラスを考えてみます。 クラスが COM オブジェクトとして公開されている場合は、生成された新しいメソッド名を `Synch` して `Synch_2`できます。  
  
 名前を変更すると、COM オブジェクトのコンシューマーに2つの問題が発生する可能性があります。  
  
1. クライアントは、生成されたメソッド名を予期していない可能性があります。  
  
2. COM オブジェクトとして公開されるクラスに生成されたメソッド名は、新しいオーバーロードがクラスまたはその基底クラスに追加されたときに変更される可能性があります。 これにより、バージョン管理の問題が発生する可能性があります。  
  
 両方の問題を解決するには、COM オブジェクトとして公開されるオブジェクトを開発するときに、オーバーロードを使用するのではなく、各メソッドに一意の名前を指定します。  
  
## <a name="vbconinteroperabilitymarshalinganchor4"></a>相互運用機能アセンブリによる COM オブジェクトの使用  
 相互運用機能アセンブリは、それらが表す COM オブジェクトのマネージコード置換の場合とほぼ同じように使用します。 ただし、これらはラッパーであり、実際の COM オブジェクトではないため、相互運用機能アセンブリと標準アセンブリの使用にはいくつかの違いがあります。 これらの違いの領域には、クラスの公開、およびパラメーターと戻り値のデータ型が含まれます。  
  
## <a name="vbconinteroperabilitymarshalinganchor5"></a>インターフェイスとクラスの両方として公開されるクラス  
 標準アセンブリのクラスとは異なり、COM クラスは相互運用機能アセンブリで、COM クラスを表すインターフェイスとクラスの両方として公開されます。 インターフェイスの名前は、COM クラスの名前と同じです。 相互運用クラスの名前は、元の COM クラスの名前と同じですが、"Class" という単語が追加されています。 たとえば、COM オブジェクトの相互運用機能アセンブリへの参照を持つプロジェクトがあるとします。 COM クラスに `MyComClass`という名前が付けられている場合、IntelliSense とオブジェクトブラウザーには、`MyComClass` という名前のインターフェイスと、`MyComClassClass`という名前のクラスが表示されます。  
  
## <a name="vbconinteroperabilitymarshalinganchor6"></a>.NET Framework クラスのインスタンスの作成  
 一般に、クラス名を指定した `New` ステートメントを使用して、.NET Framework クラスのインスタンスを作成します。 相互運用機能アセンブリによって表される COM クラスは、インターフェイスで `New` ステートメントを使用できるケースの1つです。 `Inherits` ステートメントで COM クラスを使用している場合を除き、クラスの場合と同様に、インターフェイスを使用できます。 次のコードは、Microsoft ActiveX データオブジェクト2.8 ライブラリ COM オブジェクトへの参照を含むプロジェクトに `Command` オブジェクトを作成する方法を示しています。  
  
 [!code-vb[VbVbalrInterop#20](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrInterop/VB/Class1.vb#20)]  
  
 ただし、COM クラスを派生クラスのベースとして使用している場合は、次のコードのように、COM クラスを表す interop クラスを使用する必要があります。  
  
 [!code-vb[VbVbalrInterop#21](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrInterop/VB/Class1.vb#21)]  
  
> [!NOTE]
> 相互運用機能アセンブリは、COM クラスを表すインターフェイスを暗黙的に実装します。 これらのインターフェイスを実装するために `Implements` ステートメントを使用しないでください。または、エラーが発生します。  
  
## <a name="vbconinteroperabilitymarshalinganchor7"></a>パラメーターと戻り値のデータ型  
 標準アセンブリのメンバーとは異なり、相互運用機能アセンブリのメンバーは、元のオブジェクト宣言で使用されるデータ型とは異なるデータ型を持つ場合があります。 相互運用機能アセンブリは、COM 型を互換性のある共通言語ランタイム型に暗黙的に変換しますが、ランタイムエラーを防ぐために、両側で使用されるデータ型に注意する必要があります。 たとえば、Visual Basic 6.0 以前のバージョンで作成された COM オブジェクトでは、型 `Integer` の値は、`Short`の .NET Framework 等価な型を想定しています。 インポートしたメンバーを使用する前に、その特性を確認するには、オブジェクトブラウザーを使用することをお勧めします。  
  
## <a name="vbconinteroperabilitymarshalinganchor8"></a>モジュールレベルの COM メソッド  
 ほとんどの COM オブジェクトは、`New` キーワードを使用して COM クラスのインスタンスを作成し、そのオブジェクトのメソッドを呼び出すことによって使用されます。 この規則の1つの例外として、`AppObj` または `GlobalMultiUse` COM クラスを含む COM オブジェクトがあります。 このようなクラスは Visual Basic .NET クラスのモジュールレベルメソッドに似ています。 Visual Basic 6.0 以前のバージョンでは、メソッドのいずれかを初めて呼び出すときに、これらのオブジェクトのインスタンスが暗黙的に作成されます。 たとえば、Visual Basic 6.0 では、Microsoft DAO 3.6 オブジェクトライブラリへの参照を追加し、最初にインスタンスを作成せずに `DBEngine` メソッドを呼び出すことができます。  
  
```vb  
Dim db As DAO.Database  
' Open the database.  
Set db = DBEngine.OpenDatabase("C:\nwind.mdb")  
' Use the database object.  
```  
  
 Visual Basic .NET では、メソッドを使用する前に、常に COM オブジェクトのインスタンスを作成する必要があります。 これらのメソッドを Visual Basic で使用するには、目的のクラスの変数を宣言し、new キーワードを使用してオブジェクトをオブジェクト変数に割り当てます。 `Shared` キーワードは、クラスのインスタンスが1つだけ作成されるようにする場合に使用できます。  
  
 [!code-vb[VbVbalrInterop#23](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrInterop/VB/Class1.vb#23)]  
  
## <a name="vbconinteroperabilitymarshalinganchor9"></a>イベントハンドラーでのハンドルされないエラー  
 一般的な相互運用の問題には、COM オブジェクトによって発生するイベントを処理するイベントハンドラーでのエラーが含まれます。 `On Error` または `Try...Catch...Finally` ステートメントを使用してエラーを明示的にチェックしない限り、このようなエラーは無視されます。 たとえば、次の例は、Microsoft ActiveX データオブジェクト2.8 ライブラリ COM オブジェクトへの参照を含む Visual Basic .NET プロジェクトからのものです。  
  
 [!code-vb[VbVbalrInterop#24](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrInterop/VB/Class1.vb#24)]  
  
 この例では、予期したとおりにエラーが発生します。 ただし、`Try...Catch...Finally` ブロックを指定せずに同じ例を試した場合、`OnError Resume Next` ステートメントを使用した場合と同様に、エラーは無視されます。 エラー処理がなければ、0による除算は自動的に失敗します。 このようなエラーによってハンドルされない例外エラーが発生することはないため、COM オブジェクトからのイベントを処理するイベントハンドラーで何らかの形式の例外処理を使用することが重要です。  
  
### <a name="understanding-com-interop-errors"></a>COM 相互運用エラーについて  
 エラー処理を行わないと、相互運用呼び出しによって多くの情報を提供するエラーが発生することがよくあります。 可能な限り、構造化されたエラー処理を使用して、発生した問題に関する詳細情報を提供します。 これは、アプリケーションをデバッグするときに特に役立ちます。 例 :  
  
 [!code-vb[VbVbalrInterop#25](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrInterop/VB/Class1.vb#25)]  
  
 例外オブジェクトの内容を調べることによって、エラーの説明、HRESULT、COM エラーの発生源などの情報を確認できます。  
  
## <a name="vbconinteroperabilitymarshalinganchor10"></a>ActiveX コントロールの問題  
 Visual Basic 6.0 で動作するほとんどの ActiveX コントロールは、Visual Basic .NET では問題なく動作します。 主な例外は、コンテナーコントロール、または他のコントロールを視覚的に含むコントロールです。 Visual Studio で正しく動作しない古いコントロールの例を次に示します。  
  
- Microsoft Forms 2.0 Frame コントロール  
  
- アップダウンコントロール。スピンコントロールとも呼ばれます。  
  
- Sheridan タブコントロール  
  
 サポートされていない ActiveX コントロールの問題には、いくつかの回避策があります。 元のソースコードを所有している場合は、既存のコントロールを Visual Studio に移行できます。 それ以外の場合は、ソフトウェアベンダーに更新プログラムがあるかどうかを確認できます。サポートされていない ActiveX コントロールを置き換える、互換性のあるコントロールのバージョン。  
  
## <a name="vbconinteroperabilitymarshalinganchor11"></a>コントロールの読み取り専用プロパティを ByRef で渡す  
 Visual Basic .NET では、一部の古い ActiveX コントロールの `ReadOnly` プロパティを他のプロシージャに `ByRef` パラメーターとして渡すと、"Error 0x800A017F CTL_E_SETNOTSUPPORTED" などの COM エラーが発生することがあります。 Visual Basic 6.0 からの同様のプロシージャ呼び出しではエラーは発生せず、パラメーターは値渡しで渡されたものとして扱われます。 Visual Basic .NET のエラーメッセージは、プロパティ `Set` プロシージャを持たないプロパティを変更しようとしていることを示します。  
  
 呼び出されているプロシージャにアクセスできる場合は、`ByVal` キーワードを使用して `ReadOnly` プロパティを受け取るパラメーターを宣言することで、このエラーを回避できます。 例 :  
  
 [!code-vb[VbVbalrInterop#26](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrInterop/VB/Class1.vb#26)]  
  
 呼び出されているプロシージャのソースコードにアクセスできない場合は、呼び出し元のプロシージャの周囲に余分な角かっこを追加することで、プロパティを強制的に値で渡すことができます。 たとえば、Microsoft ActiveX データオブジェクト2.8 ライブラリ COM オブジェクトへの参照を持つプロジェクトでは、次のようなを使用できます。  
  
 [!code-vb[VbVbalrInterop#27](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrInterop/VB/Class1.vb#27)]  
  
## <a name="vbconinteroperabilitymarshalinganchor12"></a>相互運用機能を公開するアセンブリの配置  
 COM インターフェイスを公開するアセンブリを配置すると、いくつかの固有の課題が生じます。 たとえば、別のアプリケーションが同じ COM アセンブリを参照すると、問題が発生する可能性があります。 この状況は、新しいバージョンのアセンブリがインストールされていて、別のアプリケーションが以前のバージョンのアセンブリを引き続き使用している場合によく発生します。 DLL を共有するアセンブリをアンインストールする場合、そのアセンブリを他のアセンブリで使用できないようにすることができます。  
  
 この問題を回避するには、共有アセンブリをグローバルアセンブリキャッシュ (GAC) にインストールし、コンポーネントにマージモジュールを使用する必要があります。 GAC にアプリケーションをインストールできない場合は、バージョン固有のサブディレクトリにある CommonFilesFolder にインストールする必要があります。  
  
 共有されていないアセンブリは、呼び出し元のアプリケーションのディレクトリに並べて配置する必要があります。  
  
## <a name="see-also"></a>参照

- <xref:System.Runtime.InteropServices.MarshalAsAttribute>
- [COM 相互運用](../../../visual-basic/programming-guide/com-interop/index.md)
- [Tlbimp.exe (タイプ ライブラリ インポーター)](../../../framework/tools/tlbimp-exe-type-library-importer.md)
- [Tlbexp.exe (タイプ ライブラリ エクスポーター)](../../../framework/tools/tlbexp-exe-type-library-exporter.md)
- [チュートリアル : COM オブジェクトによる継承の実装](../../../visual-basic/programming-guide/com-interop/walkthrough-implementing-inheritance-with-com-objects.md)
- [Inherits ステートメント](../../../visual-basic/language-reference/statements/inherits-statement.md)
- [グローバル アセンブリ キャッシュ](../../../framework/app-domains/gac.md)
