## ネットワーク通信のテスト (iOS アプリ)

ほぼすべての iOS アプリは一つ以上のリモートサービスのクライアントとして機能します。このネットワーク通信は一般に公開されたインターネットから行われ、たいていは公衆 Wi-Fi などの信頼できないネットワークを介して行われるため、古典的なネットワークベースの攻撃が潜在的な問題になります。

ほとんどの最新のモバイルアプリは http ベースの (ウェブ) サービスのバリエーションを使用しています。これらのプロトコルは十分に文書化されサポートされています。iOS では、<code>NSURLConnection</code> クラスが URL リクエストを非同期および同期的にロードする便利なクラスメソッドを提供します。

### App Transport Security のテスト

#### 概要

App Transport Security (ATS) <sup>[1]</sup> は NSURLConnection <sup>[2]</sup>, NSURLSession <sup>[3]</sup>, CFURL <sup>[4]</sup> でパブリックホスト名に接続する際にオペレーティングシステムが強制する一連のセキュリティチェックです。iOS SDK 9 および以降のアプリケーションビルドでは ATS がデフォルトで有効になっています。

ATS はパブリックホスト名に接続する際にのみ強制されます。したがって、IP アドレス、不完全なドメイン名、.local の TLD への接続は ATS で保護されません。

以下は App Transport Security Requirements <sup>[1]</sup> の要約リストです。

- HTTP 接続は許可されない。
- X.509 証明書には SHA256 フィンガープリントがあり、少なくとも 2048 ビットの RSA 鍵または 256 ビットの楕円曲線暗号 (ECC) 鍵で署名する必要がある。
- Transport Layer Security (TLS) バージョンは 1.2 もしくは以降が必要であり、Elliptic Curve Diffie-Hellman Ephemeral (ECDHE) 鍵交換による Perfect Forward Secrecy (PFS) および AES-128 もしくは AES-256 対称暗号をサポートする必要がある。

暗号スイートは以下のいずれかが必要である。

* `TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384`
* `TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256`
* `TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384`
* `TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA`
* `TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256`
* `TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA`
* `TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384`
* `TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256`
* `TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384`
* `TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256`
* `TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA`

##### ATS 例外

ATS の制限は Info.plist ファイルの `NSAppTransportSecurity` キーに例外を設定することで無効にできます。これらの例外には以下があります。
* 安全でない接続の許可 (HTTP)
* 最小 TLS バージョンの引き下げ
* PFS の無効化
* ローカルドメインへの接続の許可

ATS 例外はグローバルまたはドメイン単位で適用できます。アプリケーションは ATS をグローバルに無効化できますが、個々のドメインをオプトインできます。Apple Developer ドキュメントの以下のリストでは `NSAppTransportSecurity` Dictionary の構造を示しています <sup>[1]</sup> 。

```
NSAppTransportSecurity : Dictionary {
    NSAllowsArbitraryLoads : Boolean
    NSAllowsArbitraryLoadsForMedia : Boolean
    NSAllowsArbitraryLoadsInWebContent : Boolean
    NSAllowsLocalNetworking : Boolean
    NSExceptionDomains : Dictionary {
        <domain-name-string> : Dictionary {
            NSIncludesSubdomains : Boolean
            NSExceptionAllowsInsecureHTTPLoads : Boolean
            NSExceptionMinimumTLSVersion : String
            NSExceptionRequiresForwardSecrecy : Boolean   // Default value is YES
            NSRequiresCertificateTransparency : Boolean
        }
    }
}
```
情報源: Apple Developer Documentation <sup>[1]</sup>.

以下の表はグローバル ATS 例外をまとめたものです。これらの例外の詳細については、参考情報 [1] の Table 2 を参照ください。

