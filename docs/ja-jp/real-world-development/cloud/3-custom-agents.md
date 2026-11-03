---
title: "演習 3 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフローの特定のタスクや分野に適した AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルでエージェントを定義し、目的に合わせた指示、ベストプラクティス、コーディングパターン、専門知識を与えます。チームの知識を再利用可能なエージェントにまとめることで、[WCAG][wcag] に沿うアクセシビリティエージェント、安全なコーディングに従うセキュリティエージェント、一貫したテストパターンを守るテストエージェントなどを作成できます。

カスタムエージェントは、プロジェクト内の `.github/agents` またはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには少なくとも `name` と `description` を含む YAML frontmatter を記述し、その後にエージェントの動作、専門知識、指示を定義する Markdown プロンプトを続けます。

### カスタムエージェントとエージェントスキルの違い

カスタムエージェントと[エージェントスキル][agent-skills-concept]には共通点があります。どちらも主に Markdown ファイルで定義し、AI に作業方法を伝えます。簡潔に区別すると、**カスタムエージェントは作業者、スキルはツール**です。

カスタムエージェントは独自のコンテキストウィンドウを持ち、作業の一部としてスキルや他のエージェントを調整します。このラボのアクセシビリティエージェントは、アクセシビリティガイドラインに沿ってサイトをレビューし、更新します。その際、プルリクエストのワークフローやテスト実行・管理のスキルなどを使えます。

> [!NOTE]
> カスタムエージェントの書き方に唯一の正解はありません。AI の他の用途と同じように、環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

カスタムエージェントを使って、次の内容を学びます。

- カスタムエージェントの定義方法。
- カスタムエージェントにタスクを割り当てる方法。

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、誰もがクラウドファンディングプラットフォームを利用できるようにしたいと考えています。現在のダークテーマは文字色と背景色のコントラストが不十分で、読みにくいと感じるユーザーがいることが分かりました。デザインチームは、ユーザーが切り替えられるハイコントラストモードの実装を依頼しました。

アクセシビリティは重要なため、カスタムエージェントを使って機能をすばやく作成します。

## アクセシビリティ用カスタムエージェントを確認する

アクセシビリティ用のカスタムエージェントが用意されています。内容を確認し、Copilot への指示を把握しましょう。

Codespace に戻り、アクセシビリティエージェントのファイルを確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` を含む YAML frontmatter を確認します。

> [!CAUTION]
> カスタムエージェントには、`name` と `description` を含む frontmatter が必要です。

3. 続くセクションを読み、アクセシブルな Web サイトのコード生成に関する主要な責務、ベストプラクティス、HTML/CSS/JavaScript のコード例、よくある問題を確認します。

## Issue を作成して割り当てる

Mission control は、環境内のエージェントを管理する場所です。Copilot cloud agent にタスクを割り当て、進捗を監視し、作業中のセッションに追加の指示を送れます。まず、ハイコントラストモードの作成を依頼します。

1. リポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して新しい Issue を開きます。
4. **Blank issue** を選択します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次を入力します。

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

7. **Create** を選択して Issue を作成します。
8. 右側で **Assign to Copilot** を選択します。
9. カスタムエージェント一覧から **Accessibility agent** を選択します。

   ![カスタムエージェントの割り当てで Accessibility が選択された画面](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドで作業を開始します。

## まとめと次のステップ

[カスタムエージェント][custom-agents]は、特定のタスクや分野に適した GitHub Copilot の専門的なアシスタントです。チームの専門知識や標準を再利用可能なエージェントにまとめ、Copilot の作業を導けます。

この演習では、次の内容を学びました。

- カスタムエージェントの定義方法。
- カスタムエージェントにタスクを割り当てる方法。

Copilot がハイコントラストモードを実装している間に、[Mission control からエージェントセッションを監視して方向付け][next-lesson]ます。

## リソース

- [カスタムエージェントについて][custom-agents]
- [組織でカスタムエージェントを使うための準備][org-custom-agents]
- [Enterprise でカスタムエージェントを使うための準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
