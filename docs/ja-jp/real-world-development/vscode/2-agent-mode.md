---
title: "演習 2 - Copilot Agent Mode で機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

小さな更新でも、通常は複数ファイルの変更やテスト実行が必要です。必要なファイルを探し、変更し、テストを実行し、問題をデバッグして、不足していたファイルを見つけ、さらに変更を加える、といった作業が続きます。

このような作業には Copilot Agent Mode が役立ちます。

Copilot Agent Mode は IDE 内で、より自律的に作業します。プロジェクト構成を調べ、必要なファイルを更新し、テストなどのタスクを実行し、見つかった問題を自動的に修正します。ここでは Agent Mode を使ってサイトに機能を追加します。

> [!NOTE]
> 名前は似ていますが、エージェントモードと cloud agent は異なる体験を提供します。エージェントモードは IDE 内で動作し、すばやいフィードバックと対話を可能にします。cloud agent はチームメンバーのように非同期で動作し、Issue とプルリクエストを通じてやりとりします。

この演習では、次の内容を学びます。

- Copilot Agent Mode がプロジェクトを調べ、関連ファイルを特定して、複数ファイルにまたがる変更を行う方法。
- GitHub Copilot Agent Mode で UI とデータ層に新機能を実装する方法。
- コードベースにマージする前に、Copilot Agent Mode が作成した変更とテストをレビューする方法。

## シナリオ

ゲームの一覧が増えてきたため、カテゴリーとパブリッシャーで絞り込めるようにします。前の演習でパブリッシャーヘルパーを追加しました。ここでは Copilot Agent Mode でデータ層、UI、テストの残りを完成させます。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、Tailspin Toys の現在の機能を確認します。

この Web サイトは、開発者をテーマにしたボードゲームのクラウドファンディングプラットフォームです。ゲームの一覧と詳細を表示します。Astro アプリはビルド時に静的 HTML を生成し、ページの frontmatter から Drizzle ORM を使ってローカル SQLite データベースを直接参照します。別のバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータアクセスヘルパーは `src/lib/` に置き、インタラクティブな機能は標準 DOM API を使った小規模な Astro `<script>` で追加します。

### Web サイトを起動する

サイトを簡単に実行できるよう、Astro の開発サーバーを起動する `npm run dev` タスクが用意されています。

1. Codespace に戻ります。前の演習から `main` に戻っているはずです。
2. Codespace 内で <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. フィルタリング作業用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドで Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro の開発サーバーが起動すると、次のような URL が表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くよう促すダイアログが表示された場合は、**x** を選択して閉じます。

5. <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら、ターミナルに表示された `http://localhost:4321` を選択して Web サイトを開きます。

> [!NOTE]
> Codespace のターミナルから localhost のリンクを選ぶと、`https://<your-codespace-name>-4321.app.github.dev/` に転送されます。これは Codespace の Web サーバーへのプライベートトンネルです。

### Web サイトを確認する

Web サイトを起動したら、次の機能を確認します。

- **ホームページ**: ボードゲームのタイトル、画像、説明を一覧表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、パブリッシャー、カテゴリーなどの情報を表示します。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

Web サイトは動作していますが、さらに機能を追加します。テンプレートからプロジェクトを作成すると、Issue のバックログも自動的に用意されます。GitHub Copilot に項目を表示するよう依頼します。

