---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

多くの組織は、未解決のセキュリティ課題、更新が必要なレガシーコード、実装する時間がなくバックログに残った機能要望など、技術的負債に直面しています。GitHub Copilot cloud agent は、コードの更新や機能追加などを自律的に実行できます。作業が終わると、人がレビューできるドラフト PR が作成されます。手間のかかる作業を任せることで、開発プロセスを加速し、より大きな課題に集中できます。

Copilot cloud agent で次の内容を学びます。

- コード生成環境をカスタマイズする方法。
- 操作を安全に行う方法。
- Issue の範囲を明確に定める重要性。
- Issue を Copilot に割り当てる方法。

## シナリオ

Tailspin Toys には解消したい技術的負債があります。最初のサイトを開発した委託チームがドキュメントを残しておらず、アプリケーションのコードには説明がありません。まず、アプリケーションのすべてのエクスポート関数に TSDoc のドキュメントコメントを追加します。

デザインチームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリーに属する他のゲームを表示し、利用者が閲覧を続けられるようにします。まだ完成度の高い実装は不要で、UX の受け入れテストに使えるものを求めています。この作業は現在のブロッカーですが、優先度の高い別の Issue もあります。

どちらも、優先度が下がったままになりやすい作業の例です。Copilot cloud agent に任せれば、エージェントが非同期で作業している間に別の作業を進め、後で結果をレビューできます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同じようにバックグラウンドで作業できます。たとえば [GitHub Issue を Copilot に割り当て][assign-issue]ます。割り当てると Copilot は進捗を記録するドラフトプルリクエストを作成し、環境を準備して作業を始めます。実行中または完了後にセッションを確認できます。提案された解決策のレビュー準備ができると、プルリクエストで通知されます。

## 明確な範囲の指示が重要

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。一見簡単な操作でも、詳しく見ると複雑さがあります。[Copilot cloud agent に依頼するタスクの範囲][cloud-agent-best-practices]を慎重に決めてください。AI ペアプログラマーとして扱い、段階を踏んで学び、試し、調整します。生成 AI を加えてもソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習ではカスタム指示とその効果を確認しました。まだ `.github/copilot-instructions.md` を見ていない場合は、ここで読み取り専用で確認しておきます。後で cloud agent の PR をレビューするときに、指示の効果を見つけやすくなります。

GitHub Web UI または起動中の Codespace で、次のファイルを確認します。

- `.github/copilot-instructions.md`: **Code standards** セクション、特に TypeScript の慣習と TSDoc コメントの要件を確認します。
- `.github/instructions/unit-tests.instructions.md`: `**/*.test.ts` に指示を適用する `applyTo` frontmatter を確認します。

次のセクションで **Code lacks documentation** Issue を cloud agent に割り当てます。生成された PR で、これらの指示に由来する TSDoc、TypeScript の慣習、ファイル先頭コメントを確認してください。後の演習でも PR のレビュー時に確認します。

## Copilot cloud agent の開発環境を設定する

コードの作成には、通常、必要なツールと依存関係をインストールする環境設定が必要です。Copilot も同様に、ソフトウェアエンジニアのように作業するための環境を使います。

Cloud agent は GitHub Actions を使って作業環境を構成します。`.github/workflows/copilot-setup-steps.yml` の[セットアップワークフロー][setup-workflow]で必要な開発ツールと依存関係を準備できます。このラボ用にはすでに設定済みで、Node.js、プロジェクトの依存関係、E2E テスト用ブラウザー、単一 Astro アプリ用に移行・シードされた SQLite データベースを用意します。

