---
title: Cryptographic Services
ms.date: 03/30/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- cryptography [.NET Framework]
- pattern of derived class inheritance
- digital signatures
- asymmetric cryptographic algorithms
- digital signatures, public-key systems
- public keys
- decryption [.NET Framework]
- private keys
- MAC algorithms
- cryptographic algorithms
- private keys, overview
- encryption [.NET Framework]
- security [.NET Framework], encryption
- cryptographic services
- symmetric cryptographic algorithms
- hash
- message authentication codes
- derived class inheritance
- cryptography [.NET Framework], about
- random number generation
ms.assetid: f96284bc-7b73-44b5-ac59-fac613ad09f8
ms.openlocfilehash: 66af8126ab61d37065b9a328d5e088ed7bf4cbef
ms.sourcegitcommit: 5f236cd78cf09593c8945a7d753e0850e96a0b80
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2020
ms.locfileid: "75709206"
---
# <a name="cryptographic-services"></a>Cryptographic Services

インターネットなどの公衆ネットワークには、エンティティ間の通信を保護する手段が用意されていません。 公衆ネットワークを経由した通信は、不当な第三者によって読み取られたり、さらには変更されたりするおそれがあります。 暗号化を使用すると、データが表示されないように保護し、データが変更されたかどうかを検出する方法を提供し、通常は安全でないチャネル上に安全な通信手段を確立できます。 たとえば、暗号化アルゴリズムを使用してデータを暗号化し、暗号化された状態で送信できます。送信先の相手は、後でこのデータを復号化できます。 暗号化されたデータを第三者が傍受したとしても、復号化するのは困難です。

.NET Framework では、 <xref:System.Security.Cryptography?displayProperty=nameWithType> 名前空間のクラスが暗号化のさまざまな詳細事項を自動的に管理します。 アンマネージド Microsoft Cryptography API (CryptoAPI) 用のラッパーもあれば、純粋なマネージド実装もあります。 これらのクラスを使用するにあたって、暗号の専門家になる必要はありません。 いずれかの暗号化アルゴリズム クラスのインスタンスを新しく作成すると、使いやすいようにキーが自動生成されます。また、既定のプロパティは可能な限り安全に保たれます。

この概要では、.NET Framework 3.5 で導入された ClickOnce マニフェスト、Suite B、Cryptography Next Generation (CNG) のサポートなど、.NET Framework によってサポートされる暗号化の方法と手法の概要を説明します。

暗号化の詳細と、暗号によるセキュリティをアプリケーションに追加できる Microsoft のサービス、コンポーネント、およびツールの詳細については、このドキュメントの「セキュリティ」セクションで、Win32 および COM 開発に関するトピックを参照してください。

## <a name="cryptographic-primitives"></a>暗号プリミティブ

暗号化が使用される一般的な状況では、二者 (ここでは Alice と Bob) が安全でないチャネルを経由して通信します。 Alice と Bob は、通信が第三者に傍受されたとしても、内容は理解されないという保証を必要としています。 また、Alice と Bob は離れた場所にいるため、Alice には、Bob から受け取る情報が送信中に変更されていないという保証が必要です。 さらに Alice には、Bob のふりをしただれかからではなく、本当に Bob からの情報を受け取ることができるという保証が必要です。

暗号化は、次の目標を達成するために使用されます。

- 機密性。ユーザーの ID またはデータが読み取られないように保護するために役立ちます。

- データの整合性。データが変更されないように保護するために役立ちます。

- 信憑性。データが特定の人から送信されることを保証します。

- 否認不可。特定の人がメッセージを送信したことを拒否することを防止します。

これらの目標を達成するために、暗号プリミティブと呼ばれるアルゴリズムと手法の組み合わせを使用して暗号スキームを作成します。 暗号プリミティブとその用途の一覧を次の表に示します。

