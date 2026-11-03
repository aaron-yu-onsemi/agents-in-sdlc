---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用可能なカスタマイズ、参考資料を振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Tailspin Toys の一連の作業で GitHub Copilot CLI を使いました。次の内容を行いました。

- Codespace を準備し、Copilot CLI をインストールし、プロジェクトを確認して、用意されたフィルタリングの Issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージしました。
- フィルタリングの Issue から開始し、Plan モードで方針を定め、Autopilot モードで実装し、Interactive モードでレビューしました。
- カスタム指示を追加し、既存の `quality-checks` スキルを調整して、単体テスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルタリング機能を確認しました。
- 品質保証 (QA) 用のカスタムエージェントを作成して選択し、要件、テスト範囲、スキルの結果、ブラウザーでの検証結果を評価しました。
- フィルタリングの変更全体をレビューし、フィルタリングの PR に Agent Merge を許可しました。
- コンテキスト、モデル、共有、cloud agent への任意の委任に使えるスラッシュコマンドを確認しました。

## 公開した変更

ワークショップには、更新済みの `main` からそれぞれブランチを作る 2 つの PR の節目があります。

1. **星評価:** ゲームカードに既存の `starRating` と、評価がない場合の状態を明示して表示します。
2. **フィルタリングと品質確認のワークフロー:** フィルタリングを実装し、指示を更新して機能に適用し、`quality-checks` のレポートをカスタマイズし、QA プロファイルを作成して、関連テストを追加します。

フィルタリングの計画から PR 作成まで、同じ会話とブランチを使いました。ワークショップを簡潔にするため、これらの作業を 1 つの PR にまとめました。

## 検証の種類

自動テスト、自分でのブラウザー確認、MCP を通じた Copilot のブラウザー操作でコードを確認しました。`quality-checks` スキルは単体テスト、lint、型チェックを実行し、新しい形式で報告しました。QA はこれらの結果に要件とテスト範囲のレビューをまとめて、PR の前に確認しました。

追加テストは、実際に不足しているテスト範囲を補うものです。QA の実行時に新しいテストが不要と判断されても問題ありません。マージを許可する前に、コードと検証結果を確認します。変更後は、影響を受ける検証を再実行してください。

## ベストプラクティス

Copilot に与えるコンテキストとツールが、作業の結果を左右します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成して、MCP サーバーを構成しました。これらのカスタマイズを会話間で再利用し、チームのニーズに合わせて調整できます。指示は標準を定め、スキルは繰り返し作業を記述し、カスタムエージェントは専門家の役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合わせて**モードとモデル**を選びます。構築前に方針を考えるには **Plan**、範囲を絞った変更に関与し続けるには **Interactive**、範囲の明確なタスクには **Autopilot** を使います。定型的な編集には高速なモデル、複雑な作業にはより高性能なモデルを選びます。

インフラストラクチャと同じように、コンテキストも重要です。何を、なぜ、どのように作るかを明確に伝えることで、結果が大きく変わります。

## さらに確認する

中心的なワークフローを終えました。Copilot CLI の次の機能も試せます。

- `/review` で code review エージェントに変更を分析させます。
- `/rubber-duck` で問題を説明し、別の視点を得ます。
- `/fleet` で独立したサブタスクを並行して調整します。
- `/worktree` で別のタスクを分離します。
- `/delegate` でタスクを Copilot cloud agent に送ります。

## 次のステップ

ツールの使い方を上達させるには、使い続けることが一番です。本番コード、趣味のコード、以前から作りたかった小さなアプリなどで活用してください。チームで学びを共有し、互いの経験から学びましょう。ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys を拡張したい場合は、オプションの [Foundry Backer Concierge シリーズ][foundry]に進みます。別の作業環境を試すには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
