---
title: 暗号設定スキーマ
ms.date: 03/30/2017
helpviewer_keywords:
- configuration schema [.NET Framework], cryptography
- elements [.NET Framework], cryptography
- schema configuration settings
- cryptography, settings schema
- cryptography, mapping algorithm names
- configuration sections [.NET Framework]
- configuration settings [.NET Framework], cryptography
ms.assetid: 1f55050a-b2a3-4868-a3c0-da20826150f3
ms.openlocfilehash: c632a15552c8ba5743aac1309098b7d7ef949bbd
ms.sourcegitcommit: 7f8eeef060ddeb2cabfa52843776faf652c5a1f5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/14/2019
ms.locfileid: "74088007"
---
# <a name="cryptography-settings-schema"></a>暗号設定スキーマ
暗号設定スキーマには、アルゴリズムの表示名を、暗号化アルゴリズムを実装するクラスに割り当てる方法を指定する要素が含まれます。  
  
[ **\<configuration>** ](../configuration-element.md)\
&nbsp;&nbsp;[ **\<mscorlib>** ](mscorlib-element-for-cryptography-settings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[ **\<cryptographySettings**](cryptographysettings-element.md)>\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[ **\<cryptoNameMapping>** ](cryptonamemapping-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[ **\<cryptoClasses>** ](cryptoclasses-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[ **\<cryptoClass>** ](cryptoclass-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[ **\<nameEntry>** ](nameentry-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[ **\<oidMap**](oidmap-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\<[**oidEntry>** ](oidentry-element.md)

|要素|説明|  
|-------------|-----------------|  
|[ **\<cryptoClasses**>](cryptoclasses-element.md)|**\<nameEntry>** 要素内の表示名へのマッピングを持つ暗号化クラスのリストを含みます。|  
|[ **\<cryptoClass**>](cryptoclass-element.md)|**\<nameEntry>** 要素内の表示名へのマッピングを持つ暗号化クラスを含みます。|  
|[ **\<cryptographySettings**>](cryptographysettings-element.md)|暗号設定を含みます。|  
|[ **\<cryptoNameMapping**>](cryptonamemapping-element.md)|表示名へのクラスのマッピングを含みます。|  
|[ **\<mscorlib>** 要素 の 暗号化設定](mscorlib-element-for-cryptography-settings.md)|**\<cryptographySettings>** 要素を含みます。|  
|[ **\<nameEntry>** ](nameentry-element.md)|アルゴリズムの表示名にクラス名をマップして、1 つのクラスが多くの表示名を持つことを許可します。|  
|[ **\<oidEntry>** ](oidentry-element.md)|ASN.1 オブジェクト識別子 (OID) を表示名にマップします。|  
|[ **\<oidMap>** ](oidmap-element.md)|クラスへの ASN.1 OID マッピングを含みます。|  
  
## <a name="see-also"></a>関連項目

- [構成ファイル スキーマ](../index.md)
- [暗号サービス](../../../../standard/security/cryptographic-services.md)