1. Chat ビューのエージェント選択メニューで **Agent** を選択します。エージェントは自律的に計画し、複数ファイルを変更し、ターミナルコマンドを実行してツールを呼び出します。

   ![Chat ビューのエージェント選択メニューを示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデル一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> ワークショップの作成者が特定のモデルを推奨しているわけではありません。作成時に Claude Sonnet 4.5 を使ったため、手順にも記載しています。提案の一貫性を高める意図はありますが、LLM は確率的に動作するため、手順と異なる提案が出ることがあります。これは通常の動作です。

> [!NOTE]
> LLM の確率的な性質により、Copilot が異なる MCP コマンドを使う場合がありますが、タスクは完了できるはずです。

3. 次のプロンプトを Copilot に送り、Issue のバックログを確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選び、すべての Issue を一覧表示するコマンドを実行します。
5. 生成された Issue 一覧を確認します。

Copilot が、ユーザーに最も役立つと思われる項目を優先順位付けしたことも確認できます。

## 指示ファイルを確認する

コード生成を始める前に、作業のコンテキストを与える指示ファイルを確認します。Web サイトの機能追加方法が記載された [ユーザーインターフェイス (UI) 指示ファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)を使います。

1. Codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能追加に関する全体的なガイダンスを確認します。アーキテクチャの概要、コンポーネント設計、テスト容易性、アクセシビリティの原則、Astro、Tailwind CSS、Drizzle などのファイル用の指示へのリンクが含まれています。

> [!TIP]
> 指示ファイルから別の指示ファイルやプロジェクト内ファイルを参照できます。パスは指示ファイルからの相対パスです。指示を再利用し、大きな内容を管理しやすい単位に分け、Copilot に例やテンプレートを示すことができます。

## フィルタリング機能を実装する

フィルタリングを完成させるには、少なくとも次の 3 つの変更が必要です。

- データ層 (`src/lib/`) にフィルタリングのロジックを追加または調整します。
- フィルタリング動作のテストを追加または更新します。
- ゲーム一覧ページにフィルタリング UI を追加します。

コードベースにマージする前にテストを実行し、成功させる必要があります。Copilot Agent Mode にこれらの作業を依頼します。

1. Copilot との現在の会話を続けるか、**New Chat** を選んで新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選択して、指示ファイルを追加します。

   ![UI 指示ファイルを選択する例を示すスクリーンショット](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェント選択メニューで **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント選択メニューを示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. モデルが **Claude Sonnet 4.5** のままであることを確認します。
5. バックログの関連 Issue に基づいて機能を実装するよう、次のプロンプトを使って Copilot に依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、機能に必要なデータ層、UI、テストを見つけて変更し、テストを実行する様子を確認します。

   ![Copilot がプロジェクトのファイルを調査する様子を示すスクリーンショット](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトを調べ、ファイルを変更してテストを実行します。タスクやコードベースによって数分かかる場合があります。作業中はエディターに **Keep** と **Undo** が表示されることがあります。作業中に選択する必要はなく、完了時に変更ごとの **Keep** または **Undo** が用意されます。

7. Copilot に求められたら **Continue** を選択し、テストを実行します。

   ![Copilot Chat がテスト実行の確認を求めるダイアログを示すスクリーンショット](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストが失敗したりする場合があります。Copilot はコード生成とテストを繰り返し、タスクを完了してエラーを検出しなくなるまで進めます。

   ![Copilot Agent Mode との完了した Chat セッションを示すスクリーンショット](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot や他の AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを開いているブラウザーに戻り、新機能を試します。
11. 動作とコードを確認したら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

GitHub Copilot Agent Mode で Tailspin Toys に機能を追加する方法を確認しました。この演習では、次の内容を学びました。

- GitHub Copilot Agent Mode で UI とデータ層に新機能を実装できます。
- Copilot Agent Mode はプロジェクトを調べ、関連ファイルを特定して、複数ファイルにまたがる変更を行えます。
- コードベースにマージする前に、Copilot Agent Mode が作成した変更とテストをレビューします。

次は[Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを作成します。

### 追加の演習: ページネーションを実装する

ゲームの一覧が増えるとページネーションが必要になります。この演習で学んだことを使い、Copilot にページネーションの実装を依頼します。

- 既存の指示ファイルなど、現在のベストプラクティスに従います。
- ページサイズをユーザーが選べるようにするか、固定値にするかを検討します。
- 希望する実装に必要なガイダンスをプロンプトで伝えます。
- Copilot に変更やコンテキストを追加で伝えながら、何度か調整する場合があります。これは通常の作業の流れです。

## リソース

- [Copilot の ask、edit、agent モード: 機能と使い分け][choose-mode]
- [VS Code のエージェントモード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