|暗号プリミティブ|[使用]|
|-----------------------------|---------|
|共有キー暗号方式 (対称暗号化方式)|データに対して変換処理を実行し、データが第三者に読み取られるのを防ぎます。 このタイプの暗号方式では、単一の共有キーを使用してデータの暗号化と復号化が行われます。|
|公開キー暗号方式 (非対称暗号化方式)|データに対して変換処理を実行し、データが第三者に読み取られるのを防ぎます。 このタイプの暗号方式では、公開キーと秘密キーのペアを使用してデータの暗号化と復号化が行われます。|
|署名の暗号化|送信元に固有のデジタル署名を作成することで、データが特定の人から送信されたことを確認することに役立ちます。 この処理でもハッシュ関数が使用されます。|
|暗号ハッシュ|任意の長さのデータを固定長のバイト シーケンスに変換します。 ハッシュは統計的に一意となります。つまり、異なる 2 バイトのシーケンスが同一の値にハッシュされることはありません。|

## <a name="secret-key-encryption"></a>共有キー暗号方式

共有キー暗号方式のアルゴリズムでは、単一の共有キーを使用したデータの暗号化と復号化が行われます。 キーを取得した人はだれでもデータを復号化したり、独自のデータを暗号化したりできるため、承認されていないエージェントがアクセスしてデータの作成者になりすますことができないように、キーを保護する必要があります。

共有キー暗号方式は対称暗号化方式とも呼ばれます。これは、暗号化と復号化で同じキーが使用されるためです。 共有キー暗号化アルゴリズムは (公開キー アルゴリズムと比較して) 非常に高速であり、大量のデータ ストリームに対して暗号変換を実行する場合に適しています。 RSA などの非対称暗号化アルゴリズムは、暗号化できるデータ量に数学的に制限があります。 通常、対称暗号化アルゴリズムではこれらの問題は発生しません。

データをブロック単位で暗号化するときには、ブロック暗号と呼ばれる共有キー アルゴリズムの種類が使用されます。 DES (Data Encryption Standard)、TripleDES、AES (Advanced Encryption Standard) などのブロック暗号では、 *n* バイトの入力ブロックが、暗号化されたバイト数の出力ブロックに変換されます。 バイト シーケンスを暗号化または復号化する場合は、ブロック単位で行う必要があります。 *8* は小さいため (DES および TripleDES では 8 バイト、AES では 16 バイト (既定)、24 バイト、または 32 バイト)、 *8* よりも大きいデータ値は 1 ブロックずつ暗号化する必要があります。 *8* よりも小さいデータ値を処理するためには、 *8* に拡張する必要があります。

ブロック暗号の 1 つに、ECB (Electronic Codebook) モードと呼ばれる単純な形式があります。 ECB モードは、初期化ベクターを使用して最初の平文ブロックを初期化しないため、安全とは見なされません。 秘密キーを *k*とする場合、初期化ベクターを使用しない単純なブロック暗号では、同じ平文の入力ブロックは同じ暗号文の出力ブロックに暗号化されます。 したがって、入力平文ストリーム内に重複するブロックがある場合、暗号文ストリームにも重複するブロックが生成されることになります。 このような重複する出力ブロックが存在すると、アルゴリズムで弱い暗号化が使用されていて、攻撃が可能なモードであることが、承認されていないユーザーにわかります。 このため、ECB 暗号モードは分析に対してきわめて脆弱で、最終的にキーが検出されます。

基底クラス ライブラリに用意されているブロック暗号クラスでは、暗号ブロック チェイン (CBC: Cipher Block Chaining) と呼ばれる既定のチェイン モードが使用されます。ただし、この既定のモードは必要に応じて変更できます。

CBC 暗号は、初期化ベクター (IV: Initialization Vector) を使用して平文の最初のブロックを暗号化することにより、ECB 暗号に関連する問題を回避します。 平文の後続の各ブロックは、前の暗号文ブロックを使用してビットごとの排他的 OR (`XOR`) 演算を実行してから、暗号化されます。 このため、各平文ブロックは、前のすべてのブロックに依存します。 このシステムを使用した場合は、承認されていないユーザーが共通メッセージ ヘッダーを知っていたとしても、その情報からキーをリバース エンジニアリングすることはできません。

CBC 暗号によって暗号化されたデータを解読する 1 つの方法は、考えられるすべてのキーを徹底的に探索することです。 ただし、暗号化の実行時に使用したキーのサイズによっては、どれほど高速なコンピューターを使用してもかなりの時間がかかるため、この探索方法は現実的ではありません。 キーのサイズを大きくするほど、復号化は困難になります。 暗号化することにより、暗号データの復号化が理論的に不可能になるわけではありませんが、復号化にかかるコストを大きくできます。 3 か月をかけて徹底的な探索を行っても、取得されたデータが数日間しか意味を持たないとすると、その探索方法は実用的とはいえません。