| キー | 説明 |
| -----| -----|
| `NSAllowsArbitraryLoads` | `NSExceptionDomains` の下に指定された個々のドメインを除いてグローバルに ATS 制限を無効化する |
| `NSAllowsArbitraryLoadsInWebContent` | WebView から作成されたすべての接続に対して ATS 制限を無効にする |
| `NSAllowsLocalNetworking` | 非修飾ドメイン名と .local ドメインへの接続を許可する |
| `NSAllowsArbitraryLoadsForMedia` | AV Foundation フレームワークからロードされたメディアのすべての ATS 制限を無効にする |


以下の表はドメインごとの ATS 例外をまとめたものです。これらの例外の詳細については、参考情報 [1] の Table 3 を参照ください。

| キー | 説明 |
| -----| -----|
| `NSIncludesSubdomains` | ATS 例外を名前付きドメインのサブドメインに適用すべきかどうかを示す |
| `NSExceptionAllowsInsecureHTTPLoads` | 名前付きドメインへの HTTP 接続を許可するが、TLS 要件には影響しない |
| `NSExceptionMinimumTLSVersion` | TLS バージョン 1.2 未満のサーバーへの接続を許可する |
| `NSExceptionRequiresForwardSecrecy` | Perfect Forward Secrecy (PFS) を無効化する |


2017年1月1日から Apple App Store はレビューを開始し、以下の ATS 例外の一つが定義されている場合は正当な理由を要求します。しかし、この不同意は延期されました。後に Apple が「準備期間を増やすためにこの期限を延長し、新しい締め切りが確定したときに別のアップデートを提供する」と述べています <sup>[5]</sup> 。

* `NSAllowsArbitraryLoads`
* `NSAllowsArbitraryLoadsForMedia`
* `NSAllowsArbitraryLoadsInWebContent`
* `NSExceptionAllowsInsecureHTTPLoads`
* `NSExceptionMinimumTLSVersion`

#### 静的解析

ソースコードが利用可能である場合、テキストエディタを使用してアプリケーションバンドルディレクトリの `Info.plist` ファイルを開き、アプリケーション開発者が設定した例外を探します。このファイルはアプリケーションコンテキストを考慮して調べる必要があります。

以下のリストは ATS 制限をグローバルに無効化するように設定された例外の例です。

```
	<key>NSAppTransportSecurity</key>
	<dict>
		<key>NSAllowsArbitraryLoads</key>
		<true/>
	</dict>
```

ソースコードが利用可能ではない場合、`Info.plist` ファイルは脱獄済みデバイスから取得するか、アプリケーション IPA ファイルから抽出する必要があります。

IPA ファイルは ZIP アーカイブであるため、任意の zip ユーティリティを使用して抽出できます。

```
$ unzip app-name.ipa
```

`Info.plist` ファイルは抽出した `Payload/BundleName.app/` ディレクトリにあります。これはバイナリエンコードされたファイルであり、解析には人が読める形式に変換する必要があります。

`plutil` <sup>[6]</sup> はこの目的のために設計されたツールです。Mac OS 10.2 以降のバージョンでネイティブに提供されます。

以下のコマンドは Info.plist ファイルを XML 形式に変換する方法を示しています。
```
$ plutil -convert xml1 Info.plist
```

このファイルを人が読める形式に変換すると、例外を解析できます。アプリケーションには通常の機能を許可するために ATS 例外が定義されている場合があります。例えば、Firefox iOS アプリケーションでは ATS がグローバルに無効化されています。アプリケーションが任意のHTTP ウェブサイトや ATS 要件を満たさないウェブサイトに接続できなくなるため、この例外は許容されます。


#### 動的解析

--TODO

