---
title: "レッスン 3 - エージェントモード: Plan と Autopilot"
description: "Plan で方針を決め、Autopilot で Issue に基づくフィルター機能を実装し、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

プロジェクトに小さな機能を追加しました。大きな変更には、より確実なプロセスが必要です。幸い GitHub Copilot CLI は、組織の既存の開発フローに沿って適切な方法で開発できるよう設計されています。このレッスンから、エージェントを使った一般的な開発プロセスをいくつかのレッスンにわたって実践します。Issue に基づいて機能を作成し、コードの妥当性と機能の動作を確認して、最終的にプロジェクトにマージします。

> [!NOTE]
> 機能開発のワークフローでは、同じ会話とブランチを続けて使います。通常、ファイル形式が異なる作業には別々のブランチや PR を使いますが、ここでは主な概念に集中できるよう簡略化しています。

このレッスンでは、次の内容を学びます。

- GitHub Issue から Copilot の新しい会話を始める。
- Plan モードで要件を決める。
- Autopilot モードで新機能を実装する。
- コードをレビューする。
- 転送されたブラウザーで手動検証する。

この機能を完成させるまで、リポジトリの指示を更新し、既存の `quality-checks` スキルをカスタマイズし、MCP による検証を追加して QA エージェントを作成し、機能の PR を開きます。

## シナリオ

Tailspin Toys のカタログが増え、訪問者がゲームをカテゴリーと publisher で絞り込む必要があります。バックログには機能の Issue がありますが、カテゴリーを組み合わせる方法など、コーディング前に合意しておく必要があります。Plan モードでこれらの要件を決めてから、範囲を定めて Autopilot に実装を依頼します。

## 背景

AI コーディングエージェントを開発フローに導入しても、基本は変わりません。むしろ、より重要になります。多くの開発者は、次のような流れで作業します。

1. 実装する内容を説明した Issue から始めます。
2. 実装内容の計画を立てます。
3. コードを作成してレビューします。
4. テストを実行してコードを検証します。
5. 新しい機能を手動で検証します。
6. プルリクエスト (PR) を作成します。
7. コードレビューが完了し、継続的インテグレーションが成功したら、コードをマージします。

> [!NOTE]
> チームや組織によって、手順の詳細は異なります。ただし、ほとんどの場合は上記の流れを調整したものです。

この標準的な手順に沿うと、AI が生成したコードが要件を満たし、手作業で書いたコードと同じ審査プロセスを通るようにできます。

## 会話モード

**会話モード**では、エージェントにどの程度の自律性を与えるかを設定します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押してモードを切り替えます。

- **Interactive**: エージェントと一緒に作業します。エージェントは変更を提案し、次の作業に進む前に入力を待ちます。
- **Plan**: エージェントが先に計画を作成します。プロジェクトファイルの編集はできません。
- **Autopilot**: エージェントが自律的に作業します。コードを作成してテストを実行し、タスクが完了するまで改善を繰り返します。

まず Plan モードで計画をレビューしてから、Autopilot に切り替えて実装します。

## Issue から始める

フィルター機能の作業を始める前に、Codespace に戻り、リポジトリとターミナルを準備します。

1. Codespace に戻ります。停止している場合は再起動します。
2. 星評価の PR がマージされていることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押します。
4. `main` を更新し、フィルター機能のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押し、**Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての Issue を表示します。
8. 矢印キーを使って、**Allow users to filter games by category and publisher** というタイトルの Issue を選択します。
9. <kbd>C</kbd> を押して Issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトが `#7` (または同様の番号) で始まることに注目してください。`#` を使うと、GitHub 上の Issue やプルリクエスト (PR) をコンテキストに追加できます。

## フィルター機能を計画する

計画を立てると、機能を実装したりタスクを Copilot に任せたりする前に、採用する方法を決められます。複雑な作業では、計画に時間をかけることをお勧めします。Plan モードに切り替えて、Copilot に計画を作成してもらいましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下にあるモード表示が **Plan** になっていることを確認します。
2. 前の手順で追加した Issue の後に、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調べて、実装方法を決め、計画を作成します。

3. 作業中に Copilot からフィルターの動作について質問されることがあります。希望に基づいて回答してください。正解は 1 つではありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して展開します。
5. 計画を上下にスクロールしてレビューします。
6. 決めた内容と合わない部分があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画を作成してレビューしたら、実装に進みます。Autopilot を使って Copilot に作業を任せます。

Autopilot では、Copilot がタスクを完了したと判断するまで問題に取り組み、改善を繰り返します。

1. **Accept plan and build on autopilot (recommended)** またはインストール済みのバージョンで同様のラベルの選択肢を選びます。
2. プロンプトの下にあるモード表示が **Autopilot** になっていることを確認します。
3. Copilot が計画に沿って反復し、コードを生成してテストを実行する様子を確認します。

> [!NOTE]
> 承認すると、すぐに実装が始まる場合があります。計画を先にレビューしてください。依存関係が不足している、またはポートが競合していると Copilot が報告した場合は、チェック完了と判断する前にセットアップの問題を解決します。

## 実装をレビューして検証する

ほかのコードと同じように、生成されたコードもマージする前にレビューします。コードを確認し、サイトを実行して動作も確かめましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モード表示に **Plan** や **Autopilot** が表示されていないことを確認します。
2. `/diff` を入力し、フィルター機能とテストを確認します。
3. 結果を Issue と計画で決めた内容に照らし合わせます。
4. コードをレビューしたら <kbd>Esc</kbd> を押して差分画面を閉じます。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に修正を依頼します。

## 新しい機能を試す

コードに問題がないように見えても、実際に動作することを確かめましょう。前と同じようにアプリを起動し、Codespaces の転送ポートでサイトを開きます。

1. Copilot に次のプロンプトを送信してアプリを起動します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能と表示されたら、**Open in Browser** を選択します。
3. 計画で決めたカテゴリー、publisher、その組み合わせでフィルターを試します。
4. リセットと検索結果がない場合の動作が、Issue と決定した内容に合っていることを確認します。
5. Codespace に戻り、起動した開発サーバーを停止するよう Copilot に依頼します。

## まとめと次のステップ

会話モードを切り替えて機能を作成し、レビューしました。このレッスンでは、次の内容を学びました。

- GitHub Issue から Copilot の新しい会話を始めました。
- Plan モードで要件を決めました。
- Autopilot モードで新機能を実装しました。
- コードをレビューしました。
- 転送されたブラウザーで手動検証しました。

次は[カスタム指示を使って][next-lesson]、生成コードが文書化された規約に従うようにします。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンドリファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
