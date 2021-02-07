---
description: 詳細については、「サービスフレームワークデータ」を参照してください。
title: サービス フレームワーク データ
ms.date: 03/30/2017
ms.assetid: 2a2c8ddc-4e82-4e7f-a79f-97085c469517
ms.openlocfilehash: f2f4645e0a2473e565286a45e6d20e3d1ef7c2f4
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99686194"
---
# <a name="service-framework-data"></a>サービス フレームワーク データ

ここでは、サービス フレームワーク データによって生成されるすべての例外を示します。  
  
## <a name="exception-list"></a>例外の一覧  
  
|リソース コード|リソースの文字列|  
|-------------------|---------------------|  
|AddressingExtensionInBadNS|指定した名前空間の指定した要素が無効です。 これは、指定した要素が重複する要素であること、または、拡張要素はアドレス名前空間内に存在できないため、この要素が有効な拡張ではないことを意味します。|  
|BinaryEncoderSessionInvalid|前のメッセージのデコード中にエラーが発生したために、バイナリ エンコーダーのセッションが無効です。|  
|CannotDetectAddressingVersion|WS-Addressing のバージョンを検出できません。 EndpointAddress が要素で開始していません。|  
|CouldNotFindNamespaceForPrefix|指定したプレフィックスは、スコープ内に名前空間バインディングがありません。|  
|EncoderBadContentType|contentType を処理できません。|  
|EncoderEnvelopeVersionMismatch|指定した受信メッセージのエンベロープのバージョンが、指定したエンコーダーのバージョンと一致しません。 バインドに構成されているバージョンが、予期されるメッセージと同じであることを確認してください。|  
|EncoderMessageVersionMismatch|指定した送信メッセージのメッセージ バージョンが、指定したエンコーダーのバージョンと一致しません。 バインドに構成されているバージョンが、メッセージと同じであることを確認してください。|  
|ExtraContentIsPresentInFaultDetail|fault detail 要素に追加の XML (Extensible Markup Language) コンテンツがあります。 1 つの要素のみが許可されています。|  
|FilterBadTableType|フィルター用に作成された IMessageFilterTable を、MessageFilterTable または MessageFilterTable のサブクラスにすることはできません。|  
|FilterTableInvalidForLookup|MessageFilterTable の状態が破損しています。 要求された検索を実行できません。|  
|MandatoryHeaderNotUnderstood|要求された 1 つ以上の SOAP (Simple Object Access Protocol) ヘッダー ブロックが認識されませんでした。|  
|MessageBodyIsStream|メッセージ本文がストリームです。|  
|MessageBodyIsUnknown|メッセージ本文の形式が不明です。|  
|MessageBodyReaderInvalidReadState|メッセージ本文のリーダーの指定した ReadState を利用できません。|  
|MessageTextEncodingNotSupported|テキスト メッセージ形式に使用されている、指定したテキスト エンコードはサポートされていません。|  
|MissingMessageID|要求メッセージに MessageID ヘッダーがありません。 このヘッダーは応答を相関付けるために必要です。|  
|MultipleMessageHeaders|指定した名前と名前空間を持つヘッダーが複数あります。|  
|MultipleMessageHeadersWithActor|指定した名前、名前空間、およびロールを持つヘッダーが複数あります。|  
|MultipleRelatesToHeaders|指定したリレーションシップを持つ RelatesTo ヘッダーが複数あります。 このヘッダーは、1 つのリレーションシップにつき 1 つしか許可されていません。|  
|QueryFunctionTypeNotSupported|IXsltContextFunction の指定した戻り値の型はサポートされていません。|  
|QueryIteratorOutOfScope|XPathNodeIterator は無効にされました。 IXsltContextFunctions の引数として渡された XPathNodeIterators は、関数内でのみ有効です。 これらは、後で使用するためにキャッシュされることも、関数の結果として返されることもありません。|  
|QueryVariableNull|IXsltContextVariable メソッドは、Null を返すことができません。|  
|QueryVariableTypeNotSupported|指定した IXsltContextVariable の派生型はサポートされていません。|  
|ReceiveShutdownReturnedMessage|チャネルを閉じているときに、指定したアクションを持つ予期しない入力メッセージを受信しました。 チャネルは、追加の入力メッセージを受信する可能性がない場合にのみ閉じる必要があります。|  
|XmlBufferInInvalidState|内部エラーが発生しました。 XML バッファーの状態が原因で、操作を実行できません。|  
|XmlBufferQuotaExceeded|XML (Extensible Markup Language) コンテンツのバッファリングに必要なサイズがバッファー クォータを超過しました。|
