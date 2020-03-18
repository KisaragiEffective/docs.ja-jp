---
title: GetCustomUI
ms.date: 03/30/2017
helpviewer_keywords:
- custom error messages [WPF]
ms.assetid: e55180fc-35bb-4f80-a136-772b5eb3e4e5
ms.openlocfilehash: e9ef32912c2afb3c99e46e1e14bb3daa5a2e99af
ms.sourcegitcommit: eff6adb61852369ab690f3f047818c90580e7eb1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/07/2019
ms.locfileid: "72005710"
---
# <a name="getcustomui"></a>GetCustomUI
実装されている場合、ホストからカスタムの進行状況とエラーメッセージを取得するために、プレゼンテーションの cluster.exe によって呼び出されます。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT GetCustomUI( [out] BSTR* pwzProgressAssemblyName, [out] BSTR* pwzProgressClassName, [out] BSTR* pwzErrorAssemblyName, [out] BSTR* pwzErrorClassName );  
```  
  
## <a name="parameters"></a>パラメーター  
 `pwzProgressAssemblyName`  
  
 入出力ホストから提供された進行状況のユーザーインターフェイスを格納しているアセンブリへのポインター。  
  
 `pwzProgressClassName`  
  
 入出力ホストから提供された進行状況のユーザーインターフェイスであるクラスの名前。可能であれば、<xref:System.Windows.Controls.Page> を持つ XAML ファイルは最上位の要素です。 このクラスは、`pwzProgressAssemblyName`によって指定されたアセンブリに存在します。  
  
 `pwzErrorAssemblyName`  
  
 入出力ホストから提供されたエラーユーザーインターフェイスを格納しているアセンブリへのポインター。  
  
 `pwzErrorClassName`  
  
 入出力ホストから提供されたエラーユーザーインターフェイスであるクラスの名前。可能であれば <xref:System.Windows.Controls.Page> を持つ XAML ファイルは最上位の要素です。 このクラスは、`pwzErrorAssemblyName`によって指定されたアセンブリに存在します。  
  
## <a name="property-valuereturn-value"></a>プロパティ値/戻り値  
 HRESULT: 無視されます。  
  
## <a name="remarks"></a>コメント  
 ホストアプリケーションには、ホストの既定のユーザーインターフェイスが準拠していない可能性がある特定のテーマが含まれている場合があります。 この場合、ホストアプリケーションは[GetCustomUI](getcustomui.md)を実装して、進行状況とエラーユーザーインターフェイスをプレゼンテーションの cluster.exe に返すことができます。 [GetCustomUI](getcustomui.md) は、既定のユーザーインターフェイスを使用する前に、常にを呼び出します。  
  
 この関数は、プレゼンテーションホストの初期化中に1回呼び出されます。  
  
## <a name="see-also"></a>参照

- [IWpfHostSupport](iwpfhostsupport.md)
