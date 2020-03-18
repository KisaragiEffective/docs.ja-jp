---
title: '方法 : サンドボックスで部分信頼コードを実行する'
ms.date: 03/30/2017
helpviewer_keywords:
- partially trusted code
- sandboxing
- partial trust
- restricted security environment
- code security, sandboxing
ms.assetid: d1ad722b-5b49-4040-bff3-431b94bb8095
ms.openlocfilehash: 0191846f5589b0162ba342161fb5919ff20099d4
ms.sourcegitcommit: 9c54866bcbdc49dbb981dd55be9bbd0443837aa2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/14/2020
ms.locfileid: "77215854"
---
# <a name="how-to-run-partially-trusted-code-in-a-sandbox"></a>方法 : サンドボックスで部分信頼コードを実行する
[!INCLUDE[net_security_note](../../../includes/net-security-note-md.md)]  
  
 サンドボックス化は、制限されたセキュリティ環境でコードを実行する方法です。この方法を採用すると、コードに付与されるアクセス許可が制限されます。 たとえば、完全に信頼していない発行元のマネージド ライブラリは、完全信頼として実行しないようにする必要があります。 代わりに、必要なアクセス許可 (<xref:System.Security.Permissions.SecurityPermissionFlag.Execution> アクセス許可など) のみを与えるサンドボックスにコードを配置します。  
  
 また、部分的に信頼された環境で実行する予定のコードについて、サンドボックスを使用してテストすることもできます。  
  
 <xref:System.AppDomain> を使用すると、マネージド アプリケーションのサンドボックスを効果的に提供できます。 部分信頼コードの実行時に使用するアプリケーション ドメインには、<xref:System.AppDomain> 内で実行する際に利用できる保護対象リソースを定義するアクセス許可が与えられます。 <xref:System.AppDomain> 内で実行されるコードは、<xref:System.AppDomain> に関連付けられているアクセス許可によって制約され、指定したリソースにのみアクセスできます。 また、<xref:System.AppDomain> には、完全信頼として読み込むアセンブリを特定する <xref:System.Security.Policy.StrongName> 配列も含まれます。 これによって <xref:System.AppDomain> の作成者は、新しいサンドボックス化されたドメインを開始して、特定のヘルパー アセンブリを完全信頼とすることができます。 アセンブリを完全信頼として読み込むには、グローバル アセンブリ キャッシュに配置する方法もあります。ただし、そのコンピューター上に作成されたすべてのアプリケーション ドメインに、アセンブリが完全信頼として読み込まれます。 厳密な名前の一覧は <xref:System.AppDomain> ごとに決定できるので、対象アセンブリのより限定的な指定が可能になります。  
  
 サンドボックスで実行するアプリケーションのアクセス許可セットを指定するには、<xref:System.AppDomain.CreateDomain%28System.String%2CSystem.Security.Policy.Evidence%2CSystem.AppDomainSetup%2CSystem.Security.PermissionSet%2CSystem.Security.Policy.StrongName%5B%5D%29?displayProperty=nameWithType> メソッド オーバーロードを使用します。 このオーバーロードを使用することで、コード アクセス セキュリティのレベルを希望どおりに設定できます。 このオーバーロードを使用して <xref:System.AppDomain> に読み込まれるアセンブリには、指定の許可セットのみを付与することもできますし、完全信頼を付与することもできます。 グローバル アセンブリ キャッシュ内のアセンブリ、または `fullTrustAssemblies` (<xref:System.Security.Policy.StrongName>) 配列パラメーターに列挙されているアセンブリには、完全信頼が付与されます。 完全に信頼できるアセンブリだけを `fullTrustAssemblies` リストに追加する必要があります。  
  
 オーバーロードは次のシグネチャを持ちます。  
  
