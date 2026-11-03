---
title: "演習 4 - エージェントを監視して管理する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前の数回の演習では、Copilot cloud agent にユーザー体験や機能を改善する 3 つのタスクを依頼しました。cloud agent は非同期かつ自律的に作業しますが、進捗を監視することも重要です。

GitHub.com の[エージェントページ][agents-page]など、cloud agent に割り当てたタスクを管理するツールがあります。ここでは、開いているプルリクエスト (PR) があるすべてのエージェントタスクを確認できます。作業内容を調べ、進行中のセッションに指示を送ることもできます。

このレッスンでは、次の内容を学びます。

- エージェントページを開き、cloud agent のタスクを監視します。
- 実行中のセッションに追加の機能を依頼し、作業の方向を調整します。

## シナリオ

チームはハイコントラストモードの実装を依頼した後、ライトモードも追加することにしました。サイトのスタイルと切り替え機能をすでに更新しているため、同じ作業に含めるのが合理的です。エージェントに追加要件を伝え、ハイコントラストとライトモードの両方を実装させます。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたタスクの現在の状態を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)を開きます。
2. メインペインと左ペインのタスクリストを確認します。割り当てたコードベースのドキュメント更新、ゲーム変更用 API の生成、Web サイトのハイコントラストモード追加などのタスクが表示されます。
3. 実行中のタスクを選択し、Copilot が行った作業を確認します。リポジトリのチェックアウト、作業環境の作成、MCP サーバーの設定、タスクを完了するための各手順などが含まれます。

> [!NOTE]
> 表示される手順は、Copilot の作業状況や実装方法によって異なります。

4. 右側のプルリクエスト (PR) ペインも確認します。ここでは PR と変更ファイルを確認できます。

## Cloud agent に作業の方向を伝える

実行中のタスクを確認したので、エージェントにハイコントラストモードと併せてライトモードも追加するよう依頼します。

1. ハイコントラストモード追加のセッションを選択します。タイトルは Copilot が付けた名前や作業状況によって異なります。

   ![Mission control のアクセシビリティセッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業が始まるまで数分待ちます。セッションに Copilot の作業メッセージが表示されたら、開始したことが分かります。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Copilot のエージェントタスクにある Steer active session while Copilot is working ダイアログを示すスクリーンショット](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot が依頼を認識し、作業計画に追加したことを確認します。

## Copilot の作業を続けさせる

Copilot は更新されたタスクに取り組みます。プロンプトを送信した時点で作業中だった手順を終えた後、新しい依頼を作業に取り込みます。

この作業には数分かかります。その間に、これまで学んだことを振り返りましょう。

## まとめと次のステップ

このレッスンでは、GitHub Copilot cloud agent のタスクを監視して導くための中心的な場所、Copilot agents ページを確認しました。Mission control では、進行中や完了済みのタスクを追跡し、作業内容を確認して、実行中のタスクの範囲やガイダンスを変更できます。

このレッスンでは、次の内容を学びました。

- Mission control とエージェントページで cloud agent のタスクを監視しました。
- 実行中のセッションに機能追加を依頼しました。

Copilot がアクセシビリティ機能を実装したら、次は[Copilot が作成したプルリクエストを反復して改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
