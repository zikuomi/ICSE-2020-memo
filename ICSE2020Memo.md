# ICSE 2020 memo

# 6/23 (15 absts)

## ~I1 Metastudies~

## <font color="orange"> I2 Security </font> (4 TPs, 2 NIERs)

- Title: <font color="orange">Targeted Greybox Fuzzing with Static Lookahead Analysis</font> (paper)  
>  
背景：自動テストは新規パス探索を目的としている → **静的解析によるバグっぽい場所への誘導が大事**  
提案手法：ターゲット（プログラムの変更箇所など）へ導くためのGrey-box Fuzzer with オンラインの静的解析  
静的解析：各プログラムパスの意味的 (semantically) 分析 → Fuzzの頻度の決定に利用  
実装：Ethereumの拡張 → コード計装がないのでsmart contractの分野でも適用可能（計装するとセマンティクスが変わることがある）  
実験：27のreal-world ベンチマークで評価 → 標準のGreybox Fuzzerを大幅に上回り、困難なターゲットへ83％到達可能、最大14倍の高速化も達成

- Title: HyDiff: Hybrid Differential Software Analysis (paper)  
>  
背景：**多くの評価（回帰バグの検出やDNNのロバスト性の評価）はソフトウェアの差分解析と見做せる** → 出力や振る舞いが異なるなら実行は分岐している（差分が存在する）  
提案手法：HyDiff：GreyboxFuzzとShadowSymbolicExecution → CFGを用いたメトリクスで記号実行を効率的にガイド  
実装：AFLとSymbolic Path Finder上に実装  
実験：Javaバイトコードプログラムの回帰分析などへのHyDiffの適用例を提示

ちょっちよくわからん

- Title: Towards Characterizing Adversarial Defects of Deep Learning Software from the Lens of Uncertainty (paper)
>  
背景：Deep Learning (DL) は企業のパフォーマンス向上に寄与した。が、**安全性の観点ではまだ懸念あり**。DLに誤った判断をさせるAE（Adversarial Examples）が存在する。DL内部の不確実性とAEの関係性の研究はまだ未熟。  
調査項目：BE（Benign Examples）とAEを区別するメトリクスを調査、又、その結果から珍しいパターンのAE/BEを生成する自動テスト手法の提案  
実験：自動生成されたパターンで最新の防御技術の突破に成功

DL関連の自動化論文、いわゆるDLを用いたセキュリティの突破（顔認証とか？）を試みているっぽい

- Title: One Size Does Not Fit All: A Grounded Theory and Online Survey Study of Developer Preferences for Security Warning Types (paper)
>  
背景：ソフトウェア開発においてセキュリティ上の警告って大事だよね。色々支援ツールは存在するけど、開発者が警告に対してどう対話するかの調査はなかった。  
実験：14人の開発者、12人のCS学生、7人の研究者を対象に定性的な調査を実施（裏付けのために50人の開発者への定量調査も実施） → **誰からも優先される警告タイプは存在しない、コンテキストは重視される傾向にある** → IDEなどの対話が柔軟になればより有益となる可能性

調査メインっぽいが、開発者と開発時の警告文との在り方の研究

## I4 Clones and Changes

## I5 Deep Learning Testing and Debugging

## I6 Empirical Studies and Requirements

## ~A1 Autonomous Driving Systems~

## <font color="orange"> A2 Testing and Debugging 1 </font> (2 TPs, 2 Journals, 2 NIERs)

- Title:Studying the Use of Java Logging Utilities in the Wild (paper)
>  
背景：ログは大事。**一般にロギングではprintデバッグではなくSLF4Jなどのユーティリティが好まれる。** LUの研究は少ないが、新しいLUプロダクトなどは続々と出ている。  
下調べ：Githubの大規模プロジェクトの調査 → 大規模プロジェクトのうち、75.8%は独自のLUを実装している、さらにそのうち50%以上が3つ以上のLUを使用。  
調査：LUの理解のための定性的調査 → ログは色々な理由で使われている

ロギングの調査論文、ある種Surveyとして利用できるかも？