```csharp
AppDomain.CreateDomain( string friendlyName,  
                        Evidence securityInfo,  
                        AppDomainSetup info,  
                        PermissionSet grantSet,  
                        params StrongName[] fullTrustAssemblies);  
```  
  
 <xref:System.AppDomain.CreateDomain%28System.String%2CSystem.Security.Policy.Evidence%2CSystem.AppDomainSetup%2CSystem.Security.PermissionSet%2CSystem.Security.Policy.StrongName%5B%5D%29> メソッド オーバーロードのパラメーターでは、<xref:System.AppDomain> の名前、<xref:System.AppDomain> の証拠、サンドボックスのアプリケーション ベースを指定する <xref:System.AppDomainSetup> オブジェクト、使用するアクセス許可セット、および完全に信頼されたアセンブリの厳密な名前を指定します。  
  
 セキュリティ上の理由から、`info` パラメーターに指定するアプリケーション ベースに、ホスト アプリケーションのアプリケーション ベースを指定することはできません。  
  
 `grantSet` パラメーターには、明示的に作成したアクセス許可セットを指定するか、<xref:System.Security.SecurityManager.GetStandardSandbox%2A> メソッドによって作成された標準アクセス許可セットを指定します。  
  
 他の多くの <xref:System.AppDomain> 読み込みとは異なり、部分的に信頼されたアセンブリの許可セットの決定に (<xref:System.AppDomain> パラメーターで指定される) `securityInfo` の証拠は使用されません。 代わりに、`grantSet` パラメーターによって単独で指定されます。 ただし、分離ストレージのスコープの決定など、他の用途に証拠を使用できます。  
  
### <a name="to-run-an-application-in-a-sandbox"></a>サンドボックスでアプリケーションを実行するには  
  
1. 信頼関係のないアプリケーションに付与するアクセス許可セットを作成します。 付与できる最小限のアクセス許可は <xref:System.Security.Permissions.SecurityPermissionFlag.Execution> です。 また、信頼関係のないコードでも安全と考えられる追加のアクセス許可を付与することもできます。たとえば、<xref:System.Security.Permissions.IsolatedStorageFilePermission> などです。 次のコードでは、<xref:System.Security.Permissions.SecurityPermissionFlag.Execution> アクセス許可のみを含む新しいアクセス許可セットを作成します。  
  
    ```csharp
    PermissionSet permSet = new PermissionSet(PermissionState.None);  
    permSet.AddPermission(new SecurityPermission(SecurityPermissionFlag.Execution));  
    ```  
  
     また、"Internet" など、既存の名前付きアクセス許可セットを使用することもできます。  
  
    ```csharp
    Evidence ev = new Evidence();  
    ev.AddHostEvidence(new Zone(SecurityZone.Internet));  
    PermissionSet internetPS = SecurityManager.GetStandardSandbox(ev);  
    ```  
  
     証拠のゾーンに応じて、<xref:System.Security.SecurityManager.GetStandardSandbox%2A> メソッドは `Internet` アクセス許可セットまたは `LocalIntranet` アクセス許可セットを返します。 また、<xref:System.Security.SecurityManager.GetStandardSandbox%2A> は、参照として渡される一部の証拠オブジェクトの ID アクセス許可を構築します。  
  
2. 信頼関係のないコードを呼び出すホスト クラス (この例では `Sandboxer`) を含むアセンブリに署名します。 アセンブリの署名で使用した <xref:System.Security.Policy.StrongName> を、<xref:System.Security.Policy.StrongName> を呼び出す際の `fullTrustAssemblies` パラメーターに <xref:System.AppDomain.CreateDomain%2A> 配列として追加します。 部分信頼コードを実行できるようにする場合、または部分信頼アプリケーションにサービスを提供する場合は、ホスト クラスを完全信頼として実行する必要があります。 アセンブリの <xref:System.Security.Policy.StrongName> を読み取る方法は次のとおりです。  
  
    ```csharp
    StrongName fullTrustAssembly = typeof(Sandboxer).Assembly.Evidence.GetHostEvidence<StrongName>();  
    ```  
  
     mscorlib や System.dll などの .NET Framework アセンブリを完全信頼一覧に追加することはできません。グローバル アセンブリ キャッシュから完全信頼として読み込まれているためです。  
  
