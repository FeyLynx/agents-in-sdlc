---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前の数回の演習では、Copilot cloud agent にユーザー体験の改善と機能追加を目的とする 3 つのタスクを依頼しました。cloud agent は非同期かつ自律的に作業しますが、タスクの進行状況を監視することも重要です。

cloud agent に割り当てたタスクは、GitHub.com の[エージェントページ][agents-page]など、さまざまなツールを使って管理できます。この mission control では、オープンなプルリクエスト (PR) があるすべてのエージェントタスクを確認できます。実行内容を調べ、進行中のセッションに指示を追加することもできます。

このレッスンでは、次の内容を学びます。

- エージェントページを開き、cloud agent のタスクを監視します。
- 実行中のセッションに指示を追加し、機能を拡張します。

## シナリオ

エージェントに高コントラストモードの作成を依頼した後、チームはライトモードも追加するよい機会だと考えました。サイトのスタイルを更新し、切り替え機能を追加しているため、一緒に実装するのが合理的です。高コントラストモードだけでなくライトモードも追加するよう、エージェントの作業に指示を加えます。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたタスクの状況を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)に移動します。
2. メインペインと左ペインの両方に表示されるタスク一覧を確認します。たとえば、次のタスクが表示されます。
    - コードベースのドキュメントの更新
    - ゲーム変更用 API の生成
    - Web サイトへの高コントラストモードの追加
3. 実行中のタスクを選択し、Copilot が実行した作業を確認します。たとえば、次の作業が表示されます。
    - リポジトリからコードをチェックアウトする
    - Copilot の環境を作成する
    - MCP サーバーをセットアップする
    - 割り当てられたタスクを完了するための各種手順を実行する

> [!NOTE]
> 表示される手順は、Copilot の作業状況や進め方によって異なります。

4. 右側に表示されるプルリクエスト (PR) ペインも確認します。PR と変更されたファイルを確認して、作業をさらに監視できます。

## cloud agent に指示を追加する

実行中のタスクを確認できたので、高コントラストモードを作業中の Copilot にライトモードの切り替えも追加するよう依頼します。

1. 高コントラストモードの追加に関連するセッションを選択します。Copilot が付けた名前や作業状況によって、タイトルは異なります。

    ![mission control のアクセシビリティセッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業が始まったことが表示されるまで数分待ちます。次のようなメッセージが表示されたら作業中です。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![エージェントページで cloud agent のタスクを表示し、作業中のセッションに指示を追加するダイアログを強調したスクリーンショット](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業の計画に追加する様子を確認します。

## Copilot に作業を任せる

前回と同様に、Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で実行していた手順が終わると、新しい要望を作業に反映します。

今回も完了まで数分かかります。その間に休憩し、これまでに学んだことを振り返りましょう。

## まとめと次のステップ

このレッスンでは、GitHub Copilot cloud agent のタスクを監視し、指示するための中心的な場所である Copilot agents ページを確認しました。この mission control では、実行中と完了済みのタスクを追跡し、実行中の作業をレビューし、タスクの範囲を変更したり追加の指示を出したりできます。

次の内容を学びました。

- mission control とエージェントページで cloud agent のタスクを監視する方法
- 実行中のセッションに指示を追加して機能を拡張する方法

Copilot がアクセシビリティ機能を実装している間に、[Copilot が作成したプルリクエストを改善します][next-lesson]。

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