- Title: Causal Testing: Understanding Defects' Root Causes (paper)
>  
背景：バグ修復のためにはバグの根本的な原因を理解することが重要である。  
提案手法：バグと関連する一連の実行を特定する手法 Causal Testing  
実装：  
実験：Defect4Jベンチマークで検証 → 71%で適用可能、そのうち77%で開発者に有用な情報を提供（JUnitなどでは得られない情報も存在）。 欠陥特定までの時間が標準ツールと比較して80 → 86%までと改善（？）

具体的な内容はAbstだけでは読み取れず、JUnitとの比較からJavaの模様

- Title: Studying the Characteristics of Logging Practices in Mobile Apps: A Case Study on F-Droid. (journal)
>  
背景：モバイルアプリのロギングはなぜかあまり知られていない。  
調査：モバイルアプリのロギングの実践について調査。 F-Droidリポジトリ上の1444のOSS Androidのケーススタディ。 → サーバやデスクトップアプリほどログは普及していないが、ほぼすべてのアプリでログが活用されていた（ただし、サーバとはロギングの毛色が異なる）  
調査：メールインタビューなどから見えてきたこと → ロギングレベルと理論的根拠が頻繁に（35.4%）一致していない。 → ロギングの有用性を阻害している  
実験：上記の不一致のうち、不要なロギングを減らすことでどれだけパフォーマンスが向上するか実験 → 大幅に向上 → **モバイルロギングの体系化が必要**

- Title: A Survey on Adaptive Random Testing (journal)
>  
背景： RT (Random Test)は十分研究されており、多くのアプリのテストに利用されている。 **ART (Adaptive Random Test)はRTの障害検出強化版で、テストケースを入力ドメインごとに均一になるように分散させる**。  
調査： ARTや技術の分類、アプリケーション領域の要約、各評価を実施。 いくつかの誤解と課題にも触れる。  

アプリケーションへの適用という意味では幅広い分野。ただし根本のアイデアは深く研究されているので、新規アイデアを出すのは難しそうな印象。

## <font color="orange"> A3 Code Summarization </font> (4 TRs, 2 NIERs)

- Title: Posit: Simultaneously Tagging Natural and Programming Languages (paper)
>  
背景：開発におけるメーリングリストでは、自然言語とコードを混合して会話することが多い。 そこに **タグ付けをすることは、トレーサビリティ、コードスニペットの再利用の問題解決には重要な要素である**。  
提案手法：自然言語処理のコードスイッチの手法を混合テキストに適用して（プログラム？）言語識別とトークンタグ付けを試みるPosit。ソースコードトークンのASTタグと自然言語の単語の品詞タグを提供 → 混合テキストの言語を予測  
実装：CLANGからAST、Standard Stanfordから品詞を得て、biLSTMでトレーニング  
実験：言語識別を10.6%改善、タグ付けを23.7%正確にする

着目点こそ混合テキストだが、実施内容は自然言語×機械学習という一般的なものに見える（Abstだけ見ると）

- Title: CPC: Automatically Classifying and Propagating Natural Language Comments via Program Analysis (paper)
>  
背景： 現代のソフトウェアシステムには膨大なコードコメントが記述されている。 既存の研究ではプログラム解析を用いたコメントの記述を行っていない（**解析によってコメントの導出、改良、伝播が可能** になる、らしい）。  
提案手法：コメントを効率的に利用するためには、コメントの分類法と自動分類を可能にする分類子が必要 → 包括的な分類法を提案  
実装：プロトタイプCPCを開発  
実験：5つのプロジェクトで評価 → 41573個の新規コメントの88%が他コードからの伝播により導出可能。 バグ検出にも役立ち、すでに修正済み。 伝播結果と実際のコメントの不一致を調査すると、コメントの欠陥も発見

コメントの伝播というのは面白い。ワシはあまりコメントあまり書かないけど（あとで後悔する）

- Title: Suggesting Natural Method Names to Check Name Consistencies (paper)
>  
背景： **APIの名前が機能と一致していないと誤用につながるよね** 。  
提案手法：実装と関数名が一致しているか（一貫性）を検証するMNIREを提案。 機械学習を利用、実装から導出された名前が実際の関数名と近いかを検査。 ユニークアイデア： body, interface, class name の三つのコンテキストから名前を生成
実装：  
実験：14M超えのメソッドデータセットで学習。 MNIREはメソッド名再現率について最先端アプローチ (code2vec) と比較して recall: 10.4 %, precision: 11% 向上。 名前の提案を現行プロジェクトに31/42で改善が認められた