共有キー暗号方式の弱点は、両者のキーと IV を一致させ、それぞれの値を転送しておく必要がある点です。 IV は秘密情報とは見なされないため、平文のメッセージで転送できます。 しかし、キーは承認されていないユーザーから保護する必要があります。 このような問題のため、共有キー暗号方式は公開キー暗号方式と併用されることがよくあります。公開キー暗号方式は、キーと IV の値を秘密に通信するために使用されます。

安全でないチャネルを経由して Alice と Bob が通信しようとしている場合は、次のように共有キー暗号方式を使用することが考えられます。Alice と Bob は、特定の 1 つのアルゴリズム (たとえば AES) と、特定のキーおよび IV を使用することに合意します。 Alice はメッセージを作成し、メッセージの送信先となるネットワークストリーム (名前付きパイプやネットワーク電子メールなど) を作成します。 次に、キーと IV を使用してテキストを暗号化し、暗号化されたメッセージと IV をインターネット経由で Bob に送信します。 暗号化されたテキストを受信した Bob は、IV とあらかじめ決めてあるキーを使用して復号化を行います。 通信が傍受されたとしても、傍受した人にはキーがわからないため、元のメッセージが復元されることはありません。 このシナリオでは、秘密にしておく必要があるのはキーだけです。 実際のシナリオでは、Alice または Bob のどちらかが共有キーを生成し、公開キー (非対称) 暗号方式を使用して相手に共有 (対称) キーを転送することになります。 公開キー暗号方式の詳細については、次のセクションを参照してください。

.NET Framework には、共有キー暗号化アルゴリズムを実装する次のクラスが用意されています。

- <xref:System.Security.Cryptography.AesManaged> (.NET Framework 3.5 で導入)。

- <xref:System.Security.Cryptography.DESCryptoServiceProvider>.

