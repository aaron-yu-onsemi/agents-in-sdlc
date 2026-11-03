---
title: "レッスン 5 - quality-checks スキルをカスタマイズして使う"
description: "既存の quality-checks スキルを確認し、レポート形式をカスタマイズして、フィルタリング機能を検証します。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くこと以外にも、必要な作業があります。ここまで、手動でコードを確認し、指示ファイルで標準に沿うようにしてきました。テストや lint、継続的インテグレーション (CI) のほかの作業はどうでしょうか。

このような繰り返し作業には**エージェントスキル**が適しています。スキルを使うと、Copilot に作業を適切に実行する方法を伝えられます。

このレッスンでは、次の内容を学びます。

- 既存の `quality-checks` スキルを確認します。
- 結果のレポート形式をカスタマイズします。
- スキルを再読み込みして実行します。

## シナリオ

Tailspin Toys では、単体テスト、lint、型チェックに `quality-checks` スキルを使っています。チームは、結果を読みやすくするためにレポートを改善したいと考えています。

## 指示、スクリプト、リソース

エージェントスキルは、再利用可能なタスク指示、実行可能なスクリプト、補助リソースをまとめ、必要なときにエージェントが読み込むものです。基本はスキル名のフォルダーと、その中の `SKILL.md` です。Markdown には、スキルの名前と説明を定義する frontmatter、機能の概要、呼び出すタイミングの指針を記載します。必要に応じて、スキル用のスクリプトやリソースを含むサブフォルダーも追加できます。

> [!NOTE]
> スキルに追加のフォルダーやファイルは必須ではありません。Tailspin Toys の `quality-checks` スキルは、プロジェクトに既存のコマンドを使うため、`SKILL.md` のみで構成されています。

スキルは、チームで共有して再利用できるリポジトリアセットとして `.github/skills` に配置するか、ユーザーのスキルフォルダー `~/.copilot/skills` に配置できます。

## スキルを確認する

Tailspin Toys チームが単体テスト、lint、型チェックの実行に使う `quality-checks` スキルを確認します。

1. Codespace に戻ります。Codespaces エディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. 先頭の `name` と `description` を読みます。説明によって Copilot がスキルを呼び出すタイミングを判断します。
3. 指示を読み、テストと lint の手順を確認します。
4. **Results output formatting** セクションがまだないことを確認します。

## 変更前にスキルを実行する

Copilot CLI からスキルを直接呼び出すか、自然言語で依頼できます。スキルの 3 つのチェックを実行するよう Copilot に依頼します。

1. Interactive モードでフィルタリングの会話に戻ります。
2. 次のプロンプトを使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されるレポートを確認します。

## レポートをカスタマイズする

実行した項目、成功したかどうか、ツールが報告した内容を分かりやすく示すよう、スキルを更新します。

1. `.github/skills/quality-checks/SKILL.md` を開きます。
2. ファイルの最後に次のセクションを追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. ファイルは自動的に保存されます。

## 更新したスキルを実行する

変更を反映し、更新したスキルを実行します。Copilot CLI は会話を再起動せずに、編集されたスキルを読み込み直せます。

1. 次を入力します。

   ```plaintext
   /skills reload
   ```

2. 変更前と**同じプロンプト**を使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されるレポートを読み、最初の結果と比較します。

## まとめと次のステップ

既存のエージェントスキルをカスタマイズして実行しました。このレッスンでは、次の内容を学びました。

- 単体テスト、lint、型チェックを実行する `quality-checks` スキルを確認しました。
- 結果のレポート形式をカスタマイズしました。
- スキルを再読み込みして実行しました。

この更新はフィルタリング機能の PR に含めます。次は[Playwright MCP サーバーを使って][next-lesson]Copilot にサイトを直接操作させます。

## その他のスキルの例

以下は参考となるコミュニティの例であり、追加のタスクではありません。

- [Agent Skills 仕様][skill-spec]
- [コントリビューションのワークフロー: `make-repo-contribution`][contribution-example]
- [要件ドキュメント: `prd`][prd-example]
- [図と組み込みエクスポートスクリプト: `drawio`][drawio-example]
- [ブラウザーテスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