- Title: Retrieval-based Neural Source Code Summarization (paper)
>  
背景： ソースコード要約自動生成の既存技術は「ソースコードからの用語の取得」もしくは「類似コードスニペットから要約を検索」。 最近ではEnc/Decモデルを用いて要約を生成（課題として、低頻度の単語では精度が落ちる）  
提案手法：Enc/Decの拡張、類似スニペットのみを用いてトレーニング  
実装：  
実験：色々やって、最先端アプローチと比較して改善できることを確認

何やら色々やっている模様。サマリ生成はジャンルとして興味深いが自然言語処理とか機械学習の毛色が濃いのでとっつきにくい（もちろん機械学習だけではないんだけど）

## ~A4 Cyber Physical Systems~

## <font color="orange"> A5 Testing and Debugging 2 </font> (3 TPs, 2 Demos, 2 NIERs)

- Title: Efficient Generation of Error-Inducing Floating-Point Inputs via Symbolic Execution (paper)
>  
背景： 浮動小数点の精度を評価する場合、**丸め誤差の数値エラーを最大化する入力生成が重要** 。  
提案手法：浮動小数点エラーをより頻繁に起こす入力生成をコードカバレッジ問題へと定式化。 不正確なチェックをプログラム内に挿入し、そこをカバーするように特殊ブランチを構築（記号実行で誘導）  
実装：ツールFPGen  
実験：行列計算、統計ライブラリを含む21のプログラムで評価 → 20のエラーと平均して2桁以上多いエラーを実行可能（最先端アプローチと比較して）

浮動小数点ということはバイナリ寄りか？

- Title: A Study on the Lifecycle of Flaky Tests (paper)
>  
背景： **Flaky Tests: コード変更なしで失敗/成功する可能性のある不安定なテスト** (例えば何だ？Time関数みたいな非決定的なAPIとか？)、最近産業、Academic両方から関心 → Flaky Testsの再現性・原因などの研究は多くない  
調査： AnonCompanyのプロジェクトから Flaky Testsのライフサイクルを調査。 又、Flaky Testsの失敗再現の調査、および修正までの時間と効果を調査

調査論文。ただの非決定的テストをFlakyと言い換えているだけなのかが気になるところ

- Title: Ankou: Guiding Grey-box Fuzzing towards Combinatorial Difference (paper)
>  
背景： Greybox Fuzzing ではフィットネス関数（カバレッジ最大化など）を用いてテストケースプールを維持・進化させる。 **カバレッジが同じだと異なるプログラム実行を区別できない**。 既存のフィットネス関数はデータの和（Union）を重視し積（Combination）を考慮しない（？） → ローカル最適化で行き詰まることがままある  
提案手法： 実行情報の積を認識するGreybox Fuzzer Ankou  
実装：  
実験： バグ検索において、AFL/Angoraと比較して ×1.94/×8.0 効果的。 スケーラビリティの課題はあり

確かに（定義にも依るが）Greybox Fuzzingでは実行同士を区別できない、でも実行のCombinationって何だろ（よもや実行順ではあるまい）

# 6/24 (25 absts, total 40 absts)

## ~P7 Human Aspects~ 読んじゃった (3 TRs, 2 Journals, 1 NIER)

- Title: What Predicts Software Developers' Productivity? (journal)
>  
背景： 組織が取れるソフトウェア開発者へのサポートは様々（オフィスレイアウトからソースコードクリーンアップまで）  
調査： どのオプションが生産性に影響を与えるか 3社 622人 の開発者へ調査 → **モチベーションなど非技術的な要因に大きな相関**

ある意味ソ新Gに対するアンチテーゼ

- Title: A Study on the Prevalence of Human Values in Software Engineering Publications, 2015 – 2018 (paper)
>  
背景： ソフトウェア評価において、人間の価値基準（平等かどうかなど）を損なうとユーザの不満などにつながる。   
調査： 近年のカンファレンスやジャーナルがこうした人間の価値について論じているかを調査。 様々な価値から出版物を分類、結果として a) 価値を直接検討しているのはごく一部 b) 価値の大部分について、議論している出版物はほとんどもしくは皆無 c) SEジャーナルよりはSEカンファレンスのほうが論じているpaperは多かった  

