# ICSE 2020 memo

<style>
.waku {
  padding: 10px; margin-bottom: 10px; border: 3px solid;
}
</style>

# Tags
<details close>

|tag  | num of  |
|---- |----     |
|Tag_Learning|34|
|Tag_Survey|26|
|Tag_Dev|16|
|Tag_Automation|15
|Tag_Test|12
|Tag_Fuzz|11
|Tag_Logging|8
|Tag_Natural|8
|Tag_Human|8
|Tag_Modeling|8
|Tag_Prediction|8
|Tag_Review|7
|Tag_SymExe|5
|Tag_Static_Analysis|5
|Tag_Debug|5
|Tag_Defect4J|5
|Tag_Efficiency|5
|Tag_Regression|4
|Tag_Random|4
|Tag_AFL|4
|Tag_Recommend|4
|Tag_Patch|4
|Tag_Github|4
|Tag_Language|4
|Tag_Version|4
|Tag_Metrix|4
|Tag_Ethereum|3
|Tag_Smart_Contract|3
|Tag_Repair|3
|Tag_Android|3
|Tag_API|3
|Tag_Anti_Covarage|3
|Tag_Graph|3
|Tag_Localization|3
|Tag_Distribution|3
|Tag_Model_Checking|3
|Tag_Traceability|3
|Tag_Resource|3
|Tag_Test_Case_Gen|3
|Tag_Memory|3
|Tag_CI|3
|Tag_JIT|3
|Tag_APR|3
|Tag_Flaky|2
|Tag_Integration_Test|2
|Tag_Overhead|2
|Tag_Test_Suite|2
|Tag_Timing_Leak|2
|Tag_Hardware|2
|Tag_LtR|2
|Tag_Solidity|2
|Tag_Input|2
|Tag_CPDP|2
|Tag_Pointer|2
|Tag_Typestate|2
|Tag_AEBE|1
|Tag_Animation|1
|Tag_Assert|1
|Tag_Binary|1
|Tag_Block_Chain|1
|Tag_Cache|1
|Tag_Call_Stack|1
|Tag_CFG|1
|Tag_Class|1
|Tag_Cloud|1
|Tag_Code|1
|Tag_Code_Fix|1
|Tag_Code_Smell|1
|Tag_Code_Summarization|1
|Tag_Code_Transformation|1
|Tag_Comment|1
|Tag_Control|1
|Tag_CUDA|1
|Tag_Dependency|1
|Tag_Design_Pattern|1
|Tag_Docker|1
|Tag_Escape_Analysis|1
|Tag_Facebook|1
|Tag_Floating|1
|Tag_Game_Theory|1
|Tag_GUI|1
|Tag_Heap|1
|Tag_IaC|1
|Tag_Jigsaw|1
|Tag_JMH|1
|Tag_KLEE|1
|Tag_Library|1
|Tag_Malware|1
|Tag_MPI|1
|Tag_Naming|1
|Tag_ORIS|1
|Tag_Overflow|1
|Tag_Pair_Programming|1
|Tag_Puppet|1
|Tag_Protocol|1
|Tag_Readily_Available_Tests|1
|Tag_Robotics|1
|Tag_System_Call|1
|Tag_SILS|1
|Tag_SZZ|1
|Tag_Solver|1
|Tag_StackOverflow|1
|Tag_Slice|1
|Tag_Sync|1
|Tag_Spark|1
|Tag_Taint_Analysis|1
|Tag_TEE|1
|Tag_UoM|1
|Tag_User|1
|Tag_Warnning|1
|----|----|
|Tag_S_Analysis|13
|Tag_S_Code_Gen|4
|Tag_S_Debug|45
|Tag_S_Dev|4
|Tag_S_Fuzz|5
|Tag_S_Human_Aspects|5
|Tag_S_Performance|5
|Tag_S_Repair|11
|Tag_S_Security|9
|Tag_S_Summary|4
|Tag_S_Test|52
|Tag_S_Traceability|4
|Tag_S_Verification|12

</details>

# 6/23 (15 absts)

## ~I1 Metastudies~

## <font color="orange"> I2 Security </font> (4 TPs, 2 NIERs)
<div class="waku">
<details open>

___

- Title: <font color="orange">Targeted Greybox Fuzzing with Static Lookahead Analysis</font> (paper)  
>  
背景：自動テストは新規パス探索を目的としている → **静的解析によるバグっぽい場所への誘導が大事**  
提案手法：ターゲット（プログラムの変更箇所など）へ導くためのGrey-box Fuzzer with オンラインの静的解析  
静的解析：各プログラムパスの意味的 (semantically) 分析 → Fuzzの頻度の決定に利用  
実装：Ethereumの拡張 → コード計装がないのでsmart contractの分野でも適用可能（計装するとセマンティクスが変わることがある）  
実験：27のreal-world ベンチマークで評価 → 標準のGreybox Fuzzerを大幅に上回り、困難なターゲットへ83％到達可能、最大14倍の高速化も達成

Tag_Fuzz, Tag_Ethereum, Tag_Smart_Contract, Tag_Automation  
Tag_S_Security

___

- Title: HyDiff: Hybrid Differential Software Analysis (paper)  
>  
背景：**多くの評価（回帰バグの検出やDNNのロバスト性の評価）はソフトウェアの差分解析と見做せる** → 出力や振る舞いが異なるなら実行は分岐している（差分が存在する）  
提案手法：HyDiff：GreyboxFuzzとShadowSymbolicExecution → CFGを用いたメトリクスで記号実行を効率的にガイド  
実装：AFLとSymbolic Path Finder上に実装  
実験：Javaバイトコードプログラムの回帰分析などへのHyDiffの適用例を提示


Tag_Fuzz, Tag_Regression  
Tag_S_Security


ちょっちよくわからん
___

- Title: Towards Characterizing Adversarial Defects of Deep Learning Software from the Lens of Uncertainty (paper)
>  
背景：Deep Learning (DL) は企業のパフォーマンス向上に寄与した。が、**安全性の観点ではまだ懸念あり**。DLに誤った判断をさせるAE（Adversarial Examples）が存在する。DL内部の不確実性とAEの関係性の研究はまだ未熟。  
調査項目：BE（Benign Examples）とAEを区別するメトリクスを調査、又、その結果から珍しいパターンのAE/BEを生成する自動テスト手法の提案  
実験：自動生成されたパターンで最新の防御技術の突破に成功


Tag_Learning, Tag_Automation, Tag_AEBE  
Tag_S_Security


DL関連の自動化論文、いわゆるDLを用いたセキュリティの突破（顔認証とか？）を試みているっぽい
___

- Title: One Size Does Not Fit All: A Grounded Theory and Online Survey Study of Developer Preferences for Security Warning Types (paper)
>  
背景：ソフトウェア開発においてセキュリティ上の警告って大事だよね。色々支援ツールは存在するけど、開発者が警告に対してどう対話するかの調査はなかった。  
実験：14人の開発者、12人のCS学生、7人の研究者を対象に定性的な調査を実施（裏付けのために50人の開発者への定量調査も実施） → **誰からも優先される警告タイプは存在しない、コンテキストは重視される傾向にある** → IDEなどの対話が柔軟になればより有益となる可能性


Tag_Survey, Tag_Dev, Tag_Warnning  
Tag_S_Security


調査メインっぽいが、開発者と開発時の警告文との在り方の研究
___

</details>
</div>

## I4 Clones and Changes

## I5 Deep Learning Testing and Debugging

## I6 Empirical Studies and Requirements

## ~A1 Autonomous Driving Systems~

## <font color="orange"> A2 Testing and Debugging 1 </font> (2 TPs, 2 Journals, 2 NIERs)

<div class="waku">
<details open>

___

- Title: <font color="orange"> Studying the Use of Java Logging Utilities in the Wild </font> (paper)
>  
背景：ログは大事。**一般にロギングではprintデバッグではなくSLF4Jなどのユーティリティが好まれる。** LUの研究は少ないが、新しいLUプロダクトなどは続々と出ている。  
下調べ：Githubの大規模プロジェクトの調査 → 大規模プロジェクトのうち、75.8%は独自のLUを実装している、さらにそのうち50%以上が3つ以上のLUを使用。  
調査：LUの理解のための定性的調査 → ログは色々な理由で使われている

Tag_Logging, Tag_Survey  
Tag_S_Test, Tag_S_Debug

ロギングの調査論文、ある種Surveyとして利用できるかも？
___

- Title: Causal Testing: Understanding Defects' Root Causes (paper)
>  
背景：バグ修復のためにはバグの根本的な原因を理解することが重要である。  
提案手法：バグと関連する一連の実行を特定する手法 Causal Testing  
実装：  
実験：Defect4Jベンチマークで検証 → 71%で適用可能、そのうち77%で開発者に有用な情報を提供（JUnitなどでは得られない情報も存在）。 欠陥特定までの時間が標準ツールと比較して80 → 86%までと改善（？）

Tag_Repair, Tag_Debug, Tag_Survey Tag_Defect4J  
Tag_S_Test, Tag_S_Debug

具体的な内容はAbstだけでは読み取れず、JUnitとの比較からJavaの模様
___

- Title: Studying the Characteristics of Logging Practices in Mobile Apps: A Case Study on F-Droid. (journal)
>  
背景：モバイルアプリのロギングはなぜかあまり知られていない。  
調査：モバイルアプリのロギングの実践について調査。 F-Droidリポジトリ上の1444のOSS Androidのケーススタディ。 → サーバやデスクトップアプリほどログは普及していないが、ほぼすべてのアプリでログが活用されていた（ただし、サーバとはロギングの毛色が異なる）  
調査：メールインタビューなどから見えてきたこと → ロギングレベルと理論的根拠が頻繁に（35.4%）一致していない。 → ロギングの有用性を阻害している  
実験：上記の不一致のうち、不要なロギングを減らすことでどれだけパフォーマンスが向上するか実験 → 大幅に向上 → **モバイルロギングの体系化が必要**

Tag_Logging, Tag_Android  
Tag_S_Test, Tag_S_Debug
___

- Title: A Survey on Adaptive Random Testing (journal)
>  
背景： RT (Random Test)は十分研究されており、多くのアプリのテストに利用されている。 **ART (Adaptive Random Test)はRTの障害検出強化版で、テストケースを入力ドメインごとに均一になるように分散させる**。  
調査： ARTや技術の分類、アプリケーション領域の要約、各評価を実施。 いくつかの誤解と課題にも触れる。  

Tag_Test, Tag_Random  
Tag_S_Test, Tag_S_Debug

アプリケーションへの適用という意味では幅広い分野。ただし根本のアイデアは深く研究されているので、新規アイデアを出すのは難しそうな印象。
___

</details>
</div>

## <font color="orange"> A3 Code Summarization </font> (4 TRs, 2 NIERs)

<div class="waku">
<details open>

___

- Title: Posit: Simultaneously Tagging Natural and Programming Languages (paper)
>  
背景：開発におけるメーリングリストでは、自然言語とコードを混合して会話することが多い。 そこに **タグ付けをすることは、トレーサビリティ、コードスニペットの再利用の問題解決には重要な要素である**。  
提案手法：自然言語処理のコードスイッチの手法を混合テキストに適用して（プログラム？）言語識別とトークンタグ付けを試みるPosit。ソースコードトークンのASTタグと自然言語の単語の品詞タグを提供 → 混合テキストの言語を予測  
実装：CLANGからAST、Standard Stanfordから品詞を得て、biLSTMでトレーニング  
実験：言語識別を10.6%改善、タグ付けを23.7%正確にする

Tag_Learning, Tag_Dev, Tag_Natural  
Tag_S_Summary

着目点こそ混合テキストだが、実施内容は自然言語×機械学習という一般的なものに見える（Abstだけ見ると）
___

- Title: CPC: Automatically Classifying and Propagating Natural Language Comments via Program Analysis (paper)
>  
背景： 現代のソフトウェアシステムには膨大なコードコメントが記述されている。 既存の研究ではプログラム解析を用いたコメントの記述を行っていない（**解析によってコメントの導出、改良、伝播が可能** になる、らしい）。  
提案手法：コメントを効率的に利用するためには、コメントの分類法と自動分類を可能にする分類子が必要 → 包括的な分類法を提案  
実装：プロトタイプCPCを開発  
実験：5つのプロジェクトで評価 → 41573個の新規コメントの88%が他コードからの伝播により導出可能。 バグ検出にも役立ち、すでに修正済み。 伝播結果と実際のコメントの不一致を調査すると、コメントの欠陥も発見

