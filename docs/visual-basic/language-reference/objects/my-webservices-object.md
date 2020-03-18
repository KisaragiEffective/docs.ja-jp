---
title: My.WebServices オブジェクト
ms.date: 07/20/2015
f1_keywords:
- My.WebServices
- My.MyProject.WebServices
helpviewer_keywords:
- My.WebServices object
ms.assetid: f188dc05-2c75-41b6-bb68-122d1c3110a2
ms.openlocfilehash: 290d025985663bc45fe605a2e9904fc90fb2bc63
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74350348"
---
# <a name="mywebservices-object"></a>My.WebServices オブジェクト
現在のプロジェクトによって参照される各 XML Web サービスの1つのインスタンスを作成してアクセスするためのプロパティを提供します。  
  
## <a name="remarks"></a>コメント  
 `My.WebServices` オブジェクトは、現在のプロジェクトにより参照されている各 Web サービスのインスタンスを提供します。 各インスタンスは要求に応じてインスタンス化されます。 これらの Web サービスには `My.WebServices` オブジェクトのプロパティを介してアクセスできます。 プロパティの名前は、プロパティがアクセスする Web サービスの名前と同じになります。 <xref:System.Web.Services.Protocols.SoapHttpClientProtocol> から継承されたクラスはすべて Web サービスです。 プロジェクトへの Web サービスの追加については、「[アプリケーションの Web サービスへのアクセス](../../../visual-basic/developing-apps/programming/accessing-application-web-services.md)」を参照してください。  
  
 `My.WebServices` オブジェクトは、現在のプロジェクトに関連付けられている Web サービスだけを公開します。 参照先の Dll で宣言されている Web サービスへのアクセスを提供しません。 DLL が提供する Web サービスにアクセスするには、Web サービスの修飾名を*DllName*の形式で使用する必要があります。*Webservicename*。 詳細については、「[アプリケーション Web サービスへのアクセス](../../../visual-basic/developing-apps/programming/accessing-application-web-services.md)」を参照してください。  
  
 オブジェクトとそのプロパティは、Web アプリケーションでは使用できません。  
  
## <a name="properties"></a>プロパティ  
 `My.WebServices` オブジェクトの各プロパティは、現在のプロジェクトによって参照される Web サービスのインスタンスへのアクセスを提供します。 プロパティの名前は、プロパティがアクセスする Web サービスの名前と同じです。プロパティの型は、Web サービスの型と同じです。  
  
> [!NOTE]
> 名前の競合がある場合、Web サービスにアクセスするためのプロパティ名は*RootNamespace*_*名前空間*\_*ServiceName*です。 たとえば、`Service1`という名前の2つの Web サービスを考えてみます。 これらのサービスのいずれかがルート名前空間 `WindowsApplication1` であり、名前空間 `Namespace1`にある場合、`My.WebServices.WindowsApplication1_Namespace1_Service1`を使用してそのサービスにアクセスします。  
  
 `My.WebServices` オブジェクトのいずれかのプロパティに初めてアクセスすると、Web サービスの新しいインスタンスが作成され、保存されます。 そのプロパティの後続のアクセスでは、Web サービスのインスタンスが返されます。  
  
 Web サービスのプロパティに `Nothing` を割り当てることによって、Web サービスを破棄できます。 プロパティセッターは、格納されている値に `Nothing` を割り当てます。 `Nothing` 以外の値をプロパティに割り当てた場合、setter は <xref:System.ArgumentException> 例外をスローします。  
  
 `Is` または `IsNot` 演算子を使用して、`My.WebServices` オブジェクトのプロパティに Web サービスのインスタンスが格納されているかどうかをテストできます。 これらの演算子を使用すると、プロパティの値が `Nothing`かどうかを確認できます。  
  
> [!NOTE]
> 通常、`Is` または `IsNot` 演算子は、比較を実行するためにプロパティの値を読み取る必要があります。 ただし、プロパティが現在 `Nothing`を格納している場合は、プロパティによって Web サービスの新しいインスタンスが作成され、そのインスタンスが返されます。 ただし、Visual Basic コンパイラは、`My.WebServices` オブジェクトのプロパティを特別に処理し、`Is` または `IsNot` の演算子が、値を変更せずにプロパティの状態を確認できるようにします。  
  
## <a name="example"></a>例  
 この例では、`TemperatureConverter` XML Web サービスの `FahrenheitToCelsius` メソッドを呼び出し、その結果を返します。  
  
 [!code-vb[VbVbalrMyWebService#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyWebService/VB/Form1.vb#1)]  
  
 この例を機能させるには、プロジェクトで `Converter`という名前の Web サービスを参照し、その Web サービスが `ConvertTemperature` メソッドを公開する必要があります。 詳細については、「[アプリケーション Web サービスへのアクセス](../../../visual-basic/developing-apps/programming/accessing-application-web-services.md)」を参照してください。  
  
 このコードは、Web アプリケーションプロジェクトでは機能しません。  
  
## <a name="requirements"></a>要件  
  
### <a name="availability-by-project-type"></a>プロジェクトの種類別の可用性  
  
|プロジェクトの種類|利用可能|  
|---|---|  
|Windows アプリケーション|**はい**|  
|クラス ライブラリ|**はい**|  
|コンソール アプリケーション|**はい**|  
|Windows コントロールライブラリ|**はい**|  
|Web コントロールライブラリ|**はい**|  
|Windows サービス|**はい**|  
|Web サイト|いいえ|  
  
## <a name="see-also"></a>参照

- <xref:System.Web.Services.Protocols.SoapHttpClientProtocol>
- <xref:System.ArgumentException>
- [アプリケーションの Web サービスへのアクセス](../../../visual-basic/developing-apps/programming/accessing-application-web-services.md)