メタ会議、メタジャーナル。human valuesってなんじゃろと気にはなる

- Title: Explaining Pair Programming Session Dynamics from Knowledge Gaps (paper)
>  
背景： ペアプログラミング (PP) の有用性を論じる研究は多いのに、有用か否かの結論は未だ出ていない  
提案手法： Grounded Theory Methodology を用いて多くの産業用 PP を解析  
実装：  
実験： 必要知識の異なる（一般的なプログラミングと固有プロジェクト）場合に PP セッションでの挙動がどうなるかの比較 → プロジェクト固有の方が、知識ギャップの差が PP のより大きな妨げになる → **固有システムの理解が PP において大事**

- Title: Engineering Gender-Inclusivity into Software: Ten Teams' Tales from the Trenches (paper)
>  
背景： **Gender-Inclusivity** がSEの研究者の間で注目されている。 が、提唱されているだけで実践されてはいない。  
実験： 5か月 ～ 3年半の範囲で50人以上のチーム10個にGenderMagベースのプロセスを収集 → 9つの実践と2つの課題を提示

Gender-Inclusivityってなんじゃらほい

- Title: How does Machine Learning Change Software Development Practices? (journal)
>  
背景： **学習機能をシステムに追加すると不確実性が内包される**。 学習機能の追加の効果を調査  
調査： 26か国で14人へインタビュー、342人から調査回答 → 学習機能の有無で、要件、設計、テスト、スキルの多様性、問題解決などで明らかに差が出た

## <font color="orange"> P9 Bugs and Repair </font> (1 TR, 2 Journals, 2 SEIPs, 1 NIER)

- Title: PRECFIX: Large-Scale Patch Recommendation by Mining Defect-Patch Pairs (practice)
>  
背景： パッチのリコメンドはエラーの適切に修正を提案するプロセス。 デバッグと修正の時間削減に効果 → 生産性の向上。 既存研究はテストスイートとデバッグレポートに依存 → 現実世界では欠けることもしばしば  
提案手法： PRECFIX 過去のデバッグアクションから適切なリコメンドする手法。 クラスタリングによって一般的な再利用可能なパッチパターンを抽出。  
実装：  
実験： 様々な欠陥パターンの存在する 10K個のプロジェクトで実験 → 3000のテンプレートを抽出、また高速、FPは22%程度。 **アリババにPRECFIXをロールアウト**

- Title: On the Efficiency of Test Suite based Program Repair: A Systematic Assessment of 16 Automated Repair Systems for Java Programs (paper)
>  
背景： テストベースの自動プログラム修正は色々研究有り（テストスイートの近似として応用）。**パッチの正確性に対する研究はあるが、生成の効率性に対する研究は少ない**。   
調査： 既存のテストスイートベースのプログラム修正技術の効率性に関して調査  → 生成されたパッチの個数に着目 → 探索空間の広さ・パッチ生成までのテスト作業の少なさ・適切なパッチを生成できるか を評価項目
実験： Java用の16個のOSS修復ツールから生成されたパッチ候補を量的観点で実証 → 現在のテンプレートベース（最も効果的とされている）の修正システムでは無関係なパッチ候補を生成する傾向があるので最低効率となった

パッチの効率についての研究。実をいうと未だにパッチの仕組みが分かっていない。

- Title: SEQUENCER: Sequence-to-Sequence Learning for End-to-End Program Repair (journal)
>  
提案手法： SEQUENCER: S2S学習に基づいた新規E2Eアプローチ。 機械学習を用いたコード上のバグ修正、**大きなコードで発生していた無制限の語彙問題を克服**    
実験： 35578のサンプルでトレーニング、4711の実際のバグ修正と、Defects4JベンチマークでSEQUENCERを評価 → 950/4711で固定ライン（fixed line）(?) を予測、Defects4Jベンチで14のバグパッチを発見

unlimited vocabulary problem って何だろ