Tag_Comment, Tag_Automation, Tag_Learning   
Tag_S_Summary

コメントの伝播というのは面白い。ワシはあまりコメントあまり書かないけど（あとで後悔する）
___

- Title: Suggesting Natural Method Names to Check Name Consistencies (paper)
>  
背景： **APIの名前が機能と一致していないと誤用につながるよね** 。  
提案手法：実装と関数名が一致しているか（一貫性）を検証するMNIREを提案。 機械学習を利用、実装から導出された名前が実際の関数名と近いかを検査。 ユニークアイデア： body, interface, class name の三つのコンテキストから名前を生成
実装：  
実験：14M超えのメソッドデータセットで学習。 MNIREはメソッド名再現率について最先端アプローチ (code2vec) と比較して recall: 10.4 %, precision: 11% 向上。 名前の提案を現行プロジェクトに31/42で改善が認められた

Tag_API, Tag_Learning, Tag_Naming  
Tag_S_Summary
___

- Title: Retrieval-based Neural Source Code Summarization (paper)
>  
背景： ソースコード要約自動生成の既存技術は「ソースコードからの用語の取得」もしくは「類似コードスニペットから要約を検索」。 最近ではEnc/Decモデルを用いて要約を生成（課題として、低頻度の単語では精度が落ちる）  
提案手法：Enc/Decの拡張、類似スニペットのみを用いてトレーニング  
実装：  
実験：色々やって、最先端アプローチと比較して改善できることを確認

Tag_Learning, Tag_Code_Summarization  
Tag_S_Summary

何やら色々やっている模様。サマリ生成はジャンルとして興味深いが自然言語処理とか機械学習の毛色が濃いのでとっつきにくい（もちろん機械学習だけではないんだけど）
___

</details>
</div>

## ~A4 Cyber Physical Systems~

## <font color="orange"> A5 Testing and Debugging 2 </font> (3 TPs, 2 Demos, 2 NIERs)

<div class="waku">
<details open>

___

- Title: Efficient Generation of Error-Inducing Floating-Point Inputs via Symbolic Execution (paper)
>  
背景： 浮動小数点の精度を評価する場合、**丸め誤差の数値エラーを最大化する入力生成が重要** 。  
提案手法：浮動小数点エラーをより頻繁に起こす入力生成をコードカバレッジ問題へと定式化。 不正確なチェックをプログラム内に挿入し、そこをカバーするように特殊ブランチを構築（記号実行で誘導）  
実装：ツールFPGen  
実験：行列計算、統計ライブラリを含む21のプログラムで評価 → 20のエラーと平均して2桁以上多いエラーを実行可能（最先端アプローチと比較して）

Tag_SymExe, Tag_Floating  
Tag_S_Test, Tag_S_Debug

浮動小数点ということはバイナリ寄りか？
___

- Title: <font color="orange"> A Study on the Lifecycle of Flaky Tests </font> (paper)
>  
背景： **Flaky Tests: コード変更なしで失敗/成功する可能性のある不安定なテスト** (例えば何だ？Time関数みたいな非決定的なAPIとか？)、最近産業、Academic両方から関心 → Flaky Testsの再現性・原因などの研究は多くない  
調査： AnonCompanyのプロジェクトから Flaky Testsのライフサイクルを調査。 又、Flaky Testsの失敗再現の調査、および修正までの時間と効果を調査

Tag_Flaky, Tag_Survey  
Tag_S_Test, Tag_S_Debug

調査論文。ただの非決定的テストをFlakyと言い換えているだけなのかが気になるところ
___

- Title: <font color="orange"> Ankou: Guiding Grey-box Fuzzing towards Combinatorial Difference </font> (paper)
>  
背景： Greybox Fuzzing ではフィットネス関数（カバレッジ最大化など）を用いてテストケースプールを維持・進化させる。 **カバレッジが同じだと異なるプログラム実行を区別できない**。 既存のフィットネス関数はデータの和（Union）を重視し積（Combination）を考慮しない（？） → ローカル最適化で行き詰まることがままある  
提案手法： 実行情報の積を認識するGreybox Fuzzer Ankou  
実装：  
実験： バグ検索において、AFL/Angoraと比較して ×1.94/×8.0 効果的。 スケーラビリティの課題はあり

Tag_Fuzz, Tag_AFL, Tag_Anti_Covarage  
Tag_S_Test, Tag_S_Debug

確かに（定義にも依るが）Greybox Fuzzingでは実行同士を区別できない、でも実行のCombinationって何だろ（よもや実行順ではあるまい）
___

</details>
</div>

# 6/24 (25 absts, total 40 absts)

## ~P7 Human Aspects~ 読んじゃった (3 TRs, 2 Journals, 1 NIER)

<div class="waku">
<details open>

___

- Title: What Predicts Software Developers' Productivity? (journal)
>  
背景： 組織が取れるソフトウェア開発者へのサポートは様々（オフィスレイアウトからソースコードクリーンアップまで）  
調査： どのオプションが生産性に影響を与えるか 3社 622人 の開発者へ調査 → **モチベーションなど非技術的な要因に大きな相関**

Tag_Dev, Tag_Human, Tag_Survey  
Tag_S_Human_Aspects

ある意味グループに対するアンチテーゼ
___

- Title: A Study on the Prevalence of Human Values in Software Engineering Publications, 2015 – 2018 (paper)
>  
背景： ソフトウェア評価において、人間の価値基準（平等かどうかなど）を損なうとユーザの不満などにつながる。   
調査： 近年のカンファレンスやジャーナルがこうした人間の価値について論じているかを調査。 様々な価値から出版物を分類、結果として a) 価値を直接検討しているのはごく一部 b) 価値の大部分について、議論している出版物はほとんどもしくは皆無 c) SEジャーナルよりはSEカンファレンスのほうが論じているpaperは多かった  

Tag_Human, Tag_Survey  
Tag_S_Human_Aspects

メタ会議、メタジャーナル。human valuesってなんじゃろと気にはなる
___

- Title: Explaining Pair Programming Session Dynamics from Knowledge Gaps (paper)
>  
背景： ペアプログラミング (PP) の有用性を論じる研究は多いのに、有用か否かの結論は未だ出ていない  
提案手法： Grounded Theory Methodology を用いて多くの産業用 PP を解析  
実装：  
実験： 必要知識の異なる（一般的なプログラミングと固有プロジェクト）場合に PP セッションでの挙動がどうなるかの比較 → プロジェクト固有の方が、知識ギャップの差が PP のより大きな妨げになる → **固有システムの理解が PP において大事**

Tag_Pair_Programming, Tag_Dev  
Tag_S_Human_Aspects
___

- Title: Engineering Gender-Inclusivity into Software: Ten Teams' Tales from the Trenches (paper)
>  
背景： **Gender-Inclusivity** がSEの研究者の間で注目されている。 が、提唱されているだけで実践されてはいない。  
実験： 5か月 ～ 3年半の範囲で50人以上のチーム10個にGenderMagベースのプロセスを収集 → 9つの実践と2つの課題を提示

Tag_Human, Tag_Survey  
Tag_S_Human_Aspects

Gender-Inclusivityってなんじゃらほい
___

- Title: How does Machine Learning Change Software Development Practices? (journal)
>  
背景： **学習機能をシステムに追加すると不確実性が内包される**。 学習機能の追加の効果を調査  
調査： 26か国で14人へインタビュー、342人から調査回答 → 学習機能の有無で、要件、設計、テスト、スキルの多様性、問題解決などで明らかに差が出た

Tag_Learning Tag_Dev, Tag_Survey  
Tag_S_Human_Aspects
___

</details>
</div>

## <font color="orange"> P9 Bugs and Repair </font> (1 TR, 2 Journals, 2 SEIPs, 1 NIER)

<div class="waku">
<details open>

___

- Title: PRECFIX: Large-Scale Patch Recommendation by Mining Defect-Patch Pairs (practice)
>  
背景： パッチのリコメンドはエラーの適切に修正を提案するプロセス。 デバッグと修正の時間削減に効果 → 生産性の向上。 既存研究はテストスイートとデバッグレポートに依存 → 現実世界では欠けることもしばしば  
提案手法： PRECFIX 過去のデバッグアクションから適切なリコメンドする手法。 クラスタリングによって一般的な再利用可能なパッチパターンを抽出。  
実装：  
実験： 様々な欠陥パターンの存在する 10K個のプロジェクトで実験 → 3000のテンプレートを抽出、また高速、FPは22%程度。 **アリババにPRECFIXをロールアウト**

Tag_Recommend, Tag_Patch, Tag_Learning  
Tag_S_Debug, Tag_S_Repair
___

- Title: On the Efficiency of Test Suite based Program Repair: A Systematic Assessment of 16 Automated Repair Systems for Java Programs (paper)
>  
背景： テストベースの自動プログラム修正は色々研究有り（テストスイートの近似として応用）。**パッチの正確性に対する研究はあるが、生成の効率性に対する研究は少ない**。   
調査： 既存のテストスイートベースのプログラム修正技術の効率性に関して調査  → 生成されたパッチの個数に着目 → 探索空間の広さ・パッチ生成までのテスト作業の少なさ・適切なパッチを生成できるか を評価項目
実験： Java用の16個のOSS修復ツールから生成されたパッチ候補を量的観点で実証 → 現在のテンプレートベース（最も効果的とされている）の修正システムでは無関係なパッチ候補を生成する傾向があるので最低効率となった

Tag_Patch, Tag_Automation, Tag_Efficiency  
Tag_S_Debug, Tag_S_Repair

パッチの効率についての研究。実をいうと未だにパッチの仕組みが分かっていない。
___

- Title: SEQUENCER: Sequence-to-Sequence Learning for End-to-End Program Repair (journal)
>  
提案手法： SEQUENCER: S2S学習に基づいた新規E2Eアプローチ。 機械学習を用いたコード上のバグ修正、**大きなコードで発生していた無制限の語彙問題を克服**    
実験： 35578のサンプルでトレーニング、4711の実際のバグ修正と、Defects4JベンチマークでSEQUENCERを評価 → 950/4711で固定ライン（fixed line）(?) を予測、Defects4Jベンチで14のバグパッチを発見

Tag_Learning, Tag_Code_Fix, Tag_Defect4J  
Tag_S_Debug, Tag_S_Repair

unlimited vocabulary problem って何だろ
___

- Title: A Study of Bug Resolution Characteristics in Popular Programming Languages (journal)
>  
背景： プログラミング言語ごとのバグ解決特性をGithubのプロジェクトから調査（相関関係なので注意）。   
調査： 主な言語10の600個のプロジェクト、3Mのコミットからバグ解決データを抽出 → 解決時間とパッチサイズが言語間で大きな差が出た。 Rubyは時間がかかるなど。 静的型付け言語ではパッチが横断的になるが、解決までは短い

Tag_Github, Tag_Survey, Tag_Language  
Tag_S_Debug, Tag_S_Repair

言語ごとの調査は根が深そう（言語にもバージョンがあるし）。 静的型付け言語では修正時間が短いというのはどこかで引用できそうな予感
___

- Title: Automated Bug Reproduction from User Reviews for Android Applications (practice)
>  
背景： **ユーザーレビューのバグ報告は再現がムズイ** → 自動化できると嬉しい → が、レビューは難解で情報不足
提案手法： RevRep ユーザレビューからAndroidアプリのバグを再現するツール。 自然言語処理を利用してバグ再現に役立つ情報を抽出  
実験： Google Play 上の63件のクラッシュ関連のレビューを含むベンチマークを生成 → 人間と同程度のパフォーマンスを実現、70%のレビューを正常に再現

Tag_Review, Tag_User, Tag_Automation, Tag_Android  
Tag_S_Debug, Tag_S_Repair

ユーザレビューから再現するのが難しいのは認めるが、70%はにわかに信じがたい。ベンチマークにカラクリがありそう
___

</details>
</div>

## P10 Stack Overflow

## ~P11 Natural Language Artifacts~

## <font color="orange"> P12 Testing and Debugging </font> (4 Journals, 2 SEIPs)

<div class="waku">
<details open>

___

- Title: Debugging Crashes using Continuous Contrast Set Mining (practice)
>  
背景： Facebook では毎日数百万のレビューが送られ、エンジニアはその中からバグとかリグレッションをチェックしている → 多くは手作業、かつ時間は有限、専門知識と精密なコード検査が必要 （ツライ）  
提案手法： **Contrast Set Mining** → 判別パターンマイニングの一種 → 連続データの離散化がメイン → 提案：CSMの連続データへの直接適用（No 離散化）  
実装：  
実験：挑戦的なデータセット、ユーザナビゲーションのイベントシーケンスへの適用

