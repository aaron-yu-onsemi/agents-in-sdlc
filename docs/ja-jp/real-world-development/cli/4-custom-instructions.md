---
title: "レッスン 4 - カスタム指示で Copilot を導く"
description: "リポジトリの指示を確認してドキュメント標準を追加し、フィルタリング機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使う際はコンテキストが重要です。タスクを特定の方法で進めたい場合は、その指針を Copilot が参照できるようにします。[指示ファイル][instruction-files]には、求めるコードだけでなく、その構成方法も記述できます。フィルタリング機能を実装したところで、Copilot が使った指示を確認し、ドキュメント標準を追加して、コードに適用します。

このレッスンでは、次の内容を学びます。

- リポジトリの指示とパス指定の指示ファイルがエージェントに適用される方法を確認します。
- コーディング標準が守られるよう、指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には開発手法に関するガイドラインがあります。

- コメントにはコードの内容を繰り返すのではなく、意図や自明でない判断を説明します。
- `db/` と `src/lib/` のエクスポート関数には、目的、パラメーター、戻り値を説明する TSDoc/JSDoc を記述します。`db` 引数を注入できる場合は、その引数についても記述します。
- 再利用可能な Astro コンポーネントには `Props` の契約を記述し、関連コードが変わったらコメントも更新します。
- 既存の書式設定と lint の指針を維持します。

指示ファイルを使い、Copilot がこれらの開発慣行に沿って作業できるようにします。

## 指示ファイル

カスタム指示を使うと、コーディングスタイルや要件に関するコンテキストと希望を Copilot に伝え、より適切な提案やコード生成を促せます。リポジトリ全体、または特定のファイル種別に指示を設定できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md`: リポジトリへの**すべての**要求に適用される単一の指示ファイルです。多くの要求に関係するプロジェクト全体の情報を記載します。
- `.github/instructions/*.instructions.md`: 言語、ファイル種別、タスクに応じた指針を記載します。

> [!NOTE]
> サポートされる指示形式や適用範囲は環境によって異なります。特定の形式を使う前に、[カスタム指示のサポート情報][custom-instructions-support]を確認してください。

## プロジェクトのカスタム指示ファイルを確認する

スタータープロジェクトに含まれる指示ファイルを確認します。

1. Codespace に戻ります。
2. Codespaces のエディターで `.github/copilot-instructions.md` を開きます。
3. プロジェクトの簡単な説明とコーディング指針を確認します。この指示はリポジトリでの Copilot とのすべてのやりとりに適用されます。
4. `.github/instructions` フォルダーを開き、Astro、Drizzle データ層、テストなどの指示ファイルを確認します。
5. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。この glob に一致するファイルに指示が適用されます。
6. `.github/instructions/drizzle.instructions.md` を開き、他の指示ファイルやプロジェクト内ファイルへのリンクを確認します。指示を再利用可能な小さなファイルに分け、参考例を Copilot に示せます。

## チームのガイダンスに合わせて指示を更新する

既存のファイルには不足があります。新たに生成する TypeScript に TSDoc コメントを追加するよう、`copilot-instructions.md` を更新します。

1. `.github/copilot-instructions.md` の **Code formatting guidance** セクション (35 行目付近) を探します。
2. 最後の箇条書きとして、次を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新した指示を適用する

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。更新した指示を適用するには、フィルタリングの会話を終了して再開します。

1. フィルタリングの会話が開いている場合は、`/exit` を入力します。会話は自動的に保存されます。
2. リポジトリのルートで `game-filters-cli` ブランチをチェックアウトし、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   選択画面が表示されたら、フィルタリングの会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. 更新した指示を適用するよう Copilot に依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。追加された TSDoc がコードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot が指示ファイルからコンテキストを取得する方法を確認し、新しい標準を機能に適用しました。具体的には、次の作業を行いました。

- リポジトリの指示とパス指定の指示ファイルがエージェントに適用される方法を確認しました。
- コーディング標準が守られるよう指示ファイルを更新しました。
- 指示ファイルがコードに与える影響を確認しました。

次は[再利用可能な quality-checks スキルをカスタマイズして実行し][next-lesson]、lint とテストを一貫して実施します。

## リソース

- [Copilot CLI 用のカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート][custom-instructions-support]
- [カスタム指示のベストプラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
