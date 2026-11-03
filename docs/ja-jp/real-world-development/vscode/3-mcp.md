---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: エージェントモード][previous-lesson] · [次のレッスン: カスタムエージェント →][next-lesson]

エージェントモードでフィルタリング機能を構築しました。プルリクエストを作成する前に、ブラウザーで実際に動作することを確認します。自分でアプリを操作する代わりに、**Playwright MCP サーバー**を接続し、Copilot に実際のブラウザーで機能をテストさせます。その後、ブランチを公開して PR を作成します。

この演習では、次の内容を学びます。

- Model Context Protocol (MCP) と MCP サーバーによる Copilot の機能拡張を学びます。
- Playwright MCP サーバーを VS Code ワークスペースに追加します。
- Copilot にサーバーを使わせ、ブラウザーでフィルタリング機能を手動テストします。
- ブランチを公開し、フィルタリング作業のプルリクエストを作成します。

## Model Context Protocol (MCP) とは

エージェントモードは、エディターの外部にあるツールを利用すると、さらに強力になります。Model Context Protocol (MCP) は、Copilot が外部ツールやサービスと通信するための標準です。

![エージェントモードがコンテキスト、LLM、MCP サーバーや VS Code 拡張機能が提供するツールと連携する仕組みを示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) を使うと、AI エージェントが外部のツールやサービスと通信できます。リソースを使って最新情報にアクセスしたり、ツールを使って代わりに操作を実行したりできます。

ツールやリソースには、AI エージェントと外部のツールやサービスの橋渡しをする MCP サーバーを介してアクセスします。MCP サーバーは、既存の API や npm パッケージなどのローカルツールとの通信を管理します。各サーバーは異なるツールとリソースを提供します。

よく使われる MCP サーバーには、次のものがあります。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリ、Issue、プルリクエストを管理する API にアクセスし、新しいリポジトリの作成、既存リポジトリの更新などを行います。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright を使ったブラウザー自動化機能を提供します。Web ページの移動、フォームの入力、ボタンの選択などを行えます。

ほかにも多くの MCP サーバーがあります。GitHub は、エコシステム内のサーバーを見つけやすくし、貢献を促進するために [MCP レジストリ](https://github.com/mcp)をホストしています。

> [!CAUTION]
> セキュリティ面では、MCP サーバーもプロジェクトの他の依存関係と同様に扱ってください。使用前にソースコードと発行元を確認し、セキュリティへの影響を検討します。信頼できる MCP サーバーだけを使い、機密リソースや操作へのアクセスを許可する際は注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースで使う MCP サーバーを構成します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. 次のような `github` サーバーがすでに設定されています。

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

この `github` 設定はプロジェクトのテンプレートに含まれています。そのため、前の演習で Copilot が Issue のバックログを読み取れました。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使うため、ローカルへのインストールは不要です。VS Code は HTTP 経由で接続し、Copilot がツールを初めて使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

次に 2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が操作できるブラウザーを用意でき、機能をテストできます。

1. `.vscode/mcp.json` に `github` と並べて `playwright` を追加します。ファイルは次のようになります。

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. ファイルを保存します。

リモートの `github` サーバーと異なり、`playwright` はローカルサーバーです。VS Code は `npx @playwright/mcp@latest` を実行して起動します。`--headless` は画面を表示せずにブラウザーを実行するオプションです。デスクトップ環境のない Codespace では、この設定が必要です。

> [!NOTE]
> Tailspin Toys では E2E テストに Playwright を使用しているため、必要なブラウザーはすでにインストールされている場合が多くあります。Copilot がブラウザーの不足を報告した場合は、`npx playwright install chromium` を実行して再試行するよう依頼します。

## サーバーを起動して信頼する

VS Code は必要に応じて MCP サーバーを起動します。Copilot がそのサーバーのツールを初めて使うときに、起動して信頼するか確認します。

1. `.vscode/mcp.json` で `playwright` エントリーの上に **Start** が表示されます。選択してサーバーを起動し、設定が正しいことを確認します。
2. サーバーを信頼するか確認されたら、設定を確認し、起動を許可します。

手動で起動する必要はありません。省略した場合は、次の手順で Copilot が初めてツールを使う際に VS Code が起動を促します。

## フィルタリング機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が実際のブラウザーを操作できます。エージェントがページを開き、移動し、フィルターを適用して結果を読み取り、確認した内容をまとめます。会話を離れずに機能を検証できます。

Playwright MCP サーバーはスクリーンショットではなく、ページの[アクセシビリティツリー][playwright-mcp-server]を使います。エージェントはボタン、リンク、リスト項目など、ラベル付きの構造化された要素を扱います。これは支援技術と同様の方法で、簡単な機能確認をアクセシビリティの基本確認にもできます。

サーバーを接続してアプリを実行した状態で、Copilot にフィルタリング機能を試させます。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバー経由でブラウザーを起動し、手順を実行して結果を報告します。レポートを Issue の受け入れ条件と照らし合わせます。動作に問題があれば、PR を作成する前に追加の質問をするか、コードを修正するよう依頼します。

> [!NOTE]
> テスト時にはアプリが `http://localhost:4321` で動作している必要があります。開発サーバーを停止した場合は、プロンプトを送る前に再起動します。Copilot が Playwright MCP サーバーを初めて使うときにブラウザーをダウンロードする場合があります。不足が報告されたら、`npx playwright install chromium` を実行して再試行するよう依頼します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能が動作することを確認できました。次はチームでレビューするため、`filtering-vscode` ブランチを公開してプルリクエスト (PR) を作成します。

1. Codespace の **Source Control** パネルを開き、Copilot が行った変更を確認します。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンを使ってコミットメッセージを生成します。

   ![変更内容を表示する Source Control パネルのスクリーンショット](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択してブランチをリポジトリにプッシュします。

## プルリクエストを作成する

github.com や GitHub コマンド ライン インターフェイス (CLI) からも PR を作成できます。ここでは GitHub Copilot に依頼します。関連する Issue を探し、PR と関連付けられます。

1. Copilot Chat パネルを開き、**New Chat** を選択します。
2. GitHub のツールを使えるよう、エージェント選択メニューで **Agent** を選びます。
3. 次のプロンプトで PR の作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Copilot が情報を収集して操作を行う際は、必要に応じて **Continue** を選択して許可します。GitHub のツールを初めて使うときに認証を求められた場合は、画面の指示に従います。
5. Copilot が Issue を検索し、該当するものを見つけて PR を作成する様子を確認します。
6. Copilot が生成したリンクを選び、プルリクエストをレビューします。**まだマージしないでください**。

## まとめと次のステップ

この演習では、次の内容を学びました。

- Model Context Protocol (MCP) と MCP サーバーによる Copilot の機能拡張を学びました。
- Playwright MCP サーバーをワークスペースに追加しました。
- 出荷前に、ブラウザーでフィルタリング機能を手動テストしました。
- ブランチを公開して、フィルタリング作業のプルリクエストを作成しました。

MCP サーバーを使って機能をテストしました。Copilot を拡張する方法は MCP だけではありません。次は、アクセシビリティレビューなどの作業を効率化する[カスタムエージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP レジストリ][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
