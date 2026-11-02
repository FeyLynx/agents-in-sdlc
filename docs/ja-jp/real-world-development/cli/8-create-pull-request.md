---
title: "レッスン 8 - 機能 PR を作成してマージする"
description: "フィルター機能、指示、スキルの更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルター機能の実装、指示の更新、スキルの更新、品質保証 (QA) プロファイル、テストが、1 つのブランチに保存されています。これらをまとめてレビューし、プルリクエストを作成します。星評価のプルリクエスト (PR) は手動でマージしました。今回は **Agent Merge** にプロセスを管理してもらいます。

> [!NOTE]
> 通常は機能、指示の更新、スキルの更新、QA エージェントを複数の PR に分けます。ワークショップを効率よく進めるため、フィルター機能と品質ワークフローの作業を 1 つの会話とブランチで進め、この PR にまとめました。

このレッスンでは、次の内容を学びます。

- Agent Merge とは何か、マージまでのライフサイクルを自動化する方法を確認します。
- 機能の全変更と検証の根拠を確認します。
- フィルター機能の PR を作成します。
- レビュー後にのみ Agent Merge を有効にし、PR がマージされたことを確認します。

## シナリオ

フィルター機能の開発では、Copilot を使って機能を計画、実装、検証しました。Tailspin Toys は、マージの承認権限を開発者が保持したまま、残りの PR 作業を自動化したいと考えています。

## Agent Merge について

**Agent Merge** は、プルリクエストをマージするために必要な残りの作業を自動化します。有効にすると、Copilot は PR をブロックしている問題に取り組みます。たとえば、失敗した継続的インテグレーション (CI) チェックの修正、レビューコメントへの対応、必要に応じた rebase を行います。その後、リポジトリで許可されていれば GitHub の自動マージを有効にします。

これまでは、自分で **Merge pull request** を選択していました。Agent Merge にその役割を任せることができますが、有効にする前に作業をレビューし、マージ可能だと判断してください。

## Agent Merge で PR を管理する

コードの作成が終わったら、変更をまとめてレビューし、PR を作成して、残りの作業を Agent Merge に任せます。

1. Codespace に戻ります。
2. `/agent` を入力してエージェントダイアログを開きます。
3. 選択肢から **Default** を選び、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドを実行して、新しい PR を作成します。
5. `/pr agentmerge` を実行して Agent Merge を有効にします。
6. Copilot が PR の継続的インテグレーションを監視します。すべて成功すると、マージを実行します。
7. Copilot から `PR #14 was squash-merged successfully.` のようなメッセージが表示されたことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージキュー、リポジトリ設定、または不足している権限を回避しません。ブロックされた場合は報告された理由を確認し、リポジトリで許可されている場合はレビュー済みの変更を手動でマージします。

## まとめと次のステップ

コードの生成やテスト・検証に加えて、プルリクエストのプロセスも自動化しました。このレッスンでは、次の内容を学びました。

- Agent Merge とは何か、マージまでのライフサイクルを自動化する方法
- 機能の全変更と検証の根拠を確認する方法
- フィルター機能の PR を作成する方法
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認する方法

次は、コンテキスト、モデル、共有、省略可能なクラウドへの委任に使う[便利な Copilot CLI スラッシュコマンド][next-lesson]を確認します。

## リソース

- [Copilot CLI でプルリクエストを管理する][manage-prs]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
