# ICSE 2020 memo

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

## ~P7 Human Aspects~

## <font color="orange"> P9 Bugs and Repair </font> (3 TRs, 2 Journals, 1 NIER)

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
背景：  
提案手法：  
実装：  
実験：

- Title: How does Machine Learning Change Software Development Practices? (journal)
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
