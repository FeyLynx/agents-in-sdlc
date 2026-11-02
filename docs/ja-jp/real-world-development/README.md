---
title: "実践的な開発"
slug: ja-jp/real-world-development
authors:
  - geektrainer
lastUpdated: 2026-09-16
---

現実的なシナリオ、完成したアプリケーション、共有バックログを使って、ソフトウェア開発ライフサイクル全体で GitHub Copilot を活用します。変更の計画、コードの作成、動作のテスト、プルリクエストのレビュー、エージェントを使った作業の管理を繰り返しながら進めます。

## シナリオ

架空の企業 Tailspin Toys に新しく加わった開発者として、ボードゲームのクラウドファンディングを提供するアプリケーションに取り組みます。チームのバックログは GitHub Issue として登録済みで、すぐに着手できます。カテゴリーやページネーションなどの機能開発に加えて、アクセシビリティやコーディング規約などの品質改善にも取り組みます。

## 作業環境を選ぶ

GitHub Copilot は、普段の作業環境で利用できます。構築したい方法に合った環境を選んでください。各ワークショップでは、専用のセットアップと共通の Tailspin Toys シナリオを使用します。

### [GitHub Copilot CLI][cli]

ターミナルでエージェント型アシスタントを使います。Plan モードと Autopilot モード、指示、スキル、カスタムエージェント、Playwright MCP、Agent Merge、実用的なスラッシュコマンドを扱います。

### [GitHub Copilot app][app]

デスクトップアプリでエージェントセッションを並行して実行し、セッションモードを切り替え、Canvas で共同作業を行い、GitHub Issue とプルリクエストを管理します。

### [GitHub Copilot cloud agent][cloud]

GitHub Issue を非同期エージェントに割り当て、作業を案内して進行状況を確認し、作成されたプルリクエストをレビューします。

### [Visual Studio Code][vscode]

Visual Studio Code と GitHub Codespaces で GitHub Copilot を使います。エディターを離れずに、Copilot Chat のエージェントモード、MCP サーバー、カスタムエージェントを利用します。

[cli]: cli/
[app]: app/
[cloud]: cloud/
[vscode]: vscode/
