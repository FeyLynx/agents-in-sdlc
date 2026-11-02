---
title: "レッスン 1 - GitHub Copilot CLI をインストールする"
description: "Codespace に Copilot CLI をインストールして認証し、使い方を確認して、登録済みのフィルター機能の Issue を見つけます。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] は、ターミナルで動作する強力なエージェント型コーディングアシスタントです。コードベースを調べ、コードを生成し、コマンドを実行し、外部ツールに接続できます。すべてコマンドラインから行えるため、作業の流れを保てます。まずはツールをインストールします。すでに使い慣れているツールを使ってインストールできます。

このレッスンでは、次の内容を学びます。

- npm で GitHub Copilot CLI をインストールします。
- GitHub アカウントで認証します。
- ワークショップのリポジトリを信頼し、簡単な会話を試します。
- GitHub MCP サーバーを使って、フィルター機能の Issue を見つけます。

## シナリオ

チームでは、増え続けるバックログに対応するために AI エージェントを使い始めています。Copilot CLI は、開発者が普段作業するターミナルでこの機能を利用できるようにします。このレッスンでは、インストールと認証を済ませ、ワークショップの残りで利用できる状態にします。

## Copilot CLI をインストールする

Copilot CLI は [npm][install-cli]、WinGet、Homebrew からインストールできます。GitHub Codespaces には Node.js があらかじめインストールされているため、npm を使います。

1. Codespace に戻り、ターミナルを開きます。
2. Node.js がインストールされており、バージョン要件を満たしていることを確認します。

   ```bash
   node --version
   ```

   バージョン 24 以上が表示されます。

3. Copilot CLI をグローバルにインストールします。

   ```bash
   npm install -g @github/copilot
   ```

4. インストールを確認します。

   ```bash
   copilot --version
   ```

   バージョン番号が表示されます。

## GitHub で認証する

初回起動時に、Copilot CLI は GitHub アカウントでの認証を求めます。

1. Copilot CLI を起動します。

   ```bash
   copilot
   ```

2. 求められた場合は、デバイスコードの手順に従って認証し、Copilot CLI を承認します。
3. Copilot CLI から次のプロンプトが表示されます。

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. パスが Tailspin Toys のリポジトリであることを確認し、**Yes, and remember this folder for future sessions** を選択して「はい」と答えます。

> [!NOTE]
> Codespace では GitHub セッションを通じてすでに認証されている場合があります。Copilot CLI が認証を求めずに起動した場合は、そのまま進めて問題ありません。

## 使い方を確認する

通常のシェルプロンプトで入力したコマンドは、Codespace で直接実行されます。Copilot CLI を起動すると、自然な言葉はエージェントに送信され、スラッシュコマンドで会話を操作します。

1. `/model` を入力し、矢印キーで **Auto** を選択して <kbd>Enter</kbd> を押し、もう一度 <kbd>Enter</kbd> を押して確定します。
2. `/help` を入力して、インストール済みのバージョンで利用できるコマンドを確認し、<kbd>Esc</kbd> を押してヘルプ画面を閉じます。
3. 次の質問をして、すべてが動作していることを確認します。

   ```plaintext
   What are the key files in this project?
   ```

4. Copilot が回答前にリポジトリを調べることに注目してください。
5. `/mcp list` を入力し、組み込みの GitHub MCP サーバーが利用可能であることを確認します。
6. 次のプロンプトを送信し、フィルター機能の Issue を見つけます。

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. URL を開き、Issue を読みます。最初の簡単な変更を完了した後に使います。

> [!TIP]
> 通常の Copilot CLI セッションは、ターミナルで現在チェックアウトされているブランチを使います。自動的に worktree を作成することはありません。各変更の前に機能ブランチを作成します。

## ワークショップ用のショートカットを使う

通常、Copilot CLI は既存の権限の範囲外でツールを使う前に確認を求めます。このワークショップでは `--yolo` を付けて再起動します。ユーザーが許可したこのショートカットは Codespace 内の承認プロンプトをなくし、演習に集中できるようにします。

> [!CAUTION]
> `--yolo` は完全な自動権限 (`--allow-all-tools`、`--allow-all-paths`、`--allow-all-urls`) を有効にします。Codespace や VM などの分離された環境でのみ使用し、日常的な開発の既定値として alias に設定しないでください。詳細は[ツールの使用を許可または禁止する][allow-all-warning]を参照してください。

このワークショップでは `--enable-all-github-mcp-tools` を指定し、後のレッスンで Issue やプルリクエストを操作するために使う GitHub MCP の読み取り・書き込みツールを有効にします。Codespace はローカルコンピューターへのアクセスを制限しますが、認証済みの GitHub リソースは実際のものです。公開やマージの前に変更をレビューしてください。

1. `/exit` で Copilot CLI を終了します。
2. リポジトリのルートから再起動します。

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. プロジェクトについて簡単な質問をもう一度して、会話が動作していることを確認してから `/exit` で終了します。

Copilot は会話を自動的に保存します。後のレッスンで指示を変更したりエージェントを追加したりしたら、`copilot --resume` で同じ会話とブランチに戻ります。

## まとめと次のステップ

このレッスンでは、次のことを行いました。

- npm で GitHub Copilot CLI をインストールしました。
- GitHub アカウントで認証しました。
- ワークショップのリポジトリを信頼し、簡単な会話を試しました。
- GitHub MCP サーバーを使って、フィルター機能の Issue を見つけました。

次は[最初の小さな変更][next-lesson]を始め、Copilot CLI でゲームカードに星評価を表示します。

## リソース

- [GitHub Copilot CLI をインストールする][install-cli]
- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
