---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェントモード →][next-lesson]

生成 AI を使う際はコンテキストが重要です。タスクの進め方や、Copilot に知っておいてほしい背景情報を伝えるには、その情報を参照できるようにします。[指示ファイル][instruction-files]を使うと、コード生成に関するガイダンスを提供できます。

この演習では、次の内容を学びます。

- リポジトリのカスタム指示とパス指定の指示ファイルが、プロジェクトのコンテキスト、コーディング指針、ドキュメント標準を Copilot に伝える方法を確認します。
- 現在の指示を使って、フィルタリング機能の最初のデータ部分 (パブリッシャーヘルパー) を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。
- 同じプロンプトを再実行し、生成コードが新しい標準に従う様子を確認します。
- 指示の更新とフィルタリング機能の一部を `main` にコミットし、次の演習で新しい指示を使えるようにします。

> [!CAUTION]
> 生成コードが設定した標準から外れる場合があります。Copilot の動作は決定論的ではありません。この演習では、指示の更新後にコードが文字単位で一致するかではなく、動作の傾向が変わるかを確認します。

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
> Copilot に指示ファイルを生成させることもできます。VS Code の **Configure Chat → Generate Agent Instructions** や Copilot CLI の `/init` など、環境によって方法が異なります。必要な場合は、該当する演習で説明します。
>
> テンプレートや出発点を探す場合は、指示ファイル、カスタムエージェントなどを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## プロジェクトのカスタム指示ファイルを確認する

このリポジトリに用意された、共通の `copilot-instructions.md` と各タスク向けの `*.instructions.md` を確認します。エディターまたは GitHub Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクト概要に加え、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下には、**GitHub Actions Workflows** の指針もあります。これらは Copilot とのやりとりに適用されます。
3. `.github/instructions` フォルダーを開き、Astro、Drizzle データ層、テストなどの指示を確認します。
4. `.github/instructions/unit-tests.instructions.md` を開き、`applyTo` フィールドを確認します。この glob はリポジトリルートからの相対パスで、たとえば `**/*.test.ts` に一致する TypeScript のテストファイルに指示が適用されます。
5. このプロジェクトで単体テストを作成する際の指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾にある他の指示ファイル (例: `unit-tests.instructions.md`) やプロジェクトファイルへのリンクを確認します。指示を再利用可能な小さなファイルに分け、コード生成の参考例を示せます。ここにあるパスはリポジトリルートではなく、指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** にはコーディング標準が記載されていますが、コード内のドキュメントはまだ必須ではありません。次の手順で TSDoc とファイル先頭コメントの規則を追加します。

## 変更用ブランチを作成する

ベストプラクティスに従って、変更用ブランチを作成します。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. ブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示の更新前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

指示の効果を確認するため、まず現在の指示ファイルを使ってプロンプトを送信します。後で指示を更新し、同じプロンプトをもう一度送信して違いを見ます。

1. Copilot が必要なコンテキストだけを使うよう、前の演習で開いたエディタータブを閉じます。
2. Copilot がヘルパーの場所を把握できるよう、`src/lib/publishers.ts` を開きます。
3. Chat ビューのエージェント選択メニューで **Agent** を選び、Copilot がファイルを変更できるようにします。

   ![Chat ビューのエージェント選択メニューを示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot がプロジェクトを調べ、ヘルパーファイルやテストなどのコードを変更します。
6. 提案されたヘルパーが、`db` クライアントを最初の引数に取り、型付きのパブリッシャー配列を返す関数であることを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` の規則です。
7. 提案されたコードに TSDoc とファイル先頭コメントが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくてもドキュメントコメントを追加することがあります。その場合も問題ありません。指示を更新した後の一貫性向上がこの演習の目的です。

## リポジトリ全体に適用する標準を追加する

`.github/copilot-instructions.md` は、Copilot にプロジェクト全体の情報を伝えるファイルです。コーディング標準を明記して、コードの提案を改善します。

1. `.github/copilot-instructions.md` を再度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。既存の標準には、今回生成されたヘルパーにドキュメントコメントがなかった理由となる規則がまだありません。
3. ファイル先頭コメントと TSDoc の追加を促すため、既存の標準の直後に次の Markdown を追加します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、指示ファイルはリポジトリ全体に適用する `.github/copilot-instructions.md` と、特定の言語、ファイル種別、タスクに適用する `*.instructions.md` のどちらにも作成できます。今回追加したドキュメントコメントの規則のようなプロジェクト全体の標準は、リポジトリレベルのファイルに記載します。

## 同じプロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話を新しくします。
2. `src/lib/publishers.ts` を再度選び、Copilot が適切なファイルに注目するようにします。
3. 先ほどと**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメントブロックが追加されていることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 関数に次のような TSDoc コメントが含まれていることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

新しい標準を設定し、次の演習で使うコードに適用できました。

## 指示とブランチをコミットしてプッシュする

指示ファイルもほかのアセットと同じようにソース管理で扱います。指示の更新とブランチをリポジトリにコミットしてプッシュしましょう。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
2. 指示の更新とヘルパーの変更を確認します。

   ```bash
   git status
   ```

3. 指示の更新と、ヘルパーの基盤として Copilot が変更したファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、バックログのドキュメント標準に関する Issue と関連付けてプルリクエストを作成します。GitHub のツールを使って Copilot に Issue を探し、PR を作成してマージするよう依頼します。

> [!NOTE]
> この演習では Copilot に GitHub 上の操作を依頼します。プロジェクトのテンプレートでは GitHub のツールが構成済みです。初回の利用時に VS Code で GitHub へのサインインが求められた場合は、画面の指示に従ってください。後の演習では Model Context Protocol を使った接続方法を学びます。

1. Codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント選択メニューで **Agent** を選びます。
3. 指示ファイルの更新に関する Issue を探し、現在のブランチから PR を作成するよう Copilot に依頼します。指示の更新だけでなく、今後のフィルタリング作業のためにパブリッシャーヘルパーの基盤も追加したことを記載します。

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が Issue を探して PR を作成します。
5. Copilot に GitHub の操作を許可するよう求められたら、コマンドを確認して必要に応じて **Allow** を選択します。
6. PR が作成されたら、PR を `main` にマージし、ローカルブランチを `main` に戻して最新コードを取得するよう、次のプロンプトで依頼します。

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. GitHub の操作やシェルコマンドの実行を許可するよう求められたら、コマンドを確認して必要に応じて **Allow** を選択します。

GitHub Copilot を使って PR を作成し、マージできました。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを取得する方法を確認し、VS Code の Copilot Chat を使って次の作業を行いました。

- 既存の指示を使ったコード生成のプロンプトを送り、Copilot の提案を確認しました。
- `.github/copilot-instructions.md` に新しい標準を追加しました。
- 同じプロンプトを再実行し、提案されたコードが新しい標準に従う様子を確認しました。
- 指示の更新とヘルパーの基盤を `main` にコミットし、次の演習で使えるようにしました。

次は[エージェントモード][next-lesson]を使い、コードベース全体に新機能を追加しながら、設定した指示を適用します。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
