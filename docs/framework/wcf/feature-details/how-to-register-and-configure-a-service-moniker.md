---
title: '方法 : サービス モニカーを登録および構成する'
ms.date: 03/30/2017
helpviewer_keywords:
- COM [WCF], configure service monikers
- COM [WCF], register service monikers
ms.assetid: e5e16c80-8a8e-4eef-af53-564933b651ef
ms.openlocfilehash: fc0b2d00bcc8e3b14c491446f16297c1036b783b
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76747093"
---
# <a name="how-to-register-and-configure-a-service-moniker"></a>方法 : サービス モニカーを登録および構成する

型指定されたコントラクトを持つ COM アプリケーション内で Windows Communication Foundation (WCF) サービスモニカーを使用する前に、必要な属性型を COM に登録し、必要なバインドを使用して COM アプリケーションとモニカーを構成する必要があります。configuration.

## <a name="register-the-required-attributed-types-with-com"></a>必要な属性型を COM に登録する

1. [ServiceModel メタデータユーティリティツール (svcutil.exe)](../servicemodel-metadata-utility-tool-svcutil-exe.md)ツールを使用して、WCF サービスからメタデータコントラクトを取得します。 これにより、WCF クライアントアセンブリとクライアントアプリケーション構成ファイルのソースコードが生成されます。

2. アセンブリ内で定義されている型に `ComVisible` という設定をします。 これを行うには、Visual Studio プロジェクトの*AssemblyInfo.cs*ファイルに次の属性を追加します。

    ```csharp
    [assembly: ComVisible(true)]
    ```

3. マネージ WCF クライアントを厳密な名前付きアセンブリとしてコンパイルします。 そのためには暗号キー ペアで署名する必要があります。 詳しくは、「[厳密な名前でのアセンブリへの署名](../../../standard/assembly/sign-strong-name.md)」をご覧ください。

4. アセンブリ登録 (Regasm.exe) ツールに `-tlb` オプションを指定して、アセンブリで定義されている型を COM に登録します。

5. グローバル アセンブリ キャッシュ ツール (Gacutil.exe) で、グローバル アセンブリ キャッシュにアセンブリを追加します。

    > [!NOTE]
    > アセンブリへの署名とグローバル アセンブリ キャッシュへの追加は、必須ではありません。しかしこれを済ませておくと、実行時には、適切な場所からアセンブリを読み込むための手順が簡単になります。

## <a name="configure-the-com-application-and-the-moniker-with-the-required-binding-configuration"></a>必要なバインド構成を使用して COM アプリケーションとモニカーを構成する

- クライアントアプリケーションの構成ファイルに、生成されたクライアントアプリケーション構成ファイルの[ServiceModel メタデータユーティリティツール (svcutil.exe)](../servicemodel-metadata-utility-tool-svcutil-exe.md)によって生成されたバインディング定義を配置します。 たとえば、Visual Basic 6.0 で開発した実行可能ファイルの名前が CallCenterClient.exe の場合、これと同じディレクトリに、CallCenterConfig.exe.config という名前で構成ファイルを作成します。 するとクライアント アプリケーションはモニカーを使えるようになります。 WCF に用意されている標準のバインディングの型のいずれかを使用する場合は、バインディングの構成は必要ありません。

     次の型が登録されています。

    ```csharp
    using System.ServiceModel;

    [ServiceContract]
    public interface IMathService
    {
        [OperationContract]
        public int Add(int x, int y);
        [OperationContract]
        public int Subtract(int x, int y);
    }
    ```

     このアプリケーションは、`wsHttpBinding` バインディングを使用して公開されています。 このような型とアプリケーション設定であれば、次のようなモニカー文字列が使用されます。

    ```
    service4:address=http://localhost/MathService, binding=wsHttpBinding, bindingConfiguration=Binding1
    ```

     or

    ```
    service4:address=http://localhost/MathService, binding=wsHttpBinding, bindingConfiguration=Binding1, contract={36ADAD5A-A944-4d5c-9B7C-967E4F00A090}
    ```

     `IMathService` 型を定義するアセンブリへの参照を追加すれば、次のコード例のように、Visual Basic 6.0 アプリケーションに上記のモニカー文字列 (どちらの形式でも可) を記述できるようになります。

    ```vb
    Dim mathProxy As IMathService
    Dim result As Integer

    Set mathProxy = GetObject( _
            "service4:address=http://localhost/MathService, _
            binding=wsHttpBinding, _
            bindingConfiguration=Binding1")

    result = mathProxy.Add(3, 5)
    ```

     この例では、バインディング構成 `Binding1` の定義は、 *vb6appname.exe.config*など、クライアントアプリケーション用に適切な名前が付けられた構成ファイルに格納されています。

    > [!NOTE]
    > 同様のコードを C#、C++、およびその他の .NET 言語アプリケーションで使用できます。

    > [!NOTE]
    > モニカーの形式が正しくない場合、またはサービスが使用できない場合、`GetObject` を呼び出すと、"構文が無効です" というエラーが返されます。 このエラーが発生した場合は、使用しているモニカーが正しく、サービスが使用可能であることを確認してください。

     このトピックでは、Visual Basic 6.0 コードのサービスモニカーを使用することに焦点を当てていますが、他の言語のサービスモニカーを使用することもできます。 C++ コードからモニカーを使用している場合、次のコードに示すように、Svcutil.exe によって生成されたアセンブリを、"no_namespace named_guids raw_interfaces_only" と共にインポートする必要があります。

    ```cpp
    #import "ComTestProxy.tlb" no_namespace named_guids
    ```

     これにより、インポートされたインターフェイス定義は、すべてのメソッドが `HResult` を返すように変更されます。 他の戻り値は、出力パラメーターに変換されます。 メソッドの実行全体は、同じままです。 このために、プロキシでメソッドを呼び出したときの例外の原因を特定できます。 この機能は C++ コードからのみ使用できます。

## <a name="see-also"></a>参照

- [ServiceModel メタデータ ユーティリティ ツール (Svcutil.exe)](../servicemodel-metadata-utility-tool-svcutil-exe.md)