- Title: A Study of Bug Resolution Characteristics in Popular Programming Languages (journal)
>  
背景： プログラミング言語ごとのバグ解決特性をGithubのプロジェクトから調査（相関関係なので注意）。   
調査： 主な言語10の600個のプロジェクト、3Mのコミットからバグ解決データを抽出 → 解決時間とパッチサイズが言語間で大きな差が出た。 Rubyは時間がかかるなど。 静的型付け言語ではパッチが横断的になるが、解決までは短い

言語ごとの調査は根が深そう（言語にもバージョンがあるし）。 静的型付け言語では修正時間が短いというのはどこかで引用できそうな予感

- Title: Automated Bug Reproduction from User Reviews for Android Applications (practice)
>  
背景： **ユーザーレビューのバグ報告は再現がムズイ** → 自動化できると嬉しい → が、レビューは難解で情報不足
提案手法： RevRep ユーザレビューからAndroidアプリのバグを再現するツール。 自然言語処理を利用してバグ再現に役立つ情報を抽出  
実験： Google Play 上の63件のクラッシュ関連のレビューを含むベンチマークを生成 → 人間と同程度のパフォーマンスを実現、70%のレビューを正常に再現

ユーザレビューから再現するのが難しいのは認めるが、70%はにわかに信じがたい。ベンチマークにカラクリがありそう

## P10 Stack Overflow

## ~P11 Natural Language Artifacts~

## <font color="orange"> P12 Testing and Debugging </font> (4 Journals, 2 SEIPs)

- Title: Debugging Crashes using Continuous Contrast Set Mining (practice)
>  
背景： Facebook では毎日数百万のレビューが送られ、エンジニアはその中からバグとかリグレッションをチェックしている → 多くは手作業、かつ時間は有限、専門知識と精密なコード検査が必要 （ツライ）  
提案手法： **Contrast Set Mining** → 判別パターンマイニングの一種 → 連続データの離散化がメイン → 提案：CSMの連続データへの直接適用（No 離散化）  
実装：  
実験：挑戦的なデータセット、ユーザナビゲーションのイベントシーケンスへの適用

レビューと連続データって関連付けされるのか？元から離散的なイメージ

- Title: Automatic Abnormal Log Detection by Analyzing Log History for Providing Debugging Insight (practice)
>  
背景： ソフトウェアサイズが大きく複雑化すると欠陥の発見はより困難になる → **ログ分析がデバッグの洞察 (Insight) を得る唯一の方法** → 手動だとエラく労働力が必要  
提案手法： historian ログ分析システム → 統計的テキストマイニングでログ内の重要度とノイズ度を用いて強調表示  
実装：  
実験： Tizen Native API テストログに適用 → 平均 4%のログのみ強調 → 実際にTizen開発者へ報告、いいんんじゃない、という評価

強調表示のし損ね（FN）とか冗長（FP）とかがどれだけあるのか気になるところ。対象はTizenのみ？

- Title: Explaining Regressions via Alignment Slicing and Mending (journal)
>  
背景： Regression Faults が発生した場合の一般的な作業 1) 障害発生個所の分離 2) 障害発生へのフローの理解 → 障害箇所の特定の既存研究 Dynamic Slicing, Delta Debugging, Symbolic Analysis → 精度やスケーラビリティの問題が依然残る  
提案手法： トレースベース （Defectがどのように障害へ伝播するのか調査） → **Causality Graph** の構築。 課題１ ２つのプログラムバージョンの各トレースのアライメント（どのように重要な差分を表現するか）。 課題２ **Alignment Slice**, Mending から Causality Graph を構築、障害の説明。  
実装：  
実験： Dynamic Slicing, Delta Debugging, 記号実行との比較、検体は２４個のリアル検体 → 障害分離は正確、障害の説明は十分許容コストで理解可能、実行オーバーヘッドを十分抑えられる

障害箇所の特定という意味では徐さんのテーマとの関連性が深そう。検体的にJavaを対象としている？ 実験は豊富、強い