- <xref:System.Security.Cryptography.HMACSHA1> 。これは、暗号ハッシュ関数を共有キーと組み合わせて使用することで計算されるメッセージ認証コードを表すため、技術的には共有キー アルゴリズムです。 後で説明する「 [ハッシュ値](#hash-values)」を参照してください。

- <xref:System.Security.Cryptography.RC2CryptoServiceProvider>.

- <xref:System.Security.Cryptography.RijndaelManaged>.

- <xref:System.Security.Cryptography.TripleDESCryptoServiceProvider>.

## <a name="public-key-encryption"></a>公開キー暗号方式

公開キー暗号方式では、承認されていないユーザーから保護する必要のある秘密キーと、だれに公開してもかまわない公開キーが使用されます。 公開キーと秘密キーは正確にリンクされ、公開キーで暗号化されたデータは、対応する秘密キーでしか復号化できません。また、秘密キーで署名されたデータは、対応する公開キーでしか検査できません。 公開キーはだれに公開してもかまいません。公開キーは、秘密キーの所有者に送信するデータを暗号化するために使用されます。 公開キー暗号化アルゴリズムは、非対称アルゴリズムとしても知られています。これは、データの暗号化に 1 つのキーが使用され、データの復号化に別のキーが使用されるためです。 基本的な暗号化規則でキーの再使用を禁止し、いずれのキーも通信セッションごとに一意にする必要があります。 ただし、実際には、非対称キーの寿命は長いことが一般的です。

二者 (ここでは Alice と Bob) は、次のように公開キーの暗号化を使用できます。まず、Alice が公開キー/秘密キーのペアを生成します。 暗号メッセージを Alice に送信するとき、Bob は Alice に公開キーを送信するように依頼します。 Alice は安全でないネットワークを通して Bob に公開キーを送信し、Bob はこのキーを使用してメッセージを暗号化します。 Bob は暗号メッセージを Alice に送信し、Alice は自分の秘密キーを使用してメッセージを復号化します。 公衆ネットワークなどの安全でないチャネル経由で Alice のキーを受信した場合、Bob は man-in-the-middle 攻撃に対して無防備になります。 したがって、Bob は所有している Alice の公開キーの正しいコピーを使用して、Alice であることを検証する必要があります。

Alice の公開キーの転送中に、承認されていないエージェントによってキーが傍受される可能性があります。 さらに、同じエージェントが Bob からの暗号メッセージを傍受する可能性もあります。 しかし、公開キーを使用してもメッセージを復号化することはできません。 メッセージを復号化できるのは Alice の秘密キーだけですが、これは送信されていません。 Alice は Bob への返信メッセージを暗号化するときに自分の秘密キーを使用しません。公開キーを持つ人は、だれでもそのメッセージを復号化できるためです。 Alice から Bob にメッセージを返信するときには、Alice が Bob の公開キーをたずね、その公開キーを使用してメッセージを暗号化します。 その後、Bob は自分の秘密キーを使用してメッセージを復号化します。

このシナリオでは、Alice と Bob は公開キー (非対称) 暗号方式を使用して共有 (対称) キーを転送し、その他のセッションでは共有キー暗号方式を使用することになります。

次の一覧を使用して、公開キーと共有キーの暗号化アルゴリズムを比較します。

- 公開キー暗号化アルゴリズムでは固定バッファー サイズが使用されますが、共有キー暗号化アルゴリズムでは可変長バッファーが使用されます。

- 公開キー アルゴリズムでは少量のデータしか暗号化できないため、共有キー アルゴリズムのようにデータをチェインしてストリームを作成することはできません。 したがって、非対称操作では対称操作と同じストリーミング モデルは使用されません。

- 公開キーの暗号化方式では、秘密キーの暗号化と比較してキーのキースペース (使用できる値の範囲) が大幅に広がります。 そのため、公開キーの暗号化では、あらゆるキーを試すような徹底的な攻撃の影響を受けにくくなります。

- 送信元の身元を検証する方法が存在する場合、公開キーは保護する必要なく簡単に配布できます。

- いくつかの公開キー アルゴリズム (Diffie-Hellman 以外の RSA や DSA など) では、データの送信元の身元を検査するデジタル署名を作成できます。

- 公開キー アルゴリズムは秘密キー アルゴリズムと比較してきわめて低速であり、大量のデータを暗号化するようには設計されていません。 公開キー アルゴリズムが便利なのは、少量のデータを転送する場合に限られます。 一般に、公開キー暗号方式は、共有キー アルゴリズムで使われるキーと IV を暗号化するために使用されます。 キーと IV を転送した後の残りのセッションでは、共有キー暗号方式が使用されます。

.NET Framework には、公開キー暗号化アルゴリズムを実装する次のクラスが用意されています。

- <xref:System.Security.Cryptography.DSACryptoServiceProvider>

- <xref:System.Security.Cryptography.RSACryptoServiceProvider>

- <xref:System.Security.Cryptography.ECDiffieHellman> (基底クラス)

- <xref:System.Security.Cryptography.ECDiffieHellmanCng>

- <xref:System.Security.Cryptography.ECDiffieHellmanCngPublicKey> (基底クラス)

- <xref:System.Security.Cryptography.ECDiffieHellmanKeyDerivationFunction> (基底クラス)

- <xref:System.Security.Cryptography.ECDsaCng>

RSA は暗号化と署名の両方に使用できますが、DSA は署名にのみ使用可能で、Diffie-Hellman はキー生成にのみ使用可能です。 一般に、秘密キー アルゴリズムに比べて、公開キー アルゴリズムの使用には多くの制限があります。

## <a name="digital-signatures"></a>デジタル署名

公開キー アルゴリズムは、デジタル署名を形成するためにも使用できます。 デジタル署名は、(送信元の公開キーを信頼している場合に) 送信元の ID を認証したり、データの整合性を保護することを支援したりします。 Alice によって生成された公開キーを使用すると、Alice のデータの受信者は、Alice のデータに添付されたデジタル署名と Alice の公開キーを比較することによって、データの送信元が Alice かどうかを検査できます。

Alice は、公開キー暗号方式を使用してメッセージにデジタル署名を添付するために、まずメッセージに対してハッシュ アルゴリズムを適用してメッセージ ダイジェストを作成します。 このメッセージ ダイジェストは、一意でコンパクトなデータ表現です。 次に Alice は、自分の秘密キーを使用してメッセージ ダイジェストを暗号化し、個人用の署名を作成します。 Bob は、メッセージと署名を受信したときに Alice の公開キーを使用して署名を復号化し、メッセージ ダイジェストを復元します。そして、Alice が使用したのと同じハッシュ アルゴリズムを使用してメッセージをハッシュします。 Bob が計算したメッセージ ダイジェストが Alice から受け取ったメッセージ ダイジェストと正確に一致する場合、そのメッセージは秘密キーの所有者から送信されたことになり、データも変更されていないことが保証されます。 Alice が秘密キーの所有者であることが確かならば、Bob にはメッセージが Alice から送信されたことがわかります。

> [!NOTE]
> 送信者の公開キーは公開された情報であり、通常はデジタル署名の書式に含まれるため、だれでも署名を検査できます。 この方法では、メッセージの秘密性は保持されません。メッセージを秘密にしておくためには、メッセージ自体も暗号化する必要があります。

.NET Framework には、デジタル署名アルゴリズムを実装する次のクラスが用意されています。

- <xref:System.Security.Cryptography.DSACryptoServiceProvider>

- <xref:System.Security.Cryptography.RSACryptoServiceProvider>

- <xref:System.Security.Cryptography.ECDsa> (基底クラス)

- <xref:System.Security.Cryptography.ECDsaCng>

## <a name="hash-values"></a>ハッシュ値

ハッシュ アルゴリズムは、任意の長さのバイナリ値を、ハッシュ値と呼ばれるより小さい固定長のバイナリ値に変換します。 ハッシュ値は、ひとまとまりのデータを数値で表現したものです。 平文の段落のハッシュを計算してから、段落の 1 文字だけでも変更すると、その後のハッシュでは別の値が生成されることになります。 強力な暗号化におけるハッシュは、値が大幅に変わります。 たとえば、メッセージの 1 ビットを変更すると、強力なハッシュ関数では 50% 異なる出力が生成されます。 多くの入力値が同じ出力値にハッシュされる場合があります。 ただし、同一の値にハッシュされる 2 つの異なる入力を見つけることは、計算上不可能です。

二者 (ここでは Alice と Bob) は、ハッシュ関数を使用してメッセージの整合性を確保できます。 Alice と Bob はハッシュ アルゴリズムを選択してメッセージに署名します。 Alice はメッセージを書き込み、選択したアルゴリズムを使用してそのメッセージのハッシュを作成します。 そして、次のいずれかの方法を使用します。

- Alice はプレーンテキスト メッセージとハッシュしたメッセージ (デジタル署名) を Bob に送信します。 Bob はメッセージを受信してハッシュ値を計算し、算出したハッシュ値を Alice から受け取ったハッシュ値と比較します。 ハッシュ値が同一の場合、メッセージは変更されていません。 ハッシュ値が同一でない場合は、Alice がメッセージを作成した後でその内容が変更されています。

  残念ながら、この方法では送信元の信頼性を保証できません。 どのユーザーでも Alice を偽装して Bob にメッセージを送信できます。 だれもが同じハッシュ アルゴリズムを使用してメッセージに署名できます。Bob が判断できるのは、メッセージがその署名と一致するかどうかだけです。 これは、一種の man-in-the-middle 攻撃です。 詳細については、「 [Cryptography Next Generation (CNG) セキュリティで保護された通信の例](https://docs.microsoft.com/previous-versions/cc488018(v=vs.100))」を参照してください。

- Alice はセキュリティで保護されていないパブリック チャネルを使用して、Bob にプレーンテキスト メッセージを送信します。 そして、セキュリティで保護されたプライベート チャネルを使用して、Bob にハッシュしたメッセージを送信します。 Bob はプレーンテキスト メッセージを受信し、ハッシュを計算して、プライベートに交換したハッシュと比較します。 ハッシュが一致すると、Bob は次の 2 つのことを判断できます。

  - メッセージが変更されていないこと。

  - メッセージの送信元 (Alice) は認証されていること。

  このしくみを機能させるために、Alice は元のハッシュ値を Bob 以外の人から隠す必要があります。

- Alice はセキュリティで保護されていないパブリック チャネルを使用して Bob にプレーンテキスト メッセージを送信し、パブリックに参照できる Web サイトにハッシュしたメッセージを配置します。

  この方法は、だれかがハッシュ値を変更することを防ぐことで、メッセージの改ざんを防止します。 メッセージとそのハッシュ値はどのユーザーでも読み取ることができますが、ハッシュ値を変更できるのは Alice だけです。 Alice を偽装しようとする攻撃者は、Alice の Web サイトにアクセスする必要があります。

Alice のメッセージはプレーンテキストで送信されるため、前に説明した方法では、Alice のメッセージがだれかから読み取られないように防ぐことはできません。 セキュリティを完全にするには、一般に、デジタル署名 (メッセージ署名) と暗号化が必要です。

.NET Framework には、ハッシュアルゴリズムを実装する次のクラスが用意されています。

- <xref:System.Security.Cryptography.HMACSHA1>.

- <xref:System.Security.Cryptography.MACTripleDES>.

- <xref:System.Security.Cryptography.MD5CryptoServiceProvider>.

- <xref:System.Security.Cryptography.RIPEMD160>.

- <xref:System.Security.Cryptography.SHA1Managed>.

- <xref:System.Security.Cryptography.SHA256Managed>.

- <xref:System.Security.Cryptography.SHA384Managed>.

- <xref:System.Security.Cryptography.SHA512Managed>.

- SHA (Secure Hash Algorithm)、MD5 (Message Digest 5)、および RIPEMD-160 の各アルゴリズムすべての HMAC バリアント。

- すべての SHA アルゴリズムの CryptoServiceProvider 実装 (マネージド コード ラッパー)。

- すべての MD5 アルゴリズムおよび SHA アルゴリズムの CNG (Cryptography Next Generation) 実装。

> [!NOTE]
> 1996 年に MD5 に設計上の欠陥が発見され、代わりに SHA-1 が推奨されました。 2004 年には、新しい欠陥が発見され、MD5 は安全なアルゴリズムと見なされなくなりました。 SHA-1 アルゴリズムも安全でないことがわかり、現在は SHA-2 が推奨されています。

## <a name="random-number-generation"></a>乱数生成

乱数生成は、多くの暗号化操作に欠かせない部分です。 たとえば、暗号キーはできるだけランダムにして、再現できないようにする必要があります。 暗号乱数ジェネレーターは、予測される確率が 50% よりも低い、計算上は不可能な出力を生成しなければなりません。 したがって、当て推量をされた場合でも予測できないような手段を使用する必要があります。 .NET Framework のクラスは、乱数ジェネレーターを使用して暗号化キーを生成します。

乱数ジェネレーター アルゴリズムは、 <xref:System.Security.Cryptography.RNGCryptoServiceProvider> クラスに実装されています。

## <a name="clickonce-manifests"></a>ClickOnce マニフェスト

.NET Framework 3.5 では、次の暗号化クラスを使用して、 [ClickOnce テクノロジ](/visualstudio/deployment/clickonce-security-and-deployment)を使用して配置されたアプリケーションのマニフェスト署名に関する情報を取得および検証できます。

- <xref:System.Security.Cryptography.ManifestSignatureInformation> クラスは、 <xref:System.Security.Cryptography.ManifestSignatureInformation.VerifySignature%2A> メソッド オーバーロードの使用時に、マニフェストの署名に関する情報を取得します。

- 検証するマニフェストを指定するには、 <xref:System.Security.ManifestKinds> 列挙体を使用します。 検証の結果は、 <xref:System.Security.Cryptography.SignatureVerificationResult> のいずれかの列挙値になります。

- <xref:System.Security.Cryptography.ManifestSignatureInformationCollection> クラスは、検証された署名の <xref:System.Security.Cryptography.ManifestSignatureInformation> オブジェクトの読み取り専用コレクションを提供します。

 特定の署名情報を提供するクラスとしては、他にも次のようなものがあります。

- <xref:System.Security.Cryptography.StrongNameSignatureInformation> は、マニフェストの厳密な名前の署名情報を保持します。

- <xref:System.Security.Cryptography.X509Certificates.AuthenticodeSignatureInformation> は、マニフェストの Authenticode 署名情報を表します。

- <xref:System.Security.Cryptography.X509Certificates.TimestampInformation> は、Authenticode 署名のタイム スタンプ情報を保持します。

- <xref:System.Security.Cryptography.X509Certificates.TrustStatus> は、Authenticode 署名が信頼済みかどうかを簡単な方法でチェックできます。

## <a name="suite-b-support"></a>Suite B のサポート

.NET Framework 3.5 は、国家安全機関 (NSA) によって発行された暗号化アルゴリズムの Suite B セットをサポートしています。 Suite B の詳細については、「 [NSA の Suite B 暗号化に関するファクト シート](https://www.nsa.gov/what-we-do/information-assurance/)」を参照してください。

次のアルゴリズムが含まれています。

- キー サイズが 128 ビット、192 ビット、および 256 ビットの AES (Advanced Encryption Standard) アルゴリズム。暗号化に使用されます。

- Secure Hash Algorithm (SHA-1、SHA-256、SHA-384、および SHA-512)。ハッシュ化に使用されます (通常、最後の 3 つはまとめて SHA-2 と呼ばれます)。

- 256 ビット、384 ビット、および 521 ビットの素数の曲線を使用した ECDSA (Elliptic Curve Digital Signature Algorithm)。署名に使用されます。 NSA のドキュメントでは、これらの曲線が具体的に定義され、P-256、P-384、および P-521 と呼ばれています。 このアルゴリズムは、 <xref:System.Security.Cryptography.ECDsaCng> クラスによって提供されます。 秘密キーを使用して署名したり、公開キーを使用した署名を検証したりできます。

- 256 ビット、384 ビット、および 521 ビットの素数の曲線を使用した ECDH (Elliptic Curve Diffie-Hellman) アルゴリズム。キーの交換と秘密協定に使用されます。 このアルゴリズムは、 <xref:System.Security.Cryptography.ECDiffieHellmanCng> クラスによって提供されます。

新たに追加された <xref:System.Security.Cryptography.AesCryptoServiceProvider>、 <xref:System.Security.Cryptography.SHA256CryptoServiceProvider>、 <xref:System.Security.Cryptography.SHA384CryptoServiceProvider>、 <xref:System.Security.Cryptography.SHA512CryptoServiceProvider> の各クラスでは、連邦情報処理規格 (FIPS: Federal Information Processing Standard) によって認定された AES、SHA-256、SHA-384、および SHA-512 実装のマネージド コード ラッパーを使用できます。

## <a name="cryptography-next-generation-cng-classes"></a>CNG (Cryptography Next Generation) クラス

CNG のクラスには、ネイティブ CNG 関数を扱うマネージド ラッパーが用意されています (CNG は CryptoAPI に代わるものです)。これらのクラスは、名前の一部として "Cng" を持ちます。 CNG ラッパー クラスの中心は、CNG キーのストレージと使用を抽象化する <xref:System.Security.Cryptography.CngKey> キー コンテナー クラスです。 このクラスにより、キー ペアまたは公開キーを安全に格納したり、単純な文字列名を使って参照したりすることが可能になります。 楕円曲線ベースの <xref:System.Security.Cryptography.ECDsaCng> 署名クラスおよび <xref:System.Security.Cryptography.ECDiffieHellmanCng> 暗号化クラスは、 <xref:System.Security.Cryptography.CngKey> オブジェクトを使用できます。

<xref:System.Security.Cryptography.CngKey> クラスは、キーを開く、作成する、削除する、エクスポートするなど、さまざまな補足的な操作に使用されます。 また、ネイティブ関数を直接呼び出すときに使用する、基になるキー ハンドルへのアクセスも提供します。

.NET Framework 3.5 には、次のようなさまざまなサポート CNG クラスも含まれています。

- <xref:System.Security.Cryptography.CngProvider> は、キー ストレージ プロバイダーを管理します。

- <xref:System.Security.Cryptography.CngAlgorithm> は、CNG アルゴリズムを管理します。

- <xref:System.Security.Cryptography.CngProperty> は、よく使用される主要なプロパティを管理します。

## <a name="related-topics"></a>関連トピック

|[タイトル]|説明|
|-----------|-----------------|
|[暗号モデル](../../../docs/standard/security/cryptography-model.md)|基底クラス ライブラリに暗号化がどのように実装されているかについて説明します。|
|[チュートリアル: 暗号化アプリケーションの作成](../../../docs/standard/security/walkthrough-creating-a-cryptographic-application.md)|基本的な暗号化タスクと復号化タスクについて説明します。|
|[暗号化クラスの設定](../../../docs/framework/configure-apps/configure-cryptography-classes.md)|アルゴリズム名を暗号クラスに割り当てる方法と、オブジェクト ID を暗号化アルゴリズムに割り当てる方法について説明します。|
