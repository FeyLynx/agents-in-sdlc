---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2 件のプルリクエスト、再利用可能なカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使って、Tailspin Toys で一連の開発ワークフローに取り組みました。次のことを行いました。

- Codespace を準備し、Copilot CLI をインストールし、プロジェクトを確認して、登録済みのフィルター機能の Issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージしました。
- フィルター機能の Issue から始めて、Plan モードで方針を決め、Autopilot モードで実装し、Interactive モードでレビューしました。
- カスタム指示でエージェントを導き、既存の `quality-checks` スキルをカスタマイズして、ユニットテスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルター機能を確認しました。
- 品質保証 (QA) 用のカスタムエージェントを作成して選択し、要件、テストカバレッジ、スキルの結果、ブラウザーで確認した根拠を評価しました。
- フィルター機能の変更全体をレビューし、Agent Merge を許可して PR をマージしました。
- コンテキスト、モデル、共有、省略可能なクラウドへの委任に使うスラッシュコマンドを確認しました。

## リリースしたもの

このワークショップには、それぞれ更新済みの `main` から作成した別々のブランチで進める、2 つの PR の節目があります。

1. **星評価:** 既存の `starRating` と未評価であることを示す状態をゲームカードに表示します。
2. **フィルターと品質ワークフロー:** フィルター機能を実装し、指示を更新して機能に適用し、`quality-checks` のレポートをカスタマイズして QA プロファイルを作成し、関連テストを追加します。

フィルター機能の計画から PR の作成までは、同じ会話とブランチを使いました。ワークショップを効率よく進めるため、作業を 1 つの PR にまとめました。

## 検証の種類

自動テスト、自分で行うブラウザー確認、MCP 経由で Copilot にブラウザーを操作させる方法など、複数の方法でコードを確認しました。`quality-checks` スキルではユニットテスト、lint、型チェックを実行し、追加した書式で結果を報告しました。QA はその結果と、要件やテストカバレッジのレビューをまとめて PR の確認に使いました。

追加するテストは、実際に不足している点を補うものである必要があります。QA の実行で新しいテストが必要ない場合もあります。マージを許可する前に、コードと検証の根拠を確認してください。変更を加えたら、影響を受けた検証を再実行します。

## ベストプラクティス

Copilot に与えるコンテキストとツールが作業の結果に影響します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを構成しました。これらのカスタマイズは複数の会話で再利用し、チームのニーズに合わせて調整できます。指示は標準を設定し、スキルは繰り返し実行するタスクを説明し、カスタムエージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合った**モードとモデル**を選びます。構築前に方針を検討するには **Plan**、小さな変更を確認しながら進めるには **Interactive**、範囲の明確なタスクを委任するには **Autopilot** を使います。定型的な編集には高速なモデルを、複雑な作業にはより高性能なモデルを選びます。

インフラストラクチャと同じように、コンテキストも重要です。何を作るか、なぜ必要か、どのように作るかを明確に説明すると、出力の質が大きく変わります。

## さらに確認する

中心的なワークフローを学びました。Copilot CLI には、さらに便利な機能があります。

- `/review` を使い、コードレビューエージェントに変更を分析してもらいます。
- `/rubber-duck` を使って問題を説明し、別の視点を得ます。
- `/fleet` で独立したサブタスクを並行して調整します。
- `/worktree` で別のタスクを分離します。
- `/delegate` で Copilot cloud agent にタスクを送信します。

## 次のステップ

どのツールでも、上達する一番よい方法は使い続けることです。実際のコード、趣味のコード、長年作ってみたいと思っていた小さなアプリなどで活用してください。チームと学んだことを共有し、互いの経験から学びましょう。ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys を拡張する場合は、省略可能な [Foundry Backer Concierge シリーズ][foundry]に進みます。別の環境で学ぶ場合は、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
