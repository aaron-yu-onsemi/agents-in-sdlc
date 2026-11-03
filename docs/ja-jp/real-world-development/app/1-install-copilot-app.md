---
title: "レッスン 1 - GitHub Copilot app をインストールする"
description: "GitHub Copilot app をインストールし、作成したリポジトリを接続して、ワークスペースを確認し、クイックチャットを試します。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

**[GitHub Copilot app][about-copilot-app]** は、エージェントを使った開発向けのデスクトップアプリです。GitHub Copilot CLI を基盤とし、GitHub とネイティブに統合されているため、リポジトリ、ブランチ、CI パイプラインをすぐに利用できます。複数のエージェントをそれぞれ独立したワークスペースで並行して動かし、作業を指示して、繰り返し作業を自動化するワークフローに適しています。Node.js のインストールとプロジェクトの準備ができたら、アプリをインストールしてリポジトリを接続します。

このレッスンでは、次の内容を学びます。

- GitHub Copilot app をインストールしてサインインします。
- GitHub リポジトリからプロジェクトをアプリに追加します。
- テンプレートから用意されたバックログなど、ワークスペースを確認します。
- アプリについてのクイックチャットを試します。

## シナリオ

チームは、増え続けるバックログに対応するため AI エージェントを導入しています。Copilot app を使うと、Issue の選択、エージェントの実行、変更のレビュー、プルリクエストのマージを一か所から指示できます。このレッスンでは、アプリをインストールして接続し、プロジェクトについての会話を始められるようにします。

> [!NOTE]
> 対象となる Copilot プランが必要です。Copilot Free、Copilot Student、または有料プラン (Pro、Pro+、Business、Enterprise) を利用してください。Copilot Business または Copilot Enterprise では、管理者が **GitHub Copilot app** ポリシーを有効にしている必要があります。

## GitHub Copilot app をインストールして構成する

GitHub Copilot app を使うには、まずインストールします。Windows、macOS、Linux 向けのバージョンが用意されています。アプリをインストールして認証し、Tailspin Toys リポジトリを追加します。

1. ブラウザーで [GitHub Copilot app のランディングページ][download-app]を開きます。
2. 使用しているプラットフォーム向けのアプリをダウンロードし、ページの手順に従ってインストールします。
3. インストール後、アプリを開きます。
4. **Sign in to GitHub** を選択し、画面の指示に従って認証します。データ所在地を指定する GitHub Enterprise Cloud を使う場合は、**Use GitHub Enterprise** を選択し、求められたら `*.ghe.com` のホスト名を入力します。GitHub Enterprise Server はサポートされていません。
5. オンボーディングでリポジトリを選択できる場合は、テンプレートから作成した Tailspin Toys リポジトリを選択します。選択をスキップし、後からプロジェクトを追加することもできます。
6. テーマを選び、オンボーディングを完了します。
7. サイドバーの **Sessions** でリポジトリを確認します。すでに表示されていれば、再度追加する必要はありません。
8. リポジトリがまだ接続されていない場合は、**Sessions** の横にある **+** を選択してセッションを作成し、**Add GitHub repository** を選択します。前提条件のレッスンで記録した Owner とリポジトリ名を使います。
9. サイドバーにリポジトリが表示されていることを確認します。このレイアウトでは、`tailspin-toys` などのリポジトリフォルダーが **Sessions** の下にあり、その下に個々のセッションが表示されます。

## ワークスペースを確認する

プロジェクトを接続したら、サイドバーにある各領域を確認します。

- **New** - 新しい Copilot チャットセッションを開始します。
- **Pull requests** - アプリの GitHub 統合でプルリクエストを表示します。アプリを離れずに、一覧表示や絞り込み、CI 状態の確認、レビューができます。
- **Issues** - Issue を一覧表示して絞り込み、Issue からセッションを開始します。
- **Automations** - スケジュールまたはオンデマンドで実行する、保存済みのエージェントタスクです。To-do リスト、定期保守、その他の面倒な作業の管理に役立ちます。振り返りでは次のステップとして紹介しますが、このワークショップ内の演習ではありません。
- **Customize** - MCP サーバー、プラグイン、スキルなどのコンポーネントを追加します。ここでは Playwright MCP を設定します。
- **Chats** - 独自のブランチやワークスペースを必要としない、質問やブレインストーミング向けの軽量な会話です。最初のチャットを開始すると表示されます。まだ表示されていない場合は、**Sessions** の横にある **+** を選択してチャットを開始します。
- **Sessions** - リポジトリフォルダーと、その中のエージェントセッションをまとめます。`tailspin-toys` などのリポジトリ名を選ぶとリポジトリビューを開き、その下のセッションを選ぶと会話に戻ります。セッションごとに独立したワークスペースを持つため、変更を競合させずに複数のセッションを並行して実行できます。最初のセッションは星評価を追加する際に始めます。

ワークショップを進めながら、ワークスペースを詳しく確認していきます。

> [!TIP]
> 迷ったときは Copilot に質問してください。操作方法が分からない場合や、何かが可能か知りたい場合に案内してくれます。

### 用意されたバックログを確認する

Tailspin Toys のテンプレートからリポジトリを作成したときに、Issue のバックログが用意されました。

1. サイドバーの **Sessions** で Tailspin Toys のリポジトリフォルダーを選択します。個々のセッションではなく、リポジトリを選びます。リポジトリ上部のナビゲーションで **Issues** を選択します。
2. Issue 番号を決めつけず、次のタイトルを探します。

   - Allow users to filter games by category and publisher
   - Update our repository coding standards

3. Issue を選択して詳細を読みます。どちらの Issue からもエージェントセッションを開始できます。最初の小さな変更を完了した後、フィルタリングの Issue から作業を始めます。

> [!NOTE]
> **Issues** と **Pull requests** の一覧には、Copilot app に追加したリポジトリの項目だけが自動的に表示されます。別のリポジトリの作業項目を確認するには、そのリポジトリもアプリに追加してください。

## クイックチャットを試す

アプリ自体について質問すると、アプリに慣れることができます。**quick chat** はブランチや worktree を作らずに質問やブレインストーミングができるため、セッションを必要としない簡単な質問に適しています。

1. サイドバーで **Chats** の横にある **+** を選択して、新しいチャットを開きます。**Chats** がまだ表示されていない場合は、**Sessions** の横にある **+** で最初のチャットを始めます。その後 **Chats** が表示されます。
2. 次の質問で、アプリのセッションの仕組みを尋ねます。

   ```plaintext
   How does the GitHub Copilot app use worktrees?
   ```

3. 会話ビューで回答を読みます。各セッションが独立した git worktree で実行されるため、変更を競合させずに複数のエージェントを並行して動かせることが分かります。会話は続けることも、新しいチャットを始めることもできます。

## まとめと次のステップ

GitHub Copilot app をインストールし、プロジェクトを接続して、ワークスペースを確認しました。次の内容を学びました。

- アプリをインストールして GitHub にサインインする。
- GitHub リポジトリからプロジェクトを追加する。
- ワークスペースを確認し、**Issues** で用意されたバックログを見つける。
- クイックチャットで簡単な質問をする。

次は[最初のエージェントセッションを開始し][next-lesson]、ゲームカードに星評価を表示します。

## リソース

- [GitHub Copilot app について][about-copilot-app]
- [GitHub Copilot app の使用を開始する][getting-started]
- [GitHub Copilot app でのエージェントセッションの操作][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
