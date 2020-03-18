---
title: 共有 Web ホストの最適化
ms.date: 03/30/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- garbage collection, Web hosting
- garbage collection, optimizing
- garbage collection, shared Web hosting
ms.assetid: be98c0ab-7ef8-409f-8a0d-cb6e5b75ff20
ms.openlocfilehash: 07a100e2cd6aaff2b54b99144c9d762c8979fb47
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73140274"
---
# <a name="optimization-for-shared-web-hosting"></a>共有 Web ホストの最適化
複数の小規模な Web サイトをホストして共有されているサーバーの管理者は、.NET ディレクトリの Aspnet.config ファイルの `gcTrimCommitOnLowMemory` ノードに次の `runtime` 設定を追加することで、パフォーマンスを最適化し、サイトの容量を増やすことができます。  
  
 `<gcTrimCommitOnLowMemory enabled="true|false"/>`  
  
> [!NOTE]
> この設定は、共有 Web ホスティング シナリオの場合にのみお勧めします。  
  
 ガベージ コレクターは将来の割り当てのためにメモリを保持するので、コミットされる領域は厳密に必要な領域を超える可能性があります。 この領域を減らすと、システム メモリにかかる負荷が高くなる時間に対応できます。 このコミットされる領域を減らすと、パフォーマンスが向上し、より多くのサイトをホストできるようになります。  
  
 `gcTrimCommitOnLowMemory` 設定が有効な場合、ガベージ コレクターはシステムのメモリ負荷を評価し、負荷が 90% に達するとトリミング モードに切り替わります。 負荷が 85% 未満になるまでトリミング モードは維持されます。  
  
 条件を満たしている場合、`gcTrimCommitOnLowMemory` 設定で現在のアプリケーションを助けずに無視することをガベージ コレクターで決定することができます。  
  
## <a name="example"></a>例  
 次の XML フラグメントは、`gcTrimCommitOnLowMemory` 設定を有効にする方法を示しています。 省略記号は、`runtime` ノード内のその他の設定を示します。  
  
```xml  
<?xml version="1.0" encoding="UTF-8"?>  
<configuration>  
    <runtime>  
    . . .  
    <gcTrimCommitOnLowMemory enabled="true"/>  
    </runtime>  
    . . .  
</configuration>  
```  
  
## <a name="see-also"></a>参照

- [ガベージ コレクション](../../../docs/standard/garbage-collection/index.md)
