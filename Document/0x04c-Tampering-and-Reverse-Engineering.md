## 改竄とリバースエンジニアリング

リバースエンジニアリングや改竄の技法は長い間、クラッカー、MOD作成者、マルウェア解析者などの分野に属していました。「伝統的な」セキュリティテスト技術者や研究者にとって、リバースエンジニアリングはどちらかというと補完的なスキルでした。しかし状況は一変します。モバイルアプリのブラックボックステストではコンパイルされたアプリを逆アセンブルし、パッチを適用し、バイナリコードやライブプロセスを改竄することがますます必要になっています。多くのモバイルアプリが歓迎されない改竄に対する防御を実装しているという事実はセキュリティテスト技術者にとって物事を簡単にしてはくれません。

モバイルアプリのリバースエンジニアリングはコンパイルされたアプリを解析するプロセスであり、そのソースコードに関する情報を抽出します。リバースエンジニアリングの目的はコードを *理解すること* です。

*改竄* はモバイルアプリ (コンパイルされたアプリまたは実行中のプロセス) またはその動作に影響を与える環境を変更するプロセスです。例えば、アプリはルート化されたテストデバイス上で実行することを拒む可能性があり、一部のテストを実行できなくなる可能性があります。そのような場合には、アプリの動作を変更したいでしょう。

モバイルセキュリティテスト技術者を上手く務めるには基本的なリバースエンジニアリングの概念を理解することが必要です。モバイルデバイスやオペレーティングシステムも十分に知る必要があります。プロセッサアーキテクチャ、実行形式、プログラミング言語の複雑さなどがあります。

リバースエンジニアリングは芸術であり、そのすべてのファセットを記述することはライブラリ全体を占めるでしょう。技術と専門化の幅広い領域は驚異的です。マルウェア解析の自動化や新しい逆難読化手法の開発など、非常に特殊で独立した部分問題の取り組みに何年も費やすことがあります。セキュリティテスト技術者はジェネラリストです。有能なリバースエンジニアであるためには、膨大な量の関連情報をフィルタする必要があります。

常に機能する一般的なリバースエンジニアリングプロセスはありません。それでは、よく使われる手法やツールについてこのガイドで後ほど説明し、最も一般的な防御に取り組む例を挙げましょう。

### あなたがそれを必要とする理由

モバイルセキュリティテストにはいくつかの理由から少なくとも基本的なリバースエンジニアリングのスキルが必要です。

**1. モバイルアプリのブラックボックステストを可能にするため。** 現在のアプリには動的解析を妨げるコントロールを含むことがよくあります。SSL ピンニングとエンドツーエンド (E2E) 暗号化によりプロキシを使用したトラフィックの傍受や操作が妨げられることがあります。ルート検出はアプリがルート化されたデバイス上で実行できなくなり、高度なテストツールを使用できなくなる可能性があります。これらの防御を無効にする必要があります。

**2. ブラックボックスセキュリティテストの静的解析を強化するため。** ブラックボックステストでは、アプリバイトコードやバイナリコードの静的解析はアプリの内部ロジックを理解するのに役立ちます。また、ハードコードされた資格情報などの欠陥を識別することもできます。

**3. リバースエンジニアリングに対する耐性を評価するため。** モバイルアプリケーション検証標準のアンチリバースコントロール (MASVS-R) にリストされているソフトウェア保護対策を実装するアプリはある程度のリバースエンジニアリングに対して耐性を持つ必要があります。それぞれのコントロールの有効性を検証するには、テスト担当者は一般的なセキュリティテストの一部として *耐性評価* を実行します。耐性評価では、テスト担当者はリバースエンジニアの役割を引き受け、防御のバイパスを試みます。

モバイルアプリのリバーシングの世界に飛び込む前に、良いニュースと悪いニュースを共有します。良いニュースから始めましょう。

**最終的に、リバースエンジニアは常に勝利します。**