Tag_Facebook, Tag_Review, Tag_Regression, Tag_Learning  
Tag_S_Test, Tag_S_Debug

レビューと連続データって関連付けされるのか？元から離散的なイメージ
___

- Title: Automatic Abnormal Log Detection by Analyzing Log History for Providing Debugging Insight (practice)
>  
背景： ソフトウェアサイズが大きく複雑化すると欠陥の発見はより困難になる → **ログ分析がデバッグの洞察 (Insight) を得る唯一の方法** → 手動だとエラく労働力が必要  
提案手法： historian ログ分析システム → 統計的テキストマイニングでログ内の重要度とノイズ度を用いて強調表示  
実装：  
実験： Tizen Native API テストログに適用 → 平均 4%のログのみ強調 → 実際にTizen開発者へ報告、いいんんじゃない、という評価

Tag_Logging, Tag_Learning, Tag_Natural  
Tag_S_Test, Tag_S_Debug

強調表示のし損ね（FN）とか冗長（FP）とかがどれだけあるのか気になるところ。対象はTizenのみ？
___

- Title: <font color="orange"> Explaining Regressions via Alignment Slicing and Mending </font> (journal)
>  
背景： Regression Faults が発生した場合の一般的な作業 1) 障害発生個所の分離 2) 障害発生へのフローの理解 → 障害箇所の特定の既存研究 Dynamic Slicing, Delta Debugging, Symbolic Analysis → 精度やスケーラビリティの問題が依然残る  
提案手法： トレースベース （Defectがどのように障害へ伝播するのか調査） → **Causality Graph** の構築。 課題１ ２つのプログラムバージョンの各トレースのアライメント（どのように重要な差分を表現するか）。 課題２ **Alignment Slice**, Mending から Causality Graph を構築、障害の説明。  
実装：  
実験： Dynamic Slicing, Delta Debugging, 記号実行との比較、検体は２４個のリアル検体 → 障害分離は正確、障害の説明は十分許容コストで理解可能、実行オーバーヘッドを十分抑えられる

Tag_Regression, Tag_SymExe, Tag_Slice, Tag_Graph  
Tag_S_Test, Tag_S_Debug

障害箇所の特定という意味では徐さんのテーマとの関連性が深そう。検体的にJavaを対象としている？ 実験は豊富、強い
___

- Title: Historical Spectrum based Fault Localization (journal)
>  
背景： **Spectrum Based Fault Localization (SBFL)** は障害特定に効果的と評価、業界でも自動化されたSBFLが評価 → が、制限あり。 スペクトル構築のためのテストカバレッジ情報は障害原因を表現していない、また、SBFLはバグの有無を十分区別できない  
提案手法： バージョン履歴情報を障害特定に利用する手法 HSFL (Historical Spectrum-based Fault Localization)。 バグを引き起こすコミットを特定 → Historical Spectrum を構築（これを用いて怪しいコード要素をランク付け）  
実装：  
実験： Defects4J ベンチマークを用いて 最先端SBFL技術と比較して最大 +77.8% 多くのバグを発見（上位5つで+40.8%）。  

Tag_Localization, Tag_Anti_Covarage, Tag_Defect4J, Tag_Version  
Tag_S_Test, Tag_S_Debug

Defects4J結構頻繁に出てくるなぁ（有名なんだろうか）
___

- Title: Visualizing distributed system executions (journal)
>  
背景： 分散システムはログへの課題を提示している。 複数ホストのログからシステムログを構築したり、ホスト間の非同期処理とか色々面倒  
提案手法：3つのタスクに取り組む手法 1) イベント間の順序の理解 2) ホスト間の相互作用パターンの検索 3) ペア間の構造的な類似点・相違点の識別。 XVector イベント間のHB関係情報をエンコードするツール、 ShiViz 分散システムの実行を時空間で表現  
実装：  
実験： 109人の学生の2つの調査と2人の開発者によるケーススタディ → 提案手法が **システム理解** に有用である

Tag_Logging, Tag_Distribution  
Tag_S_Test, Tag_S_Debug

分散システムのロギングは確かに面倒くさそう。 本質的にシーケンシャルなログにできないしね。
___

- Title: An Integration Test Order Strategy to Consider Control Coupling (journal)
>  
背景： 統合テストにおいて、既存技術はクラス間の間接関係を考慮していない。   
提案手法： 統合テストのオーダー戦略。 クラス間の推移関係の概念を拡張 → テストスタブの複雑さを推定  

Tag_Integration_Test, Tag_Class  
Tag_S_Test, Tag_S_Debug

クラス間の間接関係ってどんなんだ？ 推移律とかの話から A→B（AとBは直接関係） A→B→C（AとCは間接関係） こんな感じ？ それとも動的にクラスが決定すること？（間接ジャンプとかの間接、まあ本質は同じっちゃ同じだが）
___

</details>
</div>


## ~A7 Human Aspects 1~

## A8 Machine Learning and Models

## <font color="orange"> A9 Traceability </font> (3 TRs, 1 SEIP, 1 Demo, 1 NIER)

<div class="waku">
<details open>

___

- Title: A Novel Approach to Tracing Safety Requirements and State-Based Design Models (paper)
>  
背景： Traceability はソフトウェアとシステムの安全使用の保証において重要 → が、**自動化Traceabilityでは多くのFPが発生し低精度**  
提案手法： mutant-driven method 新規アイデア：正しいと思われる Trace Targets を積極的に作成しモデルチェックを利用して安全性を自動で検証。 要件のプロパティは mutant に保存 → MCに失敗したらKill → mutant の生死から Traceability link の相関関係を分析する手順も提案  
実装：  
実験： 27の要件を持つ2つの自動車システムの評価 → 最先端技術と比較してかなりの精度向上

Tag_Model_Checking, Tag_Traceability  
Tag_S_Traceability

ここでのmutantはどういう意味だろう
___

- Title: Establishing Multilevel Test-to-Code Traceability Links (paper)
>  
背景： **test → code の Traceability Links はテストコードとテスト済みコードの同期を保てる** → テスト失敗率と障害の見落とし率が低下（テスト重複を避けつつ未テスト箇所を特定できるから？） ただし、これらは手動でやると負担が大きい  
提案手法： TCtracer test->code の Traceability Links を自動で確立する手法。 メソッド粒度、クラス粒度の両方で動作 → メソッド・クラス間の相乗的な情報の流れを利用  
実装：  
実験： 4つの大規模OSSシステムで評価 → 78%のMAP（Mean Average Precision： 平均精度）でTraceability Linksを確立可能、 テストクラス→クラス間は93%やぞ

Tag_Traceability, Tag_Test, Tag_Code  
Tag_S_Traceability

なんか大抵はクラスのテストクラスってHogeTestとかになるんじゃ？ → まさかクラス名からリンク張るとかではないな？ メソッド単位のテストリンクをどう張るのかは気になる、あと依存関係にあるクラス間とかどうするのか
___

- Title: Lack of Adoption of Units of Measurement Libraries: Survey and Anecdotes (practice)
>  
背景： **Units of Measurement (UoM)** のライブラリはUnit内の変数を適当にエンコードして変換する → 3700ものライブラリがある、車輪の再発明が起きている、冗長である  
調査： UoMライブラリについて 3人の開発者と1人の科学者からヒアリング、オンラインアンケートを実施（91人の回答） → UoMライブラリへの不満と既存ライブラリが採用されない理由が判明 → UoMライブラリ製作者へリコメンド

Tag_UoM, Tag_Survey, Tag_Recommend  
Tag_S_Traceability

車輪の再発明 (wheel is being reinvented, reinventing the wheel) → 確立済みの技術を一から再度作ること、つまり冗長な準備の意 （たまに権藤先生が口にしていた）。 既存ライブラリに乗っかるのは常套手段だが、実際には痒い所に手が届かないこと、ありますあります。
___

- Title: Improving the Effectiveness of Traceability Link Recovery using Hierarchical Bayesian Networks (paper)
>  
背景： Traceability は大きな労力と時間がかかり、エラーも発生しやすくなる → 類似度からテキスト内のアーティファクトペア間の関係を描写、自動化するアプローチが研究されてきた → まだ欠点あるでよ、**類似度評価は単一メトリクスじゃ表現できぬ、多様な構造もしくは非構造的まアーティファクトグループ間関係を表現できぬ**  
提案手法： 確率モデル COMET (hierarchiCal prObabilistic Model for softwarE Traceability) テキストの類似性を複数の尺度で表現しアーティファクト間の関係をモデル化   
実装：  
実験：既存手法と比較してデータセット全体で一貫した効果を提示。 主要な通信会社と協力しCometプラグインを開発 → 現場に適用し、実用的との評価

Tag_Traceability, Tag_Efficiency, Tag_Learning, Tag_Automation  
Tag_S_Traceability

アーティファクトが具体的に何を指すのか（ほかのTraceability論文でもそうだが）
___

</details>
</div>

## ~A10 Human Aspects 2~

## <font color="orange"> A11 Performance and Analysis </font> (1 TR, 4 Journals, 1 Demo, 1 NIER)

<div class="waku">
<details open>

___

- Title: Testing with Fewer Resources: An Adaptive Approach to Performance-Aware Test Case Generation (journal)
>  
背景： テストケースの自動生成はカバレッジを基準に据えることが多かったが、最近の傾向ではランタイムとメモリも尺度として使われるようになってきた。 **カバレッジに影響を与えずコスト下げられるといいよね**  
提案手法： aDynaMOSA (adaptive DynaMOSA) テストの実行コストを合理的に見積もるアルゴリズム  
実装： EvoSuite (JUnitの自動テスト)  
実験： 110のJavaクラスについて、DynaMOSAと比較してランタイム -25%, ヒープメモリ -15% 程度でテストスイートを生成、7つのカバレッジ基準と同様の障害検出効果を保つ → Not Adaptive では不十分

Tag_Test, Tag_Resource, Tag_Test_Case_Gen, Tag_Memory, Tag_Overhead  
Tag_S_Performance, Tag_S_Analysis

テストコストを見積もって効率的なテストケース生成をしつつカバレッジ基準は損なわない手法。 テストケース生成に何を使っているのか気になるところ（記号実行とかだとヒープメモリ食いつぶすとかザラなので…）
___

- Title: What's Wrong with My Benchmark Results? Studying Bad Practices in JMH Benchmarks (journal)
>  
背景： パフォーマンステストにおいて、マイクロベンチマークは広く使われる手法。 Java Microbenchmark Harness (JMH) などはきめ細かいテストスイートを作成可能 → が、JVMが複雑なので正確なJMHを記述するのには苦労する  
調査： **JMHベンチの悪い慣例を経験的に調査** → 5つのプラクティスを自動検出するツールを開発 → 123 の Java-based OSS でそのプラクティスが蔓延（BAD!） → 正確な JMHの記述に苦労していることに起因 → 改善案の提示

Tag_Test, Tag_JMH, Tag_Survey, Tag_Automation
Tag_S_Performance, Tag_S_Analysis

ベンチマークの不正（都合よく改変）はまま起こりそうな問題ではある。 悪い方向を調査したのは興味深い
___

- Title: Towards the Use of the Readily Available Tests from the Release Pipeline as Performance Tests. Are We There Yet? (paper)
>  
背景： パフォーマンステストはシステムの環境が整った後にテストされるなど、開発段階でのテストが困難 → 開発中に使用している **Readily Available Tests** (即利用可能テスト) をパフォーマンステストとして機能させられないか？  
調査： Hadoop、Cassandra から127個のパフォーマンス問題を収集、即利用可能なテストからパフォーマンスを評価 → 多くの問題で解決が可能、しかし役立つ即利用可能テストはごく一部

Tag_Test, Tag_Readily_Available_Tests, Tag_Survey  
Tag_S_Performance, Tag_S_Analysis
___

- Title: ModGuard: Identifying Integrity & Confidentiality Violations in Java Modules (journal)
>  
背景： Java9 では Jigsaw （モジュールシステム） が加えられた → internal type の隠蔽を目的としているが、エスケープを防止できない → **意図しないエスケープが発生すると機密性が損なわれる** → が、チェックには複雑な作業が必要  
提案手法： ModGurad Doop-based の静的分析手法 （エスケープするインスタンスの自動識別ツール）  
実装：  
実験： MIC9Benchの適用例からModGuardの有効性と欠点の提示、 Tomcatのケーススタディ → Jigsawには防げない機密インスタンスのエスケープについてModGuardは防止

