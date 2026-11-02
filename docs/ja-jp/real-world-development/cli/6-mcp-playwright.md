---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI で Playwright MCP を確認または設定し、ブラウザーでフィルター機能を試します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

コードを書くこと以外にも、データや外部サービスを使ったり、追加の自動化機能を Copilot に利用させたりする必要があります。ここで MCP サーバーが役立ちます。MCP サーバーを使うと、Copilot は CLI に組み込まれている機能以外のツールやサービスも利用できます。

このレッスンでは、次の内容を学びます。

- Model Context Protocol (MCP) とは何か、Copilot CLI がどのように使うかを理解します。
- まだ利用できない場合は Playwright MCP サーバーを追加します。
- エージェントにブラウザーを操作させ、フィルター機能を確認します。

## シナリオ

ユニットテストやエンドツーエンドテストは重要ですが、UI の更新を検証するには、実際に UI を操作する必要があります。Copilot が作業中の Web サイトをユーザーと同じように操作できるようにし、期待どおりの変更かどうかをさらに自動的に確認します。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] は、AI エージェントが外部のツールやサービスと通信するための仕組みです。MCP を使うと、AI エージェントは外部のツールやサービスとリアルタイムに通信できます。最新情報にアクセスしたり、ユーザーに代わって操作を実行したりできます。

ツールやリソースへのアクセスは MCP サーバーを通じて行います。MCP サーバーは AI エージェントと外部のツールやサービスの間を仲介します。各 MCP サーバーは、AI エージェントが利用できる異なるツールとリソースを提供します。

よく使われる MCP サーバーには、次のものがあります。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、Issue、プルリクエストを管理する API にアクセスできます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright によるブラウザー自動化機能を提供します。

ほかにも多くの MCP サーバーがあります。GitHub は、エコシステム内のサーバーを見つけやすくし、貢献できるように [MCP レジストリ][mcp-registry]を提供しています。

> [!CAUTION]
> セキュリティに関して、MCP サーバーもプロジェクト内のほかの依存関係と同様に扱ってください。使用する前にソースコードを確認し、発行元を検証して、セキュリティ上の影響を検討します。

## Playwright MCP サーバーを追加する

Playwright MCP サーバーがまだ利用できない場合は、Copilot がユーザーと同じように Web サイトを操作できるよう、サーバーを追加します。

> [!NOTE]
> Playwright MCP がすでに Copilot CLI に設定されている場合は、セットアップを省略して次のセクションに進みます。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力し、MCP サーバー追加ダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. 名前として `playwright` を入力し、<kbd>Tab</kbd> を押します。
4. サーバーの種類として **STDIO** を選び、<kbd>Enter</kbd> を押してから <kbd>Tab</kbd> を押します。
5. **Command** ダイアログに次のテキストを貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して、新しい MCP サーバーを保存します。
7. <kbd>Esc</kbd> を押して MCP ダイアログを閉じます。

## Playwright を使って Copilot に機能を確認させる

先ほどは、機能が想定どおり動作することを手動で確認しました。今度は Copilot から利用できる Playwright MCP サーバーを使って、同じ確認を行います。

1. 次のプロンプトを送信し、Copilot に Playwright MCP サーバーで機能を検証するよう依頼します。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> Copilot は通常、自動的に MCP サーバーを使うため、明示的に指定する必要はありません。ただし、使用するツールがわかっている場合は、プロンプトで案内すると結果が安定し、トークンを節約できます。

2. Copilot がブラウザーで機能を確認する手順を表示する様子を確認します。
3. レポートを読み、すべて期待どおりに動作していることを確認します。

Copilot はサーバーを起動し、Playwright で Web サイトを操作してからサーバーを停止し、結果を報告します。

## まとめと次のステップ

Copilot CLI から Playwright MCP サーバーを使って、実際のブラウザーで機能を確認しました。次のことを行いました。

- Model Context Protocol (MCP) とは何か、Copilot CLI がどのように使うかを学びました。
- まだ利用できない場合は Playwright MCP サーバーを追加しました。
- エージェントにブラウザーを操作させ、フィルター機能を確認しました。

次はスキルとブラウザーツールを組み合わせる[QA カスタムエージェント][next-lesson]を作成します。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP サーバーを追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
