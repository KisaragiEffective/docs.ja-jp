---
title: '方法: 登録を必要としないアクティベーション用の .NET Framework ベースの COM コンポーネントを構成する'
ms.date: 03/30/2017
helpviewer_keywords:
- components [.NET Framework], manifest
- application manifests [.NET Framework]
- manifests [.NET Framework]
- registration-free COM interop, configuring .NET-based components
- activation, registration-free
ms.assetid: 32f8b7c6-3f73-455d-8e13-9846895bd43b
ms.openlocfilehash: 61f5f0f3ec9a4386fa12e7511b4a518f2b56a21c
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73123666"
---
# <a name="how-to-configure-net-framework-based-com-components-for-registration-free-activation"></a>方法: 登録を必要としないアクティベーション用の .NET Framework ベースの COM コンポーネントを構成する
.NET Framework ベースのコンポーネントの登録を必要としないアクティベーションは、COM コンポーネントの場合よりも少しだけ複雑です。 セットアップには 2 つのマニフェストが必要です。  
  
- COM アプリケーションには、マネージド コンポーネントを識別するための Win32 スタイルのアプリケーション マニフェストが必要です。  
  
- .NET Framework ベースのコンポーネントには、実行時に必要なアクティベーション情報のコンポーネント マニフェストが必要です。  
  
 このトピックでは、アプリケーション マニフェストをアプリケーションに関連付ける方法、コンポーネント マニフェストをコンポーネントに関連付ける方法、およびコンポーネント マニフェストをアセンブリに埋め込む方法について説明します。  
  
### <a name="to-create-an-application-manifest"></a>アプリケーション マニフェストを作成するには  
  
1. XML エディターを使用して、1 つ以上のマネージド コンポーネントと相互運用する COM アプリケーションによって所有されるアプリケーション マニフェストを作成または編集します。  
  
2. ファイルの先頭に次の標準ヘッダーを挿入します。  
  
    ```xml  
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>  
    <assembly xmlns="urn:schemas-microsoft-com:asm.v1" manifestVersion="1.0">  
    ```  
  
     マニフェストの要素とその属性については、「[Application Manifests](/windows/desktop/SbsCs/application-manifests)」(アプリケーション マニフェスト) をご覧ください。  
  
3. マニフェストの所有者を指定します。 次の例では、`myComApp` バージョン 1 がマニフェスト ファイルを所有しています。  
  
    ```xml  
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>  
    <assembly xmlns="urn:schemas-microsoft-com:asm.v1" manifestVersion="1.0">  
      <assemblyIdentity type="win32"   
                        name="myOrganization.myDivision.myComApp"   
                        version="1.0.0.0"   
                        processorArchitecture="msil"   
      />  
    ```  
  
4. 依存アセンブリを指定します。 `myComApp` が `myManagedComp` に依存する例を次に示します。  
  
    ```xml  
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>  
    <assembly xmlns="urn:schemas-microsoft-com:asm.v1" manifestVersion="1.0">  
      <assemblyIdentity type="win32"   
                        name="myOrganization.myDivision.myComApp"   
                        version="1.0.0.0"   
                        processorArchitecture="x86"   
                        publicKeyToken="8275b28176rcbbef"  
      />  
      <dependency>  
        <dependentAssembly>  
          <assemblyIdentity type="win32"   
                        name="myOrganization.myDivision.myManagedComp"   
                        version="6.0.0.0"   
                        processorArchitecture="X86"   
                        publicKeyToken="8275b28176rcbbef"  
          />  
        </dependentAssembly>  
      </dependency>  
    </assembly>  
    ```  
  
5. マニフェスト ファイルに名前を付けて保存します。 アプリケーション マニフェストの名前は、アセンブリ実行可能ファイルの名前に拡張子 .manifest が付いたものです。 たとえば、myComApp.exe のアプリケーション マニフェスト ファイル名は myComApp.exe.manifest です。  
  
 アプリケーション マニフェストは、COM アプリケーションと同じディレクトリにインストールできます。 また、アプリケーションの .exe ファイルにリソースとして追加することもできます。 詳しくは、「[About Side-by-Side Assemblies](/windows/desktop/SbsCs/about-side-by-side-assemblies-)」(side-by-side アセンブリについて) をご覧ください。  
  
#### <a name="to-create-a-component-manifest"></a>コンポーネント マニフェストを作成するには  
  
1. XML エディターを使用して、マネージド アセンブリを記述するコンポーネント マニフェストを作成します。  
  
2. ファイルの先頭に次の標準ヘッダーを挿入します。  
  
    ```xml  
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>  
    <assembly xmlns="urn:schemas-microsoft-com:asm.v1" manifestVersion="1.0">  
    ```  
  
3. ファイルの所有者を指定します。 アプリケーション マニフェスト ファイル内の `<assemblyIdentity>` 要素の `<dependentAssembly>` 要素は、コンポーネント マニフェスト内の要素と一致している必要があります。 次の例では、`myManagedComp` バージョン 1.2.3.4 がマニフェスト ファイルを所有しています。  
  
    ```xml  
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>  
    <assembly xmlns="urn:schemas-microsoft-com:asm.v1" manifestVersion="1.0">  
           <assemblyIdentity  
                        name="myOrganization.myDivision.myManagedComp"  
                        version="1.2.3.4"  
                        publicKeyToken="8275b28176rcbbef"  
                        processorArchitecture="msil"  
           />  
    ```  
  