これはモバイルの世界では特に真実です。リバースエンジニアは本質的な利点を持っています。モバイルアプリをデプロイおよびサンドボックス化する方法は従来のデスクトップアプリのデプロイメントやサンドボックス化よりも設計上の制約があります。そのため Windows ソフトウェアでよく見られるルートキットのような防御メカニズムを含めること (DRM システムなど) は簡単には実行できません。Android のオープン性によりリバースエンジニアはオペレーティングシステムに有利な変更を加え、リバースエンジニアリングプロセスを支援することを可能にします。iOS ではリバースエンジニアはほとんどコントロールできませんが、防御の選択肢もまた制限されています。

悪いニュースとしては、マルチスレッドでのアンチデバッグコントロール、暗号化ホワイトボックス、隠れた耐タンパ性機能、非常に複雑なコントロールフロー変換を扱うことは容易ではないということです。最も効果的なソフトウェア保護スキームは独自のものであり、標準の微調整やトリックで太刀打ちできないでしょう。それらを打ち破るには、面倒な手動解析、コーディング、フラストレーション、そして - あなたの性格によっては - 眠れない夜と緊張状態の関係が要求されます。

初心者にとってリバーシングの膨大な範囲に圧倒されてしまうことはよくあります。始める際の最善の方法はいくつかの基本的なツール (Android および iOS のリバーシングの章の関連するセクションを参照) をセットアップし、簡単なリバーシングタスクや crackme を開始することです。アセンブラやバイトコード言語、オペレーティングシステム、難読化などに遭遇し学ぶ必要があるでしょう。簡単なタスクから始めて、より難しいものへ徐々にレベルアップしていきます。

以下のセクションでは、モバイルアプリのセキュリティテストで最もよく使用される技法の概要を説明します。以降の章では、Android と iOS の両方について OS 固有の詳細を掘り下げていきます。

### 基本的な改竄技法

#### バイナリパッチ適用

*パッチ適用* とはコンパイルされたアプリを変更するプロセスです。バイナリ実行形式のコード変更、Java バイトコードの改変、リソースの改竄などがあります。このプロセスはモバイルゲームのハッキングシーンで *MOD適用* として知られています。パッチは多くの方法で適用できます。16進エディタでのバイナリファイルの編集やアプリの逆コンパイル、編集、逆アセンブルなどがあります。有用なパッチの詳細な例について以降の章で説明します。

心に留めておくものとして、現在のモバイルオペレーティングシステムはコード署名を厳しく強制することがあります。そのため、改変されたアプリを実行することはデスクトップ環境で使用するほど簡単ではありません。セキュリティ専門家は90年代にははるかに簡単な人生を送っていました。幸運なことに、あなた自身のデバイスで作業する場合、パッチ適用はそれほど難しいことではありません。つまり、改変したコードを実行するには、アプリを再署名するか、デフォルトのコード署名検証機能を無効にする必要があるというだけです。

#### コードインジェクション

コードインジェクションは非常に強力な技法であり、実行時にプロセスを探索および改変できます。インジェクションはさまざまな方法で実装されますが、自由に利用でき十分に文書化されたプロセスを自動化するツールのおかげで、すべての詳細を知らなくても使用できます。これらのツールは、アプリによりインスタンス化されたライブオブジェクトなどの、プロセスメモリや重要な構造体に直接アクセスできます。また、ロードされたライブラリの解決、メソッドやネイティブ関数のフックなどに役立つ多くのユーティリティ関数があります。プロセスメモリの改竄はファイルにパッチを適用するよりも検出が難しく、大半の場合に推奨される方法です。

Substrate, Frida, XPosed はモバイル業界で最も広く使用されているフックとコードインジェクションのフレームワークです。三つのフレームワークは設計の哲学と実装の詳細が異なります。Substrate と Xposed はコードインジェクションやフックに焦点を当てています。一方、Frida は本格的な「動的計装フレームワーク」とすることを目指しており、コードインジェクション、言語バインディング、インジェクト可能な JavaScript VM およびコンソールを組み込んでいます。

