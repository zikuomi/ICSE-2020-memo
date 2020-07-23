# Tailoring Programs for Static Analysis via Program Transformation [ICSE 2020]
https://rijnard.com/pdfs/tailoring-analysis-icse-2020.pdf

## Abstract
Static analysis is a proven technique for catching bugs during software development. However, analysis tooling must approximate, both theoretically and in the interest of practicality. False positives are a pervading manifestation of such approximations—tool configuration and customization is therefore crucial for usability and directing analysis behavior. To suppress false positives, developers readily disable bug checks or insert comments that suppress spurious bug reports. Existing work shows that these mechanisms fall short of developer needs and present a significant pain point for using or adopting analyses. We draw on the insight that an analysis user always has one notable ability to influence analysis behavior regardless of analyzer options and implementation: ***modifying their program.*** We present a new technique for automated, generic, and temporary code changes that tailor to suppress spurious analysis errors. **We adopt a rule-based approach where simple, declarative templates describe general syntactic changes for code patterns that are known to be problematic for the analyzer. Our technique promotes program transformation as a general primitive for improving the fidelity of analysis reports (we treat any given analyzer as a black box).** We evaluate using five different static analyzers supporting three different languages (C, Java, and PHP) on large, real world programs (up to 800KLOC). We show that our approach is effective in sidestepping long-standing and complex issues in analysis implementations.

## 1 Introduction

Writing and maintaining high-quality, bug-free software remains a largely manual and expensive process. **Static analysis has proven indispensable in software quality assurance for automatically catching bugs early in the development process [31].** A number of open challenges underlie successful adoption and integration of static analyses in practice. Analyses must approximate, both theoretically [22, 25] and in the interest of practicality [9, 11]. Developers are sensitive to whether tools integrate seamlessly into their existing workflows [21]; at minimum analyses must be fast, surface few false positives, and provide mechanisms to suppress warnings [10]. **The problem is that the choices that tools make toward these ends are broadly generic, and the divergence between tool assumptions and program reality (i.e., language features or quality concerns) can lead to unhelpful, overwhelming, or incorrect output [20].**

静的解析は早期のバグ検出に効果的（不可欠[31]）。 しかし実際の静的解析の採用には未解決問題あり → 近似（理論的要素と実用的要素） → 高速化するが誤検知が表面化 <br>
問題点: ツールの想定と実際のコードの間に乖離があるので役に立たなくなる（誤検出オンパレード）

Tool configuration and customization is crucial for usability and directing analysis behavior. **The inability to easily and selectively disable analysis checks and suppress warnings presents a significant pain point for analysis users [10].** Common existing mechanisms include analysis options for turning off entire bug classes (generally too coarse [21]) or adding comment-like annotations that suppress spurious warnings at particular lines (a predominantly manual exercise that leads to code smells and is insufficiently granular [10, 21]). **It is notably the analysis author, not the user, who has agency over the shape of these analysis knobs: which configuration options are available and how to suppress errors. It follows that it is infeasible for analysis writers to foresee and accommodate individual user preferences or analysis corner cases through a myriad of configuration options or suppression mechanisms.**

解析ツールのカスタマイズは重要だが、解析を選択的に行えないとユーザにはツライ。 しかも選択できても粒度が粗かったり（クラス単位とか）、Code Smell につながる注釈とかだったり。 <br>
エラーの抑制方法を提示するのはツール作成者であってユーザではない → 全ユーザから気に入られる構成になんかできっこない

**An analysis user always has one notable ability to influence analysis behavior and output: modifying their program. For example, developers may slightly modify existing code in a way that suppresses false positives or undesired warnings.** A concrete example is the following change, which was made in rsyslog(\*1) to suppress a Clang Static Analyzer warning:

Modifying: 解析の動作や結果に影響を与える、ユーザ側から実行可能な処理（具体例あり、以下 Clang Static Analyzer の警告を抑制するために rsyslog で実践された例）

```c
- if(strcmp(rectype, "END")) {
--------------------------------------
+ // clang static analyzer work-around
+ const char *const const_END = "END";
+ if(strcmp(rectype, const_END))
```
<!-- \* -->

(\*1) https://github.com/rsyslog/rsyslog/commit/ea7497 <br>
説明: 変更前の場合、複雑なマクロ展開（二個目の引数がリテラルのためstrcmpへのマクロ展開がONになる）が走って out-of-bounds access の警告が出る（誤検知）。 変更後は変数を介することでマクロ展開、警告を抑制


The analyzer warns that a potential out-of-bounds access occurs when comparing two strings using strcmp. However, the warning only surfaces when complex macro expansions take place (in this case, macro expansion activates for strcmp because the second argument is a string literal). One contributor notes that under normal circumstances these warnings are suppressed for macros, but can surface if macro preprocessing is done manually.(\*2) The workaround in this case extracts the string literal out of the strcmp call so that no macro expansion takes place. After the change, the analyzer sees strcmp as a C library function and no longer emits a warning In practice, **modifying programs to avoid analyzer issues are exercised as manual, one-off changes.** Changes like the one above can have **the undesirable side-effect of persisting in the code solely to suppress unwanted analysis warnings.** Despite these shortcomings, modification of the analysis target (the program) does, however, allow a primitive for analysis users to draw on their particular domain knowledge of their code to positively influence analysis behavior. For example, a developer may recognize that a particular API call is the cause of a false positive resource leak and modify or model the call differently to suppress a false positive.(\*3) Our technique in particular affords agency to analysis users to change and suppress analysis behavior when no recourse is available in tool support (e.g., when analysis maintainers delay fixing analysis issues, or limit tool configuration options). **Our insight is that the same flavor of oneoff, manual code changes like the one above can apply generally and automatically to remedy analysis shortcomings. Additionally, workaround changes need not have the undesirable trait of persisting in production code, and are applied only temporarily while performing analysis.**

変更によって警告とか誤検出が抑制できるが、（例えば変数が増えるとか）コード変更の副作用が発生 → この変更は解析時の一回(one-off)のみ適用。 <br>
著者の洞察: この手の（解析の精度を向上させる）one-off変換は自動的に適用可能ではないか？

(\*2) https://bugs.llvm.org/show_bug.cgi?id=2014 <br>
(\*3) https://github.com/facebook/infer/issues/781

**We propose a rule-based approach where simple, declarative templates describe general syntactic changes for known problematic code patterns. Undesired warnings and false positives are thus removed during analysis by rewriting code fragments.** Our approach can be seen as a preprocessing step that tailors programs for analysis using lightweight syntactic changes. It operates on the basis of temporary suppression (a desirable trait in configuring analyses [21]) and also enables catching false positives before they happen by rewriting problematic patterns. **Since patterns can occur across projects, codifying transformations as reusable templates amortizes developer effort for suppressing false positives.**

提案手法: ***既知の問題のあるコードパターンの構文を自動で変更するルールベース。 不要な警告と誤検知を抑制 ＆ パターンをテンプレートとして再利用できるようにすることで、開発者(解析ユーザ)の抑制処理のコストを償却***