#### 改善方法
* ATS は常に有効にし、特定の状況下でのみ無効化する。
* アプリケーションはアプリケーション所有者が管理する定義された数のドメインに接続する場合、ATS 要件をサポートするようにサーバーを構成し、アプリ内の ATS 要件をオプトインする。以下の例では、`example.com` はアプリケーション所有者が所有し、そのドメインに対して ATS が有効になっています。
```
<key>NSAppTransportSecurity</key>
<dict>
    <key>NSAllowsArbitraryLoads</key>
    <true/>
    <key>NSExceptionDomains</key>
    <dict>
        <key>example.com</key>
        <dict>
            <key>NSIncludesSubdomains</key>
            <true/>
            <key>NSExceptionMinimumTLSVersion</key>
            <string>TLSv1.2</string>
            <key>NSExceptionAllowsInsecureHTTPLoads</key>
            <false/>
            <key>NSExceptionRequiresForwardSecrecy</key>
            <true/>
        </dict>
    </dict>
</dict>
```

* (アプリ所有者の管理下にない) サードパーティのドメインとの接続が行われる場合、サードパーティのドメインでサポートされていない ATS 設定を評価し、無効化する必要がある。
* アプリケーションが WebView でサードパーティのウェブサイトを開く場合、iOS 10 以降では NSAllowsArbitraryLoadsInWebContent を使用して、WebView でロードされるコンテンツの ATS 制限を無効化する。

#### 参考情報

— TODO —

##### OWASP Mobile Top 10 2016

* M3 - Insufficient Transport Layer Protection - https://www.owasp.org/index.php/Mobile_Top_10_2014-M3

##### OWASP MASVS

* V5.1: "データはネットワーク上でTLSを使用して暗号化されている。セキュアチャネルがアプリ全体を通して一貫して使用されている。"
* V5.2: "TLS 設定は現在のベストプラクティスと一致している。モバイルオペレーティングシステムが推奨された標準をサポートしていない場合には可能な限り近い状態である。"

##### CWE

— TODO —

##### その他

* [1] Information Property List Key Reference: Cocoa Keys - https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html
* [2] API Reference NSURLConnection - https://developer.apple.com/reference/foundation/nsurlconnection
* [3] API Reference NSURLSession - https://developer.apple.com/reference/foundation/urlsession
* [4] API Reference CFURL - https://developer.apple.com/reference/corefoundation/cfurl-rd7
* [5] Apple Developer Portal Announcement - Supporting App Transport Security - https://developer.apple.com/news/?id=12212016b
* [6] OS X Man Pages - Plutil - https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/plutil.1.html

##### ツール

— TODO —

### エンドポイント同一性検証のテスト

#### 概要

-- TODO [Provide a general description of the issue "Testing Endpoint Identity Verification".]

#### 静的解析

-- TODO [Describe how to assess this given either the source code or installer package (APK/IPA/etc.), but without running the app. Tailor this to the general situation (e.g., in some situations, having the decompiled classes is just as good as having the original source, in others it might make a bigger difference). If required, include a subsection about how to test with or without the original sources.] --

-- TODO [Add content on "Testing Endpoint Identity Verification" with source code] --

#### 動的解析

-- TODO [Describe how to test for this issue "Testing Endpoint Identity Verification" by running and interacting with the app. This can include everything from simply monitoring network traffic or aspects of the app’s behavior to code injection, debugging, instrumentation, etc.] --

#### 改善方法

-- TODO [Describe the best practices that developers should follow to prevent this issue "Testing Endpoint Identity Verification".] --

#### 参考情報

#### OWASP Mobile Top 10 2016

* M3 - 安全でない通信 - https://www.owasp.org/index.php/Mobile_Top_10_2016-M3-Insecure_Communication

##### OWASP MASVS

* V5.3: "セキュアチャネルが確立されたときに、アプリはリモートエンドポイントのX.509証明書を確認している。有効なCAにより署名された証明書のみが受け入れられている。"

##### CWE

* CWE-296 - Improper Following of a Certificate's Chain of Trust - https://cwe.mitre.org/data/definitions/296.html
* CWE-297 - Improper Validation of Certificate with Host Mismatch - https://cwe.mitre.org/data/definitions/297.html
* CWE-298 - Improper Validation of Certificate Expiration - https://cwe.mitre.org/data/definitions/298.html

