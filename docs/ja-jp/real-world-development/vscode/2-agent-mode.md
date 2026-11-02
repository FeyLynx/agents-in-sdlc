---
title: "演習 2 - Copilot エージェントモードで機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの単純な変更でも、通常は複数のファイルを更新し、テストを実行するなどの作業が必要です。開発者は、関連するファイルを見つけ、変更を加え、テストを実行し、問題をデバッグし、更新し忘れたファイルを見つけて、さらに変更を加える必要があります。作業は際限なく続くことがあります。

このようなときに役立つのが Copilot のエージェントモードです。

Copilot のエージェントモードは、IDE 内でより自律的に作業できるよう設計されています。開発者と同じようにプロジェクトの構造を調べ、必要な変更を行い、テストなどのタスクを実行し、見つかった問題を自動的に修正します。エージェントモードを使って、サイトに新しい機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、エージェントモードと cloud agent は異なる用途向けです。エージェントモードは IDE 内で動作するため、すばやくフィードバックを得て対話できます。cloud agent はチームメンバーのように非同期で作業し、Issue とプルリクエストを通じてやり取りします。

この演習では、次の方法を学びます。

- Copilot エージェントモードでプロジェクトを調べ、関連ファイルを特定して、複数ファイルにまたがる変更を行います。
- GitHub Copilot エージェントモードで UI とデータレイヤーに新機能を実装します。
- コードベースにマージする前に、Copilot エージェントモードが生成した変更とテストをレビューします。

## シナリオ

ゲームの一覧が増えてきたため、ユーザーがカテゴリーと publisher で絞り込めるようにします。前の演習では publisher ヘルパーを追加しました。ここでは Copilot エージェントモードを使い、残りのデータレイヤー、UI、テストの作業を完了します。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、Tailspin Toys の Web サイトを見て、現在の機能を確認しましょう。

この Web サイトは、開発者をテーマにしたボードゲームのクラウドファンディングプラットフォームです。ゲームの一覧と詳細を表示します。Astro アプリケーションはビルド時に静的 HTML としてページを生成します。ページは frontmatter 内で Drizzle ORM を使い、ローカルの SQLite データベースに直接問い合わせます。別のバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータアクセスヘルパーは `src/lib/` にあり、インタラクティブな機能は標準 DOM API を使う小さなスコープ付き Astro `<script>` で追加します。

### Web サイトを起動する

`npm run dev` タスクを使うと、Astro の単一開発サーバーを起動できます。GitHub Codespace で次の手順を実行します。

1. Codespace に戻ります。前の演習を終えていれば、`main` に戻っているはずです。
2. Codespace 内で <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルウィンドウを開きます。
3. フィルター機能用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro の開発サーバーが起動すると、次のような URL が表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くように求めるダイアログが表示された場合は、**x** を選択して閉じます。

5. <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら、ターミナルに表示された Astro サーバーのアドレス `http://localhost:4321` を選択して Web サイトを開きます。

> [!NOTE]
> Codespace のターミナルで localhost URL を選択すると、自動的に `https://<your-codespace-name>-4321.app.github.dev/` に転送されます。これは、Web サーバーを実行する Codespace へのプライベートトンネルです。

### Web サイトを確認する

Web サイトを起動したら、機能を確認します。主な機能は次のとおりです。

- **ホームページ**: ボードゲームの一覧と、タイトル、画像、説明を表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、publisher、カテゴリーなどの詳しい情報が表示されます。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めてください。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

Web サイトの初期実装は動作しますが、さらに機能を追加します。まずバックログを確認しましょう。テンプレートからプロジェクトを設定すると、Issue のバックログが自動的に作成されています。GitHub Copilot に項目を表示してもらいます。

