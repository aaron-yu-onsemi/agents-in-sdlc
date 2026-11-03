---
title: "演習 5: GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー体験や機能を改善する複数の作業を GitHub Copilot に依頼しました。コードへのドキュメント追加、デザインチーム向けの関連ゲーム機能、高コントラストとライトモードを含むアクセシビリティ機能を実装させました。変更を確認し、必要に応じて Copilot にフィードバックを送り、チームのレビューを有効にする前に改善します。

### シナリオ

生成 AI を利用しても、ソフトウェア設計と DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに沿って進めます。ここでは、Copilot が作成したドキュメント、関連ゲーム機能、アクセシビリティ機能の提案を確認し、レビューをチームに公開します。

## セキュリティと GitHub Copilot cloud agent

Copilot cloud agent は非同期に、監督者なしで作業するため、安全性を保つための制約があります。

- Copilot はリポジトリへの読み取りアクセス権を持ち、書き込みは作業用のブランチに限られます。
- Cloud agent は GitHub Actions 内に一時的な独立環境を作って実行されます。
- GitHub Actions ワークフローの実行には、人による承認が必要です。
- MCP サーバーを含む[外部リソースへのアクセスは既定で制限されています][agent-firewall]。

## 生成されたドキュメントをレビューする

まず、Copilot cloud agent が作成した最初のプルリクエスト (PR)、つまりコードへのドキュメント追加をレビューします。GitHub.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR に、GitHub Copilot がファイアウォールでブロックされたという警告が表示される場合があります。MCP サーバーなど外部リソースへのアクセスは既定で制限されているため、これは**想定された動作**です。必要であれば、[Copilot cloud agent のファイアウォールを構成または無効化][agent-firewall]できます。

1. github.com でリポジトリを開きます。
2. **Pull Requests** を選択して PR の一覧を表示します。
3. **Add missing documentation** などのタイトルのプルリクエストを開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルに **[WIP]** が付いています。完了するまで待ちます。数分かかる場合があるため、休憩してもかまいません。

4. 準備ができたら **Files changed** タブを選び、変更をレビューします。

   ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新しく追加された TSDoc のコメントやその他のドキュメントを確認します。具体的な変更内容は異なります。演習 2 で確認したカスタム指示ファイルに基づく、TSDoc、TypeScript の慣習、ファイル先頭コメントを探します。
6. 更新をレビューし、問題がなければ **Conversation** タブに戻って下にスクロールします。
7. ワークフローの承認待ちを示す表示を探します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. PR のチェック欄にワークフローが追加されます。問題がなければ、単一 Astro アプリのプロジェクトチェックが成功します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

Copilot に PR のコメントやコードへのインラインコメントでメンションして、変更を依頼できます。Copilot はコメントを確認し、別のセッションを開始して対応します。結果は確率的に変わるため、指定の依頼文はありません。次のような変更を依頼できます。

- 各コードファイルの先頭に、役割を短く説明するコメントを追加する。
- TypeScript と Astro のファイルに TSDoc のドキュメントコメントを追加する。
- Astro アプリの構成を説明する README を作成する。

1. **@copilot** をメンションして、生成されたドキュメントの変更を依頼します。上記の例から選ぶか、希望する改善を依頼します。
2. **View Session** を選択し、Copilot の作業を確認します。Copilot は更新用の新しいセッションを開始します。
3. **Back to pull request** を選択すると PR に戻れます。

   ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot の変更が完了すると、PR に新しいコミットが表示されます。
5. **Files changed** タブを選択し、変更をレビューします。

納得できるまで Copilot に変更を依頼できます。完了したら、ドラフト PR をレビュー可能な状態にして `main` ブランチにマージします。

