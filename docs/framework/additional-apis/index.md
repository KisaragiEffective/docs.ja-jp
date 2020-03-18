---
title: 追加のクラス ライブラリと API
ms.date: 11/19/2019
helpviewer_keywords:
- Additional class libraries
- Additional managed libraries
- .NET Framework out-of-band releases
- out-of-band releases
ms.assetid: cf2d9006-b631-4e5d-81cd-20aab78c60f1
ms.topic: conceptual
ms.openlocfilehash: 3a5134aa4407598e223fd2c938bfaac02cf9178c
ms.sourcegitcommit: 9c54866bcbdc49dbb981dd55be9bbd0443837aa2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/14/2020
ms.locfileid: "77215532"
---
# <a name="additional-class-libraries-and-apis"></a>追加のクラス ライブラリと API

.NET Framework は絶えず進化しています。 クロスプラットフォームの開発を改善し、新しい機能を早期に導入するために、新機能がアウトオブバンド (OOB) でリリースされます。 ここでは、ドキュメントが用意されている OOB プロジェクトの一覧を示します。  
  
さらに、いくつかのライブラリは、特定のプラットフォームまたは .NET Framework の実装を対象にしています。 たとえば、<xref:System.Text.CodePagesEncodingProvider> クラスは、.NET Framework を使用して開発された UWP アプリでコードページエンコーディングを使用できるようにします。 ここでは、これらのライブラリの一覧も示します。  
  
## <a name="oob-projects"></a>OOB プロジェクト
  
| プロジェクト | 説明 |  
| ------- | ----------- |  
| <xref:System.Collections.Immutable> | スレッド セーフであり、内容が変更されないことが保証されているコレクションを提供します。 |
| <xref:System.Net.Http.WinHttpHandler> | Windows の WinHTTP インターフェイスに基づいた <xref:System.Net.Http.HttpClient> のメッセージ ハンドラーを提供します。 |
| <xref:System.Numerics> | SIMD ハードウェア ベースのアクセラレータを利用できるベクター型のライブラリを提供します。| 
| <xref:System.Threading.Tasks.Dataflow> | TPL データフロー ライブラリはデータ フロー コンポーネントを提供し、コンカレンシー対応アプリケーションの堅牢性を強化します。 |  

## <a name="platform-specific-libraries"></a>プラットフォーム固有のライブラリ
  
| プロジェクト | 説明 |  
| ------- | ----------- |  
| <xref:System.Text.CodePagesEncodingProvider> | <xref:System.Text.EncodingProvider> クラスを拡張して、ユニバーサル Windows プラットフォームを対象とするアプリでコードページエンコーディングを使用できるようにします。 |  
  
## <a name="private-apis"></a>プライベート API  

この API は製品インフラストラクチャをサポートします。コードから直接使用することはできません。  
  