The notion that syntactic transformations abstract semantic transformations [13] underpins our intuition that **manipulating syntax can achieve desirable changes in the analysis domain and implementation.** Work on semantic properties of transformations emphasize the potential for improving analysis precision [26, 28], and **recent work suggests that automatic bug-fixing transformations can improve analysis results in popular real-world programs [33].** Despite anecdotal and theoretical appeal for tailoring analyses via transformation, there is currently little demonstrated applicability or benefit in practice.

構文操作によって分析ドメインと実装に好ましい変化を与えられる。 最近の研究では自動バグ修正変換で実世界のプログラムで解析結果を改善できることが示されている[31]。

A key objective of our work is to demonstrate the broad feasibility, applicability, and effectiveness of these ideas for the first time in practice. To this end we evaluate on large, **real-world programs written in a variety of popular languages.** **A significant challenge lies in recognizing and transforming syntactically-diverse languages for such an approach to work.** Recent techniques in declarative syntax transformation help to address this challenge [34] and forms the basis for operationalizing our approach. We address analysis issues broadly by considering (a) user-reported false positives across multiple active analyzers and (b) historic user commits for suppressing analysis warnings. We develop transformations that tailor programs to address shortcomings in analysis implementation and reasoning. Our contributions are as follows:

- We operationalize the **process of tailoring programs for static analysis using declarative syntax rewriting**.
- We show that our approach is **effective** in improving existing static analyses and **resolves real (including yet-unresolved) false positive issues affecting analysis users**.
- We show that our approach is **efficient**: transformation typically **takes one to three seconds (compared to analyses that typically take in the order of minutes)**
- We present empirical results of our approach on 15 realworld projects (including large ones, >100KLOC) across three languages (PHP, Java, and C) and develop 9 transformation templates for improving the analysis output of five modern and active analyzers (Clang, Infer, PHPStan, SpotBugs, and Codesonar).

こういったプログラム変換の有効性は評価されてこなかった → 様々な言語で書かれたプログラムで評価（多様な言語構造を認識することは重要）。<br>
貢献: 宣言型構文の静的解析に適した書き換え。改善に効果的であることの評価、効率的であることの評価（静的解析が数分かかるのに対して変換は１～３秒程度）、3つの言語（PHP, Java, C）の15の実世界プロジェクトに対して評価・5つの解析器（Clang, Infer, PHPStan, SpotBugs, CodeSonar）の精度を比較

## 2 Motivation

Fig. 2a illustrates a past issue in PHPStan, a popular PHP analyzer. A file is opened in line 2, and assigned to a handle `$file` inside the if-condition on line 2. If opening the file fails, `$file` is assigned the value `false`; the condition evaluates to `true` and the function immediately returns (line 3). On the other hand, if execution passes the check then `$file` is guaranteed to be valid (i.e., not `false`). The problem is that PHPStan would not track the effect of assignments in if-conditions, and reports an error saying that `$file` may be `false` when passed as an argument to `fputcsv` on line 7.

```php
function test () : void {
  if (( $file = @fopen ('file ', 'wb+') ) === false ) { //l2: open file (PHPStan would't consider the effect of assignments)
    return ;
  }

  // analyzer complains that $file may be false
  if (\fputcsv ($file , [1 ,2 ,3]) === false ) {
    \fclose ( $file ) ;
    return ;
  }
  ...
  \fclose ( $file ) ;
}
```
Fig 2a Code<br>
PHPStanの誤検知の例。 2行目を通過して7行目に行く場合、代入処理を考慮できないので`$file`が`false`となる可能性を指摘してしまう。

The issue for this false positive stayed open and unresolved for over a year on GitHub, and is cross-referenced in 13 related user-reported issues.(\*4) One project member responded that the issue is important and will be fixed in the future, but that “no one has yet figured out how to implement it without rewriting major part of the (analysis) core”. **The fix imposed significant effort on analysis authors which delayed a resolution for months.** Although some analysis authors were in favor of code that avoided assignments in conditionals, others found the style improves readability. **Multiple users noted that the false positive can be avoided by a code change that extracts the assignment out of the conditional (Fig. 2b)**.(\*5) However, one user also noted that this workaround was “a bit annoying” because it introduced redundancy. The proposed workaround also **imposes a burden on the user to identify and refactor all affected instances**. Our approach introduces a new way to address these tensions. The intuition is that workarounds via code changes, as in Fig. 2b, can generalize to cater for individual user preferences and overcome long-standing analysis issues. The high level idea is to write simple, declarative syntactic templates that can blanket-apply automatically over an entire code base. Although the code changes could be persisted in the code, they need not be: our approach foremost promotes code changes as a temporary suppression mechanism with respect to a particular analysis. In our approach, a match template specifies a pattern that is syntactically close to the problematic pattern in Fig. 2a:

```php
function test () : void {
  $file = @fopen ('file ', 'wb+') ;
  if ( $file === false ) {
    return ;
  }
  ...
}
```
Fig 2b Code<br>
上記の誤検出を回避する一例。代入と条件式を分離した。

```php
if ((:[v] = :[fn](:[args]) ) === false )
```
上記問題(図2a)に対する検出テンプレート例

(\*4) https://github.com/phpstan/phpstan/issues/647 <br>
(\*5) https://github.com/phpstan/phpstan/issues/1739

PHPStanのこの問題は1年以上未解決だった（解析のコア部分の修正方法の模索に大きなコストが発生）。 コードの書き換えでこれが回避可能ではある（図2b）が、そのためにリファクタリングを強要するのか、という問題が発生。 → 本稿の提案手法につながる（解析用one-offリファクタリングの自動化）

A rewrite template replaces all instances of the match template, extracting the assignment out of the if-condition. The rewrite template is syntactically close to the pattern suggested by the user in Fig. 2b:

```php
:[v] = :[fn](:[args]) ;
if (:[v] === false )
```
上記問題(図2b)に対する書き換えテンプレート例

The match template matches on the syntactic pattern where **variable `v`** is assigned the return value of calling a **function `fn` with arguments `args`** (the **`:[identifier]` notation binds matching syntax to a variable identifier**). All other syntax is matched concretely; **whitespace in the template matches all contiguous whitespace in the source code**. For illustration we use this template to match on function call syntax because the analysis particularly tracks values for modeled functions like `fopen`. The rewrite template references variables in the match template, which are substituted with their corresponding matched syntax.

変数vや引数argsなどをパターンから検出（空白の連続も処理可能）

**Using the above patterns we identified and rewrote 27 instances of the if-assign pattern in the WordPress and PHPExcel projects and removed 82 false positives due to issue #647 in PHPStan.** Our approach has the positive effect of removing more false positives than matches, because **the issue has a cascading effect of reporting false positives along multiple execution paths** (we elaborate in Section 4).

実際にこのテンプレートで82の誤検知を抑制（しかもこの誤検知はカスケード≒他所へ伝播するので、他の誤検知の抑制にも効果的）

**Writing these declarative patterns is comparatively easy to implementing additional analysis reasoning and sufficiently general for overcoming analysis shortcomings.** The format of syntactic templates is accessible to developers; indeed templates can be written in a format that is syntactically close to user-identified and user-implemented workaround transformations (as in Fig. 2b). In our experience, complex changes to an analysis implementation can have a correspondingly easy resolution via code transformation patterns; **templates can be developed in minutes for issues that take days to months to resolve in an analysis implementation (or even issues that don’t have any proposed solution whatsoever).**(\*6)

