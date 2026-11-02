---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェントモード →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。特定の方法でタスクを実行したい場合や、Copilot が知っておくべき背景情報がある場合は、そのコンテキストを参照できるようにします。Copilot に具体的なコンテキストを共有する方法はいくつかあります。その中心となるのが[指示ファイル][instruction-files]です。指示ファイルを使うと、コード生成の指針を伝えられます。

この演習では、次の内容を学びます。

- リポジトリのカスタム指示とパスを限定した指示ファイルを使って、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準を Copilot に伝える方法を確認します。
- 現在の指示を使って、フィルター機能の最初のデータ部分 (publisher ヘルパー) を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。
- 同じプロンプトをもう一度実行し、生成コードが新しい標準に従う様子を確認します。
- 指示の変更とフィルター機能の一部を `main` にコミットし、次の演習で Copilot が新しい指針を使えるようにします。

> [!CAUTION]
> 生成コードは、設定した標準から外れる場合があります。Copilot の動作は決定論的ではありません。この演習では、文字単位で同じ出力を得るのではなく、指示の変更後に動作がどのように変わるかを確認します。

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

## 変更用のブランチを作成する

ここではベストプラクティスに従い、作業用のブランチを作成します。

1. 前の演習で使用した Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めてください。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

カスタム指示の効果を確認するため、まず現在の指示ファイルを使ってプロンプトを送信します。その後、指示ファイルを更新し、同じプロンプトをもう一度送信して違いを確認します。

1. 前の演習で開いたエディタータブをすべて閉じ、Copilot が必要なコンテキストだけを使うようにします。
2. `src/lib/publishers.ts` を開き、ヘルパーの配置先を Copilot に示します。
3. Chat ビューのエージェント選択ドロップダウンから **Agent** を選び、Copilot がファイルを変更できるようにします。

   ![Chat ビューのエージェント選択を示すスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot はプロジェクトを調べてコードを更新します。ヘルパーファイルとテストなど、複数のファイルが変更されることがあります。
6. 提案されたヘルパーが、最初の引数として `db` クライアントを受け取り、型付きの publisher 配列を返す関数であることを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータレイヤー規約に基づいています。
7. 提案されたコードに TSDoc のドキュメントコメントとファイルレベルのコメントヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくてもドキュメントコメントを追加する可能性があります。その場合も問題ありません。ここで確認するのは、指示を更新した後の一貫性の改善です。

## リポジトリ全体に適用する標準を追加する

前述のとおり、`.github/copilot-instructions.md` は Copilot にプロジェクトレベルの情報を提供します。コードの提案を改善するために、リポジトリのコーディング標準を記載します。

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

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話をリセットします。
2. `src/lib/publishers.ts` をもう一度開き、Copilot が正しいファイルに注目するようにします。
3. 前と**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメントブロックが追加されていることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが追加されていることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

新しいプロジェクト標準に従うよう Copilot を誘導し、次の演習で使う実際のコードに適用できました。

## 指示とブランチをコミットしてプッシュする

指示ファイルもほかのアセットと同様に、ソース管理で管理します。変更をコミットしてリポジトリにブランチをプッシュします。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルウィンドウを開きます。
2. ターミナルで次のコマンドを実行し、指示ファイルとヘルパーの変更を確認します。

   ```bash
   git status
   ```

3. ターミナルで指示の変更と、ヘルパーの基盤作成に Copilot が変更したすべてのファイルをステージングしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status にテストなどの追加ファイルが表示された場合は、それらも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ターミナルからブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、バックログのドキュメント標準に関する Issue と関連付けたプルリクエストを作成します。手動で作成することもできますが、GitHub ツールが接続されているので Copilot に依頼しましょう。Issue を見つけ、Issue をクローズするプルリクエストを作成するようプロンプトで依頼します。

> [!NOTE]
> この演習では、Copilot に GitHub 上の操作を依頼します。プロジェクトのテンプレートでは GitHub ツールがすでにワークスペースに設定されているため、準備は不要です。初めて Copilot が GitHub ツールを使うときに、VS Code で GitHub へのサインインを求められることがあります。画面の指示に従ってください。この接続方法 (Model Context Protocol) については、後の演習で学びます。

1. Codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント選択で **Agent** が選ばれていることを確認します。
3. 指示ファイルの更新に関する Issue を探し、現在のブランチから PR を作成するよう Copilot に依頼します。PR には指示の更新と、新しい publisher ヘルパーの基盤作成の両方を記載します。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が Issue の検索と PR の作成を開始します。
5. Copilot が GitHub の操作を実行する許可を求めたら、コマンドを確認して適切に **Allow** を選択します。
6. PR が作成されたら、次のプロンプトを使って PR をマージし、ローカルブランチを `main` に戻すよう Copilot に依頼します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot が GitHub の操作やシェルコマンドの実行許可を求めたら、コマンドを確認して適切に **Allow** を選択します。

GitHub Copilot の支援でプルリクエストを作成してマージできました。

## まとめと次のステップ

Copilot が指示ファイルからコンテキストを得る方法を確認し、VS Code の Copilot Chat を使って次の作業を行いました。

- 既存の指示がある状態でコード生成のプロンプトを送信し、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加しました。
- 同じプロンプトを再実行し、生成コードが新しい指示に従う様子を確認しました。
- 指示の更新と、次のフィルター機能の基盤となるヘルパーをコミットして `main` に反映しました。

次は [エージェントモード][next-lesson]で指示を使い、コードベース全体にまたがる新機能を Copilot に追加してもらいます。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://awesome-copilot.github.com/
