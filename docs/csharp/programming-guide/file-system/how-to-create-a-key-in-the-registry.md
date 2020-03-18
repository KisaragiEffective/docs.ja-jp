---
title: レジストリにキーを作成する方法 - C# プログラミング ガイド
ms.date: 07/20/2015
helpviewer_keywords:
- registry, adding keys and values [C#]
- registry keys, creating [C#]
- keys, creating in registry
ms.assetid: 8fa475b0-e01f-483a-9327-fd03488fdf5d
ms.openlocfilehash: 16974db950a3a460416cfb917147439707e1d007
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75635445"
---
# <a name="how-to-create-a-key-in-the-registry-c-programming-guide"></a>レジストリにキーを作成する方法 (C# プログラミング ガイド)
現在のユーザーのレジストリに存在する "Names" というキーの下に "Name" と "Isabella" という値のペアを追加する例を次に示します。  
  
## <a name="example"></a>例  
  
```csharp  
Microsoft.Win32.RegistryKey key;  
key = Microsoft.Win32.Registry.CurrentUser.CreateSubKey("Names");  
key.SetValue("Name", "Isabella");  
key.Close();  
```  
  
## <a name="compiling-the-code"></a>コードのコンパイル  
  
- コードをコピーし、コンソール アプリケーションの `Main` メソッドに貼り付けます。  
  
- `Names` パラメーターをレジストリの HKEY_CURRENT_USER ノードの直下にあるキーの名前に置き換えます。  
  
- `Name` パラメーターを Names ノードの直下にある値の名前に置き換えます。  
  
## <a name="robust-programming"></a>信頼性の高いプログラミング  
 レジストリの構造を調べて、キーの適切な場所を見つけることができます。 たとえば、現在のユーザーの Software キーを開き、会社名のキーを作成できます。 その後で、会社名のキーにレジストリ値を追加します。  
  
 次の条件では例外が発生する可能性があります。  
  
- キーの名前が null である場合。  
  
- レジストリ キーを作成するためのアクセス許可がユーザーにない場合。  
  
- キー名が 255 文字の制限を超えている場合。  
  
- キーが閉じている場合。  
  
- レジストリ キーが読み取り専用の場合。  
  
## <a name="net-framework-security"></a>.NET Framework のセキュリティ  
 ローカル コンピューター (`Microsoft.Win32.Registry.CurrentUser`) よりもユーザー フォルダー (`Microsoft.Win32.Registry.LocalMachine`) にデータを書き込む方が安全です。  
  
 レジストリの値を作成するときは、その値が既存の値である場合の処理を決めておく必要があります。 悪意のあるユーザーによって作成された別のプロセスが既に値を作成し、アクセス権を持っている可能性があります。 レジストリ値にデータを設定すると、そのデータを他のプロセスから利用できるようになります。 これを回避するには、`Overload:Microsoft.Win32.RegistryKey.GetValue` メソッドをオーバーライドします。 このメソッドは、キーがまだ存在しない場合、null を返します。  
  
 レジストリ キーがアクセス制御リスト (ACL: Access Control List) によって保護されていても、パスワードなど他人に知られたくないデータをプレーン テキストでレジストリに格納するのは危険です。  
  
## <a name="see-also"></a>参照

- <xref:System.IO?displayProperty=nameWithType>
- [C# プログラミングガイド](../index.md)
- [ファイル システムとレジストリ (C# プログラミング ガイド)](./index.md)
- [C# によるレジストリからの読み取り、書き込み、および削除](https://www.codeproject.com/Articles/3389/Read-write-and-delete-from-registry-with-C)