* [SmiOrderProperty プロパティの値です。](microsoft.sqlserver.server.smiorderproperty.item.md)
* [PrepForRemoting メソッド](system.exception.prepforremoting.md)
* [SqlTypes プロパティ (system.object のプロパティ)](system.data.sqltypes.sqlchars.stream.md)
* [SqlTypes. SqlStreamChars コンストラクター](system.data.sqltypes.sqlstreamchars.-ctor.md)
* [SqlTypes プロパティの値を検索します。](system.data.sqltypes.sqlstreamchars.canseek.md)
* [SqlTypes プロパティの値を持つことができます。](system.data.sqltypes.sqlstreamchars.isnull.md)
* [SqlTypes プロパティ ("system.string" プロパティ)](system.data.sqltypes.sqlstreamchars.length.md)
* [SqlTypes メソッドを削除してください。](system.data.sqltypes.sqlstreamchars.close.md)
* [SqlTypes メソッド (system.object の置換メソッド)](system.data.sqltypes.sqlstreamchars.dispose.md)
* [System.string メソッド (SqlTypes...)](system.data.sqltypes.sqlstreamchars.flush.md)
* [SqlTypes メソッドを参照してください。](system.data.sqltypes.sqlstreamchars.read.md)
* [SqlTypes (システムのデータ. Seek メソッド)](system.data.sqltypes.sqlstreamchars.seek.md)
* [SetLength メソッドを SqlTypes しています。](system.data.sqltypes.sqlstreamchars.setlength.md)
* [SqlTypes メソッドを作成してください。](system.data.sqltypes.sqlstreamchars.write.md)
* [MemoryStream メソッドの呼び出しを行います。](system.io.memorystream.internalgetoriginandlength.md)
* [System .Net. Connection クラス](connection.md)
* [System .Net. Connection. m\_WriteList フィールド](m_writelist.md)
* [システム .Net. ConnectionGroup クラス](connectiongroup.md)
* [システム .Net. ConnectionGroup. m\_Connectiongroup フィールド](m_connectionlist.md)
* [System .Net. ConnectStream. 接続プロパティ](system.net.connectstream.connection.md)
* [CoreResponseData クラス](coreresponsedata.md)
* [CoreResponseData. m\_ResponseHeaders フィールド](coreresponsedata_m_responseheaders.md)
* [CoreResponseData\_の StatusCode フィールド](coreresponsedata_m_statuscode.md)
* [HttpWebRequest AutoRedirects フィールドを\_します。](_autoredirects.md)
* [HttpWebRequest.\_CoreResponse フィールド](httpwebrequest__coreresponse.md)
* [HttpWebRequest.\_Httpresponse.cache フィールド](_httpresponse.md)
* [System .Net. PooledStream. NetworkStream プロパティ](system.net.pooledstream.networkstream.md)
* [システム .Net. RtcState クラス](system.net.rtcstate.md)
* [System .Net. ServicePoint. m\_ConnectionGroupList フィールド](m_connectiongrouplist.md)
* [System .Net. ServicePointManager. s\_Servicepointmanager フィールド](s_servicepointtable.md)
* [M_Worker TlsStream (システム) フィールド](system.net.tlsstream.m_worker.md)
* [System .Net. Security. Sslstate プロパティ](system.net.security.sslstate.sslprotocol.md)
* [System.servicemodel. Channels. Message. BodyToString メソッド](system.servicemodel.channels.message.bodytostring.md)
* [WriteStartHeaders メソッド (System.servicemodel.)](system.servicemodel.channels.message.writestartheaders.md)
* [IsDebuggerCheckDisabledForTestPurposes フィールドの\_システム ()](s-isdebuggercheckdisabledfortestpurposes-field.md)
* [System.string クラス (DataMemberFieldEditor クラス)](datamemberfieldeditor-class.md)
* [System.string クラス (DataMemberListEditor クラス)](datamemberlisteditor-class.md)
* [System.xml. .Xml...。](system.xml.xmlreader.createsqlreader.md)
* [adodb.recordset.接続インターフェイス](adodb.connection.md)
* [adodb.recordset.EventReason 列挙型](adodb.eventreasonenum.md)
* [adodb.recordset.EventStatus 列挙型](adodb.eventstatusenum.md)
* [stdole.DISPPARAMS 構造体](stdole.dispparams.md)
* [stdole.EXCEPINFO 構造体](stdole.excepinfo.md)
* [stdole.IFont.Name プロパティ](stdole.ifont.name.md)
* [stdole.IFontDisp インターフェイス](stdole.ifontdisp.md)
* [stdole.IPicture. Handle プロパティ](stdole.ipicture.handle.md)
* [stdole.IPictureDisp プロパティ](stdole.ipicturedisp.handle.md)
* [stdole.StdFont インターフェイス](stdole.stdfont.md)
* [stdole.StdPicture インターフェイス](stdole.stdpicture.md)
  
## <a name="see-also"></a>参照

* [NET Framework および特別なリリース](../get-started/the-net-framework-and-out-of-band-releases.md)