(\*6) See, e.g., https://github.com/spotbugs/spotbugs/issues/493

宣言的にパターンを書くことは解析器に手を入れるより比較的簡単、分析の欠点を克服するのに十分に一般的。解析器の改良には数日から数か月かかるが、テンプレートなら数分やで

Match and rewrite templates appear simple, but **express nontrivial syntactic properties that go beyond the capabilities of regular expression search-and-replace**. We use recent work in syntax transformation to enable this approach for multiple languages and apply it broadly toward our objective.

一見テンプレートは簡単に見えるが、実際には正規表現などを超える簡単じゃない(nontrivial)構文プロパティを表現してる。

## 3 Approach

This section explains the overall process of our program tailoring approach. We introduce background on the rewrite technique in Section 3.1. Section 3.2 explains how we integrate program transformation for improving the quality of analyzer bug reports and the principles behind our approach.

### 3.1 Preliminaries: Lightweight Syntax Transformation

To rewrite syntax declaratively in the fashion shown, we use ***comby***,(\*7) a tool for declaratively rewriting syntax with templates. We give a brief overview of template syntax and matching behavior:

(\*7) https://comby.dev

下記構文はcombyの一致構文の流用（が、調べてみるとcombyの作者と本著者は同じ人 Tonder）

- The `:[hole]` syntax binds matched source code to an **identifier hole**. Holes match all characters (including whitespace) lazily up to its **suffix (analogous to `.\*?` in regex)**, but only within its level of **balanced delimiters**. For example, `{:[hole]}` will match all characters inside balanced braces. **Parentheses and square brackets are also treated as balanced delimiters**.

正規表現チックに識別子をバインド可能。レベルは balanced delimiters (カッコとか)で調整可能。

- `:[[hole]]` matches **only alphanumeric characters in source code**, analogous to \w+ in regex.
- Using the same identifier hole multiple times in a match template **adds the constraint that matched values be equal**.
- **All non-whitespace characters are matched verbatim**.
- **Any contiguous whitespace (spaces, newlines, tabs) in a match template matches any contiguous whitespace in the source code**. Match templates are thus sensitive to the presence of whitespace, but not the exact layout (the number of spaces do not need to correspond exactly between match template and source code).
- Matching is insensitive to comments in the source code; **comments are treated like whitespace when matching nonhole syntax in the match template**.

コメントは無視される

We additionally use rules to refine match and rewrite conditions. **Rules place constraints on matched syntax**; we explain rules as needed in the rest of the paper. The match template, rewrite template, and rule comprise the full input for a single transformation. **Each part is passed on the command-line**.

### 3.2 Tailoring Programs for Analysis

This section explains our approach to tailoring programs for analysis. Fig. 3 illustrates the main phases in our approach; we use it to characterize the process in detail.

**Who writes templates?**  
Our approach starts with a manual step where users 1 write transformation templates. A key principle of our approach is to allow particularly analysis users to influence static analyses via program transformation. The simplicity of declarative templates make syntax transformation accessible so that user observations and workarounds (as in Fig. 2b) can be straightforwardly implemented. This primitive is not exclusive to analysis users, but also accessible to analysis writers and external contributors. For example, analysis writers can create and distribute transformation templates for issues on an interim basis (such as for Fig. 2a). Users can then apply these templates as a temporary workaround before analyzing their projects. **In this paper, we (the authors) develop and evaluate transformations as external contributors—we treat analyses as a black box (as a user would) and develop transformations for extant issues in popular analyzers (as an analysis writer might).**

解析ユーザが変換テンプレートを記述することを想定。もちろん、解析器の開発者がテンプレートを作って配布するのもあり。本稿では著者が記述したテンプレートを用いて、解析器自体はブラックボックス扱いで評価。

**What properties do templates have?**  
Writing rewrite templates 2 is a one-off exercise per transformation. Templates are generally short (we develop templates that are between 3 and 15 lines long). **Our results show that there is typically a one-to-one correspondence of rewrite template to analysis issue, where an analysis issue can entail a general problem in analysis implementation, code generation, or function modeling.** Formulating rewrite templates requires competitively low effort compared to implementing complex changes analysis-side.

テンプレート自体は短い（3～15行程度）。一つのテンプレートは解析器の一つの問題（実装、コード生成、関数モデリングなど）と対応している。

Templates are customizable. For example, we can refine the template `if ((:[v] = :[fn](:[args])) === false)` to match on a particular concrete function, such as `fopen`, instead of `:[fn]`. Rudimentary mechanisms (e.g., C macros, comments, or assertions [16], and bug auto-patching [4]) have been used and recommended for suppressing false positives in existing analyzers [3, 6]. These mechanisms suffer from relying on language-specific features, are brittle and coarse, and built into the tool or hardcoded in the program. Our template-based mechanism is broadly accessible (it is easy to write templates), customizable, and generic across languages for manipulating syntax. **It operates independent of language toolchains and does not presume the availability of language-specific features (e.g., macros) and does not prescribe any configuration for external analysis tools or compilers.** Templates currently express purely syntactic properties (e.g., we do not draw on type information to inform manipulation). **However, future work may incorporate richer static information.**

テンプレートはカスタマイズ可能（例えば、`:[fn]`を`fopen`に具体化するとか）。現在は言語固有の機能（マクロなど）などとは独立している（コンパイラなどの規定がない）、が Future work として色々な静的情報を利用する可能性はある。

A set of templates form a catalog of transformations that can be reused across projects using analyses. A catalog is the starting point for an automated pipeline, driving the remaining phases in Fig. 3. **In this paper we present a catalog of human-written templates that directly targets known analysis issues.**

テンプレートのカタログは自動化の入り口となる。本稿では既知の分析問題を解消するテンプレのカタログを用意（人間が書いたもの）

**What are the principles behind automated code changes tailored to analysis?**  
The rewriter tool 3 takes as input the set of rewrite templates and a program. It rewrites matched syntax in the original program to produce a modified program that will be analyzed. **The modified program is intended to be a temporary representation of the original program that is discarded after analysis.** In practice users can identify transformational workarounds to issues (as in Section 2), but do **not want to persists those changes in their code**.(\*8) Our solution provides the ability where users **only temporarily change their code under analysis**. We implement temporary program modification by running the rewriter on a version controlled project (we use git). After running the analysis and capturing the bug reports, the project is reverted to its original state. **As a practical concern, program tailoring could be integrated into testing and continuous integration pipelines or local development workflows. We envision that the approach is better suited to continuous integration pipelines that typically integrate long-running analyses.**

(\*8) For example, one user user experience identifies that a certain change removes a false positive report, but they do not want to permanently change their code https://github.com/Microsoft/CodeContracts/issues/255

前述の通り、テンプレを用いた調整は解析にのみ利用され、解析後は破棄される（調整前のコードに戻る）。このプログラムの調整はテストはCIパイプラインに組み込み可能で、長期実行解析を組み込んでいるCIパイプラインよりも適していると想定。