- Title: Historical Spectrum based Fault Localization (journal)
>  
背景： **Spectrum Based Fault Localization (SBFL)** は障害特定に効果的と評価、業界でも自動化されたSBFLが評価 → が、制限あり。 スペクトル構築のためのテストカバレッジ情報は障害原因を表現していない、また、SBFLはバグの有無を十分区別できない  
提案手法： バージョン履歴情報を障害特定に利用する手法 HSFL (Historical Spectrum-based Fault Localization)。 バグを引き起こすコミットを特定 → Historical Spectrum を構築（これを用いて怪しいコード要素をランク付け）  
実装：  
実験： Defects4J ベンチマークを用いて 最先端SBFL技術と比較して最大 +77.8% 多くのバグを発見（上位5つで+40.8%）。  

Defects4J結構頻繁に出てくるなぁ（有名なんだろうか）

- Title: Visualizing distributed system executions (journal)
>  
背景： 分散システムはログへの課題を提示している。 複数ホストのログからシステムログを構築したり、ホスト間の非同期処理とか色々面倒  
提案手法：3つのタスクに取り組む手法 1) イベント間の順序の理解 2) ホスト間の相互作用パターンの検索 3) ペア間の構造的な類似点・相違点の識別。 XVector イベント間のHB関係情報をエンコードするツール、 ShiViz 分散システムの実行を時空間で表現  
実装：  
実験： 109人の学生の2つの調査と2人の開発者によるケーススタディ → 提案手法が **システム理解** に有用である

分散システムのロギングは確かに面倒くさそう。 本質的にシーケンシャルなログにできないしね。

- Title: An Integration Test Order Strategy to Consider Control Coupling (journal)
>  
背景： 統合テストにおいて、既存技術はクラス間の間接関係を考慮していない。   
提案手法： 統合テストのオーダー戦略。 クラス間の推移関係の概念を拡張 → テストスタブの複雑さを推定  

クラス間の間接関係ってどんなんだ？ 推移律とかの話から A→B（AとBは直接関係） A→B→C（AとCは間接関係） こんな感じ？ それとも動的にクラスが決定すること？（間接ジャンプとかの間接、まあ本質は同じっちゃ同じだが）

## ~A7 Human Aspects 1~

## A8 Machine Learning and Models

## <font color="orange"> A9 Traceability </font> (3 TRs, 1 SEIP, 1 Demo, 1 NIER)

- Title: A Novel Approach to Tracing Safety Requirements and State-Based Design Models (paper)
>  
背景： Traceability はソフトウェアとシステムの安全使用の保証において重要 → が、**自動化Traceabilityでは多くのFPが発生し低精度**  
提案手法： mutant-driven method 新規アイデア：正しいと思われる Trace Targets を積極的に作成しモデルチェックを利用して安全性を自動で検証。 要件のプロパティは mutant に保存 → MCに失敗したらKill → mutant の生死から Traceability link の相関関係を分析する手順も提案  
実装：  
実験： 27の要件を持つ2つの自動車システムの評価 → 最先端技術と比較してかなりの精度向上

ここでのmutantはどういう意味だろう

- Title: Establishing Multilevel Test-to-Code Traceability Links (paper)
>  
背景： **test -> code の Traceability Links はテストコードとテスト済みコードの同期を保てる** → テスト失敗率と障害の見落とし率が低下（テスト重複を避けつつ未テスト箇所を特定できるから？） ただし、これらは手動でやると負担が大きい  
提案手法： TCtracer test->code の Traceability Links を自動で確立する手法。 メソッド粒度、クラス粒度の両方で動作 → メソッド・クラス間の相乗的な情報の流れを利用  
実装：  
実験： 4つの大規模OSSシステムで評価 → 78%のMAP（Mean Average Precision： 平均精度）でTraceability Linksを確立可能、 テストクラス→クラス間は93%やぞ

なんか大抵はクラスのテストクラスってHogeTestとかになるんじゃ？ → まさかクラス名からリンク張るとかではないな？ メソッド単位のテストリンクをどう張るのかは気になる、あと依存関係にあるクラス間とかどうするのか

- Title: Lack of Adoption of Units of Measurement Libraries: Survey and Anecdotes (practice)
>  
背景： **Units of Measurement (UoM)** のライブラリはUnit内の変数を適当にエンコードして変換する → 3700ものライブラリがある、車輪の再発明が起きている、冗長である  
調査： UoMライブラリについて 3人の開発者と1人の科学者からヒアリング、オンラインアンケートを実施（91人の回答） → UoMライブラリへの不満と既存ライブラリが採用されない理由が判明 → UoMライブラリ製作者へリコメンド