##### その他

- [1] Meyer's Recipe for Tomato Soup - http://www.finecooking.com/recipes/meyers-classic-tomato-soup.aspx
- [2] Another Informational Article - http://www.securityfans.com/informational_article.html

##### ツール

-- TODO [Add relevant tools for "Testing Endpoint Identity Verification"] --


### カスタム証明書ストアおよび SSL ピンニングのテスト

#### 概要

証明書ピンニングはサーバーで使用されていることが分かっている証明書をクライアントにハードコードします。この技法は不正な CA や CA の侵害の脅威を軽減するために使用されます。サーバーの証明書をピンニングするとそれらの CA はゲーム終了となります。証明書ピンニングを実装するモバイルアプリケーションでは限られた数のサーバーにのみ接続します。そのため、信頼できる CA やサーバー証明書の小さなリストをアプリケーションにハードコードします。

#### 静的解析

以下に示すコードはサーバーによって提供された証明書がアプリケーションにハードコードされた証明書を反映しているかどうかを確認する方法を示しています。以下のメソッドは接続認証を実装して、接続が認証チャレンジの要求を送信することをデリゲートに通知します。

デリゲートは connection:canAuthenticateAgainstProtectionSpace: と connection: forAuthenticationChallenge を実装する必要があります。connection: forAuthenticationChallenge では、デリゲートは SecTrustEvaluate をコールして一般的な X509 チェックを実行する必要があります。以下は証明書のチェックを実装するスニペットです。

```Objective-C
(void)connection:(NSURLConnection *)connection willSendRequestForAuthenticationChallenge:(NSURLAuthenticationChallenge *)challenge
{
  SecTrustRef serverTrust = challenge.protectionSpace.serverTrust;
  SecCertificateRef certificate = SecTrustGetCertificateAtIndex(serverTrust, 0);
  NSData *remoteCertificateData = CFBridgingRelease(SecCertificateCopyData(certificate));
  NSString *cerPath = [[NSBundle mainBundle] pathForResource:@"MyLocalCertificate" ofType:@"cer"];
  NSData *localCertData = [NSData dataWithContentsOfFile:cerPath];
  The control below can verify if the certificate received by the server is matching the one pinned in the client.
  if ([remoteCertificateData isEqualToData:localCertData]) {
  NSURLCredential *credential = [NSURLCredential credentialForTrust:serverTrust];
  [[challenge sender] useCredential:credential forAuthenticationChallenge:challenge];
}
else {
  [[challenge sender] cancelAuthenticationChallenge:challenge];
}
```

#### 動的解析

##### サーバー証明書の検証

セキュアな接続を確立する中でアプリケーションの動作をテストすることで解析を開始します。テストアプローチは SSL ハンドシェイクネゴシエーションのセキュリティを少しずつ緩めて、どのセキュリティメカニズムが有効であるかを確認することです。

1. Wi-Fi 設定でプロキシとして設定された burp を使用する場合、トラストストア (設定 -> 一般 -> プロファイル) に証明書が追加されていること、および SSL キルスイッチなどのツールが無効であることを確認します。アプリケーションを起動して、Burp にトラフィックが表示されるかどうかを確認します。問題がある場合は 'Alerts' タブに報告されます。トラフィックが表示される場合、証明書検証がまったく実行されていないことを意味します。これは事実上アクティブな攻撃者があなたのアプリケーションに対して静かに MiTM を実行できることを意味します。そうではなければ、トラフィックは表示されておらず SSL ハンドシェイク失敗に関する情報があります。次の点に従います。
2. 次に、[セキュリティテスト入門 セクション](./0x06b-Basic-Security-Testing.md) で説明されているように、Burp 証明書をインストールします。ハンドシェイクが成功して Burp にトラフィックが表示されている場合、デバイスのトラストストアに対して証明書が検証されているが、ピンニングが実行されていないことを意味します。[セキュリティテスト入門 セクション](./0x06b-Basic-Security-Testing.md) で説明されているように、この時点での2つの主な攻撃シナリオは不正な CA とフィッシング攻撃であるため、リスクは以前のシナリオほどではありません。
3. 前のステップでの指示を実行してもトラフィックが burp 経由でプロキシされない場合、証明書は実際にピンされ、すべてのセキュリティ対策が実行されていることを意味します。但し、アプリケーションをテストするには依然としてピンニングをバイパスする必要があります。詳細については、[セキュリティテスト入門 セクション](./0x06b-Basic-Security-Testing.md) を参照ください。