In this paper we consider transformations primarily as a targeted suppression mechanism for analysis false positives. However, **we observe that transformations can also be tailored to surface additional bugs** (we elaborate in Section 4).

本稿の基本は偽陽性の抑制だが、追加されたバグを表面化させるようにも変換できるで（！）  
小泉: additional bugs がどういうコンテキストでの言葉なのかwktk

The rewriter can be seen **as a preprocessor for analyses**. One principle of running our approach prelude to analysis is that we can dually use templates to search for and detect (but not necessarily rewrite) problematic syntactic patterns. **This mechanism provides an early smoke signal that may trip particular analyzers even before the analysis runs**. We notably reuse our templates to detect false positive-inducing patterns across projects in our evaluation. Large scale efforts can similarly detect the extent of possibly affected projects when adopting a new analyzer; **analysis writers may use it to prioritize analysis fixes**. These capabilities are notable in contrasting our approach to existing mechanisms: syntactic templates are valuable in explicitly codifying sensitivities of analysis behavior as it relates to program structure, whereas suppression-by-comments and configuration options provide an escape hatch in anticipation of problematic analysis interactions where program structure is readily ignored.

調整テンプレの知見は、逆に誤検出を起こしそうなコードへ変換できるという意味でもある。この（逆）変換を利用して、解析器の開発者に誤検知箇所を提示し、修正の優先順位をつけることができる。

**What are the conditions for analyzing a modified program?**  
Automated program transformation is a powerful primitive, and **applying incorrect templates can produce malformed programs**. **When developing templates, it is helpful to impose validation criteria for running the analysis on the modified program.** Analyses generally accept only well-formed programs (in the respective language), and **typically rely on building artifacts or instrumenting compiler output to perform analysis**. Language allowing, we impose a validation step that all modified programs must compile 5 when performing analysis, which provides additional assurance that the analysis will not terminate early due to malformed programs. For PHP, we rely on a valid parse of target files as the validation step. Applying transformations may violate style checkers (e.g., linters) integrated into a build manifest, or cause spurious compiler warnings (e.g., unused variables), but still allow the program to compile successfully. We allow such violations or warnings; **in our results these do not directly affect the fidelity of the analysis with respect to the issue targeted by transformation. Another possibility of interference is that transformations addressing one bug class may lead to additional reports for a different bug class. This undesirable behavior depends on the analysis checks, its configuration, and template formulation. We qualify such cases in our experiments (Section 4).**

不適切なテンプレートを適用すると、プログラムを壊す可能性がある。なので変更後のプログラムでの検証基準を設けておくと便利。検証することで、変換後のプログラムへの解析が早期終了しないことを保証される。ある変換が解析に悪影響を与える可能性の一つは、あるバグクラスの問題へ対処した結果、他のバグクラスのレポートを誘発する、みたいなシナリオ。実験でもこういったケースがある。

In the final step we capture analyzer output for the modified program 6 . **The goal of our approach is that this output provides a higher quality bug report than that obtained from running the analyzer on the original program.**

この手法の目的: 解析器のバグレポートがより高品質になること。

## 4 Evaluation
This section describes our results tailoring programs for analysis. Our evaluation emphasizes real-world applicability on large and popular programs for modern analyzers. **The goal of our evaluation is to show that programs can be tailored generically and declaratively to improve analysis output.** We focus on breadth of applicability across languages and analyzers for real-world issues. Thus, our research questions are:

評価目標: プログラムを一般的かつ宣言的に調整し、解析結果を改善できることを示す

- RQ. 1 Can tailoring programs improve the fidelity of static analysis reports? Specifically: **Does the approach remove false positive reports without otherwise adversely affecting the analysis results?**

RQ1: FPを減らしつつ、悪影響が出ないようにできるか？

- RQ. 2 Does the program tailoring approach generalize? **We specifically evaluate generality with respect to multiple languages and analyzers, and pattern reuse across projects.**

RQ2: 複数の言語と解析器に関して一般できているか（パターンの再利用できるか）

Section 4.1 describes our experimental setup. Section 4.2 describes our results applying 9 rewrite templates to 15 projects across five analyzers.

### 4.1 Experimental Setup
We consider five popular analyzers: `PHPStan` [7] for PHP, `SpotBugs` [8] for Java, `Clang Static Analyzer` [1] and `CodeSonar` [2] for C, and `Infer` [5] for both Java and C. All analyzers are mature, actively used, and incorporate state-of-the-art techniques. **All analyzers are open source, except for CodeSonar which is a commercial analyzer**.(\*9) For each analyzer we were interested in current or long-standing issues that cause spurious warnings or false positives, and particularly issues that could not be easily addressed by analysis configuration or suppression mechanisms:

(\*9) We used CodeSonar under a free academic license.

評価対象の5つの静的解析器について。以下問題点の抽出手順。

- (1) **We searched the PHPStan, SpotBugs, and Infer issue trackers on GitHub for reports or comments containing the words “false positive”.**
- (2) **We found related issues for the Clang Static Analyzer (which does not have a GitHub issue tracker), by searching for GitHub commits containing the words “clang static analyzer false positive”.**
- (3) CodeSonar does not have a public issue tracker, nor did we find public commits referencing false positives. **We manually inspected warnings for false positives.**

GitHub で PHPStan, SpotBugs, Infer について issue tracker を検索（false positiveという文言で）。 GitHubのコミットで “clang static analyzer false positive” を検索。 CodeSonarについては手動で検査。

The above methods influenced project selection. For (1) we identified problematic syntax patterns and user-affected projects from user reports. **For (2) we identified committed code changes in an existing project and used this to develop a generic template.** For (3) we selected popular C projects as representative real world projects (since we did not have sources indicating false positives in CodeSonar a priori).

(2)については汎用テンプレートの参考にした。

Templates developed from the initial set of issues were reused to search for additional projects containing potentially false positiveinducing patterns. **We searched over the top 100 most popular(\*10) projects for each language.** However, to apply our approach generally, **we require that a project (a) compiles (or parses) successfully and (b) is configured for analysis**. Many of the projects identified by large-scale search **presented significant manual burden to compile** (e.g., due to various dependencies and platform-specific requirements like like Android, iOS, or Visual Studio) and **consequently configure for analysis**. We note that this burden is amplified for external users (such as ourselves) who have limited access to various platforms and who are unfamiliar with specific build configurations. We generally expect the burden to be less of an impediment for using our approach among project maintainers, who are familiar with the complexities of their own projects.

(\*10) Ranked by the number of user favorites (GitHub stars)

上記で作ったテンプレートを用いて、人気プロジェクトからそのパターンを多く含むプロジェクトを検索。 ただし、それらのプロジェクトはそもそもコンパイルのコストが高い場合があるので、一部は解析用に合わせてコンパイルした。

In aggregate, **we evaluate our approach on 15 projects**. Our selection represents a convenience sample of real world issues to substantiate our claims about tailoring programs to overcome analyzer limitations. The selection includes real issues that affect developers of large, popular codebases. We show that our approach is efficient and scales to these concerns, and that it presents reuse potential across projects.