3. <xref:System.AppDomainSetup> メソッドの <xref:System.AppDomain.CreateDomain%2A> パラメーターを初期化します。 このパラメーターを使用すると、新しい <xref:System.AppDomain> の多くの設定を制御できます。 <xref:System.AppDomainSetup.ApplicationBase%2A> プロパティは重要な設定なので、ホスト アプリケーションの <xref:System.AppDomainSetup.ApplicationBase%2A> の <xref:System.AppDomain> プロパティとは違う設定にする必要があります。 <xref:System.AppDomainSetup.ApplicationBase%2A> 設定が同じ場合、部分的に信頼されたアプリケーションは、ホスト アプリケーションが定義する例外を完全信頼として読み込み、それを悪用できるようになります。 これは、キャッシュ (例外) が推奨されないもう 1 つの理由です。 ホストのアプリケーション ベースと、サンドボックス アプリケーションのアプリケーション ベースを異なる設定にすると、悪用のリスクが軽減されます。  
  
    ```csharp
    AppDomainSetup adSetup = new AppDomainSetup();  
    adSetup.ApplicationBase = Path.GetFullPath(pathToUntrusted);  
    ```  
  
4. <xref:System.AppDomain.CreateDomain%28System.String%2CSystem.Security.Policy.Evidence%2CSystem.AppDomainSetup%2CSystem.Security.PermissionSet%2CSystem.Security.Policy.StrongName%5B%5D%29> メソッド オーバーロードを呼び出し、指定したパラメーターを使用してアプリケーション ドメインを作成します。  
  
     このメソッドのシグネチャは次のとおりです。  
  
    ```csharp
    public static AppDomain CreateDomain(string friendlyName,   
        Evidence securityInfo, AppDomainSetup info, PermissionSet grantSet,   
        params StrongName[] fullTrustAssemblies)  
    ```  
  
     追加情報:  
  
    - これは、パラメーターとして <xref:System.AppDomain.CreateDomain%2A> を使用する <xref:System.Security.PermissionSet> メソッドの唯一のオーバーロードです。つまり、部分信頼設定でアプリケーションを読み込むことができる唯一のオーバーロードになります。  
  
    - `evidence` パラメーターは、アクセス許可セットの計算では使用されません。このパラメーターは、.NET Framework の他の機能が識別目的で使用します。  
  
    - このオーバーロードでは、<xref:System.AppDomainSetup.ApplicationBase%2A> パラメーターの `info` プロパティを必ず設定します。  
  
    - `fullTrustAssemblies` パラメーターには `params` キーワードがあります。これは <xref:System.Security.Policy.StrongName> 配列を作成する必要がないことを示します。 パラメーターとして、0 個、1 個、または複数の厳密な名前を渡すことができます。  
  
    - アプリケーション ドメインを作成するコードは次のとおりです。  
  
    ```csharp
    AppDomain newDomain = AppDomain.CreateDomain("Sandbox", null, adSetup, permSet, fullTrustAssembly);  
    ```  
  
5. 作成したサンドボックスの <xref:System.AppDomain> にコードを読み込みます。 これは、2 つの方法で実行できます。  
  
    - アセンブリに対して <xref:System.AppDomain.ExecuteAssembly%2A> メソッドを呼び出します。  
  
    - <xref:System.Activator.CreateInstanceFrom%2A> メソッドを使用して、<xref:System.MarshalByRefObject> から派生したクラスのインスタンスを新しい <xref:System.AppDomain> に作成します。  
  
     2 番目の方が望ましい方法です。新しい <xref:System.AppDomain> インスタンスにパラメーターを渡しやすいためです。 <xref:System.Activator.CreateInstanceFrom%2A> メソッドには 2 つの重要な機能があります。  
  
    - アセンブリが保存されていない場所を示すコード ベースを使用できます。  
  
    - <xref:System.Security.CodeAccessPermission.Assert%2A> の下で、完全信頼 (<xref:System.Security.Permissions.PermissionState.Unrestricted?displayProperty=nameWithType>) で作成操作を実行できます。こうすることで、重要なクラスのインスタンスを作成できます。 (これは、アセンブリに透明度のマークがなく、完全に信頼されているものとして読み込まれるたびに発生します)。したがって、この関数で信頼するコードのみを作成するように注意する必要があります。また、新しいアプリケーションドメインでは、完全に信頼されたクラスのインスタンスのみを作成することをお勧めします。  
  
    ```csharp
    ObjectHandle handle = Activator.CreateInstanceFrom(  
    newDomain, typeof(Sandboxer).Assembly.ManifestModule.FullyQualifiedName,  
           typeof(Sandboxer).FullName );  
    ```  
  
     新しいドメインにクラスのインスタンスを作成するには、そのクラスで <xref:System.MarshalByRefObject> クラスを拡張する必要があります。  
  
    ```csharp
    class Sandboxer:MarshalByRefObject  
    ```  
  