Tag_Jigsaw, Tag_Escape_Analysis, Tag_Static_Analysis, Tag_Automation  
Tag_S_Performance, Tag_S_Analysis

エスケープ解析の一種っぽい。どちらかというとセキュリティ寄りの論文な気がするが… 比較対象がJigsaw（Java 9）なので言語的な視点も必要そう
___

- Title: The ORIS Tool: Quantitative Evaluation of Non-Markovian Systems (journal)
>  
背景： **ORIS** 分散にも対応する numerical solution tools (数値解法) のJava実装の提供（？） → 特にパフォーマンス効率、信頼性、保守性などの早期評価をサポート  

Tag_ORIS  
Tag_S_Performance, Tag_S_Analysis

？ 申し訳ないがいまいちORISが何なのか分らんかった（提案されたものなのか、すでにあるツールの有用性を調査したものなのか）
___

</details>
</div>

## <font color="orange"> A12 Testing </font> (2 TRs, 2 Journals, 1 Demo, 1 NIER)

<div class="waku">
<details open>

___

- Title: Practical Fault Detection in Puppet Programs (paper)
>  
背景： **Puppet システムのリソースをモデル化・抽象化しセットアップを助ける管理ツール** → 潜在的な問題点 → Puppet自身のリソースが別のリソースに依存している場合にオーダー制約が正確に指定できないとリソースの競合を引き起こしエラーとなる、リソース変更の情報がPuppetへ伝播しないと可用性・機能を低下させる可能性がある  
提案手法： 上記の問題を特定するアプローチ → Puppetリソースとファイルシステムの相互作用のモデル化 → オーダー制約違反があれば潜在的な障害として報告  
実装：  
実験： Puppetの30のモジュールで未知の問題を発見、数秒で分析可能（早い）

Tag_Resource, Tag_Puppet, Tag_Modeling  
Tag_S_Test

リソースのモデル化は非常に厄介な課題の一つ。産業的にこのモデル化をどこまで適用できるのかは分らぬ
___

- Title: Empirical Assessment of Multimorphic Testing (journal)
>  
背景： ソフトウェアシステムのパフォーマンスは機能の正確さに匹敵するほど重要 → が、そのテストの評価についてはまだ議論が足りぬ （テストが十分かや、テスト同士の比較などが十分にはできない）  
提案手法： **パフォーマンス評価の定量的プロパティについてのテストスイートの「カバレッジ」を定義・評価するフレームワーク** → プロパティはタスクの実行時間、memory usage, テスト成功率など  
実装：  
実験： フレームワークを利用して、新しいテストケースがテストスイートに追加する価値があるかを評価。 3つの実証研究を通じて評価（object tracking in videos, object recognition in images, code generation）

Tag_Test, Tag_Test_Suite  
Tag_S_Test

パフォーマンステストの評価ってカバレッジが重視される印象ないなぁ（カバレッジを高くしつつメモリ使用量を計測する、とかならわかるけども）
___

# 6/25 (24 absts, total 64 absts)

- Title: Learning from, Understanding, and Supporting DevOps Artifacts for Docker (paper)
>  
背景： DevOps ツールなどの増加によって、コードサポート以上に **ツールサポート（Dockerなどへ）の需要が高まっている。** Dockerなどを解析する支援には課題（不足）がある（ネストされた言語、ルールマイニング、セマティックルールベースの分析）  
提案手法： binnacle 900,000のGithubレポジトリを取り込めるルールセット → Dockerのビルドエラーを回避、イメージサイズとビルドレイテンシを改善  
調査： Github上のDocerfileはゴールドセット（何やらランクの高い）に比べて6倍近くルール違反

Tag_Dev, Tag_Docker, Tag_Survey  
Tag_S_Test

Dockerについてもあんまりわかっていない（仮想環境っぽいことだと思っている）。産業的には仮想環境はどこまで関心があるのかは要調査
___

- Title: Improving Change Prediction Models with Code Smell-Related Information (journal)
>  
背景： **コードの匂い (code smells) はクラス変更に悪影響を与える次善の実装選択**。 コードの匂いに関する情報から、将来どのクラスが変更されるのかを示すモデルのパフォーマンス改善を目指す  
提案手法： 強度インデックス（においの重大度）を利用  
実装：  
実験： 既存手法のアンチパターンメトリクスと比較 → 強度モデルのほうがベースラインよりも統計的に優れる、強度モデルがアンチパターンよりも優れた予測を示す。 F値も高い

Tag_Code_Smell, Tag_Version, Tag_Metrix  
Tag_S_Test

コードの匂いに関するサーベイっぽい感じ？ 確かバージョン管理関連の話だった気がする
___

</details>
</div>

## <font color="orange"> P13 Security </font> (3 TRs, 2 SEIPs)

<div class="waku">
<details open>

___

- Title: Burn After Reading: A Shadow Stack with Microsecond-level Runtime Rerandomization for Protecting Return Addresses (paper)
>  
背景： **ROP (Return-oriented Programming)** ret命令を用いたコールスタック制御（return addressの改竄）によるコード再利用攻撃 → 主な対策案 Shadow Stack, CFI, code randomization → 既存のrandomizationは高度なポインタ追跡が必要  
提案手法： BarRA return address を保護するShadow Stack メカニズム （高度なポインタ追跡を回避） → 抽象的（間接的？）なreturn addressのマッピングをマイクロ秒のオーダーでrandomization  
実装：  
実験： SPEC CPU2006 の19個のベンチマークで検証、バイナリサイズは微増（ave: +29.44%）、return address保持のために8MBのみ占有、突破率を 1/2^20 程度に下げる、動的オーバーヘッドはshadow Stackと同程度

Tag_Call_Stack, Tag_Random, Tag_Binary, Tag_Overhead  
Tag_S_Security

Shadow Stackだけでは無理なん？と思ってしまった。 Shadow Stackの問題点は本文で指摘されている？ 学生時代だと面白そうな内容（バイナリだし）、が、グループ的にはジャンル違い
___

- Title: Automated Identification of Libraries from Vulnerability Data (practice)
>  
背景： Software Composition Analysis (SCA) には脆弱性をデータベースに登録するプロセスがある。 その際、どのライブラリに脆弱性があるのかを特定したいが、それが難しいねんな → 機械学習を用いて特定できないか？  
提案手法： **extreme multi-label learning (XML)** → FastXML を用いて学習  
実装：  
実験： NVD (National Vulnerability Database) について検証 → F1@k は 0.53

Tag_Review, Tag_Learning, Tag_Library  
Tag_S_Security

F値が0.53だとまだ実用化までには課題も多そう。 XMLは初めて知った
___

- Title: Unsuccessful Story about Few Shot Malware-Family Classification and Siamese Network to the Rescue (paper)
>  
背景： **マルウェアを機能ごとに分類するファミリー分類** はマルウェア分析において効果的（機械学習ベースが提案） → 課題在り → サンプル数が少ないファミリについては分類パフォーマンスが悪い（精度が低い？）  
提案手法： 従来のダウンサンプリング → Siamese-Network-basedの分類  
実装：  
実験： 少数ファミリでは高いパフォーマンスを発揮（可能性あり） → 全体のパフォーマンス向上につながる

Tag_Malware, Tag_Learning  
Tag_S_Security

問題こそマルウェアだが、論文の内容は少サンプルの分類問題をいかに効率的に学習させるか、ということになってない？ マルウェア固有の要素はあるのだろうか（あるいはただの応用先？）
___

- Title: SpecuSym: Speculative Symbolic Execution for Cache Timing Leak Detection (paper)
>  
背景： CPUキャッシュに対するタイミングサイドチャネルはタイミングを測定されると間接的にデータをリークする可能性がある → 静的解析によって、投機的実行によってタイミングリークがないことを定性的に確認できる → リークを引き起こす入力などを生成できない（静的解析の限界）  
提案手法： **Speculative Symbolic Execution** 投機的実行とキャッシュをモデル化し、キャッシュタイミングのリークを定式化  
実装： KLEE上に実装  
実験： 14のOSSベンチマークに対して評価 → 4つの異なるキャッシュ上で6つのリークを特定、2つのプログラムで誤検知を排除

Tag_Cache, Tag_Modeling, Tag_KLEE, Tag_Timing_Leak  
Tag_S_Security

探索空間をキャッシュ状態にも広げたタイプの記号実行、LLVM上だとキャッシュ情報とかを設定しやすいのだろうか
___

- Title: Building and Maintaining a Third-Party Library Supply Chain for Productive and Secure SGX Enclave Development (practice)
>  
背景： **セキュリティ上で機密データに対する計算を Trusted Execution Environment (TEE) 内で行うことが対策の一つとなっている** → が、TEEはハードウェア支援なので、ソフトウェア全体に制約が発生し、開発が困難に（3rd Partyの依存関係にも影響）  
実践： TEE開発者向けのTPサプライチェーンの構築と維持に対する経験と成果、 Rust TPライブラリの（大規模な）コレクションを Intel SGX へ移植。 159のOSS Rust ライブラリのSGXポートを維持

Tag_TEE, Tag_Hardware, Tag_Dependency  
Tag_S_Security

ハードウェア支援系は環境が限られているなぁ。 グループGのプロダクトでハードウェア支援を採用しているのってあるのだろうか
___

</details>
</div>

## <font color="orange"> P14 Testing </font> (4 TPs, 1 SEIP)

<div class="waku">
<details open>

___

- Title:  Seenomaly: Vision-Based Linting of GUI Animation Effects Against Design-Don’t Guidelines (paper)
>  
背景： **GUI アニメーションのテスト** について、既存の静的コード解析、機能的なGUIテスト、GUI画像比較手法ではできない（アニメーションが表示されないだめ）  
提案手法： アニメーションの multi-class screencast classification task として定式化（学習させる）、教材としてオートエンコーダを利用（通常、GUIアニメーションにラベルはつかないため）  
実装：  
実験： モデルの学習能力、GUIアニメーションのリンティングの有効性と実用性の調査

Tag_GUI, Tag_Animation, Tag_Test, Tag_Learning  
Tag_S_Test

アニメーションテストの自動化という挑戦的な題材、画像処理の流行の一つか？
___

- Title: Fuzz Testing based Data Augmentation to Improve Robustness of Deep Neural Networks (paper)
>  
背景： **DNNにはデータの些細な差に弱い（過学習と似ている）** 。 テスト生成技術の最近 → 目的のプログラム動作の仕様強化に採用  
提案手法： DNNのトレーニング用にソフトウェアのテストデータをFuzzingによって最適なものへと改善する SENSEI  
実装：  
実験： 5つの一般的な画像データセットにまたがる15のDNNモデルで評価 → Robustnessが平均5.5%向上

Tag_Learning, Tag_Fuzz, Tag_Test_Case_Gen  
Tag_S_Test

テストデータは少なく、過学習が起きやすい → Fuzzでデータ増やしてやればええやん！ ということ（多分ズレてる）？
___

- Title: <font color="orange"> Modeling and Ranking Flaky Tests at Apple </font> (practice)
>  
背景： Flaky Test が発生するとツライ（が、完全排除が難しいプロダクトもある） → 成功と失敗が安定しないのでテストパイプラインが狂う  
提案手法： **テストのflakinessをモデル化、定量化する2つの手法** → テスト結果のランダム性を検出  
実装：  
実験： Appleの2つのテストスイートにどのように影響したかを調査、テストのスコア範囲などからFlakinessがテスト全体にどのように分布しているか調査 → 2つのFlakinessの原因特定に利用できた、最終的にFlakinessを44%削減

Tag_Flaky, Tag_Test, Tag_Modeling, Tag_Random  
Tag_S_Test

Flaky再び。やはりランダム性などの非決定的となるテストを言っているっぽい。意外と面白そうな分野ではある
___

- Title: Testing File System Implementations on Layered Models (paper)
>  
背景：**高品質のシステムコールシーケンスを生成するのはファイルシステムの実装テストにも重要だが、一方で入力空間がめっちゃ大きいのでチャレンジング**。  
提案手法： Layered Model Checking → ファイルシステムのワークロード（シスコのシーケンス）Genとしてインスタンス化  
実装：  
実験： Linuxカーネルのファイルシステムで1000以上のクラッシュを引き起こすのに成功（うち12個は未知）

Tag_System_Call, Tag_Test, Tag_Model_Checking  
Tag_S_Test

システムコールシーケンスのワークロードってどう作るんだろ、各システムコールをモデリングしているのか、直接カーネル内部を叩くのか…
___

