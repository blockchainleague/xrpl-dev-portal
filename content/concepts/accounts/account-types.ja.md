---
html: account-types.html
parent: accounts.html
blurb: XRP Ledgerで自動的にトランザクションを送信するビジネスは、リスクを最小限に抑えるために目的ごとに別のアドレスを設定することをおすすめします。
labels:
  - トークン
  - セキュリティ
---
# 発行アドレスと運用アドレス

{% include '_snippets/issuing-and-operational-addresses-intro.ja.md' %}
<!--{#_ #}-->

## 資金のライフサイクル

XRP LedgerのXRP以外の残高はすべて、2つのXRP Ledgerアドレス間の会計上の関係に関連付けられている _発行済み通貨_ です。Rippleが推奨する役割の分割を金融機関が行うと、その金融機関に関連する資金の流れは循環する傾向にあります。

[![図: 発行アドレスからスタンバイアドレス、運用アドレス、顧客アドレスおよびパートナーアドレスに移動し、最後に発行アドレスに戻る資金フロー](img/funds_flow_diagram.png)](img/funds_flow_diagram.png)

発行アドレスはペイメントの送金時に、XRP Ledgerの会計上の関係に残高を作成します。ユーザーはXRP Ledger内のさまざまな会計上の関係と残高を交換できます。このため、XRP以外の残高を表す用語として _イシュアンス_ を使用します。イシュアンスの額は、発行アドレスの側から見ると債務を表すため、マイナスです。同じイシュアンスの額を発行アドレスの相手側から見ると、プラスになります。発行アドレスがペイメントを受領すると、送信されたイシュアンスが消去され、発行アドレスの債務が減少します。

イシュアンスは発行アドレスからスタンバイアドレスに送信されるか、または運用アドレスに直接送信されます。これらのイシュアンスはスタンバイアドレスから運用アドレスに送信されます。運用アドレスから他の取引相手（流動性プロバイダー、パートナー、その他の顧客など）にペイメントが送信されます。すべてのイシュアンスは発行アドレスとの会計上の関係に関連付けられているため、イシュアンスのペイメントと取引は、発行アドレスを「通じてRippling」されます。ペイメントが行われると、送金元と発行アドレスの会計上の関係において送金元の残高から支払額が引き落とされ、受取人と発行アドレスの会計上の関係において受取人の残高に支払額が入金されます。XRP Ledgerでは、オーダーブックや[資金のRipplingに対応する流動性プロバイダー](rippling.html)を通じて複数のイシュアーを結び付ける、より複雑な[パス](paths.html)もサポートされています。

## 発行アドレス

発行アドレスは、金庫に似ています。パートナーアドレス、顧客アドレス、運用アドレスは、発行アドレスとの間で会計上の関係（トラストライン）を作成しますが、発行アドレスから送信されるトランザクションは可能な限り少ない数に抑えられます。人間のオペレーターが定期的に、発行アドレスからトランザクションを作成、署名し、スタンバイアドレスまたは運用アドレスの残高を補充します。このようなトランザクションの署名に使用されるシークレットキーには、インターネットに接続されたどのコンピューターからもアクセスできないことが極めて重要です。

金庫とは異なり、発行アドレスは顧客またはパートナーからのペイメントを直接受領できます。XRP Ledgerのトランザクションはすべて公開されているため、自動システムは発行アドレスからのペイメントを監視する際にシークレットキーを必要としません。

### 発行アドレスの漏えい

不正使用者は金融機関の発行アドレスのシークレットキーを入手すると、際限なく新しいイシュアンスを作成し、分散型取引所でそのイシュアンスを取引できるようになります。これにより、金融機関が正式に取得したイシュアンスを識別して適切に清算することが難しくなります。金融機関の発行アドレスが乗っ取られた場合には、金融機関が新たに発行アドレスを作成する必要があり、また古い発行アドレスと会計上の関係を有するすべてのユーザーは新しいアドレスで、アカウントとの関係を新たに作成する必要があります。

### 複数の発行アドレス

金融機関はXRP Ledgerで1つの発行アドレスから複数の通貨を発行できます。ただし、いくつかの設定（[送金手数料](transfer-fees.html)のパーセンテージや[Global Freeze](freezes.html)の状況など）は、1つのアドレスから発行されるすべての通貨に同様に適用されます。金融機関が通貨ごとに設定を変えて柔軟に管理したい場合、金融機関は通貨ごとに異なる発行アドレスを使用する必要があります。

## 運用アドレス

運用アドレスはレジに似ています。イシュアンスを顧客とパートナーに送信して、金融機関に代わってペイメントを行います。トランザクションに自動的に署名するには、運用アドレスのシークレットキーをインターネットに接続されたサーバーに保管する必要があります。（シークレットキーは暗号化して保管できますが、サーバーがトランザクションに署名する際にシークレットキーを暗号化解除する必要があります。）顧客とパートナーは、運用アドレスとの会計上の関係を作成すべきではありません。

各運用アドレスではイシュアンスの残高が限られています。運用アドレスの残高が少なくなると、金融機関は残高を補充するため、発行アドレスまたはスタンバイアドレスから送金します。

### 運用アドレスの漏えい

不正使用者が運用アドレスのシークレットキーを入手した場合に金融機関が失う可能性のある通貨額は、最大でも運用アドレスが保有している額までです。金融機関は、顧客やパートナーからのアクションなしに、新しい運用アドレスに切り替えることができます。

## スタンバイアドレス

金融機関がリスクと利便性のバランスを保つためのもう1つの手段として、発行アドレスと運用アドレスの中間ステップとして「スタンバイアドレス」を利用することができます。金融機関はスタンバイアドレスという追加のXRP Ledgerアドレスに資金を供給できます。このアドレスのキーはオンライン上に保管されず、別の信頼できるユーザーに預けられています。

運用アドレスの資金が少なくなると、信頼できるユーザーがスタンバイアドレスを使用して運用アドレスの残高を補充できます。スタンバイアドレスの資金が少なくなると、金融機関は発行アドレスを使用して1回のトランザクションでスタンバイアドレスにより多くの額の通貨を送金できます。スタンバイアドレスは、送金された通貨を必要に応じてスタンバイアドレス間で分散できます。これにより発行アドレスのセキュリティが強化され、発行アドレスが実行する合計トランザクション数が減少し、1つの自動化システムの管理下に過剰な資金が残ることがなくなります。

運用アドレスと同様に、スタンバイアドレスは顧客やパートナーではなく発行アドレスとの間に会計上の関係を確立する必要があります。運用アドレスに適用される注意事項はすべてスタンバイアドレスにも適用されます。

### スタンバイアドレスの漏えい

スタンバイアドレスの漏えいが発生した場合、運用アドレスが漏えいした場合と同様の影響が及びます。不正使用者がスタンバイアドレスに保有される残高を盗むことが可能となり、金融機関は顧客やパートナーからのアクションなしに新しいスタンバイアドレスに切り替えることができます。

## 関連項目

- **コンセプト:**
  - [アカウント](accounts.html)
  - [暗号鍵](cryptographic-keys.html)
- **チュートリアル:**
  - [XRP Ledgerゲートウェイの開設](stablecoin-issuer.html)
  - [レギュラーキーペアの割り当て](assign-a-regular-key-pair.html)
  - [レギュラーキーペアの変更または削除](change-or-remove-a-regular-key-pair.html)
- **リファレンス:**
  - [account_infoメソッド][]
  - [SetRegularKeyトランザクション][]
  - [AccountRootオブジェクト](accountroot.html)

<!--{# common link defs #}-->
{% include '_snippets/rippled-api-links.md' %}
{% include '_snippets/tx-type-links.md' %}
{% include '_snippets/rippled_versions.md' %}