6. 新しいドメイン インスタンスをこのドメイン内の参照にラップ解除します。 この参照は、信頼関係のないコードを実行するときに使用されます。  
  
    ```csharp
    Sandboxer newDomainInstance = (Sandboxer) handle.Unwrap();  
    ```  
  
7. 作成した `ExecuteUntrustedCode` クラスのインスタンスで、`Sandboxer` メソッドを呼び出します。  
  
    ```csharp
    newDomainInstance.ExecuteUntrustedCode(untrustedAssembly, untrustedClass, entryPoint, parameters);  
    ```  
  
     この呼び出しは、アクセス許可が制限されているサンドボックス アプリケーション ドメインで実行されます。  
  
    ```csharp
    public void ExecuteUntrustedCode(string assemblyName, string typeName, string entryPoint, Object[] parameters)  
    {  
        //Load the MethodInfo for a method in the new assembly. This might be a method you know, or   
        //you can use Assembly.EntryPoint to get to the entry point in an executable.  
        MethodInfo target = Assembly.Load(assemblyName).GetType(typeName).GetMethod(entryPoint);  
        try  
        {  
            // Invoke the method.  
            target.Invoke(null, parameters);  
        }  
        catch (Exception ex)  
        {  
        //When information is obtained from a SecurityException extra information is provided if it is   
        //accessed in full-trust.  
            new PermissionSet(PermissionState.Unrestricted).Assert();  
            Console.WriteLine("SecurityException caught:\n{0}", ex.ToString());  
            CodeAccessPermission.RevertAssert();  
            Console.ReadLine();  
        }  
    }  
    ```  
  
     <xref:System.Reflection> を使用して、部分的に信頼されたアセンブリでメソッドのハンドルを取得します。 このハンドルは、最小限のアクセス許可を持つ安全な方法でコードを実行するために使用できます。  
  
     上記のコードでは、<xref:System.Security.PermissionSet.Assert%2A> を出力する前に完全信頼のアクセス許可の <xref:System.Security.SecurityException> があることがわかります。  
  
    ```csharp
    new PermissionSet(PermissionState.Unrestricted).Assert()  
    ```  
  
     完全信頼アサートは、<xref:System.Security.SecurityException> から拡張情報を取得するために使用します。 <xref:System.Security.PermissionSet.Assert%2A> を使用しない場合、<xref:System.Security.SecurityException.ToString%2A> の <xref:System.Security.SecurityException> メソッドは、スタック上に部分的に信頼されたコードがあることを検出します。また、返される情報を制限します。 部分信頼コードがその情報を読み取ることができる場合、これによってセキュリティ上の問題が発生する可能性もありますが、<xref:System.Security.Permissions.UIPermission> を付与しないことでそのリスクは軽減されます。 完全信頼アサートは慎重に使用する必要があります。部分信頼コードが完全信頼に昇格できないことが確実である場合にのみ使用します。 通則として、信頼できないコードは、完全信頼のアサートを呼び出した後に同じ関数で呼び出すのは避ける必要があります。 アサートの使用を完了したときは、常にアサートを元に戻すことをお勧めします。  
  
## <a name="example"></a>例  
 次の例では、前のセクションの手順を実装しています。 この例では、Visual Studio ソリューションの `Sandboxer` というプロジェクトにも `UntrustedCode` というプロジェクトが含まれます。これはクラス `UntrustedClass` を実装します。 このシナリオでは、指定した数字がフィボナッチ数かどうかを示すために、`true` または `false` を返すことが期待されているメソッドを含むライブラリ アセンブリをダウンロードしたことを想定しています。 代わりに、このメソッドはコンピューターからのファイルの読み込みを試みます。 次の例は信頼関係のないコードを示します。  
  
