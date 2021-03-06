## テストツール

セキュリティテストを実行するためにさまざまなツールが利用できます。リクエストやレスポンスを操作したり、アプリを逆コンパイルしたり、実行中のアプリの挙動を調査したり、テストケースを自動化したりできます。

### モバイルアプリケーションセキュリティテストディストリビューション

- [Appie](https://manifestsecurity.com/appie) - Android Pentesting Portable Integrated Environment. Android ペネトレーションテスト用のポータブルソフトウェアパッケージで、既存の仮想マシンに代わるものです。
- [Android Tamer](https://androidtamer.com/) - Android Tamer は Android セキュリティ専門家向けの Debian ベースの仮想／ライブプラットフォームです。
- [AppUse](https://appsec-labs.com/AppUse/) - AppUse は AppSec Labs により開発された VM (仮想マシン) です。
- [Androl4b](https://github.com/sh4hin/Androl4b) - Android アプリケーションの評価、リバースエンジニアリング、マルウェア解析のための仮想マシンです。
- [Mobisec](http://sourceforge.net/projects/mobisec/) - モバイルセキュリティテストのライブ環境です。
- [Santoku](https://santoku-linux.com/) - Santoku は OS であり、スタンドアロンのオペレーティングシステムとして VM の外部で実行できます。
- [Vezir Project](https://github.com/oguzhantopgul/Vezir-Project) - モバイルアプリケーションのペネトレーションテストおよびマルウェア解析の環境です。
- [Nathan](https://github.com/mseclab/nathan) - Nathan は AOSP Android エミュレータであり、モバイルセキュリティ評価を実行するためにカスタマイズされています。

### 静的ソースコード解析

- [Checkmarx](https://www.checkmarx.com/technology/static-code-analysis-sca/) - 静的ソースコードスキャナであり、Android および iOS 用のソースコードもスキャンします。
- [Fortify](https://saas.hpe.com/en-us/software/fortify-on-demand/mobile-security) - 静的ソースコードスキャナであり、Android および iOS 用のソースコードもスキャンします。
- [Acccenture](https://digitalsecurity.accenture.com) - 静的ソースコードスキャナであり、Android および iOS 用のソースコードもスキャンします。
- [Veracode](https://www.veracode.com/products/binary-static-analysis-sast "Veracode Static Analysis") - iOS および Android バイナリの静的解析。

### オールインワンモバイルセキュリティフレームワーク

- [Mobile Security Framework - MobSF](https://github.com/ajinabraham/Mobile-Security-Framework-MobSF) - Mobile Security Framework は静的解析および動的解析を実行できるインテリジェントでオールインワンのオープンソースモバイルアプリケーション (Android/iOS) 自動ペネトレーションテストフレームワークです。
- [Needle](https://github.com/mwrlabs/needle) - Needle はオープンソースのモジュラーフレームワークであり、バイナリ解析、静的コード解析、Cycript や Frida フッキングを使用した実行時操作などの iOS アプリのセキュリティ評価実施のプロセスを効率化します。
- [Appmon](https://github.com/dpnishant/appmon/) - AppMon はネイティブ macOS, iOS, android アプリのシステム API コールを監視および改竄するための自動化フレームワークです。
- [objection](https://github.com/sensepost/objection) - objection は iOS および android の両方に対応し、脱獄やルート化デバイスを必要としない実行時モバイルセキュリティ評価フレームワークです。


### Android 用ツール

#### リバースエンジニアリングおよび静的解析

- [Androguard](https://github.com/androguard/androguard) - Androguard は python ベースのツールで、android アプリの逆アセンブルや逆コンパイルに使用できます。
- [Android Debug Bridge - adb](https://developer.android.com/studio/command-line/adb.html) - Android Debug Bridge (adb) はエミュレータインスタンスや接続された Android デバイスと通信するための多彩なコマンドラインツールです。
- [APKInspector](https://github.com/honeynet/apkinspector/) - APKinspector は Android アプリケーションを解析するアナリスト向けの強力な GUI ツールです。
- [APKTool](http://ibotpeaches.github.io/Apktool/) - サードパーティ製でクローズなバイナリ Android アプリをリバースエンジニアリングするためのツールです。リソースをほぼ元の形にデコードし、改変後に再構築することができます。
- [android-classyshark](https://github.com/google/android-classyshark) - ClassyShark は Android 開発者向けのスタンドアロンのバイナリインスペクションツールです。
- [Sign](https://github.com/appium/sign) - Sign.jar は自動的に Android テスト証明書で apk に署名します。
- [Jadx](https://github.com/skylot/jadx) - Dex から Java への逆コンパイラです。Android Dex および Apk ファイルから Java ソースコードを生成するコマンドラインおよび GUI ツールです。
- [Oat2dex](https://github.com/testwhat/SmaliEx) - .oat ファイルから .dex ファイルに変換するためのツールです。
- [FindBugs](http://findbugs.sourceforge.net) - Java 用の静的解析ツールです。
- [FindSecurityBugs](http://h3xstream.github.io/find-sec-bugs) - FindSecurityBugs は FindBugs の拡張機能であり、Java アプリケーション向けのセキュリティルールを含んでいます。
- [Qark](https://github.com/linkedin/qark) - このツールは Android アプリケーション脆弱性に関連するいくつかのセキュリティをソースコードかパッケージ化された APK のいずれかで検索するように設計されています。
- [SUPER](https://github.com/SUPERAndroidAnalyzer/super) - SUPER は Windows, MacOS X, Linux で使用できるコマンドラインアプリケーションで、.apk ファイルを検索して脆弱性を探します。APK を展開し、脆弱性を検出するための一連のルールを適用することでこれを行います。
- [AndroBugs](https://github.com/AndroBugs/AndroBugs_Framework) - AndroBugs フレームワークは開発者やハッカーが Android アプリケーションの潜在的なセキュリティ脆弱性を発見するのに役立つ効率的な Android 脆弱性スキャナです。Windows にインストールする必要はありません。
- [Simplify](https://github.com/CalebFenton/simplify) - Classes.dex 内の android パッケージを逆難読化するツールです。Dex2jar や JD-GUI を使用して dex ファイルの内容を抽出できます。
- [ClassNameDeobfuscator](https://github.com/HamiltonianCycle/ClassNameDeobfuscator) - apktool により生成される .smali ファイルを解析して .source アノテーション行を抽出するシンプルなスクリプトです。
- [Android backup extractor](https://github.com/nelenkov/android-backup-extractor) - adb backup (ICS 以降) で作成された Android バックアップを抽出および再パックするユーティリティです。主に AOSP の BackupManagerService.java をベースとしています。
- [VisualCodeGrepper](https://sourceforge.net/projects/visualcodegrepp/) - Java などのいくつかのプログラミング言語向けの静的コード解析ツールです。
- [ByteCodeViewer](http://bytecodeviewer.com/) - 五つの異なる Java 逆コンパイラ、二つのバイトコードエディタ、Java コンパイラ、プラグイン、検索。Class, JAR, Android APK などからのロードをサポートしています。

#### 動的解析および実行時解析

- [Cydia Substrate](http://www.cydiasubstrate.com) - Android 向けの Cydia Substrate を使用すると、開発者はターゲットプロセスのメモリにインジェクトされる Substrate extension で既存のソフトウェアを改造することができます。
- [Xposed Framework](http://forum.xda-developers.com/xposed/xposed-installer-versions-changelog-t2714053) - Xposed framework を使用すると、Android アプリケーションパッケージ (APK) の改変や再フラッシュを行わずに、実行時にシステムやアプリケーションのアスペクトや動作を変更できます。
- [logcat-color](https://github.com/marshall/logcat-color) - Android SDK の adb logcat コマンドに代わるカラフルで高度な設定が可能なツールです。
- [Inspeckage](https://github.com/ac-pm/Inspeckage) - Inspeckage は Android アプリケーションの動的解析を提供するために開発されたツールです。Android API の関数にフックを適用することで、Inspeckage は Android アプリケーションが実行時に何をしているのかを理解するのに役立ちます。
- [Frida](http://www.frida.re) - このツールキットはクライアントサーバーモデルを使用して動作し、Android の上だけでなく iOS, Windows, Mac の上でも実行中のプロセスにインジェクトすることができます。
- [Diff-GUI](https://github.com/antojoseph/diff-gui) - 利用可能なモジュールで計装を開始し、ネイティブでフックし、Frida を使用して JavaScript をインジェクトする Web フレームワークです。
- [AndBug](https://github.com/swdunlop/AndBug) - AndBug は Android プラットフォーム Dalvik 仮想マシンを対象とするリバースエンジニアおよび開発者向けのデバッガです。
- [Cydia Substrate: Introspy-Android](https://github.com/iSECPartners/Introspy-Android) - Android アプリケーションが実行時に何をしているかを理解し、潜在的なセキュリティ問題の特定を支援するブラックボックスツールです。
- [Drozer](https://www.mwrinfosecurity.com/products/drozer/) - Drozer はアプリの役割を想定し、Dalvik VM と他のアプリの IPC エンドポイントや基礎をなす OS とのやり取りを行うことで、アプリやデバイスのセキュリティ脆弱性を検索することができます。
- [VirtualHook](https://github.com/rk700/VirtualHook) - VirtualHook は Android ART(>=5.0) のアプリケーション用のフッキングツールです。VirtualApp をベースにしており、フックを挿入するためにルート権限は必要ありません。

#### ルート検出と証明書ピンニングのバイパス

- [Xposed Module: Just Trust Me](https://github.com/Fuzion24/JustTrustMe) - SSL 証明書ピンニングをバイパスする Xposed モジュールです。
- [Xposed Module: SSLUnpinning](https://github.com/ac-pm/SSLUnpinning_Xposed) - SSL 証明書検証 (証明書ピンニング) をバイパスする Android Xposed モジュールです。
- [Cydia Substrate Module: Android SSL Trust Killer](https://github.com/iSECPartners/Android-SSL-TrustKiller) - デバイス上で動作するほとんどのアプリケーションの SSL 証明書ピンニングをバイパスするブラックボックスツールです。
- [Cydia Substrate Module: RootCoak Plus](https://github.com/devadvance/rootcloakplus) - 一般的に知られているルートの兆候に対するルートチェックにパッチを適用します。
- [Android-ssl-bypass](https://github.com/iSECPartners/android-ssl-bypass) - 他のデバッグタスクと同様に、証明書ピンニングが実装されている場合でも SSL をバイパスすることに使用できる Android デバッグツールです。このツールは対話型コンソールとして実行します。


### iOS 用ツール

#### iDevice 上のファイルシステムへのアクセス

- [FileZilla](https://filezilla-project.org/download.php?show_all=1) - FTP, SFTP, FTPS (FTP over SSL/TLS) をサポートしています。
- [Cyberduck](https://cyberduck.io) - Mac および Windows 向けの Libre FTP, SFTP, WebDAV, S3, Azure & OpenStack Swift ブラウザです。
- [itunnel](https://code.google.com/p/iphonetunnel-usbmuxconnectbyport/downloads/list) -  USB 経由で SSH を転送するために使用します。
- [iFunbox](http://www.i-funbox.com) - iPhone, iPad, iPod Touch 向けのファイルおよびアプリ管理ツールです。

#### リバースエンジニアリングおよび静的解析

- [otool](http://www.unix.com/man-page/osx/1/otool/) - otool コマンドはオブジェクトファイルやライブラリの指定された箇所を表示します。
- [Clutch](http://cydia.radare.org/) - アプリケーションを解読し、バイナリや .ipa ファイルに指定された bundleID をダンプします。
- [Dumpdecrypted](https://github.com/stefanesser/dumpdecrypted) - 暗号化された iPhone アプリケーションから復号された mach-o ファイルをメモリからディスクにダンプします。このツールはセキュリティ研究者が暗号化の中身を具体的に見ることができるようにするために必要となります。
- [class-dump](http://stevenygard.com/projects/class-dump/) - Mach-O ファイルに格納されている Objective-C ランタイム情報を調べるためのコマンドラインユーティリティです。
- [Flex2](http://cydia.saurik.com/package/com.fuyuchi.flex2/) - Flex はアプリを改変し動作を変更する力を与えます。
- [Weak Classdump](https://github.com/limneos/weak_classdump) - 関数に渡されるクラスのヘッダファイルを生成する Cycript スクリプトです。classdump や dumpdecrypted ができない場合やバイナリが暗号化されている場合などにとても便利です。
- [IDA Pro](https://www.hex-rays.com/products/ida/index.shtml) - IDA は Windows, Linux, Mac OS X でホストされているマルチプロセッサ逆アセンブラおよびデバッガであり、すべてを記述するのは難しいほどの多くの機能を提供しています。
- [HopperApp](http://hopperapp.com/) - Hopper は OS X および Linux 用のリバースエンジニアリングツールで、32/64 ビット Intel Mac, Linux, Windows, iOS 実行可能ファイルを逆アセンブル、逆コンパイル、デバッグすることができます。
- [Radare2](http://www.radare.org/) - Radare2 は Unix ライクなリバースエンジニアリングフレームワークおよびコマンドラインツールです。
- [iRET](https://www.veracode.com/iret-ios-reverse-engineering-toolkit) - iOS Reverse Engineering Toolkit は iOS ペネトレーションテストに関連する多くの一般的なタスクを自動化するためのツールキットです。
- [Plutil](https://www.theiphonewiki.com/wiki/Plutil) - plutil は .plist ファイルをバイナリバージョンと XML バージョンの間で変換できるプログラムです。

#### 動的解析および実行時解析

- [cycript](http://www.cycript.org) - Cycript を使用すると、開発者は構文強調表示とタブ補完機能を備えた対話型コンソールを通じて Objective-C++ および JavaScript 構文をハイブリッドに使用して iOS もしくは Mac OS X 上で実行中のアプリケーションを探索および改変できます。
- [iNalyzer](https://appsec-labs.com/cydia/) - AppSec Labs iNalyzer はパラメータやメソッドを改竄して iOS アプリケーションを操作するためのフレームワークです。
- [idb](https://github.com/dmayer/idb) - idb は iOS ペネトレーションテストおよび研究のための一般的なタスクを簡素化するツールです。
- [snoop-it](http://cydia.radare.org/) - iOS アプリのセキュリティ評価および動的解析を支援するツールです。
- [Introspy-iOS](https://github.com/iSECPartners/Introspy-iOS) - iOS アプリケーションが実行時に何をしているかを理解し、潜在的なセキュリティ問題の特定を支援するブラックボックスツールです。
- [gdb](http://cydia.radare.org/) - iOS アプリケーションの実行時解析を行うためのツールです。
- [lldb](https://lldb.llvm.org/) - Apple の Xcode に付属する LLDB デバッガは iOS アプリケーションをデバッグするために使用されます。
- [keychaindumper](http://cydia.radare.org/) - iOS デバイスが脱獄された場合に攻撃者が利用可能となるキーチェーンアイテムを確認するためのツールです。
- [BinaryCookieReader](http://securitylearn.net/wp-content/uploads/tools/iOS/BinaryCookieReader.py) - バイナリ Cookies.binarycookies ファイルからすべてのクッキーをダンプするツールです。
- [Burp Suite Mobile Assistant](https://portswigger.net/burp/help/mobile_testing_using_mobile_assistant.html) - 証明書ピンニングをバイパスし、アプリにインジェクトできるツールです。

#### ルート検出および SSL ピンニングのバイパス

- [SSL Kill Switch 2](https://github.com/nabla-c0d3/ssl-kill-switch2) - iOS および OS X アプリ内の SSL 証明書検証 (証明書ピンニングを含む) を無効にするブラックボックスツールです。
- [iOS TrustMe](https://github.com/intrepidusgroup/trustme) - iOS デバイスの証明書の信頼チェックを無効にします。
- [Xcon](http://apt.modmyi.com) - 脱獄検出をバイパスするためのツールです。
- [tsProtector](http://cydia.saurik.com/package/kr.typostudio.tsprotector8) - 脱獄検出をバイパスするためのもうひとつのツールです。

### ネットワーク傍受および監視用ツール

- [Tcpdump](http://www.androidtcpdump.com) - コマンドラインパケットキャプチャユーティリティです。
- [Wireshark](https://www.wireshark.org/download.html) - オープンソースのパケットアナライザです。
- [Canape](http://www.contextis.com/services/research/canape/) - 任意のプロトコル用のネットワークテストツールです。
- [Mallory](https://intrepidusgroup.com/insight/mallory/) - モバイルデバイスやアプリケーションのトラフィックを監視および操作するために使用する中間者攻撃 (MiTM) ツールです。

### 傍受プロキシ

- [Burp Suite](https://portswigger.net/burp/download.html) - Burp Suite はアプリケーションのセキュリティテストを実行するための統合プラットフォームです。
- [OWASP ZAP](https://github.com/zaproxy/zaproxy) - OWASP Zed Attack Proxy (ZAP) はウェブアプリケーションやウェブサービスのセキュリティ脆弱性を自動的に発見するのに役立つフリーのセキュリティツールです。
- [Fiddler](http://www.telerik.com/fiddler) - Fiddler は HTTP および HTTPS トラフィックをキャプチャしてユーザーが確認するためにログに記録できる HTTP デバッグプロキシサーバーアプリケーションです。また、Fiddler は送受信時のトラブルシューティングを行うために HTTP トラフィックの改変にも使用できます。
- [Charles Proxy](http://www.charlesproxy.com) - 開発者がマシンとインターネットの間のすべての HTTP および SSL / HTTPS トラフィックを表示することができる HTTP プロキシ / HTTP モニタ / リバースプロキシです。

### IDE

- [Android Studio](https://developer.android.com/studio/index.html) - Android Studio は Google の Android オペレーティングシステム向けの公式の統合開発環境 (IDE) です。JetBrains の IntelliJ IDEA で構築されており、特に Android 開発向けに設計されています。
- [IntelliJ](https://www.jetbrains.com/idea/download/) - IntelliJ IDEA はコンピュータソフトウェアを開発するための Java 統合開発環境 (IDE) です。
- [Eclipse](https://eclipse.org/) - Eclipse はコンピュータプログラミングに使用される統合開発環境 (IDE) であり、最も広く使用されている Java IDE です。