![Convert PR to ready](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する Issue を解決するため、Copilot が作成した PR を確認します。

1. GitHub.com でリポジトリを開きます。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルの PR を開きます。
4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 更新に問題がなければ **Conversation** タブに戻り、下にスクロールします。
6. ワークフローの承認待ちを示す表示を確認します。
7. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. PR のチェック欄にワークフローが追加されます。問題がなければ、単一 Astro アプリのプロジェクトチェックが成功します。完了まで数分かかる場合があります。
9. **省略可能:** Codespace でこのブランチに切り替えて関連ゲーム機能を手動テストできます。Codespace でターミナルを開き、Copilot が作成したブランチ名 (例: **copilot/fix-8**) に置き換えて次を実行します。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot が関連ゲーム機能を構築しました。ほかの PR と同様に、Copilot cloud agent に追加の変更を依頼できます。たとえば関連ゲームの表示件数やコメントヘッダー、TSDoc の追加を依頼できます。この作業はカスタム指示を更新する前に依頼したため、指示が反映されていない場合があります。**Conversation** タブにコメントを追加すると、Copilot が確認して新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ用カスタムエージェントが実装した機能をレビューします。この PR には、演習 3 で依頼したハイコントラストモードと、演習 4 の Mission control から追加依頼したライトモードが含まれるはずです。

1. GitHub.com でリポジトリを開きます。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルの PR を開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、PR に **[WIP]** が付いています。完了まで数分待ちます。

4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 実装を確認し、特に次の点を確認します。
   - 表示モードを切り替える UI コンポーネント。
   - ユーザー設定を保持する local storage の利用。
   - ハイコントラストとライトモード用の CSS またはスタイルの変更。
   - アクセシビリティ属性 (ARIA ラベル、キーボード操作など)。
   - モード切り替えを管理する JavaScript/TypeScript コード。

6. 更新をレビューし、問題がなければ **Conversation** タブに戻って下にスクロールします。
7. ワークフローの承認待ちを示す表示を確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. PR のチェック欄にワークフローが追加されます。問題がなければ、単一 Astro アプリのプロジェクトチェックが成功します。完了まで数分かかる場合があります。
10. **省略可能:** Codespace でこのブランチに切り替え、アクセシビリティ機能を手動テストできます。Copilot が作成したブランチ名に置き換えて、Codespace のターミナルで次を実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    次にアプリケーションを起動し、ハイコントラストとライトモードの切り替えが動作し、ページを再読み込みしても設定が保持されることを確認します。

アクセシビリティエージェントは、アクセシビリティのベストプラクティスに沿って機能を実装するよう Copilot を導きました。問題や改善点があれば、他の PR と同様に **@copilot** をメンションして変更を依頼できます。

## 省略可能な演習: さらにタスクを委任する

Cloud agent に実際のバックログ項目を割り当てれば、別の作業に取り組めます。リポジトリに Issue を追加し、Copilot に割り当てる練習をしましょう。例:

- ゲーム詳細ページに出資者向けの関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データアクセスヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent 環境のワークショップを完了しました。演習全体で、次の作業を行いました。

- cloud agent の出力に指示がどのように影響するかを後で確認できるよう、リポジトリに含まれるカスタム指示ファイルを確認しました。
- Issue を Copilot cloud agent に割り当て、非同期に環境を準備し、計画して実行する様子を確認しました。
- アクセシビリティ用カスタムエージェントを作成して使い、ハイコントラストとライトモードを追加しました。
- Copilot agents ページを Mission control として使い、アクセシビリティセッションを監視して方向付けしました。
- Copilot の PR をレビューして反復し、**@copilot** をメンションして変更を依頼し、ワークフローを承認しました。

## レビューと次のステップ

Cloud agent 環境のワークショップを完了しました。さらに学ぶ場合は、ほかの環境のワークショップも役立ちます。

- 🖥️ **[VS Code 環境][vscode-harness]** — IDE から Copilot Chat のエージェントモードと MCP の連携を試します。
- 💻 **[CLI 環境][cli-harness]** — Plan と Autopilot、エージェントスキル、QA カスタムエージェント、Playwright MCP、Agent Merge、cloud agent への任意の `/delegate` を使い、ターミナルから 2 つの変更をレビューして公開します。

自分のリポジトリでも、次の作業を試せます。

- リファクタリングやテスト範囲の拡大に関する Issue を cloud agent に割り当てる。
- 別の分野のカスタムエージェントを作成する。
- 別の技術スタック用に `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローに関するベストプラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを構成する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
[cli-harness]: ../../cli/
[vscode-harness]: ../../vscode/