We ran our large-scale search experiments on an `Ubuntu 16.04 LTS server`, with `20 Xeon E5-2699 CPU cores` and `20GB of RAM`. We evaluated analysis improvement on this same hardware for large projects (>100KLOC) and the CodeSonar analyzer. All other analysis improvement experiments were run on an `Ubuntu 16.04 VM` with `two 2.2 GHz i7 CPU cores` and `4GB RAM`. We make our tooling and data available online.(\*11)

(\*11) https://doi.org/10.5281/zenodo.3629098

### 4.2 Experimental Results
#### 4.2.1 Overview.
Table 1 shows our results for each analyzer. We develop transformations for 9 syntactic Patterns across PHP, Java, and C projects. We discuss the patterns in detail in Section 4.2.5. The Issues column shows **the total number of warnings across all bug classes in the Before column** (each analyzer is run with its default checks). We ran each analyzer on the entire project (warnings are thus for the entire project) except for PHPStan where the number of warnings is reported for a single file. **PHPStan can operate at the file level, and using targeted transformation we demonstrate that our approach can isolate issues in individual files without incurring a project-wide analysis.** **The Cls column is the subset of all bugs that fall into the bug Class that the pattern targets.** **The ∆ FP column is the number of false positives removed for that bug class by transformation.** **The #R column is the number of rewritten instances in the source code for each pattern.** There may be more or fewer rewritten instances compared to removed false positives due to the effect of transformations on analyses (cf. pattern free-model and if-assign-resources); we elaborate in Section 4.2.5. The time to transform the program (Time Rewr) is negligible compared to analysis runtime (Time Anlyz). In aggregate these transformations **remove 111 false positives (∆ FP)**, with a median of 2 per project.

Ref: 調整前の誤検出総数  
CIs: バグパターンを含んでいるクラスの総数  
∆FP: 削減された誤検出の数  
\#R: 書き換えられたインスタンスの総数  
書き換え時間（Rewr）は解析時間に比べるとわずか。合計111の誤検出の除去を達成。

#### 4.2.2 Effects of transformations on analysis behavior.
Providing a primitive for arbitrarily modifying programs means that our approach can hypothetically introduce adverse effects which do not exist in the original program. **The negative possibilities are that a change either removes true positives in the original program, introduces more false positives in the modified program, or both.** We evaluated whether our patterns cause such adverse effects by **manually** inspecting bug reports before and after applying a transformation for every analysis run. **From our inspection, no true positives are removed for any project.** No additional bug reports are introduced for any project, **except OpenSSL and Presto**. Infer nondeterministically reports different numbers of bugs for large Java projects in the case of Presto, irrespective of whether we perform a change. We confirmed that our change removes the false positive on five independent runs; **nondeterminism for large numbers of bug reports (>800) make it difficult to conclude whether our change has any meaningful effect on other reports.**

調整によって悪影響（真に陽性なバグを見逃すが、新しい誤検知を追加するか）が出るかを調査。手動で検査すると、前者はなし。後者はOpenSSLとPresto（いずれも解析器はInfer）で発見。 原因はInferの非決定的な報告（なので報告しない場合も存在）。 大規模なプロジェクトでの非決定的な要素は影響の予測を困難にする。

We observed that Infer reports an additional 6 potential null dereferences after applying the free-model pattern. **This is interesting because the free-model pattern targets memory leak false positives, not null dereference reports.** Interestingly, Infer analyzes and reports bugs in functions after transforming the program, whereas it previously short-circuits analysis and reporting for functions containing free-wrapper functions. We inspected these reports and **could not obviously discern whether they were false positive or true positive reports**.

Inferについては、Null への潜在的なデリファレンスが6件報告されたが、このときの検査パターンはfree-model、つまりメモリリークのFPを対象にしていた（どちらかというとNotNullのせいで誤検知となるはずなのに）。実際にこれがFPなのかFNなのかは判然とせず。

In summary: **Our approach removes false positives without adversely effecting the analysis in the majority of cases (13 out of 15 projects), while two projects are inconclusive**. In general, program tailoring is effective because transformations perform small, local changes that affect only the reasoning of the analysis for that instance or bug class (as detailed in Section 4.2.5). It follows that changes which are closely semantics-preserving of the original program ought not make an analysis less precise, and our results affirm this intuition.

13/15のプロジェクトで悪影響はなく精度を改善した。が、2/15では決定的ではなかった。

#### 4.2.3 Amortizing human effort for codifying and detecting patterns.
Pattern reuse is an especially appealing property of tailoring programs. We developed four patterns from user-reported issues in an initial project(\*12) which we then used to detect and fix multiple false positive issues in six additional independent projects. These results show that **patterns may generalize to benefit multiple projects, and imply that the cost and human effort of writing broadly applicable templates can be amortized across software stakeholders** (i.e., both analysis users and analysis writers develop, distribute, and benefit from patterns). In contrast, **existing mechanisms using comment suppression or command line options cannot likewise generalize, and consequently induce recurring developer effort**.

(\*12) if-assign-resources, substr-model, create-socket, and null-on-resources

パターンの再利用（テンプレート化）によって、複数のプロジェクトでテンプレを用いることでコストの償却が期待できる。一方で、コメントの抑制やコマンドラインオプションを使う既存メカニズムは一般化を達成できなかった（開発者のコストは変わらず）

We performed a large-scale search using each pattern to identify the additional projects above, and to quantify overall efficiency.(\*13) Table 2 shows our results running each pattern on the top 100 most popular GitHub repositories for PHP, Java, and C. **In general search is fast and can identify potential false-positive inducing patterns before an analysis even runs.**

(\*13) Note that the patterns identified projects which failed to run under the analysis or compile, and hence are not included in our final Table 1.

表1での実験で使ったパターンを用いて、GitHub上でパターンマッチする件数を調査(表2)。解析のFPを導くようなパターンが結構ある＝パターンの再利用が十分可能である。

#### 4.2.4 Issue duration and resolution for analysis end users.
Table 3 characterizes six open source issues that our patterns address.(\*14) Interestingly, issues are long-standing (unresolved for over a year, on average), and all but one remain unresolved at the time of writing. **These issues generally reveal that analysis end users are subject to long delays and lack of support, having little recourse for resolving false positives using existing mechanisms.** On the one hand, **analysis writers may not have the time to support user- or project-specific needs, and may deprioritize less pressing requests** (e.g., infer/781 is an individual user request with no cross-references to other issues). On the other hand, an issue may affect many users (as shown by multiple cross-referenced issues for phpstan/647, spotbugs/756), but very complex to solve for analysis maintainers.

(\*14) The three remaining patterns, rsyslog, and those for CodeSonar, do not have open source issues.

表3: 作ったパターンに対応するOSSのIssueの特徴。問題の多くは平均1年以上未解決のままで、phpstan/647を除いて未だに未解決。 エンドユーザはサポートを受けられずにいる状態が続いている。一方、開発者視点だと、緊急でもない限り、個々のユーザに対応する優先度が低い現状がある。