```csharp
using System;  
using System.IO;  
namespace UntrustedCode  
{  
    public class UntrustedClass  
    {  
        // Pretend to be a method checking if a number is a Fibonacci  
        // but which actually attempts to read a file.  
        public static bool IsFibonacci(int number)  
        {  
           File.ReadAllText("C:\\Temp\\file.txt");  
           return false;  
        }  
    }  
}  
```  
  
 次の例は、信頼関係のないコードを実行する `Sandboxer` アプリケーション コードを示します。  
  
```csharp
using System;  
using System.Collections.Generic;  
using System.Linq;  
using System.Text;  
using System.IO;  
using System.Security;  
using System.Security.Policy;  
using System.Security.Permissions;  
using System.Reflection;  
using System.Runtime.Remoting;  
  
//The Sandboxer class needs to derive from MarshalByRefObject so that we can create it in another   
// AppDomain and refer to it from the default AppDomain.  
class Sandboxer : MarshalByRefObject  
{  
    const string pathToUntrusted = @"..\..\..\UntrustedCode\bin\Debug";  
    const string untrustedAssembly = "UntrustedCode";  
    const string untrustedClass = "UntrustedCode.UntrustedClass";  
    const string entryPoint = "IsFibonacci";  
    private static Object[] parameters = { 45 };  
    static void Main()  
    {  
        //Setting the AppDomainSetup. It is very important to set the ApplicationBase to a folder   
        //other than the one in which the sandboxer resides.  
        AppDomainSetup adSetup = new AppDomainSetup();  
        adSetup.ApplicationBase = Path.GetFullPath(pathToUntrusted);  
  
        //Setting the permissions for the AppDomain. We give the permission to execute and to   
        //read/discover the location where the untrusted code is loaded.  
        PermissionSet permSet = new PermissionSet(PermissionState.None);  
        permSet.AddPermission(new SecurityPermission(SecurityPermissionFlag.Execution));  
  
        //We want the sandboxer assembly's strong name, so that we can add it to the full trust list.  
        StrongName fullTrustAssembly = typeof(Sandboxer).Assembly.Evidence.GetHostEvidence<StrongName>();  
  
        //Now we have everything we need to create the AppDomain, so let's create it.  
        AppDomain newDomain = AppDomain.CreateDomain("Sandbox", null, adSetup, permSet, fullTrustAssembly);  
  
        //Use CreateInstanceFrom to load an instance of the Sandboxer class into the  
        //new AppDomain.   
        ObjectHandle handle = Activator.CreateInstanceFrom(  
            newDomain, typeof(Sandboxer).Assembly.ManifestModule.FullyQualifiedName,  
            typeof(Sandboxer).FullName  
            );  
        //Unwrap the new domain instance into a reference in this domain and use it to execute the   
        //untrusted code.  
        Sandboxer newDomainInstance = (Sandboxer) handle.Unwrap();  
        newDomainInstance.ExecuteUntrustedCode(untrustedAssembly, untrustedClass, entryPoint, parameters);  
    }  
    public void ExecuteUntrustedCode(string assemblyName, string typeName, string entryPoint, Object[] parameters)  
    {  
        //Load the MethodInfo for a method in the new Assembly. This might be a method you know, or   
        //you can use Assembly.EntryPoint to get to the main function in an executable.  
        MethodInfo target = Assembly.Load(assemblyName).GetType(typeName).GetMethod(entryPoint);  
        try  
        {  
            //Now invoke the method.  
            bool retVal = (bool)target.Invoke(null, parameters);  
        }  
        catch (Exception ex)  
        {  
            // When we print informations from a SecurityException extra information can be printed if we are   
            //calling it with a full-trust stack.  
            new PermissionSet(PermissionState.Unrestricted).Assert();  
            Console.WriteLine("SecurityException caught:\n{0}", ex.ToString());  
            CodeAccessPermission.RevertAssert();  
            Console.ReadLine();  
        }  
    }  
}  
```  
  
## <a name="see-also"></a>参照

- [安全なコーディングのガイドライン](../../standard/security/secure-coding-guidelines.md)
