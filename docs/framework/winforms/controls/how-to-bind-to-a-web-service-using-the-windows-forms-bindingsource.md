---
title: BindingSource を使用して Web サービスにバインドする
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- Web services [Windows Forms], binding controls
- BindingSource component [Windows Forms], binding to a Web service
- examples [Windows Forms], BindingSource component
- controls [Windows Forms], binding to Web service
- BindingSource component [Windows Forms], examples
ms.assetid: ee261207-4573-4cb9-a8cb-5185037e0fba
ms.openlocfilehash: 0680c73e578577cf40158761f6c635fe30ff9f4d
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76746673"
---
# <a name="how-to-bind-to-a-web-service-using-the-windows-forms-bindingsource"></a>方法 : Windows フォーム BindingSource を使用して Web サービスにバインドする
XML Web サービスを呼び出して取得した結果に対して Windows フォーム コントロールをバインドする場合は、<xref:System.Windows.Forms.BindingSource> コンポーネントを使用します。 この手順は、<xref:System.Windows.Forms.BindingSource> コンポーネントを型にバインディングする場合と似ています。 Web サービスが公開するメソッドおよび型を含むクライアント側プロキシを作成する必要があります。 クライアント側プロキシは、Web サービス (.asmx) 自体またはその Web サービス記述言語 (WSDL: Web Services Description Language) ファイルから生成できます。 また、クライアント側プロキシでは Web サービスが使用する複合型のフィールドをパブリック プロパティとして公開する必要があります。 その後、Web サービス プロキシ内で公開された型のいずれかに <xref:System.Windows.Forms.BindingSource> をバインドします。  
  
### <a name="to-create-and-bind-to-a-client-side-proxy"></a>クライアント側プロキシを作成してバインドするには  
  
1. 適切な名前空間を使用して、任意のディレクトリに Windows フォームを作成します。  
  
2. フォームに <xref:System.Windows.Forms.BindingSource> コンポーネントを追加します。  
  
3. Windows Software Development Kit (SDK) コマンドプロンプトを開き、フォームが配置されているディレクトリに移動します。  
  
4. WSDL ツールを使用して、`wsdl`、および Web サービスの .asmx ファイルまたは WSDL ファイルの URL を入力し、次にアプリケーションの名前空間を入力し、使用している言語 (これはオプション) を入力します。  
  
     次のコード例では、`http://webservices.eraserver.net/zipcoderesolver/zipcoderesolver.asmx`にある Web サービスを使用します。 たとえば、C# の型の場合は `wsdl http://webservices.eraserver.net.zipcoderesolver/zipcoderesolver.asmx /n:BindToWebService`、Visual Basic の型の場合は `wsdl http://webservices.eraserver.net.zipcoderesolver/zipcoderesolver.asmx /n:BindToWebService /language:VB` です。 パスを引数として WSDL ツールに渡すことで、指定した言語で、アプリケーションと同じディレクトリおよび名前空間にクライアント側プロキシが生成されます。 Visual Studio を使用している場合は、ファイルをプロジェクトに追加します。  
  
5. バインド先のクライアント側プロキシの型を選択します。  
  
     これは、通常、Web サービスによって提供されるメソッドが返す型です。 選択した型のフィールドは、バインド用にパブリック プロパティとして公開する必要があります。  
  
     [!code-cpp[System.Windows.Forms.DataConnectorWebService#4](~/samples/snippets/cpp/VS_Snippets_Winforms/System.Windows.Forms.DataConnectorWebService/CPP/form1.cpp#4)]
     [!code-csharp[System.Windows.Forms.DataConnectorWebService#4](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataConnectorWebService/CS/form1.cs#4)]
     [!code-vb[System.Windows.Forms.DataConnectorWebService#4](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataConnectorWebService/VB/form1.vb#4)]  
  
6. <xref:System.Windows.Forms.BindingSource.DataSource%2A> の <xref:System.Windows.Forms.BindingSource> プロパティに、Web サービスのクライアント側プロキシに含まれる任意の型を設定します。  
  
     [!code-cpp[System.Windows.Forms.DataConnectorWebService#2](~/samples/snippets/cpp/VS_Snippets_Winforms/System.Windows.Forms.DataConnectorWebService/CPP/form1.cpp#2)]
     [!code-csharp[System.Windows.Forms.DataConnectorWebService#2](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataConnectorWebService/CS/form1.cs#2)]
     [!code-vb[System.Windows.Forms.DataConnectorWebService#2](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataConnectorWebService/VB/form1.vb#2)]  
  
### <a name="to-bind-controls-to-the-bindingsource-that-is-bound-to-a-web-service"></a>Web サービスにバインドされた BindingSource にコントロールをバインドするには  
  
- コントロールを <xref:System.Windows.Forms.BindingSource> にバインドし、Web サービスの任意の型のパブリック プロパティをパラメーターとして渡します。  
  
     [!code-cpp[System.Windows.Forms.DataConnectorWebService#3](~/samples/snippets/cpp/VS_Snippets_Winforms/System.Windows.Forms.DataConnectorWebService/CPP/form1.cpp#3)]
     [!code-csharp[System.Windows.Forms.DataConnectorWebService#3](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataConnectorWebService/CS/form1.cs#3)]
     [!code-vb[System.Windows.Forms.DataConnectorWebService#3](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataConnectorWebService/VB/form1.vb#3)]  
  
## <a name="example"></a>例  
 <xref:System.Windows.Forms.BindingSource> コンポーネントを Web サービスにバインドし、テキスト ボックスを <xref:System.Windows.Forms.BindingSource> コンポーネントにバインドするコード例を次に示します。 ボタンをクリックすると、Web サービス メソッドが呼び出され、結果が `textbox1` に表示されます。  
  
 [!code-cpp[System.Windows.Forms.DataConnectorWebService#1](~/samples/snippets/cpp/VS_Snippets_Winforms/System.Windows.Forms.DataConnectorWebService/CPP/form1.cpp#1)]
 [!code-csharp[System.Windows.Forms.DataConnectorWebService#1](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataConnectorWebService/CS/form1.cs#1)]
 [!code-vb[System.Windows.Forms.DataConnectorWebService#1](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataConnectorWebService/VB/form1.vb#1)]  
  
## <a name="compiling-the-code"></a>コードのコンパイル  
 これは、`Main` メソッドを含む完全な例であり、クライアント側プロキシのコードを短縮したバージョンです。  
  
 この例で必要な要素は次のとおりです。  
  
- System、System.Drawing、System.Web.Services、System.Windows.Forms、および System.Xml の各アセンブリへの参照。  
  
## <a name="see-also"></a>参照

- [BindingSource コンポーネント](bindingsource-component.md)
- [方法: Windows フォーム コントロールを型にバインドする](how-to-bind-a-windows-forms-control-to-a-type.md)