Our approach handles these issues via program transformation, and give agency to end users for implementing workarounds. Furthermore, user-reported errors can be significantly more severe than other analysis reports as they may break existing software workflows. For example, even a single false positive report in SpotBugs due to null-on-resources breaks the continuous integration (CI) build of cross-referenced projects, and caused users to disable the check wholesale for their projects across all versions of Java. **Because of this profound effect, bug severity and build integration must be weighed into analysis configuration mechanisms. Moreover, although the relative size of false positive reduction is small for some reports in Table 1, the correspondence to real-world issues and extended impact on end users and software workflows make the false positives we handle more significant than others.** Our results show that our approach can uniquely address such complexities.

ユーザが提供するエラーは既存のソフトウェアワークフローを破壊する可能性がある。テンプレートを用いた変換は相対的にサイズが小さいが、大きな影響を持つ。

#### 4.2.5 Rewrite patterns.
We now discuss analyzer issues in greater detail, explain what our template transformation does to resolve the issue, and why it induces a positive change in analysis behavior. There is generally a **one-to-one correspondence of template to pattern**. The **exception is patterns if-assign-resources and null-on-resources** where we implemented two templates to account for syntactic variants in function call names and optional catch blocks.

各パターンについて掘り下げ。基本1パターン1テンプレートだが、if-assign-resourcesとnull-on-resourcesは例外的に2つのテンプレートを実装。

**Pattern: `if-assign-resources`.**  
This pattern addresses the issue of variable assignment in if-conditionals (as introduced in Section 2). At the original time of writing PHPStan did not accurately track the effects of such assignments, and it took over a year to fix in the analysis implementation. False positives particularly manifest for cases where states of resources (like files) are opened. Because PHPStan does not track that the variable assigned cannot be `false`, it reports an error when the variable is passed to a function such as `fwrite` or `fclose`, saying “Parameter `#1` of function `fclose` expects resource, resource|`false` given”. The transformation pulls the assignment out of the `if-condition`, which allows PHPStan to accurately track the effect (Fig. 4). An interesting result is that rewriting this pattern can remove many false positive reports because multiple functions may use a file resource along multiple paths, and each use raises an error. For example, rewriting 16 cases in WordPress removes 44 false positives.

```php
if ((:[v] = @fopen (:[a]) ) == 0)
```
Match Template

```php
:[v] = @fopen (:[a]) ;
if (:[v] === false )
```
Rewrite Template

Motivでも出てきた、条件分内に代入の副作用があるパターン（特にリソースが代入されることを想定）。PHPStanでは変数がfalseになりえないことを追跡できず誤検出を生む。多くの誤検出を削除できる例。

**Pattern: `substr-model`.**  
The `substr` function in PHP performs a substring operation. PHPStan models return value types of functions like `substr`. However, PHPStan did not track the fact that `substr` may return `false` if the length of the string is shorter than the requested substring range. PHPStan, thinking the value can only ever be a string, thus emits a false positive when a user’s code checks whether the return value of `substr` is `false`, saying “comparison using `===` between string and `false` will always evaluate to `false`”.

PHPのsubstr関数は部分文字列操作だが、falseを返す場合がある（変数の値が文字列とfalseになりえる）。ユーザがコード内でfalseチェックをすると、===を文字列とfalseで比較してるのでfalseになるぞ、と誤検出する。

It took eight months as of the first user report for maintainers to implement the solution properly. **It is particularly interesting that the maintainers were unwilling to make a simple change to the function model to reflect that `substr` can return `false`**, because it would propagate spurious warnings when used as an argument in other contexts. However, users primarily had issues with the model when they checked return values, not when the value was used as an argument. Our transformation in Fig. 5 matches problematic cases that users experienced while avoiding the difficulty of changing the `substr` model wholesale. We introduce the expression `isset($maybe_false)` which lets PHPStan reason that the return value of an unset variable may be `false`. Statements that assign the result of `substr` before an if check are changed with an or-clause, which effectively remodels the `substr` call to possibly return a `false` value. Note that the use of `:[[v]]` in the assignment statement (line 1) and conditional expression (line 2) of the match template introduce a constraint that both matching instances must be syntactically equal for the rewrite rule to fire. This pattern removes six false positive reports in four PHP projects.

```php
$:[[v]] = substr (:[a]) ;
if ($:[[v]] === false )
```
Match Template

```php
$:[[v]] = substr (:[a]) || isset ( $maybe_false ) ;
if ($:[[v]] === false )
```
Rewrite Template

解析器の管理者がこれを修正するまでに8か月を要した（警告がカスケードするのに、この簡単な修正を中々しなかったのは興味深い）。テンプレートとしては、issetを導入することでPHPStanでも正確になるようにした。ただし、テンプレ内の:[[v]]が2行に渡って一致しないといけないという制約が生じた。

**Pattern: `free-model`.**  
Infer may timeout when analyzing functions and fail to summarize their effect. **Infer reports false positive memory leaks when it fails to realize that a function frees memory— this happens particularly when the C library `free` call is wrapped inside custom free functions.** The OpenSSL project follows the convention of wrapping `free` calls in functions like `OPENSSL_free`, which Infer fails to analyze. As an approximation of these custom `free` functions, our transformation (Fig. 6) rewrites the wrapping functions to call the plain C library `free` version. Due to syntactic ambiguity, the pattern may match and rewrite function declarations as well. We found that prepending a semicolon can sufficiently disambiguate candidate `free` statements from function declarations. OpenSSL compiles successfully despite transforming more than three thousand calls, and removes 6 false positives because Infer can then track the effect of `free` on memory for rewritten calls.

```c
:[[f]](:[args])
_______________________
where match :[args] {
| ":[_] ,:[_]" -> false
| ":[_]" -> true
}
```
Match Template (引数が一つならおｋ)

```c
:[[f]](:[args])
_______________________
where rewrite :[f] {
":[_] free :[_]" -> " free "
}
```
Rewrite Template (freeの前後に何か文字列がくっついてれば、通常のfreeにする)

Inferはメモリの解放を見逃すと、メモリリークを誤検知する。特にfree関数のラッパーについて起きやすい。変換ルールとしては、ラッパーを普通のfree関数へ置換する（unwrap）。ただし、この置換についてはラッパー関数が"XXX_free"であることと、引数が一つ(freeと同じ)である必要がある。OpenSSLでは、この変換でも正常にコンパイルでき、6つの誤検出を抑制。  
小泉: 結構ガバガバな気もするが…。まあ静的解析で解析できるようにunwrapしてると考えればまあ。

**Pattern: `create-socket`.**  
The `createSocket(socket, ..)` call is typically used in conjunction with the Java SSL library. It returns a server socket that wraps an existing socket in the first argument. The last argument, when `true`, tells the call that the underlying socket should be closed when the returned socket is closed. **Infer fails to model the `createSocket` call, and a false positive report states that the underlying socket is leaked.** The boolean toggle in the last argument makes this function difficult to model generically.

InferはcreateSocket関数のモデリングに失敗している。本来解放済みのソケットをリークとして報告してしまう。

Interestingly, a user reports a false positive for a particular case where the last argument is always `true`. **Our transformation (Fig. 7) addresses the issue by simulating the `close` operation early, and simply returning a fresh socket for Infer to track.** Note that we would never persist this change in practice as it loses the implementation details of `createSocket`; however, it is sufficient for making the analysis more precise. This pattern removes two false positive reports in two Java projects; no resolution has yet been proposed in the Infer issue tracker.

