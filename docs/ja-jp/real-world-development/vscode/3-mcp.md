---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: エージェントモード][previous-lesson] · [次のレッスン: カスタムエージェント →][next-lesson]

エージェントモードを使って、フィルター機能を実装しました。プルリクエストを作成する前に、ブラウザーで実際に動作することを確認します。自分でアプリを操作する代わりに、**Playwright MCP サーバー**を接続し、Copilot に実際のブラウザーを操作させて機能をテストします。その後、ブランチを公開して PR を作成します。

この演習では、次の内容を学びます。

- Model Context Protocol (MCP) と、MCP サーバーが Copilot に新しいツールを追加する仕組みを確認します。
- Playwright MCP サーバーをワークスペースに追加します。
- Copilot に Playwright MCP サーバーを使わせ、ブラウザーでフィルター機能を手動テストします。
- ブランチを公開し、フィルター機能のプルリクエストを作成します。

## Model Context Protocol (MCP) とは

エージェントモードは、エディターの外部にアクセスできると、さらに多くのことを実行できます。Copilot が外部にアクセスする仕組みが Model Context Protocol (MCP) です。エージェントが外部のツールやサービスと通信するための標準です。

![エージェントモードの内部と、コンテキスト、LLM、MCP サーバーや VS Code 拡張機能が提供するツールとのやり取りを示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) を使うと、AI エージェントが外部のツールやサービスと通信できます。リソースを使って最新情報にアクセスしたり、ツールを使ってユーザーに代わって操作したりできます。

ツールやリソースへのアクセスは MCP サーバーを通じて行います。MCP サーバーは AI エージェントと外部のツールやサービスの間を仲介します。既存の API や NPM パッケージのようなローカルツールとの通信を管理します。各 MCP サーバーは、AI エージェントがアクセスできる異なるツールとリソースを提供します。

よく使われる MCP サーバーには、次のものがあります。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリ、Issue、プルリクエストを管理する API にアクセスできます。AI エージェントはリポジトリの作成や更新、Issue やプルリクエストの管理などを実行できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright によるブラウザー自動化機能を提供します。AI エージェントは Web ページへの移動、フォームへの入力、ボタンの選択などを実行できます。

ほかにも、さまざまなツールやリソースを利用できる MCP サーバーがあります。GitHub は、エコシステム内の MCP サーバーを見つけやすくし、貢献できるように [MCP レジストリ](https://github.com/mcp)を提供しています。

> [!CAUTION]
> セキュリティに関して、MCP サーバーもプロジェクト内のほかの依存関係と同様に扱ってください。使用する前にソースコードをよく確認し、発行元を検証して、セキュリティ上の影響を検討します。信頼できる MCP サーバーだけを使い、機密リソースや操作へのアクセスを許可する際は注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースから利用できる MCP サーバーを構成します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. 次のように、`github` サーバーがすでに設定されています。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

この `github` エントリはプロジェクトテンプレートに含まれているため、前の演習で Copilot が Issue のバックログを読み取れました。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使います。ローカルにインストールする必要はありません。VS Code は HTTP 経由で接続し、Copilot が初めてツールを使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

次に、2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]は、Copilot が操作できるブラウザーを提供します。機能をテストするために使います。

1. `.vscode/mcp.json` で `github` と並べて `playwright` エントリを追加し、次のようにします。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. ファイルを保存します。

リモートサーバーの `github` と異なり、`playwright` は**ローカル**サーバーです。VS Code が `npx @playwright/mcp@latest` を実行して起動します。`--headless` フラグは、画面を表示せずにブラウザーを実行するよう Playwright に指示します。Codespace にはデスクトップ画面がないため、この設定が必要です。

> [!NOTE]
> Tailspin Toys プロジェクトはエンドツーエンドテストに Playwright を使うため、Playwright が必要とするブラウザーはすでにインストールされていることがほとんどです。Copilot がブラウザーが見つからないと報告した場合は、`npx playwright install chromium` を実行して再試行するよう依頼します。

## サーバーを起動して信頼する