```yaml
name: "Copilot Setup Steps"

# Allows you to test the setup steps from your repository's "Actions" tab
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Set the permissions to the lowest permissions possible needed for *your steps*. Copilot will be given its own token for its operations.
    permissions:
      # If you want to clone the repository as part of your setup steps, for example to install dependencies, you'll need the `contents: read` permission. If you don't clone the repository in your setup steps, Copilot will do this for you automatically after the steps complete.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app setup - Node.js (the whole app is now Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migrate + seed the local SQLite database so builds and tests have data.
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub ワークフローと同じ構成ですが、次の点に注目してください。

- `copilot-setup-steps` というジョブが 1 つあります。このジョブは Copilot が PR の作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを使うと、リポジトリの **Actions** タブからワークフローを手動実行できます。Copilot が実行するのを待たずに、正常に動くかテストできます。

## ドキュメントを追加する

ドキュメントの重要性は広く理解されていますが、情報が古かったり、そもそもなかったりするプロジェクトは少なくありません。こうした技術的負債は、生産性を下げ、コードベースの保守や新しいメンバーの受け入れを難しくします。Copilot はドキュメント作成が得意なため、cloud agent に任せるのに適した Issue です。エージェントがバックグラウンドで必要なドキュメントを生成します。後の演習で結果をレビューします。

1. 新しいブラウザータブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して新しい Issue を開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次を入力します。

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. **Create** を選択して Issue を作成します。
8. 右側で **Assign to Copilot** を選択して割り当てダイアログを開きます。

   ![Issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選びます。
11. 新しく作成されたプルリクエスト (PR) を開きます。タイトルは **[WIP]: Code lacks documentation** のようになります。PR が一覧にない場合は、少し待ってからブラウザーを更新します。
12. 数分後、Copilot が TODO リストを作成します。

> [!NOTE]
> TODO リストが PR に表示されるまで数分かかる場合があります。Copilot が環境を構築して先ほどのワークフローを実行し、プロジェクトを分析して、問題への最適な取り組み方を決めているためです。

13. TODO リストと作業内容を確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が Issue の作業を開始したことを示す更新を確認します。
15. **View session** ボタンを選択します。

   ![Copilot のセッションビュー](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された状態が表示されない場合は、ウィンドウを再読み込みしてください。

16. ライブセッションをスクロールし、Copilot が問題を解決する様子を確認します。コードを調査して状態を把握し、計画を検討してコードを作成する手順が表示されます。

この作業には数分かかる場合があります。Copilot cloud agent の大きな利点は非同期に作業できることです。別の作業に取り組めるよう、もう 1 つタスクを割り当ててから、アプリケーションの機能追加に取り組みます。

## ゲーム詳細ページに関連ゲームを追加する

Cloud agent では作業を分け、エージェントが別のタスクに取り組む間に自分の作業を進められます。デザインチーム向けの関連ゲーム欄は短時間で実装できる場合でも、その時間を別の作業に使えます。Copilot cloud agent に依頼しましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して新しい Issue を開きます。
4. 空のテンプレートとして **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次を入力します。

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. **Create** を選択して Issue を作成します。
8. 右側で **Assign to Copilot** を選択します。

   ![Issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、Copilot が作業を開始したことを示す 👀 が Issue の最初のコメントに表示されます。

![Copilot が Issue に取り組んでいることを目の絵文字で示す画面](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼に取り組みます。ソフトウェアエンジニアと同じように作業するため、常時監視する必要はありません。完了後に結果をレビューします。次はカスタムエージェントを作成して使います。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents]に Issue を割り当て、非同期に作業させる方法を学びました。技術的負債への対応、新機能の作成、別のフレームワークからのコード移行などを依頼できます。

この演習では、次の内容を学びました。

- コード生成環境をカスタマイズする方法。
- 操作を安全に行う方法。
- Issue の範囲を明確に定める重要性。
- Issue を Copilot に割り当てる方法。

Cloud agent がバックグラウンドで作業する間、カスタムエージェントの作成と使用に進みます。[Copilot cloud agent は MCP サーバーも使えます][cloud-agent-mcp]。また、前のモジュールで確認したカスタム指示も利用します。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローに関するベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