4. アセンブリ内の各クラスを指定します。 マネージド アセンブリ内の各クラスを一意に識別するには `<clrClass>` 要素を使用します。 この要素は、`<assembly>` 要素のサブ要素であり、次の表に示す属性を持っています。  
  
    |属性|説明|必要|  
    |---------------|-----------------|--------------|  
    |`clsid`|アクティブにするクラスを指定する識別子。|[はい]|  
    |`description`|ユーザーにコンポーネントを説明する文字列。 既定では文字列は空です。|Ｘ|  
    |`name`|マネージド クラスを表す文字列。|[はい]|  
    |`progid`|遅延バインディングによるアクティベーションで使用される識別子。|Ｘ|  
    |`threadingModel`|COM スレッド モデル。 "Both" が既定値です。|Ｘ|  
    |`runtimeVersion`|使用する共通言語ランタイム (CLR: Common Language Runtime) のバージョンを指定します。 この属性を指定せず、CLR がまだ読み込まれていない場合は、インストールされている最新の CLR (CLR Version 4 よりも前のバージョン) でコンポーネントが読み込まれます。 v1.0.3705、v1.1.4322、または v2.0.50727 を指定すると、インストールされている最新の CLR バージョン (CLR Version 4 よりも前のバージョン。通常は v2.0.50727) に自動的にロールフォワードされます。 別のバージョンの CLR が既に読み込まれていて、指定されたバージョンをインプロセスで並行して (side-by-side で) 読み込むことができる場合は、指定されたバージョンが読み込まれます。それ以外の場合は、読み込まれた CLR が使用されます。 これにより、読み込みエラーが発生する可能性があります。|Ｘ|  
    |`tlbid`|クラスに関する型情報を格納するタイプ ライブラリの識別子。|Ｘ|  
  
     属性タグはすべて大文字と小文字が区別されます。 OLE/COM オブジェクト ビューアー (Oleview.exe) で、エクスポートされたアセンブリのタイプ ライブラリを表示することによって、CLSID、ProgID、スレッド モデル、およびランタイムのバージョンを取得できます。  
  
     `testClass1` および `testClass2` という 2 つのクラスを識別するコンポーネント マニフェストを次に示します。  
  
    ```xml  
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>  
    <assembly xmlns="urn:schemas-microsoft-com:asm.v1" manifestVersion="1.0">  
           <assemblyIdentity  
                        name="myOrganization.myDivision.myManagedComp"  
                        version="1.2.3.4"   
                        publicKeyToken="8275b28176rcbbef"  
           />  
           <clrClass  
                        clsid="{65722BE6-3449-4628-ABD3-74B6864F9739}"  
                        progid="myManagedComp.testClass1"  
                        threadingModel="Both"  
                        name="myManagedComp.testClass1"  
                        runtimeVersion="v1.0.3705">  
           </clrClass>  
           <clrClass  
                        clsid="{367221D6-3559-3328-ABD3-45B6825F9732}"  
                        progid="myManagedComp.testClass2"  
                        threadingModel="Both"  
                        name="myManagedComp.testClass2"  
                        runtimeVersion="v1.0.3705">  
           </clrClass>  
           <file name="MyManagedComp.dll">  
           </file>  
    </assembly>  
    ```  
  
5. マニフェスト ファイルに名前を付けて保存します。 コンポーネント マニフェストの名前は、アセンブリ ライブラリの名前に拡張子 .manifest が付いたものです。 たとえば、myManagedComp.dll の場合は、myManagedComp.manifest になります。  
  
 コンポーネント マニフェストはリソースとしてアセンブリに埋め込む必要があります。  
  
#### <a name="to-embed-a-component-manifest-in-a-managed-assembly"></a>コンポーネント マニフェストをマネージド アセンブリに埋め込むには  
  
1. 次のステートメントを含むリソース スクリプトを作成します。  
  
     `RT_MANIFEST 1 myManagedComp.manifest`  
  
     このステートメントで、`myManagedComp.manifest` は埋め込むコンポーネント マニフェストの名前です。 この例では、スクリプト ファイル名は `myresource.rc` です。  
  
2. Microsoft Windows リソース コンパイラ (Rc.exe) を使用してスクリプトをコンパイルします。 コマンド プロンプトに次のコマンドを入力します。  
  
     `rc myresource.rc`  
  
     Rc.exe は `myresource.res` リソース ファイルを生成します。  
  
3. もう一度アセンブリのソース ファイルをコンパイルし、 **/win32res** オプションを使用してリソース ファイルを指定します。  
  
    `/win32res:myresource.res`  
  
     ここでも、`myresource.res` は埋め込むリソースを含むリソース ファイルの名前です。  
  
## <a name="see-also"></a>関連項目

- [登録を必要としない COM 相互運用機能](registration-free-com-interop.md)
- [登録を必要としない COM 相互運用機能の要件](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/f8h7012w(v=vs.100))
- [登録を必要としないアクティベーション用の COM コンポーネントの構成](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/x65a421a(v=vs.100))
- [.NET ベースのコンポーネントの登録を必要としないアクティベーション: チュートリアル](https://docs.microsoft.com/previous-versions/dotnet/articles/ms973915(v=msdn.10))