それだけでなく、Cycript をインジェクトするために Substrate を使用してアプリを計装することもできます。Cycript は Cydia で有名な Saurik が作成したプログラミング環境 (通称 "Cycript-to-JavaScript" コンパイラ) です。さらに物事は複雑になりますが、Frida の作者も ["frida-cycript"](https://github.com/nowsecure/frida-cycript "Cycript fork powered by Frida") と呼ばれる Cycript のフォークを作成しました。これは Cycript のランタイムを Mjølner と呼ばれる Frida ベースのランタイムに置き換えます。これにより frida-core で保守されているすべてのプラットフォームとアーキテクチャで Cycript を実行できます (この時点で混乱しても、心配ありません) 。frida-cycript のリリースには Frida の開発者 Ole によるブログ記事 "Cycript on Steroids," が付いていました。このタイトルは [Saurik はあまり好きではありませんでした](https://www.reddit.com/r/ReverseEngineering/comments/50uweq/cycript_on_steroids_pumping_up_portability_and/ "Cycript on steroids: Pumping up portability and performance with Frida") 。

三つすべてのフレームワークについて例を紹介します。私たちは Frida で始めることをお勧めします。これは三つの中で最も汎用性が高いからです (このため、Frida の詳細と事例が多く紹介されています) 。特に、Frida は Android と iOS の両方のプロセスに JavaScript VM をインジェクトできます。一方で Substrate での Cycript インジェクションは iOS でのみ動作します。しかし最終的には、いずれのフレームワークでも多くの同じ目標に到達できます。

### 静的および動的バイナリ解析

リバースエンジニアリングはコンパイルされたプログラムのソースコードの意味を再構築するプロセスです。言い換えると、何をしているのか、どのようにしているのかを理解するために、プログラムを分割し、実行し、その一部をシミュレートし、他では言い表せないものにします。

#### 逆アセンブラと逆コンパイラの使用

逆アセンブラと逆コンパイラはアプリのバイナリコードやバイトコードを多かれ少なかれ理解できる形式に逆変換できます。ネイティブバイナリにこれらのツールを使用することで、アプリがコンパイルされたアーキテクチャに一致するアセンブラコードを取得できます。Android Java アプリは Smali に逆アセンブルできます。これは Android の Java VM である dalvik で使用される dex 形式のアセンブラ言語です。Smali アセンブリは逆コンパイルして Java コードに戻すことも簡単です。

高価ですが便利な GUI ツール、オープンソースの逆アセンブラエンジン、リバースエンジニアリングフレームワークなど、幅広いツールやフレームワークが利用可能です。これらのツールについての高度な使用方法はたいていそれ自体で簡単に一冊の本になってしまいます。開始する最適な方法はあなたのニーズと予算にあったツールを選び、十分にレビューされたユーザーガイドを購入するだけです。OS ごとの「リバースエンジニアリングと改竄」の章で最も一般的なツールをいくつか紹介します。

#### デバッグとトレース

従来の意味では、デバッグはソフトウェアライフサイクルの一部としてプログラム内の問題を特定および分離するプロセスです。デバッグに使用される同じツールは、バグを特定することが主な目的ではありませんがリバースエンジニアリングにとって価値があります。デバッガは実行時に任意の箇所でプログラムを一時停止したり、プロセスの内部状態を検査したり、レジスタやメモリの改変さえも可能です。これらの能力はプログラムの検査を容易にします。

*デバッグ* は一般的には対話的デバッグセッションを意味し、デバッガは実行中のプロセスにアタッチされます。対照的に、*トレース* は (API コールなどの) アプリの実行に関する情報の受動的なログ出力を指します。トレースは、デバッグ API、関数フック、カーネルトレース機能などのいくつかの方法で実行できます。改めて、これらの技法の多くは OS ごとの「リバースエンジニアリングと改竄」の章で説明します。

### 高度な技法

強力に難読化されたバイナリを逆難読化するなど、より複雑なタスクの場合、解析の特定の部分を自動化することなく成功することはありません。例えば、逆アセンブラでの手動解析を基にして複雑なコントロールフローグラフを理解および単純化するには何年もかかるでしょう (そして、完了する前におそらく気が狂うことでしょう) 。代わりに、カスタムメイドのツールでワークフローを増強します。幸いにも、現代の逆アセンブラにはスクリプティングと拡張 API が付属しており、一般的な逆アセンブラには多くの便利な拡張が用意されています。さらに、オープンソースの逆アセンブラエンジンやバイナリ解析フレームワークも存在します。

ハッキングの常として、何でもありのルールが適用されます。単純に最も効率的なものを使用します。すべてのバイナリは異なり、すべてのリバースエンジニアは独自のスタイルを持っています。多くの場合、目標に到達する最善の方法は (エミュレータベースのトレースやシンボリック実行など) アプローチを組み合わせることです。始めるには、優れた逆アセンブラやリバースエンジニアリングフレームワークを選択し、それらの特定の機能や格拡張 API に慣れることです。最終的には、より良くなる最善の方法は実践的な経験を積むことです。

#### 動的バイナリ計装

ネイティブバイナリに対するもう一つの便利なアプローチには動的バイナリ計装 (DBI) があります。Valgrind や PIN などの計装フレームワークは単一プロセスの細かい命令レベルのトレースをサポートします。これは動的に生成されたコードを実行時に挿入することにより実現されます。Valgrind は Android でうまくコンパイルされ、事前にビルドされたバイナリをダウンロードして利用できます。

[Valgrind README](http://valgrind.org/docs/manual/dist.readme-android.html "Valgrind README") には Android 向けのコンパイル手順が記述されています。

#### エミュレーションベースの動的解析

エミュレータでアプリを実行することにより、その環境を監視および操作するための強力な方法が得られます。一部のリバースエンジニアリングタスク、特に低レベルの命令トレースが必要な場合、エミュレーションは最善の (または唯一の) 選択肢です。残念ながら、このタイプの解析は Android の場合にのみ実行可能です。iOS にはエミュレータが存在しないためです (iOS シミュレータはエミュレータではなく、iOS デバイス向けにコンパイルされたアプリは実行できません) 。「Android の改竄とリバースエンジニアリング」の章で Android 向けの一般的なエミュレーションベースの解析フレームワークの概要を説明します。

#### リバースエンジニアリングフレームワークを使用したカスタムツール

ほとんどのプロフェッショナルな GUI ベースの逆アセンブラはスクリプト機能と拡張性を備えていますが、特定の問題を解決するにはあまり適していないことがあります。リバースエンジニアリングフレームワークは重量のある GUI に依存することなくある種のリバースエンジニアリングタスクを実行および自動化できます。特に、ほとんどのリバーシングフレームワークはオープンソースであるか、フリーで利用可能です。モバイルアーキテクチャをサポートする一般的なフレームワークには [Radare2](https://github.com/radare/radare2 "radare2") と [Angr](http://angr.io) があります。

##### 例：シンボリック実行やコンコリック実行を使用したプログラム解析

2000年代後半には、シンボリック実行ベースのテストがセキュリティ脆弱性を特定する手段として普及しました。シンボリック「実行」とは実際にプログラムを通る可能性のあるパスを一次論理の式として表現するプロセスを指します。充足可能性モジュロ理論 (SMT) ソルバを使用してこれらの式の充足可能性をチェックし、解決された式に対するパス上の特定の実行点に到達するために必要な変数の具体的な値などのソリューションを提供します。

典型的には、シンボリック実行は動的実行などの他の技法と組み合わされ、従来のシンボリック実行に特有のパス爆発の問題を軽減します。このコンクリート (実際の) 実行とシンボリック実行の組み合わせは *コンコリック実行* (コンコリックという名前は *conc* rete と symb *olic* に由来します) と呼ばれます。改善された SMT ソルバと現在のハードウェアスピードを併せて、コンコリック実行は中規模のソフトウェアモジュール (すなわち、10 KLOC オーダー) のパスを探索することが可能です。それだけでなく、コントロールフローグラフの簡素化など、逆難読化タスクをサポートする場合にも便利です。例えば、Jonathan Salwan と Romain Thomas は [動的シンボリック実行を使用して VM ベースのソフトウェア保護をリバースエンジニアリングする方法を示しました](https://triton.quarkslab.com/files/csaw2016-sos-rthomas-jsalwan.pdf "Jonathan Salwan and Romain Thomas: How Triton can help to reverse virtual machine based software protections") (すなわち、実際の実行トレース、シミュレーション、シンボリック実行を組み合わせて使用します) 。

Android のセクションでは、シンボリック実行を使用して Android アプリケーションの簡単なライセンスチェックをクラックするためのウォークスルーを説明します。