車輪の再発明 (wheel is being reinvented, reinventing the wheel) -> 確立済みの技術を一から再度作ること、つまり冗長な準備の意 （たまに権藤先生が口にしていた）。 既存ライブラリに乗っかるのは常套手段だが、実際には痒い所に手が届かないこと、ありますあります。

- Title: Improving the Effectiveness of Traceability Link Recovery using Hierarchical Bayesian Networks (paper)
>  
背景： Traceability は大きな労力と時間がかかり、エラーも発生しやすくなる → 類似度からテキスト内のアーティファクトペア間の関係を描写、自動化するアプローチが研究されてきた → まだ欠点あるでよ、**類似度評価は単一メトリクスじゃ表現できぬ、多様な構造もしくは非構造的まアーティファクトグループ間関係を表現できぬ**  
提案手法： 確率モデル COMET (hierarchiCal prObabilistic Model for softwarE Traceability) テキストの類似性を複数の尺度で表現しアーティファクト間の関係をモデル化   
実装：  
実験：既存手法と比較してデータセット全体で一貫した効果を提示。 主要な通信会社と協力しCometプラグインを開発 → 現場に適用し、実用的との評価

アーティファクトが具体的に何を指すのか（ほかのTraceability論文でもそうだが）

## ~A10 Human Aspects 2~

## <font color="orange"> A11 Performance and Analysis </font> (1 TR, 4 Journals, 1 Demo, 1 NIER)

- Title: Testing with Fewer Resources: An Adaptive Approach to Performance-Aware Test Case Generation (journal)
>  
背景： テストケースの自動生成はカバレッジを基準に据えることが多かったが、最近の傾向ではランタイムとメモリも尺度として使われるようになってきた。 **カバレッジに影響を与えずコスト下げられるといいよね**  
提案手法： aDynaMOSA (adaptive DynaMOSA) テストの実行コストを合理的に見積もるアルゴリズム  
実装：  
実験： 110のJavaクラスについて、DynaMOSAと比較してランタイム -25%, ヒープメモリ -15% 程度でテストスイートを生成、7つのカバレッジ基準と同様の障害検出効果を保つ → Not Adaptive では不十分

テストコストを見積もって効率的なテストケース生成をしつつカバレッジ基準は損なわない手法。 テストケース生成に何を使っているのか気になるところ（記号実行とかだとヒープメモリ食いつぶすとかザラなので…）

- Title: What's Wrong with My Benchmark Results? Studying Bad Practices in JMH Benchmarks (journal)
>  
背景： パフォーマンステストにおいて、マイクロベンチマークは広く使われる手法。 Java Microbenchmark Harness (JMH) などはきめ細かいテストスイートを作成可能 → が、JVMが複雑なので正確なJMHを記述するのには苦労する  
調査： **JMHベンチの悪い慣例を経験的に調査** → 5つのプラクティスを自動検出するツールを開発 → 123 の Java-based OSS でそのプラクティスが蔓延（BAD!） → 正確な JMHの記述に苦労していることに起因 → 改善案の提示

ベンチマークの不正（都合よく改変）はまま起こりそうな問題ではある。 悪い方向を調査したのは興味深い

- Title: Towards the Use of the Readily Available Tests from the Release Pipeline as Performance Tests. Are We There Yet? (paper)
>  
背景： パフォーマンステストはシステムの環境が整った後にテストされるなど、開発段階でのテストが困難 → 開発中に使用している **Readily Available Tests** (即利用可能テスト) をパフォーマンステストとして機能させられないか？  
調査： Hadoop、Cassandra から127個のパフォーマンス問題を収集、即利用可能なテストからパフォーマンスを評価 → 多くの問題で解決が可能、しかし役立つ即利用可能テストはごく一部

- Title: ModGuard: Identifying Integrity & Confidentiality Violations in Java Modules (journal)
>  
背景： Java9 では Jigsaw （モジュールシステム） が加えられた → internal type の隠蔽を目的としているが、エスケープを防止できない → **意図しないエスケープが発生すると機密性が損なわれる** → が、チェックには複雑な作業が必要  
提案手法： ModGurad Doop-based の静的分析手法 （エスケープするインスタンスの自動識別ツール）  
実装：  
実験： MIC9Benchの適用例からModGuardの有効性と欠点の提示、 Tomcatのケーススタディ → Jigsawには防げない機密インスタンスのエスケープについてModGuardは防止

