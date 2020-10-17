# The State of Messaging Security 2020:<br>メールおよびメッセージングアプリのセキュリティプロトコルの現在
- 公開日: 2020年10月
- 主に、メールおよびメッセージングアプリのセキュリティプロトコル全体について、2020年現在の状況を、具体的な技術観点から、約40ページにわたって図も加えた上で説明している。
- 本文書を読むだけで、メールの仕様・各セキュリティプロトコルの機能・制約・普及率などの現況、そしてWhatsAppやFacebook Messengerなどに代表されるメッセージングアプリのEnd-to-end暗号化プロトコル仕様を把握できるようになることを目指した。
- 本文書の各章には要点を記載しているため、その箇所のみ読み結論を把握するという読み方もできるようにしている。

---

- 文書リンク
  - [Google Docs版(推奨)](https://docs.google.com/document/d/1SZbOlOcEF8BuuQfeTc1BQ6JFk9S83kpdYrLlrLh02sU/edit)
  - [PDF版](messaging-security-2020.pdf)
- [なぜ上記文書を書いたのか](#なぜ上記文書を書いたのか)
- [目次・各章の要点](#目次各章の要点)
- [参考文献](#参考文献)
- [注意事項](#注意事項)

## 想定読者
- 自分の使っているメールやメッセージングアプリのセキュリティがどのように守られているのか関心のある方
- PrivacyやTLS、End-to-end暗号化などのセキュリティについて関心のある方
- ネットワークプロトコルに興味のある方
- トップシークレット情報を扱うジャーナリストなど、情報の保護が必要となる方

## 本文リンク
- The State of Messaging Security 2020: <br>メールおよびメッセージングアプリのセキュリティプロトコルの現在

|[Google Docs版 (推奨)](https://docs.google.com/document/d/1SZbOlOcEF8BuuQfeTc1BQ6JFk9S83kpdYrLlrLh02sU/edit)|[PDF版](messaging-security-2020.pdf)|
|---|---|
|[<img src="img/docs-google.png" width="300px">](https://docs.google.com/document/d/1SZbOlOcEF8BuuQfeTc1BQ6JFk9S83kpdYrLlrLh02sU/edit)|[<img src="img/docs-pdf.png" width="300px">](messaging-security-2020.pdf)|


## なぜ上記文書を書いたのか
2013年のスノーデン事件以降、PrivacyおよびSecurityは、インターネットの基盤として最重要視されるようになった。無償でTLS証明書を発行するLet's Encryptが設立され、Mozilla、Googleなどのブラウザベンダ、各大手CDNがスポンサーとなった。そして、日本を含む世界で、HTTPSの普及が一気に進んだ。  
しかしながら、メールに関連する暗号化やセキュリティについての情報は、特に日本において、エンドユーザ目線からの議論が十分に行われているとは言い難い。  
Googleの公開する[透明性レポート](https://transparencyreport.google.com/safer-email/overview?hl=ja)において、メールにおけるTLS暗号化の普及率が、世界的には2014年の約30%から2020年には約90%に増加した。しかし、2020年10月時点でGmailなどから外部に送信されたメールドメインのうち、暗号化されず平文で送信されたメールのTop10のうち半分を日本(.jp)ドメインが占める状況になっており、日本ではメールにおける暗号化の議論は活発に行われなかった可能性がある。  

メールは依然としてインターネットを支える根幹の技術として位置し続けている。いくら個人間コミュニケーションとしてWhatsApp、LINE、Facebook Messengerなどが普及しようとも、オープンに標準化され、誰もが使えるメッセージングプロトコルとして、メールは特定のメッセージングアプリでは置き換えることの出来ないインターネットの基盤技術である。たとえば多くのサービスは、メールを使ったパスワードリセットを提供しているし、HTTPSを支えるTLS証明書は、メールを使ったドメイン所有の確認により発行することが出来る。  

メールが基盤技術として存在する一方、個人間コミュニケーションとしては、スマートフォンの普及と同時にメッセージングアプリが流行した。スノーデン事件も受け現在は主なメッセージングアプリがEnd-to-End暗号化(エンドユーザ間の暗号化)を提供している。その中、2020年10月11日、アメリカ・イギリスを始めとする機密情報の交換ネットワークであるファイブ・アイズおよび日本・インド政府から、「国際声明: End-to-end暗号化と公共の安全 (原題: International statement: End-to-end encryption and public safety)」という声明が発表された (参考: 日本経済新聞: 「[対話アプリ、暗号化見直しで声明　日米英など7カ国](https://www.nikkei.com/article/DGXMZO64874600R11C20A0EAF000/)」)。この声明では、End-to-end暗号化を行っているメッセージングアプリに対して、暗号化された内容にアクセスする手段を設けることを要請している。日本政府も署名に加わっているものの、日本国内においてEnd-to-end暗号化について、活発に議論されているわけではない。  
End-to-end暗号化については、様々なアプローチや制約がある。読者がどういった意見を持つにせよ、End-to-end暗号化に賛成・反対といった単純な議論では終わらないテーマであろう。End-to-end暗号化を行うにせよ、「サービス提供者によるあらゆるコミュニケーションの保管が不可能になる方向を目指すべき」「FacebookがWhatsAppで行っているような、メッセージ以外のMetadata（コンタクトリスト・グループ参加情報・位置情報など）の取得は認めるべき」「メールのPGP暗号化のレベルのように、前方秘匿性(PFS)のない状態のみ認めるべき」「バックドアやMetadata取得は認めるが、GoogleおよびFacebookのような透明性レポートの提供を条件とするべき」など、様々な立場を取りうる。  

本文書では、上記のようなPrivacyやSecurityについてのトピックを議論するための前提となる、メールの仕様・各セキュリティプロトコルの機能・制約・普及率などの現況、そしてWhatsAppやFacebook Messengerなどに代表されるメッセージングアプリのEnd-to-end暗号化プロトコル仕様を説明している。メールおよび、現在広く個人間コミュニケーションで使用されているメッセージングアプリのセキュリティ仕様について、2020年現在の状況を読者が把握できることを目指した。
本文書が、今後のより発展した議論に寄与することを期待している。


## 目次・各章の要点
### Chapter 1: はじめに
- 本文書の問題意識および目的、概要を説明している


### Chapter 2: メールの仕組み
#### 要点
- メールの配送を行うソフトウェア（Sendmail, Postfixなど）をMTAと呼ぶ
- MTAによるメールの配送にはSMTPという80年代に策定されたプロトコルが使用される
  - そのままの使用だと平文での通信となる
  - 上記の問題の補強を行うために、Chapter 3から説明するセキュリティプロトコルが存在する
- メールにはEnvelope（封筒）と、Headerというものがあり、両方にメール送信元と送信先が記載されている
  - Envelope From/To: メールの配送のためにMTAが使用
  - Header From/To: メールクライアントが表示する送信元
- エンドユーザが送信元として認識しているのは、メーラーで表示されるHeader Fromである


### Chapter 3: SMTP Over TLS（STARTTLS）の守るもの
#### 要点
- メールのMTA間での経路暗号化を行う方法としてはSTARTTLSが普及している
- Googleの透明性レポートによると、2020年10月時点で世界的には90%程度が暗号化に対応しているが、日本国内の大手メールサービスなどはサポートしていないケースも多く見受けられる
- STARTTLSは、盗聴などの受動的攻撃を防ぐ手段であり、通信に介入されるなどの能動的な攻撃には無力である
  - MTA Server（受信側）での送信元の認証、MTA Client（送信側）での接続先の認証などは提供されない
- エンドユーザが、メール中継する各MTAにTLS暗号化を強制することはできない
- 能動的な攻撃も防ぐための手段として2018年にMTA-STSという仕様がIETFにて策定され、アメリカの大手メールサービスを始めとする一部のサービスが利用を始めている段階である
  - MTA-STSを用いると、MTA Clientが接続先が正しいことを認証出来る
  - ただし、MTA-STSを用いてもエンドユーザがメール中継する各MTAにTLS暗号化を強制することはできない
  - STARTTLSの普及率を考えると、全てのMTAがMTA-STSをサポートする日が来るかは疑問


### Chapter 4: メール送信元認証が守るもの
#### 要点
- 送信元メールアドレスを認証する場合、エンドユーザのメーラーに表示されるHeader Fromを確認する必要がある
- 2011年に策定されたDMARCは、メールのHeader From（送信元）ドメインをインターネット上のMTA間で認証する手段を提供する
  - メールの送信元メールアドレス全体をEnd-to-endで認証する手段ではない
- ただし2020年10月時点で送信元メールドメイン認証の強制化は普及しきっておらず、いまだに強制を望む送信者（MTA Client）は少数派である。
- 受け取ったメールの送信元が正しいと信用することは、いまだに非常に困難な状況である。


### Chapter 5: End-to-end暗号化のための仕様（PGPとS/MIME）
#### 要点
- PGPおよびS/MIMEは、End-to-end暗号化もしくは署名を提供する
- 暗号化の制約
  - End-to-end暗号化での暗号化範囲は本文（Body）である。メール配送のために必要なEnvelopeおよびメールヘッダ（Date, From, To, 件名など）は暗号化されていない。つまり、「誰が誰に、どういう件名のメールをいつ送ったのか」という情報は暗号化されない。各MTAやメールを保管するメールボックスサーバ上では、それらの情報は平文で取り扱われる。もちろん、各サーバでメール配送のログやヘッダ情報を保管することが出来るし、Chapter 3で説明したように、SMTP Over TLS（STARTTLS）による経路暗号化が行われている保証もない。
  - その他、前方秘匿性（PFS）と呼ばれる特性がないなど、暗号化には様々な制約がある。
- S/MIMEにおける署名は利用しているメール送信者がいるものの、暗号化という観点では、PGPおよびS/MIMEは利便性が悪く一般的に普及しておらず、一部のメッセージを秘匿しなければならない人々にのみ利用されている
- 大きな脆弱性が2018年に見つかったこともあり、WhatsAppやSignalといった、利便性も高くPGPよりもセキュアなEnd-to-end暗号化を提供するメッセージングアプリが普及している今、PGPを利用するケースは非常にまれと言える


### Chapter 6: WhatsAppとSignal - メール以外のメッセージングアプリの普及とE2E暗号化のもたらすもの
#### 要点
- 2016年、アメリカにおいて主に利用されているメッセージングアプリであるWhatsApp、Facebook MessengerはSignal Protocolという暗号化プロトコルを採用したEnd-to-end暗号化を開始した
- Signal ProtocolによるEnd-to-end暗号化を用いると、メールでのPGP暗号化では提供されなかった多くのセキュリティ機能が提供される
  - メールのような分散型ではなく、中央サーバと通信する形式であり、中央サーバさえ信用できれば、メッセージの送信元・送信先は保証されている
  - 中央サーバを信用しない場合でも、手動での認証を提供する
  - 暗号鍵はPGPと違い、メッセージ送信先ごとに違う鍵が使用される。秘密鍵から全ての過去のメッセージを復号できる状態ではなく、前方秘匿性（PFS）がある
  - また、過去だけでなく、将来のメッセージも読めないようになっている。（Post-Compromise Security）
- 同じSignal ProtocolでEnd-to-end暗号化をしていると言っても、各社でMetadataの収集については方針が異なっている。Signalは可能な限り全てのMetadataをEnd-to-endで暗号化する方針である一方、Facebook社のWhatsAppは、中央サーバで一定の情報を収集する。

### Chapter 7: おわりに
- 最後に、各国政府からのEnd-to-end暗号化への批判などがあることを説明している


## 参考文献
各脚注で触れた書籍・URL以外に、下記を主に参考にした

- スノーデン事件を始めとする諜報活動などの確からしい事実関係の把握
    - 土屋 大洋『サイバーセキュリティと国際政治』（千倉書房，2015年）
    - 土屋 大洋『暴露の世紀　国家を揺るがすサイバーテロリズム』（KADOKAWA、2016年）

※スノーデン事件により告発されたインテリジェンス機関による諜報活動について、一個人には事実関係を把握することが難しいため、研究者による書籍を参考にしている

- SMTPおよびTLSに関する書籍・Webサイト
    - Philip Miller『マスタリングTCP/IP 応用編』（オーム社、1998年）
    - Kevin Johnson『電子メールプロトコル詳説―インターネット電子メールアーキテクチャからIETF標準プロトコル群の詳細』（ピアソンエデュケーション、2000年）
    - Eric Rescorla『マスタリングTCP/IP SSL/TLS編』（オーム社、2003年）
    - Ivan Ristić『プロフェッショナル SSL/TLS』（ラムダノート、2017年）
    - [M3AAWG Recommendations for Senders Handling of Complaints](https://www.m3aawg.org/sites/default/files/m3aawg-senders-complaint-handling-2017-12.pdf)
    - [M3AAWG Email Authentication Recommended Best Practices](https://www.m3aawg.org/sites/default/files/m3aawg-email-authentication-recommended-best-practices-09-2020.pdf)
    - [Postfix Documentation](http://www.postfix.org/documentation.html)
    - [STARTTLS Everywhere](https://starttls-everywhere.org/)
    - RFC821: SIMPLE MAIL TRANSFER PROTOCOL
    - RFC3207: SMTP Service Extension for Secure SMTP over Transport Layer Security
    - RFC8461: SMTP MTA Strict Transport Security （MTA-STS）
    - RFC7208: Sender Policy Framework （SPF） for Authorizing Use of Domains in Email, Version 1
    - RFC6376: DomainKeys Identified Mail （DKIM） Signatures
    - RFC7489: Domain-based Message Authentication, Reporting, and Conformance （DMARC）

- PGPおよびS/MIME
    - [電子メールのセキュリティ S/MIMEを利用した暗号化と電子署名（IPA）](https://www.ipa.go.jp/security/fy12/contents/smime/email_sec.pdf)
    - [The PGP Problem](https://latacora.micro.blog/2019/07/16/the-pgp-problem.html)
    - 結城 浩『暗号技術入門 第3版　秘密の国のアリス』（SBクリエイティブ、2015年）

- Signal Protocol
    - [The State of Private Messaging Applications](https://medium.com/@LokiNetwork/the-state-of-private-messaging-applications-5e7dc0614ef7)
    - [Specifications >> The X3DH Key Agreement Protocol](https://signal.org/docs/specifications/x3dh/)
    - [Blog >> Simplifying OTR deniability.](https://signal.org/blog/simplifying-otr-deniability/)
    - [Blog >> Technology preview: Sealed sender for Signal](https://signal.org/blog/sealed-sender/)
    - [Specifications >> The Double Ratchet Algorithm](https://signal.org/docs/specifications/doubleratchet/)
    - [WhatsApp Encryption Overview](https://www.whatsapp.com/security/WhatsApp-Security-Whitepaper.pdf)
    - [CS 465 Signal](https://cs465.internet.byu.edu/static/lectures/v1/Signal.pdf) （Brigham Young University）
    - [CS 255: Intro to Cryptography](https://crypto.stanford.edu/~dabo/courses/cs255_winter19/hw_and_proj/proj2.pdf) （Stanford University）


## 注意事項
- 本文書の説明事項を悪用しないでください
- 本文書は、立場に関わらず、個人間のコミュニケーションを支えるメッセージングプロトコルの仕様を説明し、PrivacyやSecurityに関する議論の前提となる情報を提供することを目的としています。迷惑メールとの脅威と戦い続けているメール事業者、各メッセージングアプリの提供会社を批判する意図はありません。
- 本文書に誤りがあった場合、速やかに訂正します。本RepositoryのIssueもしくはsuezawa@gmail.comまでご連絡お願いします。