VS Code は MCP サーバーを**必要に応じて**起動します。Copilot がサーバーのツールを初めて必要としたときに VS Code が起動し、サーバーを信頼するか確認します。

1. `.vscode/mcp.json` で `playwright` エントリの上に **Start** 操作が表示されます。選択してサーバーを起動し、設定が正しいことを確認します。
2. VS Code からサーバーを信頼するか尋ねられた場合は、構成を確認して信頼することを選択し、サーバーを起動します。

手動での起動は省略できます。その場合は、次の手順で Copilot がツールを初めて必要としたときに VS Code がサーバーを起動し、信頼するか確認します。

## フィルター機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]は、Copilot が操作できる実際のブラウザーを提供します。ユーザーがアプリを操作して確認する代わりに、エージェントがページを開き、移動し、フィルターを適用し、結果を確認して要約します。会話を中断せず、機能が想定どおりに動くかすばやく確認できます。

Playwright MCP サーバーは、スクリーンショットではなくページの[アクセシビリティツリー][playwright-mcp-server]を使います。つまり、エージェントはボタン、リンク、リスト項目など、ラベル付きの構造化された要素を基に動作します。支援技術と同様に要素を認識するため、簡単な機能チェックがアクセシビリティの基本確認にもなります。

サーバーを接続し、アプリを起動した状態で、次のプロンプトを Copilot に送信してフィルター機能をテストします。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバーを通じてブラウザーを起動し、各手順を実行して結果を報告します。Issue に記載された受け入れ条件に照らして報告を確認してください。問題がある場合は、質問を追加するか、プルリクエストを作成する前にコードを修正するよう依頼します。

> [!NOTE]
> テストするには、アプリが `http://localhost:4321` で実行されている必要があります。開発サーバーを停止している場合は、プロンプトを送る前にもう一度起動します。Copilot が Playwright MCP サーバーを初めて使う際、ブラウザーのダウンロードが必要な場合があります。ブラウザーがないと報告されたら、`npx playwright install chromium` を実行して再試行するよう依頼します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能が動作することを確認できたら、チームのレビュー用にプルリクエスト (PR) を作成します。まず `filtering-vscode` ブランチを公開します。

1. Codespace の **Source Control** パネルを開き、Copilot が行った変更を確認します。
2. **+** アイコンを選択して変更をステージングします。
3. **Sparkle** ボタンを使ってコミットメッセージを生成します。

   ![変更内容が表示された Source Control パネルのスクリーンショット](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択して、ブランチをリポジトリにプッシュします。

## プルリクエストを作成する

github.com や GitHub コマンドラインインターフェイス (CLI) を使うなど、プルリクエストの作成方法はいくつかあります。GitHub Copilot を使っているので、PR の作成も依頼しましょう。関連する Issue を見つけ、PR と関連付けられます。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを開始します。
2. エージェント選択で **Agent** が選ばれていることを確認します。Copilot が GitHub ツールを使用できるようにします。
3. 次のプロンプトを Copilot に送信して PR を作成します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 必要に応じて **Continue** を選択し、Copilot が情報を収集して操作を実行できるようにします。GitHub ツールを初めて使うときに GitHub への認証を求められることがあります。画面の指示に従ってください。
5. Copilot が Issue を検索し、該当するものを見つけて PR を作成する様子を確認します。
6. Copilot が作成したリンクを選択してプルリクエストをレビューします。ただし、**まだマージしないでください**。

## まとめと次のステップ

この演習では、次の作業を行いました。

- Model Context Protocol (MCP) と、MCP サーバーが Copilot に新しいツールを追加する仕組みを学びました。
- Playwright MCP サーバーをワークスペースに追加しました。
- リリース前に、ブラウザーでフィルター機能をテストしました。
- ブランチを公開し、フィルター機能のプルリクエストを作成しました。

MCP サーバーを使って機能を**テスト**しましたが、Copilot を拡張する方法は MCP だけではありません。次は[カスタムエージェントを作成し][next-lesson]、アクセシビリティレビューなど、対象を絞ったタスクを効率化します。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