エスケープ解析の一種っぽい。どちらかというとセキュリティ寄りの論文な気がするが… 比較対象がJigsaw（Java 9）なので言語的な視点も必要そう

- Title: The ORIS Tool: Quantitative Evaluation of Non-Markovian Systems (journal)
>  
背景： **ORIS** 分散にも対応する numerical solution tools (数値解法) のJava実装の提供（？） → 特にパフォーマンス効率、信頼性、保守性などの早期評価をサポート  

？ 申し訳ないがいまいちORISが何なのか分らんかった（提案されたものなのか、すでにあるツールの有用性を調査したものなのか）

## <font color="orange"> A12 Testing </font> (2 TRs, 2 Journals, 1 Demo, 1 NIER)

- Title: Practical Fault Detection in Puppet Programs (paper)
>  
背景： **Puppet システムのリソースをモデル化・抽象化しセットアップを助ける管理ツール** → 潜在的な問題点 → Puppet自身のリソースが別のリソースに依存している場合にオーダー制約が正確に指定できないとリソースの競合を引き起こしエラーとなる、リソース変更の情報がPuppetへ伝播しないと可用性・機能を低下させる可能性がある  
提案手法： 上記の問題を特定するアプローチ → Puppetリソースとファイルシステムの相互作用のモデル化 → オーダー制約違反があれば潜在的な障害として報告  
実装：  
実験： Puppetの30のモジュールで未知の問題を発見、数秒で分析可能（早い）

リソースのモデル化は非常に厄介な課題の一つ。産業的にこのモデル化をどこまで適用できるのかは分らぬ

- Title: Empirical Assessment of Multimorphic Testing (journal)
>  
背景： ソフトウェアシステムのパフォーマンスは機能の正確さに匹敵するほど重要 → が、そのテストの評価についてはまだ議論が足りぬ （テストが十分かや、テスト同士の比較などが十分にはできない）  
提案手法： **パフォーマンス評価の定量的プロパティについてのテストスイートの「カバレッジ」を定義・評価するフレームワーク** → プロパティはタスクの実行時間、memory usage, テスト成功率など  
実装：  
実験： フレームワークを利用して、新しいテストケースがテストスイートに追加する価値があるかを評価。 3つの実証研究を通じて評価（object tracking in videos, object recognition in images, code generation）

パフォーマンステストの評価ってカバレッジが重視される印象ないなぁ（カバレッジを高くしつつメモリ使用量を計測する、とかならわかるけども）

6/25 (* absts, total * absts)

- Title: Learning from, Understanding, and Supporting DevOps Artifacts for Docker (paper)
>  
背景：  
提案手法：  
実装：  
実験：

- Title: Improving Change Prediction Models with Code Smell-Related Information (journal)
>  
背景：  
提案手法：  
実装：  
実験：

## <font color="orange"> P13 Security </font> (3 TRs, 2 SEIPs)

- Title: Burn After Reading: A Shadow Stack with Microsecond-level Runtime Rerandomization for Protecting Return Addresses (paper)
>  
背景：  
提案手法：  
実装：  
実験：

- Title: Automated Identification of Libraries from Vulnerability Data (practice)
>  
背景：  
提案手法：  
実装：  
実験：

- Title: Unsuccessful Story about Few Shot Malware-Family Classification and Siamese Network to the Rescue (paper)
>  
背景：  
提案手法：  
実装：  
実験：

- Title: SpecuSym: Speculative Symbolic Execution for Cache Timing Leak Detection (paper)
>  
背景：  
提案手法：  
実装：  
実験：

- Title: Building and Maintaining a Third-Party Library Supply Chain for Productive and Secure SGX Enclave Development (practice)
>  
背景：  
提案手法：  
実装：  
実験：

- Title:
>  
背景：  
提案手法：  
実装：  
実験：

- Title:
>  
背景：  
提案手法：  
実装：  
実験：