- Title: A Cost-efficient Approach to Building in Continuous Integration (paper)
>  
背景： **Continuous Integration (CI: 継続的インテグレーション) は広く使われているが、費用がかさむんじゃ** → GoogleとMozillaでは数百万ドル規模らしいっすよ  
提案手法： CIコストの削減アプローチ → 開発者がバグを早期に発見できるのが肝要 → 一連のビルドエラーについてビルドエラーを分割・予測する SmartBuildSkip（ビルド失敗を早期に発見）  
実装：  
実験： ビルドの節約（中央値30%）を達成

Tag_CI, Tag_Dev, Tag_Prediction  
Tag_S_Test

ビルドエラーは確かにつらいねんな。具体的な規模も出てきている（Googleとかの何%になるのかは知らんが）
___

</details>
</div>

## P16 Security and Learning

## <font color="orange"> P17 Software Development </font> (2 TPs, 2 Journals, 1 Demo)

<div class="waku">
<details open>

___

- Title: Improving the Pull Requests Review Process Using Learning-to-rank Algorithms (journal)
>  
背景： Githubのような開発プラットフォームにおいて、プルリクエストを介して貢献できることが多い → **が、そのプルリクエストをレビューする人へのサポートが足りとらん（要レビューが増えすぎている）**  
提案手法： プルリクエストをランキングし、レビュアーへリコメンドする Learning-to-Rank (LtR) → 即マージ/即拒否できる可能性でランキング  
実装： 18のメトリクスを用いて LtRモデルを構築  
実験： 74のJavaプロジェクトで実証

Tag_Github, Tag_Review, Tag_Recommend, Tag_LtR, Tag_Learning  
Tag_S_Dev

プルリクエストについて学習を行う模様、（即適用/不適用をどう判定しているのかは気になるが、機械学習だとそこらへんがボケる気がする）
___

- Title: Understanding the motivations, challenges and needs of Blockchain software developers: a survey (journal)
>  
背景： ブロックチェーンソフトウェア（BCS）のプロジェクトはGitHubに8000以上あるけど、そのプロジェクトとContributorを調査した論文（研究）はない → **いまだにBCS周辺のニーズや課題が明らかになっていない**  
調査： 1604人のContributorへオンライン調査（回答156件） → BCS開発者の大半は非BCS開発の経験あり（主に金融関連）、BCSと非BCS開発はやや異なると感じている（アップグレードの困難さ、欠陥コストの高さなど）、また非BCSの開発支援ではBCS開発の要求を満たしきれていない

Tag_Block_Chain, Tag_Github, Tag_Survey, Tag_Dev  
Tag_S_Dev

すげえ規模の調査だと思った（小並感）。ブロックチェーン自体の研究はあってもその開発についての研究は少ないのか
___

- Title: Gap between Theory and Practice : An Empirical Study of Security Patches in Solidity (paper)
>  
背景： Ethereum BCプラットフォームの一つ。 学界がスマートコントラクタのセキュリティについて色々研究しているが、果たして実用的か？  
調査： **Solidity が言語の場合のセキュリティの安全性を調査** → 既知の脆弱性の多くについて未だパッチが充てられていない、かつ開発者も最新のコンパイラを使っていない（98%）ので脆弱である → Solidity開発者の誤りを特定

Tag_Ethereum, Tag_Smart_Contract, Tag_Solidity, Tag_Patch  
Tag_S_Dev

Solidityという言語があるのは初めて知った。未だにブロックチェーン回りでは理論と実践の間に乖離がある模様
___

- Title: A Tale from the Trenches: Cognitive Biases and Software Development (paper)
>  
背景： 認知バイアスはその後の行動に悪影響を与える可能性がある → **開発タスクでも認知バイアスは発生する** が、このときの影響についての調査は多くない  
調査： 開発おける認知バイアスの発生頻度、影響、対処の実践とツールについて調査

Tag_Human, Tag_Dev, Tag_Survey  
Tag_S_Dev

えらく文体が本じみている。認知バイアス：人間が自身の記憶などを都合よく解釈すること
___

</details>
</div>

## P18 OSS

## <font color="orange"> I13 Testing and Debugging 1 </font> (2 TPs, 2 Journals, 1 SEIP, 1 Demo)

<div class="waku">
<details open>

___

- Title: Learning-to-Rank vs Ranking-to-Learn: Strategies for Regression Testing in Continuous Integration (paper)
>  
背景： CIにおいてリグレッションテストは時間という制約がある → **テストスイートの中で優先順位をつける必要がある** → 機械学習ベースの手法（LtRとRtL）  
調査： 10個のアルゴリズムを紹介、 Apache Commons プロジェクトを用いて比較

Tag_CI, Tag_Regression, Tag_LtR, Tag_Test_Suite, Tag_Learning  
Tag_S_Test, Tag_S_Debug

RtLアゲイン。リグレッションテストをランキングするというのはまあわかる（具体的な手法はさっぱりだが）
___

- Title: Debugging Inputs
>  
背景： プログラムが入力の処理に失敗したとき、必要になるのは障害を起こしたコードとは限らない → **入力と障害の関連性も含めてデバッグする必要がある**  
提案手法： ddmax 入力データのどこが障害を引き起こすのか、また入力が通るように修復する汎用アルゴリズム（プログラム解析を必要としない）  
実装：  
実験： 入力ファイルの69%を修復、入力ごとに1分以内にデータの78%を回復

Tag_Localization, Tag_Input, Tag_Debug  
Tag_S_Test, Tag_S_Debug

解析不要と言っているが、入力と処理を対応づけているなら解析不要は無理では？ それともパスする入力から合成するのだろうか
___

- Title: Property-based Testing for LG Home Appliances using Accelerated Software-in-the-Loop Simulation (practice)
>  
背景： LG家電は多くの便利な機能が組み込まれているが制御ソフトウェアも複雑に → **検証するにしてもハードウェア依存が大きいので統合テストがツライ**  
提案手法： Software-in-the-Loop (SILS) を用いたプロパティベースのテストフレームワーク → ハードウェア完成前にソフトウェアのチェックが可能に  
実装：  
実験： 開発中の二つの製品に障害事例を発見（手動テストでは発見できんかった）、　リコールしていたら費用は数千万ドルにいくで

Tag_Hardware, Tag_Integration_Test, Tag_SILS, Tag_Test  
Tag_S_Test, Tag_S_Debug

LG：韓国のメーカーのことでいいのか（著者がそもそもLGの人やった） ハードウェアを開発するとしたモデムぐらいだが…
___

- Title: Predicting Software Defect Type using Concept-based Classification (journal)
>  
背景：欠陥の説明（自然言語）からソフトウェアの欠陥タイプを自動的に予測 → 欠陥管理プロセスが大幅にスピードアップ → 主なアプローチは教師あり学習だが、**分類子を学習するためのラベル付きデータの用意にはめっちゃ苦労すんねん**  
提案手法： レポートを Concept-based Classification (CBC: 概念ベースの分類) を用いて分類 → Wikipediaの記事を横断する概念空間の説明を投影 → 意味的類似性を計算（類似度から分類）  
実装：  
実験：トレーニング不要で精度を達成できた（F1値が63.16）

Tag_Learning, Tag_Natural, Tag_Prediction  
Tag_S_Test, Tag_S_Debug

障害タイプの粒度が気になるところ。ある意味混合テキストとは相反する論文？
___

- Title: <font color="orange"> The Art, Science, and Engineering of Fuzzing: A Survey </font> (journal)
>  
背景： Fuzzingは人気あるよね → コミュニティなんかも活発だが、**各ツールの重要な設定などは共有されていないし、用語も断片化している（統一されていない）** → 今後の研究の妨害になりかねない  
調査： 過去10年の主要な論文とGitHubで100スター以上のプロジェクトを調査、体系的な調査を実施

Tag_Fuzz, Tag_Survey, Tag_Github  
Tag_S_Test, Tag_S_Debug

確かにFuzz（記号実行の分野でも）の用語にはブレがある。個人的に興味深いサーベイ
___

</details>
</div>

## ~I15 Ecosystems 1~

## <font color="orange"> I16 Testing and Debugging 2 </font> (2 TPs, 4 Journals)

<div class="waku">
<details open>

___

- Title: Low-Overhead Deadlock Prediction (paper)
>  
背景： リリース後のプログラムでもデッドロックは起きる → **デッドロック検知器は動的オバヘが大きいので開発段階でしか適用できない** → エンドユーザ側でも使えるのが欲しい  
提案手法： AirLock 低オバヘのデッドロック予測器 → Reachability Graph を維持して少サイクルでデッドロックを検出  
実装：  
実験： 平均オバヘは3.5%（やるじゃない）、既存研究より3桁少ない → AFLと併用も可能だし、オンザフライでの使用にも適している

Tag_Graph, Tag_Efficiency, Tag_Logging  
Tag_S_Test, Tag_S_Debug

グラフ探索問題に帰結しているということなんだろうけど、対象言語が気になるところ
___

- Title: The Impact of Feature Reduction Techniques on Defect Prediction Models (journal)
>  
背景： 欠陥予測はソフトウェアの品質維持のための大事なタスク。 **既存研究の多くは学習の特徴数を少なくしている（特徴が多いと爆発する可能性が高くなる） → が、特徴の選択（選別）は欠陥予測へ影響があることが分かっている** → 大規模調査は未実施   
調査： 5つの教師あり学習モデルと5つの教師なし欠陥予測モデルのパフォーマンスと特徴削減の影響を調査、比較

Tag_Prediction, Tag_Learning, Tag_Survey  
Tag_S_Test, Tag_S_Debug

九大、京大の先生のサーベイ。欠陥予測はある意味運用・保守の1タスクだが…

curse of dimensionality (次元の呪い: 次元が増えると探索空間も指数的に増えること)
___

- Title: <font color="orange"> The Impact of Correlated Metrics on the Interpretation of Defect Models </font> (journal)
>  
背景： 欠陥モデルはソフトウェアの品質に関する経験的理論を構築するための分析モデル → **モデルから知識の抽出ができることも** → 最近は相関メトリクスが解釈へ影響を与える可能性についての懸念（が、未調査）  
調査： 14の公的に利用可能な欠陥データセットのケーススタディを調査 → 相関メトリクスの一貫性などへ影響がある

Tag_Modeling, Tag_Learning, Tag_Metrix, Tag_Survey  
Tag_S_Test, Tag_S_Debug

共同研究で参考になるかも？ 知識の抽出という点で。 論文はIEEEとかからじゃないと閲覧できない模様
___

# 6/26 (11 absts, total 75 absts)

- Title: The Impact of Mislabeled Changes by SZZ on Just-in-Time Defect Prediction (journal)
>  
背景： **JITの欠陥予測** は注目度高いで （SZZを利用） → 中のSZZがノイズによって影響を受ける → トレーニング用のラベルが誤ったものになる可能性  
調査： SZZ（とそのバリアント）の誤ラベルとその影響について調査、10のApache オープンソースプロジェクトじゃら126,526の変更を用いて学習 → SZZによって誤ったラベルが付くと、検査作業に悪影響が発生

Tag_JIT, Tag_Prediction, Tag_Learning, Tag_SZZ, Tag_Survey  
Tag_S_Test, Tag_S_Debug

SZZ: Sliwerski, Zimmermann, Zeller の頭文字（人名）、バグを引き起こす変更を識別する手法。 主にSZZのサーベイ。
___

- Title: Which Variables Should I Log? (journal)
>  
背景： 開発者はログをよく使うが、出力変数を厳密に決定するにはドメイン知識が必要なのでツライ  
提案手法： **学習によって開発者へロギング変数のリコメンドする手法**（他の予測タスクとは異なる課題が存在、アクセス可能な変数セットの識別など） → ロギングコードなしのスニペットを入力として、RNNで表現を学習→事前トレーニングの単語埋め込みも利用  
実装：  
実験： 9つのJavaプロジェクトで評価、MAPが0.84を超えている

Tag_Logging, Tag_Learning, Tag_Recommend  
Tag_S_Test, Tag_S_Debug

利用可能変数も識別しないといけないのは静的解析要素もあり面白そう。精度も高いので意外と実用的？
___

- Title: Understanding the Automated Parameter Optimization on Transfer Learning for Cross-Project Defect Prediction: An Empirical Study (paper)
>  
背景： Data-driven な欠陥予測は重要度が増加 → 対象プロジェクトからのデータだけでは学習が十分ではないことも多いので、他のプロジェクトからデータを持ってくることは多い (**CPDP: Cross-Project Defect Prediction**) → が、色々なCPDP技術の自動パラメータ最適化の影響については十分研究されていない  
調査： 62のCPDP手法の影響を調査、13は他の論文でも調査されているが、49は未調査 → 20以上のソフトウェアプロジェクトで欠陥予測モデルを構築 → 自動化パラメータ最適化は77%のCPDPでパフォーマンス改善に効果的、転移学習はCPDPの肝（費用対効果大）、既存の転移学習と分類手法の組み合わせで代替を見つけられてしまう（CPDPはまだ未成熟）

