---
title: Mgmtclassgen.exe (厳密型クラス ジェネレーター)
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- CIM types
- Management Strongly Typed Class Generator
- WMI class
- Mgmtclassgen.exe
- early-bound managed classes
ms.assetid: 02ce6699-49b5-4a0b-b0d5-1003c491232e
ms.openlocfilehash: 5002d7a180e480b0e1d38f1c1180fe565dc5e1dc
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73105017"
---
# <a name="mgmtclassgenexe-management-strongly-typed-class-generator"></a>Mgmtclassgen.exe (厳密型クラス ジェネレーター)
厳密型クラス ジェネレーター (Mgmtclassgen.exe) ツールを使用すると、指定した WMI (Windows Management Instrumentation) クラスに対して、事前バインディングされたマネージド クラスをすばやく生成できます。 生成されたクラスを使用すると、WMI クラスのインスタンスにアクセスするために書く必要のあるコードを簡略化できます。  
  
## <a name="syntax"></a>構文  
  
```console  
mgmtclassgen   
WMIClass [options]   
```  
  
|引数|説明|  
|--------------|-----------------|  
|*WMIClass*|事前バインディングしたマネージド クラスを生成する対象の WMI クラスです。|  
  
|オプション|説明|  
|------------|-----------------|  
|**/l**  *language*|事前バインディングされたマネージド クラスの生成に使用する言語を指定します。 language 引数として **CS** (C#、既定値)、**VB** (Visual Basic)、**MC** (C++)、または **JS** (JScript) を指定できます。|  
|**/m**  *machine*|WMI クラスが常駐する、接続先のコンピューターを指定します。 既定値はローカル コンピューターです。|  
|**/n**  *path*|WMI クラスが含まれている WMI 名前空間へのパスを指定します。 このオプションを指定しない場合、このツールは既定の **Root\cimv2** 名前空間に *WMIClass*のコードを生成します。|  
|**/o**  *classnamespace*|マネージド コード クラスを生成する先の .NET 名前空間を指定します。 このオプションを指定しない場合、このツールは WMI 名前空間およびスキーマ プリフィックスを使用して名前空間を指定します。 スキーマ プリフィックスはクラス名の一部で、アンダースコア (_) 文字の前に付きます。 たとえば、**Root\cimv2** 名前空間内の **Win32_OperatingSystem**クラスの場合、このツールは **ROOT.CIMV2.Win32** にクラスを生成します。|  
|**/p**  *filepath*|生成されたコードを保存するファイルへのパスを指定します。 このオプションを指定しない場合、このツールは現在のディレクトリにファイルを作成します。 *WMIClass* 引数を使用して生成したクラスと、そのクラスを保存したファイルには名前が付けられます。 クラスおよびファイルの名前は、*WMIClass* の名前と同じです。 *WMIClass* にアンダースコア (_) 文字が含まれている場合は、アンダースコア (_) 文字の後に続く、クラス名の一部が使用されます。 たとえば、*WMIClass* 名が **Win32_LogicalDisk** という形式の場合、生成されたクラスおよびファイルには "logicaldisk" という名前が付けられます。 ファイルが既に存在している場合は、既存のファイルが上書きされます。|  
|**/pw**  *password*|**/m** オプションで指定したコンピューターにログオンするときに使用するパスワードを指定します。|  
|**/u**  *user name*|**/m** オプションで指定したコンピューターにログオンするときに使用するユーザー名を指定します。|  
|**/?**|このツールのコマンド構文とオプションを表示します。|  
  
## <a name="remarks"></a>解説  
 Mgmtclassgen.exe は、<xref:System.Management.ManagementClass.GetStronglyTypedClassCode%2A?displayProperty=nameWithType> メソッドを使用します。 したがって、任意のカスタム コード プロバイダーを使用して、C#、Visual Basic、および JScript 以外のマネージド言語でコードを生成できます。  
  
 生成されたクラスは、それらのクラスを生成する目的となったスキーマにバインドされます。 基になるスキーマが変更された場合に、その変更をこのスキーマに反映するには、クラスを生成し直す必要があります。  
  
 WMI CIM (Common Information Model) 型の、生成されたクラスのデータ型への割り当てを次の表に示します。  
  
|CIM 型|生成したクラスのデータ型|  
|--------------|--------------------------------------|  
|CIM_SINT8|**SByte**|  
|CIM_UINT8|**Byte**|  
|CIM_SINT16|**Int16**|  
|CIM_UINT16|**UInt16**|  
|CIM_SINT32|**Int32**|  
|SIM_UINT32|**UInt32**|  
|CIM_SINT64|**Int64**|  
|CIM_UINT64|**UInt64**|  
|CIM_REAL32|**Single**|  
|CIM_REAL64|**Double**|  
|CIM_BOOLEAN|**Boolean**|  
|CIM_String|**String**|  
|CIM_DATETIME|**DateTime** または **TimeSpan**|  
|CIM_REFERENCE|**ManagementPath**|  
|CIM_CHAR16|**Char**|  
|CIM_OBJECT|**ManagementBaseObject**|  
|CIM_IUNKNOWN|**オブジェクト**|  
|CIM_ARRAY|上記のオブジェクトの配列|  
  
 WMI クラスを生成する場合は、次の動作に注意してください｡  
  
- 標準パブリック プロパティまたはメソッドの名前と既存のプロパティまたはメソッドの名前が同じになる可能性があります。 このような事態が発生した場合、このツールは生成したクラスのプロパティまたはメソッドの名前を変更して、名前付けの競合を回避します。  
  
- 生成したクラスのプロパティまたはメソッドの名前がプログラミング言語のキーワード名と同じになる可能性があります。 このような事態が発生した場合、このツールは生成したクラスのプロパティまたはメソッドの名前を変更して、名前付けの競合を回避します。  
  
- WMI では、修飾子は、クラス、インスタンス、プロパティ、またはメソッドを説明する情報を含む修飾子となります。 WMI は、**Read**、**Write**、**Key** などの標準の修飾子を使用して、生成したクラスのプロパティについて説明します。 たとえば、**Read** 修飾子で修飾されたプロパティは、生成したクラスではプロパティ **get** アクセサーだけを伴って定義されます。 **Read** 修飾子でマークされたプロパティは読み取り専用であるため、**set** アクセサーは定義されません。  
  
- 数値プロパティを **Values** 修飾子および **ValueMaps** 修飾子によって修飾すると、このプロパティには、指定した許容値しか設定できないことを示すことができます。 列挙体は、このような **Values** および **ValueMaps** を使用して生成され、このプロパティは列挙体に割り当てられます。  
  
- WMI では、インスタンスを 1 つだけを持つことのできるクラスを説明するために、シングルトンという用語を使用します。 したがって、シングルトン クラスのパラメーターなしのコンストラクターは、クラスをそのクラスの唯一のインスタンスに初期化します。  
  
- WMI クラスは、オブジェクトであるプロパティを持つ場合があります。 そのような型の WMI クラスに対して、厳密に型指定されたクラスを生成する場合は、埋め込まれたオブジェクト プロパティの型それぞれに対して厳密に型指定されたクラスを生成することを検討してください。 これにより、厳密に型指定する方法で、埋め込まれたオブジェクトにアクセスできるようになります。 生成されたコードが、埋め込まれたオブジェクトの型を検出できない場合もあります。 このような場合は、生成されたコード内に、その旨をユーザーに通知するコメントが作成されます。 これに応じて、生成された他のクラスをプロパティの型として指定するように、生成したコードを変更できます。  
  
- WMI では、CIM_DATETIME データ型のデータ値は、特定の日時または時間間隔を表すことができます。 データ値が日時を表す場合、生成されたクラスのデータ型は **DateTime** です。 データ値が時間間隔を表す場合、生成されたクラスのデータ型は **TimeSpan** です。  
  
 Visual Studio .NET のサーバー エクスプローラー管理拡張機能を使用して、厳密に型指定されたクラスを生成することもできます。  
  
 WMI の詳細については、プラットフォーム SDK の「**Windows Management Instrumentation**」のトピックを参照してください。  
  
## <a name="examples"></a>使用例  
 **Root\cimv2** 名前空間内の **Win32_LogicalDisk** WMI クラスに対してマネージド クラスを C# コードで生成するコマンドを次に示します。 このツールは、**ROOT.CIMV2.Win32** 名前空間内の c:\disk.cs にあるソース ファイルにマネージド クラスを書き込みます。  
  
```console  
mgmtclassgen Win32_LogicalDisk /n root\cimv2 /l CS /p c:\disk.cs  
```  
  
 生成したクラスをプログラムによって使用する方法を次のコード例に示します。 まず、クラスのインスタンスが列挙され、パスが出力されます。 次に、初期化の対象となる生成したクラスのインスタンスが、WMI のインスタンスを使用して作成されます。 **Root\cimv2** 名前空間において `Process` は **Win32_Process** に対して生成されたクラスであり、`LogicalDisk` は **Win32_LogicalDisk** に対して生成されたクラスです。  
  
```vb  
Imports System  
Imports System.Management  
Imports ROOT.CIMV2.Win32  
  
Public Class App     
   Public Shared Sub Main()        
      ' Enumerate instances of the Win32_process.  
      ' Print the Name property of the instance.  
      Dim ps As Process     
      For Each ps In  Process.GetInstances()  
         Console.WriteLine(ps.Name)  
      Next ps  
  
      ' Initialize the instance of LogicalDisk with  
      ' the WMI instance pointing to logical drive d:.  
      Dim dskD As New LogicalDisk(New _  
         ManagementPath("win32_LogicalDisk.DeviceId=""d:"""))  
      Console.WriteLine(dskD.Caption)  
   End Sub  
End Class  
```  
  
```csharp  
using System;  
using System.Management;  
using ROOT.CIMV2.Win32;  
  
public class App  
{  
   public static void Main()  
   {  
      // Enumerate instances of the Win32_process.  
      // Print the Name property of the instance.  
      foreach(Process ps in Process.GetInstances())  
      {  
         Console.WriteLine(ps.Name);  
      }  
  
      // Initialize the instance of LogicalDisk with  
      // the WMI instance pointing to logical drive d:.  
      LogicalDisk dskD = new LogicalDisk(new ManagementPath(  
        "win32_LogicalDisk.DeviceId=\"d:\""));  
      Console.WriteLine(dskD.Caption);  
   }  
}  
```  
  
## <a name="see-also"></a>関連項目

- <xref:System.Management>
- <xref:System.Management.ManagementClass.GetStronglyTypedClassCode%2A?displayProperty=nameWithType>
- <xref:System.CodeDom.Compiler.CodeDomProvider?displayProperty=nameWithType>
- [ツール](index.md)
- [Visual Studio 用開発者コマンド プロンプト](developer-command-prompt-for-vs.md)
