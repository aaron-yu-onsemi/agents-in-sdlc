---
title: "レッスン 3 - エージェントモード: Plan と Autopilot"
description: "Plan で方針に合意し、Autopilot で Issue からフィルタリング機能を構築し、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

まずプロジェクトに小さな機能を追加しました。しかし、大きな変更にはより堅牢なプロセスが必要です。GitHub Copilot CLI は組織の既存の開発フローに沿って動作します。このレッスンから数回にわたり、一般的なエージェント主導の開発プロセスを実践します。Issue から新機能を作り、コードと動作を確認し、最終的にプロジェクトへマージします。

> [!NOTE]
> 機能の作業を続ける間は、同じ会話とブランチを使います。通常はファイルの種類ごとに異なるブランチや PR を使いますが、ここでは中心的な概念に集中できるよう手順を簡略化します。

このレッスンでは、次の内容を学びます。

- GitHub Issue から Copilot との会話を開始します。
- Plan モードで要件を定義します。
- Autopilot モードで新機能を実装します。
- コードをレビューします。
- 転送されたブラウザーで機能を手動で検証します。

この機能の作業を続けながら、リポジトリの指示を更新し、既存の `quality-checks` スキルをカスタマイズし、MCP による検証を追加して QA エージェントを作成し、機能の PR を開きます。

## シナリオ

Tailspin Toys のカタログが増え、訪問者がカテゴリーとパブリッシャーでゲームを絞り込む必要が出てきました。バックログの Issue に機能は記載されていますが、カテゴリーの組み合わせ方など、実装前に合意が必要な点もあります。Plan モードで決定し、Autopilot による範囲を定めた実装を承認します。

## 背景

AI コーディングエージェントを開発フローに導入しても、基本は変わりません。むしろ、より重要になります。多くの開発者は、次のような流れで作業します。

1. 必要な作業を記載した Issue を開きます。
2. 実装内容を計画します。
3. コードを作成してレビューします。
4. テストを実行して検証します。
5. 新機能を手動で検証します。
6. プルリクエスト (PR) を作成します。
7. コードレビューと継続的インテグレーションが成功したら、コードをマージします。

> [!NOTE]
> 詳細な手順はチームや組織によって異なりますが、上記の流れを基本にしている場合が多くあります。

この標準的な手順に沿うことで、AI が生成したコードが要件を満たし、人が書いたコードと同じレビューを受けるようにできます。

## 会話モード

**会話モード**は、エージェントの自律性を制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押してモードを切り替えます。

- **Interactive**: エージェントが変更を提案し、続行前にユーザーの入力を待ちます。
- **Plan**: エージェントが最初に計画を作成し、プロジェクトファイルの編集は行いません。
- **Autopilot**: エージェントが自律的にコードを書き、テストを実行し、タスクが完了するまで反復します。

Plan モードで開始して計画をレビューし、その後 Autopilot で実装します。

## Issue から開始する

フィルタリング作業を始める前に、Codespace に戻り、リポジトリとターミナルの準備をします。

1. Codespace に戻ります。停止中の場合は再起動します。
2. 星評価の PR がマージ済みであることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
4. `main` を更新し、フィルタリング用ブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての Issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** というタイトルの Issue を選択します。
9. <kbd>C</kbd> を押して Issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトの先頭に `#7` (または同様の番号) が表示されます。`#` を使うと、GitHub 上の Issue やプルリクエスト (PR) をコンテキストに追加できます。

## フィルタリング機能を計画する

計画を立てると、Copilot がコードを書く前に実装方針を決められます。複雑な作業では、事前に計画することをお勧めします。Plan モードに切り替えて、Copilot に計画を作成させましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプト下のモード表示が **Plan** であることを確認します。
2. 直前の手順で追加した Issue の参照に続けて、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調べ、適切な実装方法を検討して計画を作成します。

3. フィルタリングの動作について質問された場合は、希望に沿って回答します。
4. 計画ができたら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して計画を展開します。
5. 計画を上下にスクロールしてレビューします。
6. 決定した内容と異なる部分があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画ができてレビューも終わったら、Autopilot で実装します。Autopilot では、Copilot がタスクの完了を判断するまで問題に取り組みます。

1. **Accept plan and build on autopilot (recommended)**、またはインストール済みのバージョンに表示される同等の選択肢を選びます。
2. プロンプト下のモード表示が **Autopilot** であることを確認します。
3. Copilot が計画に沿って反復し、コードを生成してテストを実行する様子を確認します。

> [!NOTE]
> 承認すると実装がすぐ始まる場合があるため、先に計画をレビューしてください。依存関係の不足やポートの競合が報告された場合は、チェック完了と判断する前にセットアップの問題を解決します。

## 実装をレビューして検証する

コードが生成されたら、マージ前にレビューします。サイトを実行して動作も確認しましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モード表示が **Plan** または **Autopilot** ではないことを確認します。
2. `/diff` を入力し、フィルタリングの実装とテストを確認します。
3. 結果を Issue および計画時に決めた内容と照らし合わせます。
4. レビューが終わったら <kbd>Esc</kbd> を押して差分ビューを閉じます。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に解決を依頼します。

## 新機能を確認する

コードに問題がないように見えても、実際に動くか確認しましょう。アプリを起動し、Codespaces の転送ポートからサイトを開きます。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能になったら、**Open in Browser** を選択します。
3. 計画で合意したカテゴリーとパブリッシャーのフィルター、およびその組み合わせを試します。
4. リセットと検索結果が空の場合の動作が、Issue と決定内容に合っていることを確認します。
5. Codespace に戻り、Copilot に起動した開発サーバーを停止するよう依頼します。

## まとめと次のステップ

会話モードを使って機能を構築し、レビューしました。このレッスンでは、次の作業を行いました。

- GitHub Issue から Copilot との会話を開始しました。
- Plan モードで要件を定義しました。
- Autopilot モードで新機能を実装しました。
- コードをレビューしました。
- 転送されたブラウザーで機能を手動検証しました。

次は、[カスタム指示を使用して][next-lesson]、文書化されたプラクティスにコードを従わせる方法を確認します。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンド リファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
