---
title: Microsoft.Win32 名前空間を使用したレジストリの読み取りと書き込み
ms.date: 07/20/2015
helpviewer_keywords:
- registry [Visual Basic]
ms.assetid: 4a0dcce0-c27b-4199-baa8-ee4528da6a56
ms.openlocfilehash: 841344186b8e56717b81e90397aabc608bdc6dab
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "74345499"
---
# <a name="reading-from-and-writing-to-the-registry-using-the-microsoftwin32-namespace-visual-basic"></a>Microsoft.Win32 名前空間を使用したレジストリの読み取りと書き込み (Visual Basic)

レジストリに対してプログラミングする際の基本的なニーズには `My.Computer.Registry` で対応できますが、.NET Framework の <xref:Microsoft.Win32.Registry> 名前空間の <xref:Microsoft.Win32.RegistryKey> クラスと <xref:Microsoft.Win32> クラスを使用することもできます。  
  
## <a name="keys-in-the-registry-class"></a>Registry クラスのキー  

 <xref:Microsoft.Win32.Registry> クラスでは、サブキーとその値にアクセスするために使用できるベース レジストリ キーが提供されます。 ベース キー自体は読み取り専用です。 次の表では、<xref:Microsoft.Win32.Registry> クラスによって公開される 7 つのキーについて説明します。  
  
|**[キー]**|**説明**|  
|-------------|---------------------|  
|<xref:Microsoft.Win32.Registry.ClassesRoot>|ドキュメントの種類と、それらの種類に関連付けられているプロパティを定義します。|  
|<xref:Microsoft.Win32.Registry.CurrentConfig>|ユーザー固有ではないハードウェア構成情報が含まれます。|  
|<xref:Microsoft.Win32.Registry.CurrentUser>|環境変数など、現在のユーザー設定に関する情報が含まれます。|  
|<xref:Microsoft.Win32.Registry.DynData>|仮想デバイス ドライバーによって使用されるデータなど、動的なレジストリ データが含まれます。|  
|<xref:Microsoft.Win32.Registry.LocalMachine>|ローカル コンピューターの構成データを保持する 5 つのサブキー (Hardware、SAM、Security、Software、System) が含まれます。|  
|<xref:Microsoft.Win32.Registry.PerformanceData>|ソフトウェア コンポーネントのパフォーマンス情報が含まれます。|  
|<xref:Microsoft.Win32.Registry.Users>|既定のユーザー設定についての情報が含まれます。|  
  
> [!IMPORTANT]
> ローカル コンピューター (<xref:Microsoft.Win32.Registry.CurrentUser>) よりも、現在のユーザー (<xref:Microsoft.Win32.Registry.LocalMachine>) にデータを書き込む方が安全です。 作成しようとするキーが、以前に悪意のある可能性のある別のプロセスによって作成されたことがある場合、一般に "スクワッティング" と呼ばれる状況が発生します。 スクワッティングの発生を防ぐには、キーがまだ存在しない場合は <xref:Microsoft.Win32.RegistryKey.GetValue%2A> を返す `Nothing` などのメソッドを使用します。  
  
## <a name="reading-a-value-from-the-registry"></a>レジストリから値を読み取る  

 次のコードでは、HKEY_CURRENT_USER から文字列を読み取る方法を示します。  
  
 [!code-vb[VbResourceTasks#20](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbResourceTasks/VB/Class1.vb#20)]  
  
 次のコードは、HKEY_CURRENT_USER から文字列を読み取り、値を増分した後、書き込みます。  
  
 [!code-vb[VbResourceTasks#21](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbResourceTasks/VB/Class1.vb#21)]  
  
## <a name="see-also"></a>参照

- <xref:System.SystemException>
- <xref:System.ApplicationException>
- <xref:Microsoft.VisualBasic.MyServices.RegistryProxy>
- [Try...Catch...Finally ステートメント](../../../../visual-basic/language-reference/statements/try-catch-finally-statement.md)
- [レジストリからの読み取りとレジストリへの書き込み](../../../../visual-basic/developing-apps/programming/computer-resources/reading-from-and-writing-to-the-registry.md)
- [セキュリティとレジストリ](../../../../visual-basic/developing-apps/programming/computer-resources/security-and-the-registry.md)
