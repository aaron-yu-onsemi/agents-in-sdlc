---
title: "レッスン 8 - 機能の PR を作成してマージする"
description: "フィルタリング機能、指示、スキルの更新、QA プロファイル、テストを一緒にレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

フィルタリング機能の実装、指示の更新、スキルの更新、品質保証 (QA) プロファイル、テストを 1 つのブランチに保存しました。これらを一緒にレビューし、プルリクエストを作成します。星評価のプルリクエスト (PR) は自分でマージしました。今回は **Agent Merge** にプロセスを任せます。

> [!NOTE]
> 通常は、機能、指示の更新、スキルの更新、QA エージェントを複数の PR に分けます。ワークショップを簡潔にするため、フィルタリングと品質確認の作業全体を 1 つのセッションとブランチにまとめ、この PR に含めました。

このレッスンでは、次の内容を学びます。

- Agent Merge と、マージまでの手順を自動化する方法を学びます。
- 機能 PR 全体と検証結果を確認します。
- レビュー後にのみ Agent Merge を許可し、PR がマージされたことを確認します。

## シナリオ

フィルタリング機能の作業を通じて、Copilot を使って機能を計画、実装、検証しました。Tailspin Toys では、マージの許可を開発者の管理下に置きながら、残りの PR 作業を自動化したいと考えています。

## Agent Merge について

**Agent Merge** は、GitHub Copilot app でプルリクエストをマージするまでに必要な作業を自動化します。有効にすると、アプリのセッションがプルリクエストを読み取り、失敗した継続的インテグレーション (CI) チェックの修正、レビューコメントへの対応、必要に応じたリベースなど、マージを妨げている問題に取り組みます。GitHub が許可するとマージし、バックグラウンドで作業を続けます。アプリを再起動しても実行され、プルリクエストがマージされると自動的に停止します。

ここまでは自分で **Merge pull request** を選択していました。Agent Merge にその役割を任せることもできますが、コードの編集とマージには明示的な許可が必要です。マージを許可する前に、Agent Merge が実行できる操作と作業内容を確認してください。

## Agent Merge で PR を管理する

コードの作成とレビューが完了したら、Agent Merge に PR の処理を任せます。

1. エージェントピッカーで **Default agent** を選択します。
2. **Create PR** の横にあるドロップダウンを選択します。
3. **Agent Merge** を選択します。ボタンが **Agent Merge** に変わります。
4. **Agent Merge** ボタンを選択してプロセスを開始します。ドロップダウンの選択だけでは始まりません。

Agent Merge が開始され、次の作業を行います。

- タイトルと説明を含むプルリクエストを作成します。
- セッションを Issue から開始した場合は、説明本文で関連する Issue を参照します。
- リベースを行うか、ターゲットブランチとのマージ競合に対処します。
- CI を監視し、すべてのチェックが成功するようにします。
- 他の開発者や Copilot code review からのフィードバックを確認し、コメントを解決するために変更を加えます。
- 必要に応じて、すべてが成功したら PR を自動的にマージします。

すべてのチェックが成功したら、Agent Merge に PR のマージも任せます。

5. マージを許可する前に、PR の差分と検証結果をレビューします。
6. **Agent Merge** の横にあるドロップダウンを選択します。
7. **Merge pull request** にチェックが付いていることを確認します。
8. エージェントに明示的にマージを依頼します。

   ```plaintext
   Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
   ```

9. 進行状況を確認し、報告された問題を解決します。続行前に、PR の状態が **Merged** になっていることを確認します。マージ許可が有効になっただけ、またはエージェントが実行中というだけでは、マージ完了を意味しません。

> [!IMPORTANT]
> Agent Merge は、リポジトリの保護や権限不足を回避しません。続行前に阻害要因を解決してください。

## まとめと次のステップ

コード生成、テストと検証、プルリクエストの処理など、開発プロセスの複数の部分を自動化しました。このレッスンでは、次の作業を行いました。

- Agent Merge と、マージまでの手順を自動化する方法を学びました。
- 機能 PR 全体と検証結果を確認しました。
- レビュー後にのみ Agent Merge を許可し、PR がマージされたことを確認しました。

次は[既存の Canvas を使い、トリアージ用 Canvas を作成します][next-lesson]。エージェントと一緒に作業を確認、計画、視覚化する方法を学びます。

## リソース

- [GitHub Copilot app で Issue とプルリクエストを管理する][managing-issues-prs]
- [GitHub Copilot app について][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
