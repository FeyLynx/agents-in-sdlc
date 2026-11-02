---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュコマンド"
description: "コンテキストの管理、モデルの選択、セッションの共有、省略可能なクラウドへの委任に使うスラッシュコマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

多くの CLI ツールと同様に、GitHub Copilot CLI には対話に使えるスラッシュコマンドが多数あります。高度な機能、内部情報、追加の構成オプションを利用できます。これまでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などのコマンドを使いました。ここでは、ほかの便利なコマンドを確認します。

このレッスンでは、次の内容を学びます。

- `/context` と `/compact` を使い、Copilot が会話のコンテキストを管理する方法を確認します。
- `/model` を使って、利用可能なモデルを確認します。
- `/share` でセッションをエクスポートまたは共有する方法を学びます。
- 並列作業、worktree、cloud agent への委任に使う省略可能なコマンドを確認します。

## シナリオ

CLI の主要なワークフローを完了しました。次は、コンテキストの管理、モデルの切り替え、セッションの共有など、ほかの機能を確認します。必要に応じて、[Copilot cloud agent][about-cloud-agent] に作業を委任することもできます。

## Copilot CLI のコンテキストを確認する

大規模または複雑なタスクに取り組むと、モデルの最大コンテキストウィンドウに達することがあります。Copilot CLI は必要に応じて会話を自動的に圧縮します。スラッシュコマンドを使って、状況を確認したり自分で圧縮したりできます。

1. Codespace に戻り、Copilot CLI がまだ起動していない場合はリポジトリルートから起動します。
2. 次のコマンドを入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム指示、ツール、メッセージ、空き領域にコンテキストがどのように分配されているかを確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を入力し、結果を比較します。会話が短い場合は、大きな違いがないこともあります。

> [!NOTE]
> コンテキストウィンドウが埋まると、Copilot CLI は自動的にコンテキストを圧縮します。タイミングを選びたい場合は `/compact` を使います。関係のない別タスクに切り替えるときに新しい会話を始めるには、`/clear` または `/new` を使います。

## モデルを選ぶ

モデルによって得意な作業は異なり、開発者ごとに好みも異なります。Copilot CLI では、利用できるモデルを表示して選択できます。

1. 次のコマンドを入力します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用状況に関する情報を確認します。
3. 現在のモデルを使い続けるか、別のモデルを選択するか、<kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで協力して学んだことを共有すると、全員が AI ツールの使い方を改善できます。`/share` コマンドを使うと、セッションを Markdown または HTML ファイルにエクスポートしたり、共有リンクを作成したり、GitHub gist を公開したりできます。

1. `/help` を入力し、インストール済みのバージョンで利用できる `/share` のオプションを確認します。
2. セッションを共有する場合は、ローカルに Markdown としてエクスポートする `/share file` など、必要に合った方法を選択します。
3. エクスポートした内容を送信または公開する前に確認します。セッションのエクスポートにはプロンプト、応答、プロジェクトの詳細が含まれる場合があります。

リンクや gist の公開は省略可能です。チームが共有するつもりのないリポジトリや会話の内容を公開しないでください。

## 省略可能: 作業を並列化または委任する

ワークショップの主要な内容は完了しました。Copilot CLI には、より大きなタスクで使えるコマンドもあります。

- `/fleet` は、独立したサブタスクをサブエージェントに分割し、並列に実行できます。
- `/worktree` は、別のタスク用に分離された Git worktree を作成できます。
- `/delegate` は、タスクを Copilot cloud agent に送信できます。cloud agent は非同期に作業し、プルリクエストを作成する場合があります。

これらのコマンドを使うと、worktree の追加やリモートでの作業が発生するため、省略可能です。試す場合は、範囲の明確な新しいタスクから始め、通常のワークフローに沿って結果をレビューしてください。非同期エージェントの作業をさらに学ぶ場合は、[cloud agent ワークショップ][cloud-workshop]に進みます。

## まとめと次のステップ

Copilot CLI のスラッシュコマンドを使うと、構成を変更し、セッションを共有し、内部の動作を確認できます。このレッスンでは、次の内容を学びました。

- `/context` と `/compact` を使い、Copilot が会話のコンテキストを管理する方法を確認しました。
- `/model` を使って、利用可能なモデルを確認しました。
- `/share` でセッションをエクスポートまたは共有する方法を学びました。
- 並列作業、worktree、cloud agent への委任に使う省略可能なコマンドを確認しました。

ほかにも多くのスラッシュコマンドがあります。次は[学んだ内容と次のステップを振り返り][next-lesson]、Copilot CLI の学習を続けましょう。

## リソース

- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