Tag_CPDP, Tag_Prediction, Tag_Learning, Tag_Survey  
Tag_S_Test, Tag_S_Debug

サーベイっぽい論文（TPだけど）。このセクション欠陥予測（＝機械学習の一応用先）がほとんどだったな…
___

</details>
</div>

## I17 Contracts and Analysis

## I18 APIs and Commits

## <font color="orange"> I19 Code Generation and Verification </font> (2 TPs, 2 SEIPs, 2 NIERs)

<div class="waku">
<details open>

___

- Title: <font color="orange"> Automatically Testing String Solvers </font> (paper)
>  
背景： SMTソルバは多くのテストとか検証の基礎となっているが、ソルバ自身が正確かどうかのチェックはどうするん？    
実験： 文字列ソルバの実装における健全性（解なしは真に解なし性質）と完全性（誤った解を出力しない性質）を調査。 **ある解が分かっている式を合成 → 解を維持する変換を適用し、ソルバへ食わせてみる** → 効果的な検証になったのでは？

Tag_Solver, Tag_Automation  
Tag_S_Code_Gen, Tag_S_Verification

ソルバの検証自体は面白い（が、学術的側面が大きい → 直接の実用性は薄め）。 そもそも取り上げた変換がソルバ内でも適用されていればソルバは解けてしまうのでは？
___

- Title: Co-Evolving Code with Evolving Metamodels (paper)
>  
背景： Metamodels はソフトウェア言語 (SL) 構築の土台 → コアAPIコードが生成され、それを用いた高度機能コードを開発者が記述、言語を強化する → 既存アプローチは SLとMetamodel が **共進化 (Co-Evolution)** するとき、コードはハブられる（？）  
提案手法： 半自動化共進化アプローチ

Tag_Modeling, Tag_Language, Tag_Automation  
Tag_S_Code_Gen, Tag_S_Verification

共進化: 生物学上は、ハチドリと蘭のような、形質が他種へ依存するように進化する現象のこと、SE的にはメタモデルとSLが互いに影響を与えながら発展することを表現しているのか？  どうにもコードの位置づけがよくわからん（SE的な共進化はよく使われる表現なのかも不明）
___

- Title: Rule-based Code Generation in Industrial Automation: Four Large-scale Case Studies applying the CAYENNE Method (practice)
>  
背景： 制御ソフトウェアの設計には手作業の部分が依然多く、コストが高いまま → **要件ドキュメントから制御ロジックの自動生成** が研究されているが、限定的なラボ環境でしか動作せん  
提案手法： CAYENNE ルールベースの制御ロジック生成手法  
実装：  
実験： 数千のセンサを備えた産業プラントに関して4つのケーススタディ → 必要な制御ロジックのうち70%以上を自動生成可能 → コストを最大21%削減可能

Tag_Dev, Tag_Natural, Tag_Control, Tag_Automation  
Tag_S_Code_Gen, Tag_S_Verification

制御ロジックの生成なので、プログラミング言語よりも上層（あるいは自然言語かも）の分野。 開発サポートという点ではもう片方のグループ的な視点
___

- Title: <font color="orange"> Understanding and Handling Alert Storm for Online Service Systems </font> (practice)
>  
背景： **サービスの障害発生時には、 Alert Storm と言われるアラートの乱立がログとして記録** される → このアラートの集団を解析するのにもコストがかかる  
提案手法： 大量のアラートデータを分析 → 調査結果から Alert Storm の検出と代表Alertの抽出を行う手法の提案  
実装：  
実験： Storm 検出については F値が 0.9 を超えた、 Stromの概要抽出によって調査するAlertを98%削減。 China EverBright Bank のサービス保守に適用（成功例）

Tag_Logging, Tag_Review, Tag_Natural  
Tag_S_Code_Gen, Tag_S_Verification

ログ解析という点ではワシのグループのテーマとして利用可能（ただし自然言語っぽい感じになりそう）。 具体的なアイデアが書かれていないのが気になるところ
___

</details>
</div>

## I20 Android Testing

## I21 Version Control and Programming

## <font color="orange"> I22 Testing </font> (4 TPs, 2 Demos)

<div class="waku">
<details open>

___

- Title: <font color="orange"> MemLock: Memory Usage Guided Fuzzing </font> (paper)
>  
背景： 制御できていないメモリ消費はメモリを食いつぶす可能性がある → **既存のカバレッジ志向では検出できない問題**  
提案手法： MemLock メモリ消費量をモデリングしたFuzzer、よりメモリ消費量が大きいパスを積極的に探索していく  
実装：  
実験： AFL, AFLFast, PerfFuzz, FairFuzz, QSYM などのFuzz手法と比較して、メモリ消費バグを大幅に検出 → 15の新規CVE登録

Tag_Fuzz, Tag_Memory, Tag_AFL, Tag_Modeling  
Tag_S_Test

4月にワシが読んだ論文。 CVE15個登録は何度聞いてもやべぇ。 ワシの修士論文との完全競合ではないが、実検体で検証できるかの重要性を再認識させられた論文
___

- Title: Symbolic Verification of Message Passing Interface Programs (paper)
>  
背景： メッセージパッシングはよく使われるパラダイム → が、**非決定性や非ブロッキング操作などのせいで Message Passing Interface (MPI) の検証はめっちゃ困難**  
提案手法： MPIプログラムを自動検証する初の手法 MPI-SV (MPI Symbolic Verifier) 記号実行とモデルチェッキングの相乗効果を期待  
実装：  
実験： 111の実際のMPI検証タスクで評価 → 記号実行オンリー（打ち切り1h）だと 61/111 だが MPI-SVだと 100/111 を検証可能 → プロパティ充足を平均19倍の高速化、プロパティ違反を平均5倍の高速化を達成

Tag_SymExe, Tag_Distribution, Tag_MPI, Tag_Model_Checking  
Tag_S_Test

モデルチェックのモデル構築のコストとかがどうなるのか気になるところ（最適なモデルを長時間使って作ったんじゃあるまいな？） メッセージパッシングみたいな分散システム関連キーワードも再勉強せにゃなぁ
___

- Title: <font color="orange"> SAVER: Scalable, Precise, and Safe Memory-Error Repair </font> (paper)
>  
背景： memory leak, double-free, use-after-free などのメモリエラーの修正はツライねんな → 自動プログラム修正技術が解決の可能性を秘めているがまだ不十分（特にスケーラビリティ）  
提案手法： SAVER C言語用のメモリエラー自動修正手法、 **Object Flow Graph という新しい静的プログラム表現** に基づく → 修正をOFGのラベル付け問題として定式化  
実装：  
実験： 産業レベルの静的バグファインダーと組み合わせて評価 → 報告されたエラーの75%は色々なOSS CプログラムについてSAVERで修復可能

Tag_Memory, Tag_Graph, Tag_APR  
Tag_S_Test

記号実行とかFuzzにおいてグラフ問題への落とし込みが一般的になってきている感がある（単純に解決へのいい影響も多いので）。 自動修正ということはコード生成もしているということ？
___

- Title: A Large-Scale Empirical Study on Vulnerability Distribution within Projects and the Lessons Learned (paper)
>  
背景： 脆弱性発見技術が進歩したので、報告される脆弱性はめっちゃ増えた → 分析データがそろってきたぞ！
調査： 自動クローラーと手作業で **代表的なOSSプロジェクトに関連する基地の脆弱性すべてを網羅するデータセットを収集** → ファイル、機能、脆弱性の種類なども分析    
提案手法： 調査に基づいた知見から静的解析と動的Fuzzを包含する脆弱性発見ソリューション  
実装：  
実験： ターゲットプロジェクトの10個のゼロデイ発見に役立った

Tag_Fuzz, Tag_Static_Analysis, Tag_Survey  
Tag_S_Test

サーベイそ側面もありそうな脆弱性解析論文。静的解析と動的Fuzzについては興味あり（動的Fuzzって何だ、静的Fuzzがあるのか？ …まあ記号実行もFuzzの一部っちゃ一部だから静的Fuzzもあるか）
___

</details>
</div>

## I23 Code Artifact Analysis

# 6/30 (18 absts, total 93 absts)

## <font color="orange"> A21 Testing and Debugging 3 </font> (2 TPs, 4 Journals)

<div class="waku">
<details open>

___

- Title: Schrödinger's Security: Opening the Box on App Developers' Security Rationale (paper)
>  
背景： モバイルアプリのセキュリティ障害がなぜ混入するのか（**開発者は何故障害を起こすような導入を決定するのか**） → 開発環境の多様化と増加で複雑に → 必要なサポートを受けられていないのでは？  
調査： 1. 六つのタスク（セットアップ、コードレビュー、ヘルプ検索、テスターの募集、広告の選択、SDKの選択）について44人の開発者への調査 → 開発者はセキュリティを重視するが、コードタスクのみその重視基準は「セキュリティだから外部SDKを利用」のみ → セキュリティコード内部で何が起きているか把握していない？ 2. 274人について幅広い調査 → 多くの開発者がセキュリティに影響する行動についてそのリスクを無視

Tag_Human, Tag_Dev, Tag_Survey  
Tag_S_Test, Tag_S_Debug

セキュリティリスクの混入原因を開発者の慣習などから調査。セキュリティは外部ライブラリに頼ればおｋ、という認識が広がっている、らしい
___

- Title: <font color="orange"> Smart Greybox Fuzzing </font> (journal)
>  
背景： CGF (Coverage-based Greybox Fuzzing) はいいアプローチ。 その入力（シードファイル、ファイル）をビットシーケンスとして表現することが多い → **複雑なファイル形式だと、 Random Bit Flip が有効になることはほぼない**  
提案手法： Smart Greybox Fuzzing (SGF) 新しい入力ドメインを探索できるように、ファイルをビット列ではなく、仮想ファイル構造での入力形式を定義  
実装： AFL  
実験： AFLとの比較、ブランチカバレッジの大きな改善（最大+87%）。 42のゼロデイ検出、22のCVE登録を達成

Tag_Fuzz, Tag_Random, Tag_AFL, Tag_Input  
Tag_S_Test, Tag_S_Debug

突然変異形式はいいアプローチだが、適用できないところがあった → 適用できるように入力形式をひねってみたぜ、ということ？ にしてもとんでもねぇ成果だなおい。 個人的に興味あり
___

- Title: Deep Transfer Bug Localization (journal)
>  
背景： 多くのプロジェクトで、処理できない数のバグレポートが来る → サポートが必要 → **バグのLocalizationが有効な一手だが、多くの環境でバグデータが不十分**  
提案手法： TRANP-CNN セマンティックの抽出、ターゲットプロジェクトのラベル付きデータを活用、プロジェクト間のバグを効率的にLocalize  
実装：  
実験： ほかのDL手法と比較して精度指標が大幅に改善

Tag_Localization, Tag_Debug, Tag_Review, Tag_Learning  
Tag_S_Test, Tag_S_Debug

セマンティックの抽出が肝っぽいが、詳しいことは不明。
___

- Title: A Benchmark-Based Evaluation of Search-Based Crash Reproduction (journal)
>  
背景： クラッシュの再現はそのテストケースを生成することでデバッグを支援 → **自動化手法がいくつかあるでよ → 検体がバラバラなので比較ができぬ**  
提案手法： JCrashPack, ExRunner クラッシュ再現用のベンチマークと手法評価の実行ツール。 ベンチには様々なJavaプロジェクトの200のスタックトレースが包含  
実装：  
実験： EvoCrashの評価を実行 → EvoCrashは43%を再現成功。 再現しやすさ： ヌルぽ、引数例外、状態例外 > キャスト例外、範囲外配列、範囲外文字列

Tag_Debug, Tag_Test_Case_Gen, Tag_Metrix  
Tag_S_Test, Tag_S_Debug

クラッシュ再現も保守の点ではいいテーマ。 この論文はあくまで手法の評価の手法。
___

- Title: An Investigation of Cross-Project Learning in Online Just-In-Time Software Defect Prediction (paper)
>  
背景： JIT Software Defect Prediction (JIT-SDP) には十分な量のトレーニングデータが必要 → Cross Project 手法では他のプロジェクトから学習してきて、 Within Project の予測に対処する → **ソフトウェア変更による変化を続けていった場合にも対応できるのか** は未調査  
調査： 3つのCP JIT-SDP手法について調査。 3つのプロジェクトから2048のコミットを収集、学習に10個のOSSプロジェクトから198,468のコミットを利用 → CPデータは時間経過によって発生する精度低下を大幅に軽減