1. Chat ビューのエージェント選択ドロップダウンから **Agent** を選びます。**Agent** は変更の計画と実装を自律的に行い、ファイルをまたぐ変更、ターミナルコマンド、ツールの呼び出しを実行します。

   ![Chat ビューのエージェント選択を示すスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデル一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者が特定のモデルを推奨するものではありません。ワークショップの作成時に Claude Sonnet 4.5 を使ったため、手順に含めています。これにより比較的安定した体験になることを期待しています。ただし、LLM は確率的に動作するため、ワークショップの説明と異なる提案が表示される場合があります。これは通常の動作です。

> [!NOTE]
> LLM の確率的な性質により、Copilot は別の MCP コマンドを使うことがありますが、タスクは同様に完了できるはずです。

3. 次のプロンプトを Copilot に送信して、リポジトリの Issue バックログを表示してもらいます。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての Issue を一覧表示するコマンドを実行します。
5. 生成された Issue の一覧を確認します。

Copilot がユーザーに最も役立つと思われる項目を基準に、優先順位も付けたことを確認してください。

## 指示ファイルを確認する

エージェントにコードを生成させる前に、作業のコンテキストを Copilot に伝える指示ファイルを確認します。Web サイトに機能を追加する方法を説明する [ユーザーインターフェイス (UI) 指示ファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)を使います。

1. Codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能追加に関する全体的な指針を確認します。たとえば、次の内容があります。
   - アーキテクチャの概要
   - コンポーネントの設計、テスト容易性、アクセシビリティの原則
   - Astro、Tailwind CSS、Drizzle など、ファイル形式に応じた指示へのリンク

> [!TIP]
> 指示ファイルからほかの指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルからの相対パスです。指針を再利用できる小さなファイルに分け、例やテンプレートを示すことができます。

## フィルター機能を実装する

フィルターを完成させるには、少なくとも次の 3 つの変更が必要です。

- データレイヤー (`src/lib/`) にフィルター処理を追加または調整します。
- フィルターの動作を確認するテストを追加または更新します。
- ゲーム一覧ページにフィルター UI を追加します。

さらに、コードベースにマージする前にテストを実行し、成功させる必要があります。Copilot エージェントモードに作業を依頼しましょう。

1. 現在の Copilot 会話を続けるか、**New Chat** を選択して新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選択し、指示ファイルを追加します。

   ![UI 指示ファイルを選択する例を示すスクリーンショット](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェント選択で **Agent** が選ばれていることを確認します。

   ![Chat ビューのエージェント選択を示すスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

4. モデルとして **Claude Sonnet 4.5** が引き続き選択されていることを確認します。
5. バックログの関連 Issue に基づいて機能を実装するよう、次のプロンプトを Copilot に送信します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、目的の機能に関連するファイルを特定する様子を確認します。データレイヤーのヘルパー、UI、テストを見つけて変更し、テストを実行します。

   ![Copilot がプロジェクトのファイルを調べるスクリーンショット](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの編集、テストの実行などを行います。タスクやコードベースの複雑さによっては数分かかります。作業中にエディターに **Keep** と **Undo** が表示される場合があります。完了するとすべての変更に **Keep** または **Undo** が表示されるため、作業中に選択する必要はありません。

7. Copilot が確認を求めたら **Continue** を選択してテストを実行します。

   ![Copilot Chat ペインにテスト実行の確認ダイアログが表示されたスクリーンショット](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストが失敗したりすることがあります。問題ありません。Copilot はコード生成とテストを繰り返し、タスクが完了してエラーが見つからなくなるまで作業します。

   ![Copilot エージェントモードの Chat セッションが完了した状態を示すスクリーンショット](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを開いたブラウザーに戻り、新しい機能を試します。
11. 動作を確認してコードをレビューしたら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

この演習では GitHub Copilot エージェントモードを使って、Tailspin Toys の Web サイトに機能を追加しました。次の方法を学びました。

- GitHub Copilot エージェントモードで UI とデータレイヤーに新機能を実装します。
- Copilot エージェントモードでプロジェクトを調べ、関連ファイルを特定して、複数ファイルにまたがる変更を行います。
- コードベースにマージする前に、Copilot エージェントモードが生成した変更とテストをレビューします。

次は [Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを作成します。

### 追加の演習 - ページングを実装する

ゲーム一覧が増えると、ページングが必要になります。この演習で学んだことを使い、Copilot にページングを追加するよう依頼してください。次の点を検討します。

- 既存の指示ファイルを使うなど、現在のベストプラクティスに従います。
- ユーザーがページサイズを選べるようにするか、値を固定するかなど、ページングの方式を検討します。
- 希望する実装に必要な指示を Copilot に伝えるプロンプトを作成します。
- GitHub Copilot に変更やコンテキストを伝え、複数回やり取りする必要がある場合があります。Copilot を使うときには一般的な流れです。

## リソース

- [Copilot の ask、edit、agent モード: 機能と使い分け][choose-mode]
- [VS Code のエージェントモード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
