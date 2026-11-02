---
title: "レッスン 7 - QA エージェントを作成して使う"
description: "Issue の要件、quality-checks スキル、Playwright MCP を組み合わせる QA カスタムエージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` スキルで自動チェックを実行し、Playwright MCP でブラウザーからフィルター機能を確認しました。次は、これらの機能を明確な QA プロセスを持つカスタムエージェントにまとめます。

このレッスンでは、次の内容を学びます。

- カスタムエージェントが指示、スキル、MCP ツールと連携する方法を確認します。
- 再利用可能な品質保証プロファイルを作成して確認します。
- QA エージェントを選択し、フィルター機能の Issue に対する調査結果をレビューします。

## シナリオ

Tailspin Toys は、プルリクエスト (PR) を作成する前に、要件、コード品質、自動チェック、テストカバレッジ、ブラウザーでの動作を一貫してレビューしたいと考えています。カスタムエージェントでこの QA プロセスを調整し、再利用可能なレポートを作成できます。

## カスタムエージェントとは

カスタムエージェントは、Markdown プロファイルで定義する Copilot の専門的なバージョンです。プロファイルにエージェントの目的、指示、利用可能なツールを記載します。このワークショップでは `.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

これまでに使ったカスタマイズには、それぞれ異なる役割があります。リポジトリの指示はチームの標準を示し、`quality-checks` スキルは繰り返し実行するチェックをまとめ、Playwright MCP はブラウザーツールを提供します。QA プロファイルは、それらを使って要件を評価し、結果を報告するよう Copilot に指示します。既存の機能に代わるものではなく、別の会話も必要ありません。

## QA プロファイルを作成する

機能の PR を開く前に、再利用可能な QA プロファイルを Copilot に作成してもらいます。QA が実行するチェックと、作業上の制約をプロファイルに記載します。

1. Codespace に戻り、フィルター機能の会話が Interactive モードであることを確認します。
2. 既定のエージェントに次のプロンプトを送信し、カスタムエージェントを作成します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

新しいエージェントを使う前に、Copilot が意図した QA ワークフローと制約を設定できたことを確認します。これにより、検証だけを依頼したのに不完全なエージェントや範囲の広すぎるエージェントが機能を変更するのを防げます。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. フロントマターを確認します。`description` は必須です。`name` は省略可能ですが、設定するとエージェントの表示名が明確になります。
3. プロファイルの指示を読み、QA が要件を起点とし、リポジトリの指示に従い、`quality-checks` スキルを実行し、Playwright MCP を使うことを確認します。
4. QA が根拠を示してレポートを作成し、実装コードを変更する前に確認を求め、変更をコミットせず、プルリクエストも作成しないことを確認します。
5. これらの役割や制約が抜けている場合は、続行する前に既定のエージェントに修正を依頼します。

## Issue を基に QA を実行する

Copilot CLI は、会話の開始時にプロジェクトのエージェントを読み込みます。プロファイルを作成した後、同じフィルター機能の会話を再開し、QA を選択します。これにより、Issue と計画の決定事項をそのまま利用できます。

1. 次のプロンプトでエージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> エージェントを作成した直後は、利用可能なエージェントの一覧にまだ表示されないことがあります。次のコマンドで有効にできます。

2. 次のプロンプトを送信し、QA エージェントに機能をレビューするよう依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントが作業を開始します。
4. 作業が終わったら、作成されたレポートを確認します。

## まとめと次のステップ

ワークフローに再利用可能な専門的な役割を追加し、その作業をレビューしました。このレッスンでは、次の内容を学びました。

- カスタムエージェントが指示、スキル、MCP ツールと連携する方法
- 再利用可能な品質保証プロファイルを作成して確認する方法
- QA エージェントを選択し、フィルター機能の Issue に対する調査結果をレビューする方法

機能実装、スキルの更新、QA プロファイル、テスト、検証レポートが揃いました。次は[機能 PR にまとめ、Agent Merge を使います][next-lesson]。

## リソース

- [Copilot CLI 向けのカスタムエージェントを作成する][create-agents]
- [カスタムエージェントの構成][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