Tag_CPDP, Tag_Prediction, Tag_Learning, Tag_Survey, Tag_JIT  
Tag_S_Test, Tag_S_Debug
___

- Title: An Empirical Study of the Long Duration of Continuous Integration Builds (journal)
>  
背景： CIはソフトウェア開発において迅速なビルドが可能 → 開発者の専従的になってしまう問題がある（特にビルド期間が長くなるほどオーバーヘッドが大きくなる） → **ビルドが長くなる背景はあいまい、要調査**  
調査： CIビルドの長期化に関連する特性を調査 → 一般的な要素（プロジェクトサイズ、チームサイズ、ビルド構成サイズ、テスト密度）だけではなく、ほかにも重要な要素があるでよ

Tag_CI, Tag_Human, Tag_Dev, Tag_Survey  
Tag_S_Test, Tag_S_Debug

結論が書かれていないではないか(´・ω・\`) なんだかんだCIも色々論じられているなー、という印象。 運用・保守という意味では同じ畑ではある
___

</details>
</div>

## ~A22 Cognition~

## A23 Requirements

## <font color="orange"> A24 Testing and Debugging 4 </font> (2 TPs, 2 Journals, 1 Demo, 2 NIERs)

<div class="waku">
<details open>

___

- Title: On Learning Meaningful Assert Statements for Unit Test Cases (paper)
>  
背景： テストは重要だが、開発者は開発全体の50%もテストに費やしている（多い） → テストの自動化は大きな関心 → **Assert Statement について品質と有用性が疑問視**  
提案手法： ATLAS (AuTomatic Learning of Assert Statement) Neural Machine Transition をベースとした Assert Statement の自動生成手法。 テストメソッドとテストメインメソッドからAssertを予測・テストメインメソッドの精度を評価  
実装：  
実験： Github上の 数千のテストメソッドで学習 → 予測の上位一つだと31%、上位5つだと50%でAssertが完全一致の予測を達成

Tag_Test, Tag_Assert, Tag_Automation, Tag_Prediction, Tag_Learning  
Tag_S_Test, Tag_S_Debug

Assertの自動生成（学習ベース）。シンプルな奴なら役立ちそう（まあ、精度は決して高くないような印象だが）
___

- Title: TRADER: Trace Divergence Analysis and Embedding Regulation for Debugging Recurrent Neural Networks (paper)
>  
背景： RNNはテキスト入力を数値にエンコードするが、そのエンコードは 3rd Party の事前トレーニングの単語埋め込みに依存 → **ヘンテコな単語があると精度に悪影響**  
提案手法： TRADER サンプルを正誤で区別し比較、モデル化 → 問題となる埋め込みがモデルのパフォーマンスに与える影響を自動的に診断→診断結果から埋め込みを強化/修復  
実装：  
実験： 実際のモデルとデータセットの精度を平均5.37%改善（RNN的には大幅な改善）

Tag_Learning, Tag_Natural, Tag_Modeling  
Tag_S_Test, Tag_S_Debug
___

- Title: Specification Patterns for Robotic Missions (journal)
>  
背景： ロボット開発においての大きな課題 → ロボソフトウェアのミッションとその自動推論を可能にする形式への変換 → 前者は一般に自然言語で記述されるため曖昧になる  
提案手法： **ミッション要件の定義サポートのためにロボ動作に焦点を当てた新規パターン** → 構築のために245の自然言語のミッション定義文を分析  
実装：  
実験： 12個のモデルセットでパターンをテスト → エラーは存在。 10個のミッション要件を仕様に変換、ロボ計画を生成 → シミュレーター上で仕様が要件と対応していることを確認

Tag_Robotics, Tag_Dev, Tag_Natural  
Tag_S_Test, Tag_S_Debug

開発工程におけるより上流の話ではある。（ただ、ロボティクスの話は知見がなさ過ぎて…）。どうしても自然言語関連だと（この論文は使ってなさそうだけど）機械学習っぽくなるのが辛み
___

- Title: ProXray: Protocol Model Learning and Guided Firmware Analysis (journal)
>  
背景： IoTデバイスはあちこちに存在 → デバイスはファームウェア内に通信プロトコル（USB/Bluetooth）を実装し、幅広い機能を実現 → プロトコルからファームウェアを分析するのは実装バグの検出にも不可欠 → プロトコルの複雑性のせいで分析の自動化に役立つ仕様はない → 手動でやらざるを得ない  
提案手法： ProXray **記号実行とプロトコル学習モデルの組み合わせ** → 道のファームウェア内もモデルからプロトコルを識別し、記号実行でそこへ導くことが可能  
実装：  
実験： ProXrayをUSBとBluetoothプロトコルへ適用 → ファーム内のプロトコル関連のターゲットへ高速に到達、当該範囲の高いカバレッジを達成

Tag_SymExe, Tag_Learning, Tag_Protocol, Tag_Automation  
Tag_S_Test, Tag_S_Debug

記号実行と機械学習の組み合わせは珍しい気がする。が、やっていること自体は誘導に機械学習を用いた、ということなので分離可能な要素同士ではある。
___

</details>
</div>

## <font color="orange"> P25 Fuzzing </font> (5 TPs)


<div class="waku">
<details open>

___

- Title: Typestate-Guided Fuzzer for Discovering Use-after-Free Vulnerabilities (paper)
>  
背景： **カバレッジベースのFuzzingでは見つけにくバグにはUaFがあるでよ** → カバレッジではなく、ブランチ（エッジ）の順番が重要なため  
提案手法： UAFL typestate ベースのFuzzer → 静的 typestate解析とFuzzingでStaging（絞る→実行）  
実装：  
実験： AFL, AFLFast, FairFuzz, MOpt, Angora, QSYM と比較してUAF発見にかかる時間を大幅に改善。 未知の10個の脆弱性の発見、5つのCVE登録

Tag_Fuzz, Tag_AFL, Tag_Pointer, Tag_Anti_Covarage, Tag_Typestate  
Tag_S_Fuzz, Tag_S_Test

UAFに特化したFuzzer。 typestate automaton と同系の手法（各変数の持つ状態・遷移をオートマトンで表現する手法）と思われる。 問題はCFGが多分intraなのと、オートマトンをどう構築しているのかという部分
___

- Title: sFuzz: An Efficient Adaptive Fuzzer for Solidity Smart Contracts (paper)
>  
背景： **Smart Contrast はリリースすると簡単にパッチ当てができない → リリース前に徹底的にテストすべし**  
提案手法： sFuzz Ethereum 上での Smart Contrast 用のFuzzer。 よりブランチカバレッジを得られるように色々な戦略を採用   
実装：  
実験： Solidity 用の他のFuzzerと比較して効率的（2桁程度高速）、高いカバレッジを達成、採用戦略が補完的

Tag_Fuzz, Tag_Smart_Contract, Tag_Ethereum, Tag_Solidity, Tag_Test  
Tag_S_Fuzz, Tag_S_Test

Solidityで動作するFuzzer。Smart Contract固有の特徴とかを利用しているのかが気になるところ。指標がカバレッジなので、メモリ関連には弱そう
___

- Title: Planning for Untangling: Predicting the Difficulty of Merge Conflicts (paper)
>  
背景： マージは破壊的な動作なので時には多くの人が関係することになる（結果、コストがかかる） → マージ解決のためのサポートはあまりない → **競合の予測ができればおいしい**  
調査： マージ競合の特性を調査、自動的な分類。 128のJavaプロジェクトで発生した6380の競合を分析、学習 → 困難さを予測することを確認（AUC 0.76）

Tag_Version, Tag_Prediction, Tag_Learning, Tag_Metrix  
Tag_S_Fuzz, Tag_S_Test

Fuzzing？ パット見Fuzzingとは直接関係なさそうに見える。 共同開発したことないからいまいち背景を呑み込めてない
___

- Title: Gang of Eight: A Defect Taxonomy for Infrastructure as Code Scripts (paper)
>  
背景： **Infrastructure-as-Code (IaC) Scripts** の欠陥は大規模システムの障害など、重大になる可能性 → 欠陥の分類が修正と防止に寄与する  
提案手法： 定性分析 (qualitative analysis) を通じた欠陥分類手法、8つのカテゴリ → OSSリポジトリから欠陥関連コミットを1448個収集、分析。 291のリポジトリから80425個のコミットを収集、分析しカテゴリを定量化  
実装：  
実験： 分類法を66人の開発者に同意できるか調査 → 冪等性（同じIaCスクリプトが複数回実行されると起きる欠陥）に最も同意

Tag_Version, Tag_IaC  
Tag_S_Fuzz, Tag_S_Test

Fuzzing？ こちらもぱっと見Fuzzingとは直接関係なさげ。 IaCというのは聞きなれないが、アプリケーションにおけるインフラ層で利用されるスクリプトとかと捉えればおｋ？
___

- Title: <font color="orange"> JVM Fuzzing for JIT-Induced Side-Channel Detection </font> (paper)
>  
背景： タイミングサイドチャネル防止のための研究はソースコードと紐づいていることが多い → **が、実際にはJITによってサイドチャネルが発生・導入されてしまうことがある**  
提案手法： Java JITコンパイラによって発生するサイドチャネル攻撃を自動検出するパターンの紹介。 JVM上の動作を探索（Fuzzing）する手法  
実装：  
実験： サイドチャネル検出の研究で利用された3つのデータセットで評価 → これまで安全とされてきたコードが実際にはJITによってサイドチャネル攻撃に脆弱 → 過去の4つのツールと矛盾

Tag_Timing_Leak, Tag_Fuzz, Tag_JIT  
Tag_S_Fuzz, Tag_S_Test

JVMでFuzzingするって結構すごいことなのでは？ タイミングサイドチャネル (Timing Side-Channel) 処理時間の差から暗号鍵などを推測する攻撃手法。
___

</details>
</div>

## P26 Deep Learning Testing and Debugging

## P27 Applications

## <font color="orange"> P28 Analysis and Verification </font> (3 TPs, 1 SEIP, 2 Demos)

<div class="waku">
<details open>

___

- Title: <font color="orange"> Tailoring Programs for Static Analysis via Program Transformation </font> (paper)
>  
背景： 静的解析は深く研究されてきた手法だが、どうしても近似が必要になり、誤検知が増える → 抑制のための手法があるが、これらは開発者のニーズとかみ合っていない（コメントの追加挿入など） → 洞察: 分析ユーザが持つ背景に関係なく分析に影響を与える能力 → **modify code → わずかな変更で誤検出が抑制可能だが、冗長なコードが発生したりする**  
提案手法： 汎用的な自動コード変更手法（プログラム変換） → 静的解析器自体はブラックボックス扱い  
実装：  
実験： C, Java, PHP をサポートする5つの静的解析器を実世界のプログラム (>800KLoc) で評価 → 長期的な複雑課題を回避するのに効果的

Tag_Language, Tag_Static_Analysis, Tag_Code_Transformation  
Tag_S_Analysis, Tag_S_Verification

要するに静的解析に適したコードへ変換する、ということだと理解。 静的解析なのでスケーラビリティが気になるところ
___

- Title: BCFA: Bespoke Control Flow Analysis for CFA at Scale (paper)
>  
背景： 多くのソースコード解析において、CFGに対して分析を行う、というのが一般的 → が、検査対象のCFGが数百万にもなると解析コストがバカにならない → **CFGのトラバーサル戦略が固定だとマッチしない（効率的なトラバースができない）CFGが出てくる**  
提案手法： Bespoke Control-Flow Analysis (BCFA) 制御フロー解析を用いて、各CFGに対して最も効率的なトラバース戦略を選択  
実装： Boa  
実験： 287,000 と 162.000,000 のCFGを持つ二つのデータセットについて解析を実施 → BCFAは1-28%程度の高速化が可能、オーバーヘッドも0.2%未満と低い、予測ミスは0.01%未満

Tag_CFG, Tag_Static_Analysis, Tag_Efficiency  
Tag_S_Analysis, Tag_S_Verification

気になるのはCFGの探索（トラバース）戦略の選択肢ってそこまで多くなくね？ということ。 トラバース戦略を切り替えるというのはいいが、選択肢が少ないならミスマッチ率が低いのは全然意外じゃない
___

- Title: On the Recall of Static Call Graph Construction in Practice (paper)
>  
背景： 静的解析では動的要素のモデリングが必要になるが、これが難しいねんな → **難しいってのは議論されているが、どんな影響を与えるのかは未調査**  
調査： 実世界のJavaプログラムのセットを用いてCall Graph構築する上での問題を調査、どの言語機能がFNにつながるのか調査 → recall の中央値は0.884, 組み込みテストの方が合成テストよりも動作モデルを明確にする、追加の静的解析はあまり影響しない、動的要素をサポートしているかでrecallが大きく向上する（が、パフォーマンスは低下する）、unsoundnessの主な原因はreflected methodではなく、JVMから始まるmethod call

Tag_Static_Analysis, Tag_Modeling, Tag_Survey  
Tag_S_Analysis, Tag_S_Verification

あくまでCall Graph構築を一例に調査した、ということなんだろうけど、得られた知見がCG構築の例と近すぎて、一般化できてなくない？ ほかの静的解析で得られる知見は全然異なる可能性が高いし
___

# 7/1 (11 absts, total 104 absts)

- Title: DeCaf: Diagnosing & Triaging Performance Issues in Large-Scale Cloud Services (practice)
>  
背景： 大規模クラウドサービスはパフォーマンス追跡・監視のために KPIs (Key Performance Indicators) を利用する。 **コードバグなどで達成度合いの遅れ（つまり、KPIの低下）が生じた場合に、問題の切り分け・診断（とそのコストの最小化）が重要 → 問題には色々関連性が波及しているので自動/手動の診断は簡単ではない**  
提案手法： DeCaf KPI問題の自動診断とトリアージ（問題の切り分けと優先順位付け）システム → 機械学習とパターンマイニングを利用  
実装：  
実験： Microsoftの2つの大規模クラウドシステム上の10個の既知の問題と31個の未知の問題を診断したケーススタディ → 大量のサービスログとその属性付けがスケール、多種なKPIとランキング関数をサポート

Tag_Learning, Tag_Cloud, Tag_Dev, Tag_Logging  
Tag_S_Analysis, Tag_S_Verification

ジャンルとしてはClassificationに当たるのか？
___

</details>
</div>

## P29 Android and Web Testing

## ~P30 Ecosystems 2~

## A25 Android Testing

## <font color="orange"> A26 Bugs and Repair </font> (2 TPs, 4 Journals)

<div class="waku">
<details open>

___

- Title: Simulee: Detecting CUDA Synchronization Bugs via Memory-Access Modeling (paper)
>  
背景： **CUDAの同期バグの効率的な検出は未解決問題**  
提案手法： Simulee CUDA同期バグ検出フレームワーク → LLVMを解析、メモリアクセス情報を収集  
実装：  
実験： Github上の7つのCUDA関連プロジェクトでベンチマークを構築・評価。 手動で識別済みの24のバグのうち21個を検出、未知のバグ24個を検出（10個は確認済み）

Tag_CUDA, Tag_Efficiency, Tag_Debug, Tag_Sync  
Tag_S_Debug, Tag_S_Repair

同期（raceとか）問題のフレームワーク。 同期だから動的かと思っていたが、LLVMコードを解析しているあたり静的要素のほうが強いか？
___

- Title: Fine-Grained Dynamic Resource Allocation for Big-Data Applications (journal)
>  
背景： ビッグデータアプリでは大規模な並列計算を実行 → **有名なフレームワークとしてSparkがあるが、Sparkはリソース設定(allocation)を動的に変更できない → アプリの応答時間がデッドラインを超える一因になりえる**  
提案手法： dynaSpark Sparkのリソース拡張版 → アプリケーションへ割り当てる (allocate) リソースを動的に決定 → 不測の事態に対処しやすい → 高速なリソース割り当てにチャレンジ  
実装：  
実験： リソースの効率的な割り当てと、動的割り当てによる期限違反 (deadline violations) の減少を確認

Tag_Distribution, Tag_Spark, Tag_Resource  
Tag_S_Debug, Tag_S_Repair

リソース分配を可能にした並列アプリの拡張。 最初はタスク割り当てのバランシングかとも思ったが違う模様。 Sparkというのは有名なんだろうか。
___

- Title: The Assessor's Dilemma: Improving Bug Repair via Empirical Game Theory (journal)
>  
背景： 優先インフレが発生すると、開発者の時間割り当てが狂うことになる  
提案手法： **Empirical Game-Theoretic Analysis (EGTA: 経験的ゲーム理論解析) をSEの優先インフレへ適用** → 新しい優先順位付けプロセス  
実装：  
実験： （真に）優先度の高いタスクの97%が解決できるようになった

Tag_Human, Tag_Review, Tag_Learning, Tag_Game_Theory, Tag_Dev  
Tag_S_Debug, Tag_S_Repair

Priority Inflation: 例えば報告するバグに優先順位を付ける環境下で、多くのバグが報告されると優先度の高いバグしか参照されなくなり（結果的に低優先度は無視されるのが嫌で）、どんなバグでも優先度が高く報告されてしまう状況など。  
どのようにゲーム理論を適用したのかは分らぬまま（ある意味開発者心理学に近い、のか？）。 あと何で評価したのかAbstではサッパリわからん
___

- Title: FixMiner: Mining Relevant Fix Patterns for Automated Program Repair (journal)
>  
背景： パッチ自体は何度も適用されるので再利用性が抽出できて自動修復に導ければ嬉しい → **が、類似性抽出などと自動プログラム修正技術は、入力の扱いなどで組み合わせがムズイ**  
提案手法： FizMiner パターンマイニングを利用して他のパッチ生成システムで活用できるパッチ修正パターンの推測 → コード変更のASTやらスクリプトのツリー情報などを利用  
実装：  
実験： OSSプロジェクトから数千のパッチを収集・評価 → 正確なマイニングができてそう。 Defect4Jの26個のバグも修正できるようになったでよ

Tag_Learning, Tag_Patch, Tag_Automation, Tag_Defect4J, Tag_APR  
Tag_S_Debug, Tag_S_Repair

あくまで論点はAPRの入力とできるようなパッチパターンのマイニング。が、後々パッチ生成もしている（こんがらがってきた）
___

- Title: IntRepair: Informed Repairing of Integer Overflows (journal)
>  
背景： 整数オーバーフローは長年の脅威  
提案手法： IntRepair **Cコードのオーバーフローを静的記号実行を用いて検出・自動修復する手法**  
実装：  
実験： SAMATE Juliet テストスイートの2052個のCコード（計100万行ぐらい）と最大20KLoCの合成ベンチ50個に適用 → 正常に検知・修正、コードサイズの増加は1%程度。 30人のユーザ調査も実施 → 手動の10倍以上の効率

Tag_SymExe, Tag_Overflow, Tag_Repair, Tag_Automation  
Tag_S_Debug, Tag_S_Repair

検出と自動修正ができるのはオーバーフローの修正がパターン化されているため？ 懸念はどれだけ偽陽性があるのかってところくらい（疑わしきは修正のはず） → エラーハンドリングも組み込んでいるなら修正済みコードが動的パフォーマンスの悪化を招かないか？
___

- Title: DLFix: Context-based Code Transformation Learning for Automated Program Repair (paper)
>  
背景： APRは開発と保守において役立つ → **DLは発達してきたが、APRの場合、バグ修正のコード周辺のコンテキストの学習が制限** → 修正位置・修正が不正確に  
提案手法： DLFix 以前のバグ修正とその*周辺コンテキスト*から学習する2層DLモデル（1st層はコンテキスト学習のRNN、2nd層はコード変化の学習モデル）  
実装：  
実験： Defect4J、Bugs.jar、8つのプロジェクトのそれぞれのデータセットで評価（vs 13 state-of-the-art Approaches） → 11/13 で改善、他二つとは互いに補完的な結果

Tag_Learning, Tag_Defect4J, Tag_Repair, Tag_APR  
Tag_S_Debug, Tag_S_Repair

バグ修正だけでなく、周辺のコンテキストも学習するようにした（できるようにした？）のが新規性。 度々出てくるDefect4Jはどういうバグが内包されてんだろ？
___

</details>
</div>

## A27 Software Architecture

## A28 Android and Web Testing

## <font color="orange"> A29 Code Analysis and Verification </font> (4 TPs, 1 NIER)

<div class="waku">
<details open>

___


- Title: Heaps'n Leaks: How Heap Snapshots Improve Android Taint Analysis (paper)
>  
背景： フロー解析はAndroidアプリの分析に不可欠 → 静的汚染解析が多め → ポインタの指し先が不正確になりがちで、悪影響あり → **動的ヒープスナップショットを補助的に与えると解析精度が向上することが分かってきた**  
調査： 動的ヒープスナップショットが静的汚染解析にも効果（健全性と精度について）があるのかを調査 → FlowDroidを拡張、ヒープスナップショットを組み込み → DroidMacroBenchで評価 → 精度向上と解析の効率化を観測、精度と健全性トレードオフが優良化、単一スナップショットよりも複数スナップショットの方がトレードオフがより優れる

Tag_Android, Tag_Taint_Analysis, Tag_Pointer, Tag_Heap, Tag_Survey  
Tag_S_Analysis, Tag_S_Verification

静的解析に動的解析の補助を入れるという比較的直観通りの調査（ヒープ内部のオブジェクト情報を与えることで、ヒープを分化できる）。 スナップショットを取るオーバーヘッドは、まあ静的解析全体から見れば誤さっちゃ誤差か
___

- Title: Verifying Object Construction (paper)
>  
背景： オブジェクト指向言語において、コンストラクタ記述の手間を減らすためにデザインパターンを利用することが多いが、**コンパイラによって隠蔽されてしまう部分が発生 → 結果的にセキュリティホールを見落としたり、クラッシュの原因を見落としたりする可能性**  
提案手法： ビルダーパターンなどのオブジェクト構築の利用を静的に検証する手法 → 仕様言語から引数の組み合わせを指定 → 非安全なコンストラクタ構築コードを検出  
実装：  
実験： （計？）900万行以上のコードで評価 → 未知のセキュリティ脆弱性やヌルぽ違反などを発見、FPは少なめでアノテーションの負担も低い

Tag_Typestate, Tag_Language, Tag_Design_Pattern  
Tag_S_Analysis, Tag_S_Verification

実際コンストラクタの記述って手間、ここは同意。で、要するに提案しているのもアノテーションベースなわけだが、注釈の負担が少ないは本当か？
___

- Title: When APIs are Intentionally Bypassed: An Exploratory Study of API Workarounds (paper)
>  
背景： APIは色んな所で使われているわけだが、時に様々な要素によってAPIを切り替えるユーザが存在 → このとき、ユーザと開発者の間にチャンネルがない → **ユーザがコミュニティなどで問い合わせた内容が開発者へ共有されないこともある（バグの見逃しにもつながる）**  
提案手法： Stack OverflowでのAPI回避策の問い合わせについての解説（8つの一般的な問い合わせ理由と8つの解決策） → 得られた知見からAPI回避策の実装パターンの開発  
実装：  
実験： 現実世界のOSSプロジェクトでパターンのインスタンスを特定、API開発者視点での価値を判断

Tag_StackOverflow, Tag_API, Tag_Dev, Tag_Human  
Tag_S_Analysis, Tag_S_Verification

API Workarounds はここでは何かしら不都合のあるAPIの使用を避ける方法（もしくは代替法）のことを指している、でいいのか。 パターンマイニングしているのかは不明
___

- Title: <font color="orange"> Demystify Official API Usage Directives with Crowdsourced API Misuse Scenarios, Erroneous Code Examples and Patches </font> (paper)
>  
背景： APIにはそれの使い方やらを記載した **自然言語のガイドラインなどが存在 → が、APIの誤用シナリオの原因が存在** → 使用コンテキストの欠如、協調APIとの関係、API間の微妙な違い  
提案手法： Stack Overflow でのAPI誤用シナリオを発見するテキストマイニング手法 → 誤ったコード例、パッチ、関連APIの抽出を行い、自然言語のレポートを生成  
実装：  
実験： 3148クラス、7471メソッド → 23969 API使用 → 159116 誤用シナリオを抽出。 8つのAPI誤用に関するレポートを14人の開発者へ見せ調査 → 誤用エラーを高速かつ正確に把握できた

Tag_Learning, Tag_Natural, Tag_API  
Tag_S_Analysis, Tag_S_Verification

提案したのは、誤用シナリオを発見するテキストマイニング（つまり、学習の結果、誤用か否かを判断している）。 マニュアルやガイドラインもある意味開発の重要な要素ではある。
___

</details>
</div>

## A30 Dependencies and Configuration


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
