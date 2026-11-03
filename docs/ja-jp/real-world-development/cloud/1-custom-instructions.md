---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使う際はコンテキストが重要です。タスクの進め方や、Copilot に知っておいてほしい背景情報を伝えるには、その情報を参照できるようにします。[指示ファイル][instruction-files]を使うと、Copilot に求めることだけでなく、その実行方法も伝えられます。

この演習では、次の内容を学びます。

- リポジトリのカスタム指示とパス指定の指示ファイルが、プロジェクトのコンテキスト、コーディング指針、ドキュメント標準を Copilot に伝える方法を確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code や CLI の環境とは異なり、ここでは同じプロンプトの変更前後を比較しません。Copilot cloud agent は GitHub Issue に対して非同期に作業するため、影響をリアルタイムで並べて示すのが難しいためです。後の演習で cloud agent が作成したプルリクエストをレビューし、指示ファイルの効果を確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には開発手法に関するガイドラインがあります。

- データ層には必ず単体テストを用意します。
- UI はダークモードで、現代的な印象にします。
- コードには TSDoc のドキュメントコメントを追加します。
- 各ファイルの先頭に、ファイルの役割を説明するコメントブロックを追加します。

指示ファイルを使い、Copilot がこれらの開発慣行に沿って作業できるようにします。

### カスタム指示

カスタム指示を使うと、コーディングスタイルや要件に関するコンテキストと希望を Copilot に伝え、より適切な提案やコード生成を促せます。リポジトリ全体、または特定のファイル種別に指示を設定できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md`: リポジトリへの**すべての**要求に適用される単一の指示ファイルです。技術スタック、開発中のもの、ベストプラクティスなど、ほとんどの Copilot 要求に関連するプロジェクト情報を記載します。
- `.github/instructions/*.instructions.md`: 特定のタスクやファイル種別に使う指示ファイルです。TypeScript や Astro などの言語、UI コンポーネントや単体テストの作成などのタスクを対象にできます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat によるコード生成に適用されます。コード補完や次の編集候補には適用されません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリレベルの指示と `applyTo` frontmatter を持つ `*.instructions.md` ファイルを使います。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの標準の指示ファイルも[サポートしています][custom-instructions-support]。

### 指示ファイルを管理するためのベストプラクティス

このワークショップでは指示ファイルの作成方法すべてを扱いませんが、サンプルプロジェクトには代表的な構成例があります。

- `copilot-instructions.md` には、構築中のもの、プロジェクト構成、全体的なコーディング標準など、プロジェクトレベルの指針を記載します。
- `*.instructions.md` は、単体テスト、Astro コンポーネント、データ層などのファイル種別や、特定のタスクに関する指示に使います。
- 自然な言葉で、明確に記述します。コードの良い例と避けるべき例を示します。

指示ファイルの書き方に唯一の正解はありません。プロジェクトで試しながら、最適な方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、十分な指示ファイルを用意しましょう。このプロジェクトには、[UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けの指示があります。
>
> Copilot に指示ファイルを生成させることもできます。VS Code の **Configure Chat → Generate Agent Instructions** や Copilot CLI の `/init` など、環境によって方法が異なります。必要な場合は該当する演習で説明します。
>
> テンプレートや出発点を探す場合は、指示ファイル、カスタムエージェントなどを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## プロジェクトのカスタム指示ファイルを確認する

このリポジトリには共通の `copilot-instructions.md` と、タスク別の `*.instructions.md` があります。エディターまたは GitHub Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの概要に加え、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下には **GitHub Actions Workflows** の指針もあります。
3. `.github/instructions` フォルダーを開き、Astro、Drizzle データ層、テストなどの指示を確認します。
4. `.github/instructions/unit-tests.instructions.md` を開き、`applyTo` frontmatter を確認します。この glob はリポジトリルートからの相対パスで、たとえば `**/*.test.ts` に一致する TypeScript のテストファイルに指示が適用されます。
5. このプロジェクトの単体テスト作成に関する指示を確認します。
6. `.github/instructions/drizzle.instructions.md` の末尾にある他の指示ファイル (例: `unit-tests.instructions.md`) やプロジェクトファイルへのリンクを確認します。指示を再利用可能な小さなファイルに分け、Copilot に例を示せます。そこに記載されたパスは指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** にはコーディング標準が記載されていますが、コード内のドキュメントはまだ必須ではありません。次の手順で TSDoc とファイル先頭コメントの規則を追加します。

## リポジトリ全体に適用する標準を追加する

ここで編集するのは、プロジェクト全体に適用する指示を追加する作業だけです。cloud agent は、この後割り当てる Issue に取り組む際に新しい指示を使います。

1. Codespace のターミナルで変更用ブランチを作成して切り替えます。cloud agent は Issue の対象ブランチから指示を読みますが、変更をコミットするためにブランチを作成します。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` にプロジェクト全体の情報を記載し、コード提案を改善します。

2. `.github/copilot-instructions.md` を開きます。
3. 27 行目付近にある **Code formatting requirements** セクションを探します。既存の標準には、今回生成されたヘルパーにドキュメントコメントがなかった理由となる規則がまだありません。
4. ファイル先頭コメントと TSDoc の追加を促すため、既存の標準の直後に次の行を追加します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

5. `copilot-instructions.md` を保存します。

> [!TIP]
> 指示ファイルはリポジトリ全体に適用する `.github/copilot-instructions.md` と、特定の言語、ファイル種別、タスクに適用する `*.instructions.md` のどちらにも作成できます。今回追加したドキュメントコメントの規則のようなプロジェクト全体の標準は、リポジトリレベルのファイルに記載します。

## 指示の更新をコミット、プッシュ、マージする

cloud agent は、Issue の対象ブランチにある指示ファイルを読みます。次の演習で Issue を Copilot に割り当てると、`main` からブランチが作成されます。cloud agent に新しい指示を適用するには、更新を `main` にマージする必要があります。

1. 変更をステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを作成してマージします。次の演習で Issue を割り当てる際、cloud agent はこの指示を読み込みます。

> [!TIP]
> このワークショップでは `main` に直接作業してもかまいません。その場合はブランチの手順を省略して、`main` に直接コミットします。ブランチを使う手順は、実際のプロジェクトでの作業方法を体験できるようにしています。

## まとめと次のステップ

Copilot がこのプロジェクトの指示ファイルからコンテキストを取得する方法を確認し、`.github/copilot-instructions.md` に新しいプロジェクト標準を追加しました。残りの演習で cloud agent が作成する PR をレビューすると、この標準が適用されていることを確認できます。

次は[最初の Issue を Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
