---
description: '詳細情報: <useLegacyJit> 要素'
title: <useLegacyJit> 要素
ms.date: 04/26/2017
ms.assetid: c2cf97f0-9262-4f1f-a754-5568b51110ad
ms.openlocfilehash: a1449cbc69f0aa1b91cc427fbfc5b984bf605169
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99640005"
---
# <a name="uselegacyjit-element"></a>\<useLegacyJit> 要素

共通言語ランタイムが Just-In-Time コンパイルの従来の 64 ビット JIT コンパイラを使用するかどうかを決定します。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<runtime>**](runtime-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;**\<useLegacyJit>**  
  
## <a name="syntax"></a>構文  
  
```xml
<useLegacyJit enabled=0|1 />
```

要素名 `useLegacyJit` では、大文字と小文字が区別されます。
  
## <a name="attributes-and-elements"></a>属性と要素

以降のセクションでは、属性、子要素、および親要素について説明します。  
  
### <a name="attributes"></a>属性  
  
| 属性 | 説明                                                                                   |  
| --------- | --------------------------------------------------------------------------------------------- |  
| `enabled` | 必須の属性です。<br><br>ランタイムで従来の 64 ビット JIT コンパイラを使用するかどうかを指定します。 |  
  
### <a name="enabled-attribute"></a>enabled 属性  
  
| [値] | 説明                                                                                                         |  
| ----- | ------------------------------------------------------------------------------------------------------------------- |  
| 0     | 共通言語ランタイムで、.NET Framework 4.6 以降のバージョンに含まれる新しい 64 ビット JIT コンパイラを使用します。 |  
| 1     | 共通言語ランタイムで、古い 64 ビット JIT コンパイラを使用します。                                                     |  
  
### <a name="child-elements"></a>子要素

なし
  
### <a name="parent-elements"></a>親要素  
  
| 要素         | 説明                                                                                                       |  
| --------------- | ----------------------------------------------------------------------------------------------------------------- |  
| `configuration` | 共通言語ランタイムおよび .NET Framework アプリケーションで使用されるすべての構成ファイルのルート要素です。 |  
| `runtime`       | ランタイム初期化オプションに関する情報を含んでいます。                                                        |  
  
## <a name="remarks"></a>解説  

.NET Framework 4.6 以降では、既定で、共通言語ランタイムに Just-In-Time (JIT) コンパイル用の新しい 64 ビット コンパイラが使用されます。 場合によっては、以前のバージョンの 64 ビット JIT コンパイラによって JIT コンパイルされたアプリケーション コードと動作が異なる場合があります。 `<useLegacyJit>` 要素の `enabled` 属性を `1` に設定すると、新しい 64 ビット JIT コンパイラを無効にし、代わりに従来の 64 ビット JIT コンパイラを使用してアプリをコンパイルすることができます。  
  
> [!NOTE]
> `<useLegacyJit>` 要素は、64 ビットの JIT コンパイルのみに影響します。 32 ビットの JIT コンパイラを使ったコンパイルには影響しません。  
  
構成ファイルの設定を使用する代わりに、次の 2 つの方法で従来の 64 ビット JIT コンパイラを有効にすることもできます。  
  
- 環境変数の設定

  `COMPLUS_useLegacyJit` 環境変数を、`0` (新しい 64 ビット JIT コンパイラを使用) または `1` (古い 64 ビット JIT コンパイラを使用) のいずれかに設定します。
  
  ```env  
  COMPLUS_useLegacyJit=0|1  
  ```  
  
  環境変数には *グローバル スコープ* があります。これは、その変数がコンピューター上で実行されるすべてのアプリケーションに影響を与えることを意味します。 設定されている場合、これはアプリケーション構成ファイルの設定によってオーバーライドできます。 環境変数の名前では大文字と小文字は区別されません。
  
- レジストリ キーの追加

  レジストリ内の `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework` または `HKEY_CURRENT_USER\SOFTWARE\Microsoft\.NETFramework` キーのいずれかに `REG_DWORD` 値を追加することで、従来の 64ビット JIT コンパイラを有効にすることができます。 この値は `useLegacyJit` という名前です。 値が 0 の場合は、新しいコンパイラが使用されます。 値が 1 の場合は、従来の 64 ビット JIT コンパイラが有効になります。 レジストリ値の名前では大文字と小文字は区別されません。
  
  `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework` キーへの値の追加は、コンピューター上で実行されているすべてのアプリに影響します。 `HKEY_CURRENT_USER\SOFTWARE\Microsoft\.NETFramework` キーへの値の追加は、現在のユーザーによって実行されているすべてのアプリに影響します。 コンピューターに複数のユーザー アカウントが構成されている場合は、その値が他のユーザーのレジストリ キーにも追加されない限り、影響を受けるのは現在のユーザーが実行しているアプリのみとなります。 構成ファイルに `<useLegacyJit>` 要素を追加すると、レジストリ設定がオーバーライドされます (設定が存在する場合)。  
  
## <a name="example"></a>例  

次の構成ファイルでは、新しい 64 ビット JIT コンパイラでのコンパイルを無効にし、代わりに従来の 64 ビット JIT コンパイラを使用しています。  
  
```xml  
<?xml version ="1.0"?>  
<configuration>  
  <runtime>  
    <useLegacyJit enabled="1" />  
  </runtime>  
</configuration>  
```  
  
## <a name="see-also"></a>関連項目

- [\<runtime> 要素](runtime-element.md)
- [\<configuration> 要素](../configuration-element.md)
- [軽減策:新しい 64 ビット JIT コンパイラ](../../../migration-guide/mitigation-new-64-bit-jit-compiler.md)
