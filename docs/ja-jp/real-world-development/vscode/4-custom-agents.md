---
title: "演習 4 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフロー内の特定のタスクや分野に合わせて、専門的な AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルでエージェントを定義すると、特定の作業をより効果的に行うための指示、ベストプラクティス、コーディングパターン、分野固有の知識を Copilot に与えられます。チームの専門知識を再利用可能なエージェントとしてまとめることができます。たとえば、[WCAG][wcag] 準拠を確認するアクセシビリティエージェント、安全なコーディングを確認するセキュリティエージェント、一貫したテストパターンを守るテストエージェントなどです。

カスタムエージェントは、プロジェクト内の `.github/agents` フォルダー、またはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには少なくとも `name` と `description` を含む YAML フロントマターがあり、その後にエージェントの動作、専門知識、指示を定義する Markdown プロンプトを記載します。

### カスタムエージェントとエージェントスキルの違い

カスタムエージェントと[エージェントスキル][agent-skills-concept]には、役割が一部重なるところがあります。どちらも主に Markdown ファイルで定義され、AI に作業の進め方を伝えます。わかりやすく区別すると、**カスタムエージェントは作業者、スキルはツール**です。

カスタムエージェントには専用のコンテキストウィンドウがあり、作業の一環としてスキルやほかのエージェントを連携させます。このラボのアクセシビリティ用カスタムエージェントは、アクセシビリティガイドラインに照らしてサイトをレビューし、更新します。その作業の一環で、プルリクエストのワークフロースキルやテスト実行・管理スキルを呼び出すこともできます。

> [!NOTE]
> カスタムエージェントの作成に唯一の正解はありません。AI のほかの使い方と同様に、環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディングプラットフォームを利用できるようにすることに取り組んでいます。最近、現在のダークテーマではテキストと背景色のコントラストが十分ではなく、読みにくいと感じるユーザーがいることがフィードバックでわかりました。このアクセシビリティ上の課題に対応するため、デザインチームはユーザーが切り替えられる高コントラストモードの実装を依頼しました。

アクセシビリティは重要なため、できるだけ早く実装する必要があります。カスタムエージェントを使って機能を生成します。

この演習では、次の内容を学びます。

- 既存のアクセシビリティ用カスタムエージェントを確認します。
- Copilot Chat でアクセシビリティエージェントを使い、高コントラストモードを実装します。

## アクセシビリティ用カスタムエージェントを確認する

アクセシビリティ用カスタムエージェントはすでに用意されています。Copilot をどのように導くか理解するため、内容を確認しましょう。

Codespace に戻り、ターミナルを開いて `main` からアクセシビリティ作業用の新しいブランチを作成します。演習 3 のフィルター PR とは分けて作業します。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` フィールドと `description` フィールドを含む YAML フロントマターを確認します。

> [!CAUTION]
> `name` と `description` を含むフロントマターは、カスタムエージェントに必須です。

3. 続くセクションを読み、次の内容を確認します。
   - アクセシブルな Web サイトのコード生成における主な責任
   - アクセシビリティのベストプラクティス
   - HTML、CSS、JavaScript のコード例
   - よくある問題と避けるべき事項

## Copilot Chat でカスタムエージェントを使う

VS Code では、`.github/agents` に定義されたすべてのカスタムエージェントが Copilot Chat ビュー下部のエージェント選択に表示されます。カスタムエージェントを選択すると、そのエージェントの指示とツールに合わせたチャットセッションを開始できます。

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めてください。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

1. Chat ビューのエージェント選択で **Agent** が選ばれていない場合は選択します。

   ![Chat ビューのエージェント選択を示すスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

2. チャットビューの下部にあるエージェント選択を開きます。既定では、選択中のエージェントとして **default** が表示されます。
3. エージェント一覧から **Accessibility agent** を選びます。
4. 次のプロンプトをアクセシビリティエージェントに送信します。

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat がコードベースを調べ、変更を提案してプロジェクトファイルに適用します。各編集はファイルパスと差分とともにチャットに表示されます。
6. エージェントが変更した作業ファイルが増えると、**Files changed** の表示が更新されます。

> [!NOTE]
> 実際にリポジトリを変更するため、処理には数分かかることがあります。作業中、Astro コンポーネント、CSS、関連テストなどの既存ファイルが編集されます。

次の演習では、進行中の作業をレビューし、エージェントに追加の指示を出します。

## まとめと次のステップ

このレッスンでは、特定のタスクや分野に対応する GitHub Copilot のカスタムエージェントを確認しました。カスタムエージェントを使うと、チームの専門知識や標準を再利用可能な形にまとめ、Copilot がさまざまな作業を効果的に行えるようにします。

次の内容を学びました。

- カスタムエージェントの定義方法
- Copilot Chat のエージェントモードでカスタムエージェントを使う方法

次は[エージェントの作業を監視して方向付けます][next-lesson]。変更を確認し、同じセッションにライトモードの切り替え機能も追加します。

## リソース

- [カスタムエージェントについて][custom-agents]
- [IDE でカスタムエージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタムエージェント][custom-agents-vscode]
- [カスタムエージェントの構成][custom-agents-config]
- [awesome-copilot のカスタムエージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