##### クライアント証明書の検証

アプリケーションによっては双方向 SSL ハンドシェイクを使用するものがあります。つまり、アプリケーションがサーバーの証明書を検証し、サーバーがクライアントの証明書を検証します。Burp 'Alerts' タブにクライアントが接続のネゴシエーションに失敗したことを示すエラーがあるかどうかを確認します。

注目すべきことがいくつかあります。

1. クライアント証明書には鍵交換で使用される秘密鍵が含まれています
2. 一般的に証明書には使用(復号)するためにパスワードも必要です
3. 証明書自体はバイナリ自体、データディレクトリ、もしくはキーチェーンに格納されます

双方向ハンドシェイクを行う最も一般的で不適切な方法は、アプリケーションバンドル内にクライアント証明書を格納し、パスワードをハードコードすることです。すべてのクライアントが同じ証明書を共有するため、これはほとんどセキュリティをもたらさないことが明らかです。

証明書(および場合によってはパスワード)を格納する第2の方法はキーチェーンを使用するものです。最初のログイン時に、アプリケーションは個人証明書をダウンロードし、キーチェーンにセキュアに格納します。

アプリケーションはハードコードされ最初のログイン時に使用される1つの証明書を持ち、それから個人証明書がダウンロードされることがあります。この場合、サーバーに接続するために「汎用」証明書を使用できるかどうかを確認します。

(CycriptやFridaを使用して)アプリケーションから証明書を抽出し、Burp のクライアント証明書としてそれを追加すると、トラフィックを傍受することが可能となります。

#### 改善方法

ベストプラクティスとしては、証明書をピンすべきです。これにはいくつかの方法がありますが、最も一般的な方法は以下のとおりです。

1. アプリケーションバンドルにサーバーの証明書を含め、各接続で検証を実行します。これはサーバーの証明書が更新されるたびに更新メカニズムが必要となります。
2. 証明書の発行者を1つのエンティティなどに制限し、ルート CA の公開鍵をアプリケーションにバンドルします。このようにして攻撃対象領域を制限して有効な証明書を取得します。
3. 独自の PKI を所有および管理します。アプリケーションにはルート CA の公開鍵が含まれます。これは期限切れなどによるサーバーの証明書を変更するたびにアプリケーションを更新することを回避します。独自の CA を使用すると証明書が自己署名されることに注意します。

#### 参考情報

##### OWASP Mobile Top 10 2016

* M3 - 安全でない通信 - https://www.owasp.org/index.php/Mobile_Top_10_2016-M3-Insecure_Communication

##### OWASP MASVS

* V5.4 "アプリは独自の証明書ストアを使用するか、エンドポイント証明書もしくは公開鍵を固定化しており、信頼できるCAにより署名された場合でも、別の証明書や鍵を提供するエンドポイントとの接続を確立していない。"

##### CWE

* CWE-295 - Improper Certificate Validation

##### その他

* [1] Setting Burp Suite as a proxy for iOS Devices : https://support.portswigger.net/customer/portal/articles/1841108-configuring-an-ios-device-to-work-with-burp
* [2] OWASP - Certificate Pinning for iOS : https://www.owasp.org/index.php/Certificate_and_Public_Key_Pinning#iOS