```java
:[[sock]] = :[_].createSocket (:[sock], :[_], :[_], true ) ;
```
Matching Template

```java
:[[sock]].close() ;
:[[sock]] = new Socket() ;
```
Rewrite Template

誤検知が発生するのが最後の引数がtrueの場合に起きる。結局はcloseしてSocketインスタンスを生成するやり方に切り替え。

**Pattern: `wrapped-resources`.**  
Java 7 introduced the `try-with-resources` statement which automatically closes a resource after the block executes, preventing a leak. Infer reports a false positive leak when a resource constructor is nested inside another resource constructor within a `try-with-resources` statement (this happens, e.g., when passing a `FileInputStream` to an `InputStreamReader`). The underlying problem is similar to pattern `create-socket`: Infer fails to track that the wrapped resource will be closed. We use a conceptually similar transformation as in `create-socket`, but account for the syntactic variation introduced by `try-with` blocks (Fig. 8).

```java
try (:[[o]] :[[v]] = new :[[c1]](new :[[c2]](:[args]) ) )
```
Matching Template

```java
:[c2] wrapped = new :[c2](:[args]);
try {
  wrapped.close () ;
} catch ( IOException e ) {}
  try (:[o] :[v] = new :[c1]( wrapped ) )
```
Rewrite Template
<!-- ** ``  -->

Infer が Java 7 の try-with-resources 構文に対応できない（ブロックを抜けると自動でリソースを解消する動きを追跡できない）。

**Pattern: `null-on-resources`.**  
SpotBugs reports a redundant `null` check on resources inside `try-with-resources` statements (i.e., a resource is `null` checked after being previously dereferenced).(\*15) However, the error is only reported for code compiled with Java 11 and 12, and not Java 10. **The reason is pernicious: the Java compiler in later versions inserts a `null` check in the bytecode which does appear to be indeed redundant.** From the user’s perspective, however, the report is a false positive—no `null` check is visible in the source code. The issue is cross-referenced by a large number of projects, and remains unresolved for over a year. Various projects have added annotations or disabled the check completely. No official solution has been proposed. The transformation in Fig. 9 converts a `try-with-resources` statement to a traditional `try-catch-finally` block. In effect, we normalize the `try-with-resources` syntax across Java versions to sidestep the null-check generation that only happens for Java versions 11 and 12. This suppresses three spurious bug reports in two of the projects.

(\*15) Note this bug is orthogonal to wrapped-resources.

```java
try (:[x] :[[v]] = :[expr]) {
  :[body]
}:[rest]
_______________________
where match :[rest] with
| " catch " -> false
| " finally " -> false
| ":[_]" -> true
```
Matching Template

```java
:[x] :[v] = :[expr];
try {
  :[body]
} finally {
  if (:[v] != null ) {
    :[v]. close () ;
  }
}
:[rest]
```
Rewrite Template

SpotBugs Java 10, 11 でデリファレンス後のリソースへの冗長なnullチェックを実施。原因は根が深くて、Javaコンパイラがnullチェックを勝手に挿入することが原因（なのでソースコードとにらめっこしても分からない）。これも一年以上放置されている。テンプレートとしては、従来のtry-catch-finally構文へ変換を行う。

**Pattern: `const-strcmp`.**  
The Clang Static Analyzer may report a potential out-of-bounds access when comparing strings with `strcmp`. This only happens when macro-expansion (defined in glibc headers) is triggered, in this case by the fact that a string literal is passed in the second argument to `strcmp`. The transformation in Fig. 10 extracts the string literal in the comparison to a string `const`, causing the analyzer to analyze the `strcmp` C library function rather than the macro expansion.

```c
if( strcmp (:[1],":[2]") )
```
Matching Template

```c
const char * const t1 = ":[2]";
if( strcmp (:[1],t1) )
```
Rewrite Template

Clang Static Analyzer はstrcmpのマクロ展開（引数にリテラルが入った場合）が生じた場合、範囲外アクセスを誤検知する可能性がある。なのでconstの変数への代入を挟んで対処。

**Pattern: `strncpy-null`**  
The C `strncpy` function does not necessarily `null-terminated` its destination buffer, which can lead to memory corruption. CodeSonar warns about this possibility, but also notes that if a subsequent statement definitely `null-terminates` the string, then the warning can be ignored. We found that the warning was indeed a false positive in the swoole project: a subsequent check always `null-terminates` the buffer safely. However, a possible reason why CodeSonar conservatively reported an error is that the subsequent statement is guarded and was not considered safe (see line 10, Fig. 12). To avoid the false positive, our pattern checks whether a condition on the `strncpy` buffer length terminates that same buffer with a `null` character (Fig. 11). If so, the rewrite unconditionally `null-terminates` the buffer. Although this transformation is not generally strong enough to match syntax that guarantees a `null-terminated` buffer, it does provide flexibility for refining analysis warnings. For example, we found exactly the same pattern using our template in the PHP source code, where it appears the `php_ssl_cipher...` function was borrowed from.

```c
strncpy (:[dst], :[src], :[len]) ;
if (:[len] :[rest]) { :[dst][:[idx]]] = 0; }
```
Matching Template

```c
strncpy (:[dst], :[src], :[len] - 1) ;
:[dst][:[len] - 1] = '\0 ';
if (:[len] :[rest]) { :[dst][:[idx]] = 0; }
```
Rewrite Template

CodeSonar の誤検知例。 strncpy は書き込み先バッファをヌル文字で終わらせることを保証しないことへの警告を出す。本来後続の文でヌル文字が保証されていれば警告を出さないが、後続文がガードされると保守的に警告を出す（まあ冗長）。 なので書き換え後にヌル文字終端かどうかのチェックを入れている。

**Pattern: `snprintf-null`**  
CodeSonar reports an “Unterminated C String” error when a possibly unterminated string is passed to a function such as `strcat`. The analysis believes a string may not be `null-terminated` when, for example, space is allocated in the heap but not subsequently `null-terminated`. We identified two false positives where heap-allocated memory for a string is `null-terminated`, but only because we know that the buffer starts out with positive length and passes through `snprintf` which always `null-terminates`. The analysis introduces imprecision where it believes the string can be of zero length, but does not, however, then model the possibility that the buffer can subsequently be treated as a null pointer when passed to `snprintf`. Our transformation (Fig. 13) makes this possibility explicit and adds explicit `null-termination`, which suppresses two unterminated string warnings.

```c
snprintf (:[dst], :[len], :[rest]);
```
Matching Template

```c
snprintf (:[dst], :[len], :[rest]);
if (:[len] > 0) {
  :[dst][:[len]] = '\0 ';
} else {
  :[dst] = NULL ;
}
```
Rewrite Template

ヒープの文字列でヌル文字を入れていない場合などで、未終端の文字列として扱われる（エラー報告される）。 CodeSonarはsnprintfのモデリングが不完全なので、変換後は明示的にヌル文字を入れるようにした。

#### 4.2.6 Discussion.
We further characterize considerations and limitations of our approach.

