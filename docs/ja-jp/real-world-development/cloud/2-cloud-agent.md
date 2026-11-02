---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

解決していないセキュリティの問題、更新が必要なレガシーコード、実装する時間がなくバックログに残った機能リクエストなど、技術的負債に悩まされていない組織はほとんどありません。GitHub Copilot cloud agent は、コードの更新や機能の追加などのタスクを自律的に実行できます。エージェントの作業が完了すると、人間の開発者がレビューできるドラフト PR が作成されます。面倒な作業を委任して開発を加速し、開発者はより大きな課題に集中できます。

この演習では、Copilot cloud agent を使って次の内容を学びます。

- コード生成環境をカスタマイズする方法
- 操作を安全に実行する方法
- 明確に範囲を定めた Issue の重要性
- Issue を Copilot に割り当てる方法

## シナリオ

Tailspin Toys には、解決したい技術的負債があります。最初のサイトを開発した契約開発者がドキュメントを適切に整備しませんでした。実際には、ドキュメントがまったくありません。まず、アプリケーションのエクスポートされたすべての関数に TSDoc のドキュメントコメントを追加します。

さらに、デザインチームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリーのほかのゲームを表示します。まだ完成度の高い実装は必要ありませんが、UX の受け入れテストに使えるものを求めています。この作業は現在のブロッカーですが、ほかに優先度の高い Issue があります。

どちらも、優先度が下がりやすいタスクです。Copilot cloud agent に割り当てると、非同期で作業を進めてもらえます。その間、別の作業を行い、後から Copilot の結果をレビューして想定どおりか確認できます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同様にバックグラウンドでタスクを実行できます。[GitHub Issue を Copilot に割り当てる][assign-issue]など、いくつかの方法で依頼できます。割り当てると、Copilot は進行状況を追跡するドラフトプルリクエストを作成し、環境を準備してタスクに取り組みます。作業中も完了後も Copilot のセッションを確認できます。提案された解決策のレビュー準備ができると、プルリクエストで通知されます。

## 適切な範囲の指示が重要

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。一見簡単な操作でも、詳しく調べると複雑さが明らかになります。[Copilot cloud agent に依頼するタスクの範囲を慎重に決めてください][cloud-agent-best-practices]。AI ペアプログラマーとして、段階を踏み、学び、試し、状況に応じて調整しましょう。生成 AI を加えても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習では、カスタム指示と Copilot の動作を導く方法を学びました。すでにカスタム指示の演習を完了していれば、`.github/copilot-instructions.md` が機能することを確認しています。まだの場合は、ここで簡単に確認しましょう。Copilot cloud agent に作業を割り当てる前に、このリポジトリに含まれる指示ファイルを読み取り専用で確認し、後からどのように影響するかを確かめます。

GitHub の Web UI または起動済みの Codespace で、次のファイルを開きます。

- `.github/copilot-instructions.md`: **Code standards** セクション、特に TypeScript 規約と TSDoc ドキュメントコメントの要件を確認します。
- `.github/instructions/unit-tests.instructions.md`: `applyTo` フロントマターが `**/*.test.ts` ファイルに指示を適用することを確認します。

次のセクションで `Code lacks documentation` Issue を cloud agent に割り当てるとき、作成されたプルリクエストに TSDoc ドキュメントコメント、TypeScript 規約、コメントヘッダーが反映されているか確認してください。後の演習で PR をレビューするときにも説明します。

## Copilot cloud agent の開発環境をセットアップする

コードを作成する際は、通常、必要な開発環境を整え、状態を確認するためのセットアップスクリプトを実行します。Copilot にタスクを割り当てる場合も同様です。Copilot はソフトウェアエンジニアのように作業します。

Cloud agent は GitHub Actions を使って作業環境を構築します。作業開始前に実行する[セットアップワークフロー][setup-workflow]を `.github/workflows/copilot-setup-steps.yml` に作成して、環境をカスタマイズできます。これにより、必要な開発ツールと依存関係を利用できるようになります。ラボを進めて学習できるよう、このファイルは事前設定済みです。Node.js、プロジェクトの依存関係、エンドツーエンドテスト用ブラウザー、単一 Astro アプリ用にマイグレーションとシードが済んだ SQLite データベースを Copilot が利用できます。

