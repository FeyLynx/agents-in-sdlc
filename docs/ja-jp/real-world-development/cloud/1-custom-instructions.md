---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。特定の方法でタスクを実行したい場合や、Copilot が知っておくべき背景情報がある場合は、そのコンテキストにアクセスできるようにします。[指示ファイル][instruction-files]を使うと、Copilot に「何を」するかだけでなく、「どのように」実行するかも伝えられます。

この演習では、次の内容を学びます。

- リポジトリのカスタム指示とパスを限定した指示ファイルを使って、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準を Copilot に伝える方法を確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、この演習では変更前後のプロンプトを比較しません。Copilot cloud agent は GitHub Issue に対して非同期で作業するため、影響をリアルタイムで並べて示すのが難しいためです。このハーネスの後半で、cloud agent が作成したプルリクエストをレビューするときに、指示ファイルの影響を確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発手法に関するガイドラインと要件があります。たとえば、次のようなものです。

- データレイヤーには必ずユニットテストを用意します。
- UI はダークモードにし、現代的な見た目にします。
- コードのドキュメントには TSDoc のドキュメントコメントを追加します。
- 各ファイルの先頭に、ファイルの役割を説明するコメントブロックを追加します。

指示ファイルを使い、Copilot がこれらの開発方針に沿ってタスクを実行できるようにします。

### カスタム指示

カスタム指示では、コーディングスタイルや要件に関するコンテキストと希望を Copilot に伝えます。コーディング規約、ライブラリ、コードに含めたいコメントの種類などを指定でき、より適切な提案やコードスニペットを得るのに役立ちます。リポジトリ全体、または特定の種類のファイルやタスクに対する指示を作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に送信される単一の指示ファイルです。チャットや CLI のリクエストの多くに関連するプロジェクトレベルの情報を記載します。使用する技術スタック、開発対象の概要、ベストプラクティス、全体に適用する指針などが該当します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイル形式に対する指針を記載します。TypeScript などの言語や、UI コンポーネントまたは新しいユニットテストの作成などのタスクを対象にできます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使用され、コード補完や次の編集候補には使用されません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリレベルの指示と、`applyTo` フロントマターで対象を限定した `*.instructions.md` ファイルの両方をコード生成に使用します。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの標準に従った指示ファイルにも対応しています。[対応する指示ファイルの種類][custom-instructions-support]を参照してください。

### 指示ファイルを管理するためのベストプラクティス

指示ファイルの作成について詳しく説明することはこの演習の範囲外ですが、サンプルプロジェクトに含まれるファイルは一般的な方法を示しています。基本的には、次のようにします。

- `copilot-instructions.md` には、開発対象やプロジェクト構成、全体に適用されるコーディング標準など、プロジェクトレベルの指針を記載します。
- `*.instructions.md` には、ユニットテスト、Astro コンポーネント、データレイヤーなどのファイル形式や特定のタスクに関する指針を記載します。
- 自然な言葉を使い、明確な指針と、コードのよい例・悪い例を示します。

指示ファイルの書き方にも、AI の使い方と同じように唯一の正解はありません。プロジェクトで試しながら、最適な方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、十分な指示ファイルを用意することをお勧めします。このプロジェクトには、[UI の更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けのファイルがあります。
>
> Copilot に指示ファイルを生成させることもできます。たとえば VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init` を使います。操作方法は各サーフェスで異なり、該当するレッスンで説明します。
>
> テンプレートや出発点を探している場合は、指示ファイル、カスタムエージェント、その他のリソースをまとめた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに用意された指示ファイルを確認してください。中心となる `copilot-instructions.md` と、各種タスク向けの `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開きます。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡潔な説明に加えて、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下には、**GitHub Actions Workflows** に関する指針もあります。これらは、Copilot を使うどの操作にも適用されます。
3. `.github/instructions` フォルダーを開いて確認します。Astro ファイル、Drizzle データレイヤー、テストなどに関する指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドは、リポジトリルートを基準とする glob を設定し、指示の適用先を決めます。たとえば `**/*.test.ts` に一致する TypeScript テストファイルが対象です。
5. このプロジェクトのユニットテスト作成に関する指針を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などの指示ファイルや、プロジェクト内の既存ファイルへのリンクがあります。大きな指示を再利用しやすい小さなファイルに分け、コード生成の参考となる例に Copilot を誘導できます。ここにあるパスはリポジトリルートではなく、指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントはまだ必須ではありません。次の手順で、TSDoc のドキュメントコメントとファイルヘッダーコメントの規則を追加します。

## リポジトリ全体に適用する標準を追加する

ここで編集するのは、リポジトリ全体に適用するルールを追加する作業だけです。コード内のドキュメントは TSDoc のドキュメントコメントとファイルレベルのコメントヘッダーに記載するよう、`.github/copilot-instructions.md` にルールを追加します。後続の演習で Issue を割り当てたときに、cloud agent がこの指示を使います。

編集する前に、作業ブランチを作成します。cloud agent は Issue の対象ブランチから指示を読み込みますが、変更を整理してコミットできるようにブランチを使います。

1. Codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

前の手順で説明したとおり、`.github/copilot-instructions.md` は Copilot にプロジェクトレベルの情報を提供します。コードの提案を改善するために、リポジトリのコーディング標準を記載します。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。プロジェクトのコーディング標準は記載されていますが、コード内のドキュメントに関する規則がまだないため、生成されたヘルパーにドキュメントコメントがありません。
3. 既存の標準の直後に、次の Markdown を追加します。TSDoc のドキュメントコメントとファイルヘッダーコメントを Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、指示ファイルはリポジトリレベル (`.github/copilot-instructions.md`) に作成して全体に適用することも、特定の言語、ファイル形式、タスク向けの `*.instructions.md` として作成することもできます。今回追加したドキュメントコメントの規則など、プロジェクト全体の標準にはリポジトリレベルのファイルが適しています。

## 指示の変更をコミット、プッシュ、マージする

cloud agent は、対象の Issue があるブランチから指示ファイルを読み込みます。次の演習で Issue を Copilot に割り当てると、Copilot は `main` からブランチを作成します。そのため、指示の変更を `main` に反映しておく必要があります。

1. ステージングしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` に向けたプルリクエストを作成し、マージします。次の演習で割り当てる Issue に取り組むとき、cloud agent がこの指示を読み込めるようになります。

> [!TIP]
> このワークショップでは、ブランチを使わず `main` で直接作業してもかまいません。実際のプロジェクトで行う方法を体験できるよう、ここではブランチを作成しています。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを得る方法を確認し、`.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加しました。このハーネスの後半で、cloud agent が作成するプルリクエストにその標準が反映されます。

次に、[最初の Issue を Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot の指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