**Pattern development.**  
**We found that developing patterns can take a few iterations to refine until they precisely match syntax of interest.** For example, we iteratively added constraints in the null-on-resources pattern to filter out try statements that already contained catch and finally statements (without this constraint we would generate malformed programs). We expect users of our approach to similarly develop patterns incrementally. While there exists a learning curve for developing patterns, the process is analogous to existing practice at Google where analysis writers tune checks based on results of running over the codebase [31].

パターン開発には何回かの反復（調整）が必要。なので、パターンを作る場合には、漸進的にすすめるのがベネ。

**Tailoring applicability and usability.**  
**Analyzers on GitHub have numerous open issues** related to “false positive” reports (at the time of writing, Infer has 36 open issues and PHPStan has 32, and SpotBugs has 38), and **our approach can fall outside the scope of these**. For example, **we may need type information to check whether a method or object may cause a false positive, while our approach is purely syntactic**. As a practical matter, **implementing an effective tailoring approach requires a robust and maintainable process for changing programs**. We used comby to perform language-specific parsing and guarantee well-formedness with respect to certain syntactic properties (e.g., balanced parentheses). However, **syntactic ambiguity in the target language can reduce the precision of purely syntactic transformations**. **Integrating static properties, like types, can achieve greater robustness and expressivity in transformations.** We envision that leveraging existing rewrite tooling (e.g., **Refaster [35]** for Java) for program tailoring can further achieve robust transformations for very language-specific properties.

解析器のいくつかの誤検知問題については、対応しきれない部分がある。また、パターンマッチにはどうしても構造的な処理しかできないので、型情報などが必要なパターンには対応できない（他の技術、例えばRefaster[35]などと補完的に動作させることは可能）。言語のあいまいさも構文の弱点になりえる。

We note the compelling case for applying our technique in “black box” analysis settings. Users of commercial analyzers, like CodeSonar, do not have agency over the closed-source implementation or configuration options outside those provided by the distributor. **Our approach demonstrates a new way to fine-tune results that is complementary to a black box analysis.**

あくまでも解析器の内部はブラックボックスとして扱っている（テンプレートを作るのもパターンを見つけているだけで、内部処理はブラックボックス扱い）。

**True positive warnings in the modified program may appear at different lines compared to the original program.** As a usability concern, affected lines in the modified program should map to those in the original. This is primarily an engineering concern, as **transformations keep track of precise changes in offsets so that no information is lost**.

変換によって行番が変わるので、報告されたTrue Positiveな警告がそれまで（あるいはエディター上）と変わる可能性がある。で、これには対応しきれていないので課題。

## 5 RELATED WORK
Program transformation has been used in various contexts to augment a procedure, technique, or system. Harman et al. introduce the **idea of testability transformation [18, 19]** where the goal is to transform a program to be more amenable to testing (e.g., by altering control flow) while still satisfying a chosen test adequacy criterion. **Program transformation can improve fuzz testing coverage and reveal more bugs [29]** and **enable new crash bucketing strategies to accurately triage bugs [32]**. Failure-oblivious computing [30] adds (for example) bounds checking that allow programs to execute through memory errors at runtime. We similarly develop source-to-source transformations; however, we focus on improving analysis output fidelity. Our technique also aims to improve a static procedure and thus must be fast enough to integrate into static workflows. Randomized program transformation is an approach for testing static analyzers [14] and compiler internals, and excels at finding bugs in optimization passes [15, 36]. Our approach differs generally from these in using tailored program transformations to deterministically rewrite syntax.

プログラム変換の関連研究  
[18, 19] testability を維持しつつ、テストに適した変換をする（CFの変更など）  
[29] 変換によってFuzzingのカバレッジが上昇  
[32] バグのトリアージを容易に  
[30] 障害の発生が分かりにくいバグ（メモリ関連とか）で境界チェックなどを自動挿入  
[14] 検体としてプログラム変換をして、静的解析器やコンパイラのバグを検出する（入力としてのプログラムをFuzzingする感じ）  
[15, 36] 最適化パスのバグ検出に適用  
本稿は上記の関連研究と比べて、構文的な決定的書き換えが可能なので差分あり。

**Program transformation on intermediate representations can improve analysis precision** (e.g., by adding bounds on arrays [12, 23]). Recent work formalizes the impact of program transformations on static analysis in the abstract [28]; for example 3-address code transformation can introduce analyzer imprecision [26]. These works adopt a predominantly semantic view of program transformations and their influence on analysis; **Cousot and Cousot develop a language-agnostic framework for reasoning about the correspondence of syntax and semantics under transformation [13].** **These ideas underlie our intuition that semantic changes can improve analysis.** However, abstract representations are difficult for developers to manipulate. Our work promotes changing program syntax directly as a proxy for inducing semantic changes that enhance analysis reasoning.

中間表現への変換は分析精度の向上に役立つ。[13]は構文とセマンティクスの対応について推論する言語非依存のフレームワークを研究。ただし、抽象表現だと開発者が操作するのは難しくなる。

Various tools exist for rewriting programs, and could implement the program tailoring approach in this paper. Notable declarative tools include **Coccinelle for C [24] and Refaster for Java [35]**. **In practice, programs are difficult to transform generically [27].** A key aspect of our work is to make automated program tailoring language-accessible. We therefore used our own recent work in efficient and declarative transformation for multiple languages [34].

プログラム調整 (Program Tailoring) の注目度が高いツール → Coccinelle for C[24] と、Refaser for Java[35]。 ただし、プログラムを一般的に変換するのは困難[27]

**In practice analyzers compromise on soundness [25] and implementation tradeoffs manifest as implicit tool assumptions that are difficult to trace and modify [11].** Existing work shows that analyzer configuration options and suppression mechanisms fall short of developer needs in practice [10, 21]. **Recent work by Gorogiannis et al. [17] emphasizes the value of reducing false positives over false negatives, where the objective is to never report a false positive.** In terms of this work, we introduce a new program transformational approach toward false positives while sidestepping the difficulties of modifying analyzer implementations or configurations.

静的解析器は健全性(ここでは、FPが出ないこと)について妥協していることが多く、追跡困難な暗黙のルールを設定している。最近の研究[17]では、FNよりもFPを減らすことが価値が大きいとしている（FNを出さないように設計して、いかにFPを減らすか）。

## 6 CONCLUSION
**We introduced a new approach for effecting changes in static analysis behavior via program transformation. Our approach uses human-written templates that declaratively describe syntax transformations. Transformations are tailored to suppress spurious errors and false positives that arise due to problematic patterns and limitations in analyzer reasoning.** We made the observation that analysis users have little agency over the format of analysis configuration options provided to them, but that program transformation offers a fresh primitive for leveraging influence over analysis behavior. To this end we showed that manipulating concrete syntax can resolve diverse and long-standing issues in existing analyzers, where configuration and suppression mechanisms fall short. Our evaluation presents the first study for empirically validating this program transformational technique, which we evaluated on active analyzers and large real world programs.

変換テンプレートは人間が宣言的に作成する。変換は解析器の制限によって発生する誤検知を抑制するために作られる（なので、解析器が完璧ならこのテンプレートは不要=FNを抑制するものではない＆効率性を上げるものではない）