```yaml
name: "Copilot Setup Steps"

# Allows you to test the setup steps from your repository's "Actions" tab
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Set the permissions to the lowest permissions possible needed for *your steps*. Copilot will be given its own token for its operations.
    permissions:
      # If you want to clone the repository as part of your setup steps, for example to install dependencies, you'll need the `contents: read` permission. If you don't clone the repository in your setup steps, Copilot will do this for you automatically after the steps complete.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app setup - Node.js (the whole app is now Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migrate + seed the local SQLite database so builds and tests have data.
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub ワークフローファイルとよく似ていますが、いくつか重要な点があります。

- `copilot-setup-steps` という 1 つのジョブが定義されています。このジョブは、Copilot がプルリクエストの作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーが設定されています。リポジトリの **Actions** タブからワークフローを手動実行できるため、Copilot の実行を待たずに正しく動作するかテストできます。

## ドキュメントを追加する

ドキュメントの重要性は広く理解されていますが、情報が古いままのプロジェクトや、ドキュメントがまったくないプロジェクトもあります。こうした技術的負債は放置されやすく、生産性を下げ、コードベースの保守や新しいメンバーの参加を難しくします。Copilot はドキュメントの作成を得意とするため、Copilot cloud agent に依頼するのに適したタスクです。バックグラウンドで必要なドキュメントを作成してもらい、後の演習で結果をレビューします。

1. 新しいブラウザータブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択して Issue を作成します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次のテキストを入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して Issue を作成します。
8. 右側で **Assign to Copilot** を選択して割り当てダイアログを開きます。

  ![Issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく作成されたプルリクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のようになります。新しい PR が一覧に表示されない場合は、少し待ってからブラウザーを更新します。
12. 数分後、Copilot が TODO リストを作成していることを確認できます。

> [!NOTE]
> Copilot が PR に TODO リストを表示するまで数分かかることがあります。Copilot が環境を構築し、前述のワークフローを実行して、プロジェクトを分析し、問題の解決方法を決めるためです。

13. リストと、Copilot が実行するタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が Issue の作業を開始したことを示す更新を確認します。
15. **View session** ボタンを選択します。

  ![Copilot のセッションビュー](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された表示を確認するために、ウィンドウを更新する必要がある場合があります。

16. ライブセッションをスクロールし、Copilot が問題を解決する様子を確認します。コードを調べて状態を把握し、計画を考えて判断し、コードを作成している様子がわかります。

完了まで数分かかることがあります。Copilot cloud agent の主な目的は、タスクを非同期で実行し、その間にほかの作業をできるようにすることです。別のタスクも Copilot cloud agent に割り当て、その間にアプリへ機能を追加するコードを書きます。

## ゲーム詳細ページに関連ゲームを追加する

GitHub Copilot cloud agent の大きな利点は作業を分担できることです。デザインチーム向けの関連ゲームの追加に時間がかからないとしても、その時間をほかの作業に使えます。このタスクも Copilot cloud agent に割り当てます。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択して、空のテンプレートを使います。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次のテキストを入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選択して Issue を作成します。
8. 右側で **Assign to Copilot** を選択して割り当てダイアログを開きます。

  ![Issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

ほどなくして、Issue の最初のコメントに 👀 が表示され、Copilot が作業を開始したことがわかります。

![Issue に Copilot が取り組んでいることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼にも取り組んでいます。Copilot cloud agent はソフトウェアエンジニアと同じように作業するため、常に監視する必要はありません。完了後にレビューできます。次はカスタムエージェントの作成と使用を確認します。

## まとめと次のステップ

このレッスンでは [GitHub Copilot cloud agent][copilot-agents] を確認しました。cloud agent に Issue を割り当てると、非同期に作業を実行できます。技術的負債の解消、新機能の作成、別のフレームワークからのコード移行などを依頼できます。

次の内容を学びました。

- コード生成環境をカスタマイズする方法
- 操作を安全に実行する方法
- 明確に範囲を定めた Issue の重要性
- Issue を Copilot に割り当てる方法

cloud agent がバックグラウンドで作業している間に、カスタムエージェントを作成して使う方法を確認します。[Copilot cloud agent は MCP サーバーも利用でき][cloud-agent-mcp]、前のモジュールで学んだカスタム指示にも対応しています。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローのベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
