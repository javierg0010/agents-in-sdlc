---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェントモード →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。特定の方法でタスクを進める必要がある場合や、Copilot に把握しておいてほしい背景情報がある場合は、そのコンテキストを利用できるようにします。コンテキストを伝える方法はいくつかあります。その中心となるのが、コード生成のガイダンスを提供する [instruction file][instruction-files] です。

この演習では、次の内容を学習します。

- リポジトリの custom instruction とパス単位の instruction file を通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot に渡る方法を確認する。
- 現在の指示を使って、フィルター用データの最初の部分となる publishers helper を生成する。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加する。
- 同じプロンプトを再実行し、生成コードが新しい標準に従うことを確認する。
- 指示の変更とフィルター用の最初の実装を `main` に commit し、次の演習で新しい指示を使えるようにする。

> [!CAUTION]
> 生成コードが設定した標準から外れる場合があります。Copilot は非決定的に動作します。この演習の目的は、指示の更新後に傾向がどう変化するかを見ることであり、出力を1文字ずつ一致させることではありません。

## Instruction file

### シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。たとえば、次のような内容です。

- データレイヤーには必ずユニットテストを追加する。
- UI はダークモードにし、現代的な見た目にする。
- コードのドキュメントには TSDoc の doc comment を使う。
- 各ファイルの先頭に、ファイルの役割を説明するコメントブロックを追加する。

instruction file を使って、Copilot がこれらのプラクティスに沿って作業するために必要な情報を提供します。

### Custom instruction

Custom instruction を使うと、コーディングスタイルや要件などのコンテキストや好みを Copilot に伝えられます。コーディング規約、ライブラリ、追加したいコメントの種類などを指定でき、より関連性の高い提案やコードスニペットを得られます。リポジトリ全体に適用する指示も、特定のファイル種別に適用するタスクレベルの指示も作成できます。

Instruction file には次の2種類があります。

- `.github/copilot-instructions.md` は、リポジトリへの**すべて**のリクエストで Copilot に送られる単一の instruction file です。使用する技術スタック、開発中のものの概要、ベストプラクティスなど、ほとんどのチャットや CLI リクエストに関係するプロジェクト情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイル種別向けの指示です。TypeScript や Astro などの言語、UI コンポーネントの作成、ユニットテストの追加といったタスクのガイドラインを記載できます。

> [!NOTE]
> IDE では、instruction file は Copilot Chat によるコード生成で使われ、コード補完や next-edit suggestion には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリレベルの指示と `applyTo` front matter を持つ `*.instructions.md` ファイルの両方を使います。
>
> Copilot は、AGENTS.md や CLAUDE.md など、[ほかの標準に基づく instruction file][custom-instructions-support]もサポートしています。

### Instruction file を管理するためのベストプラクティス

instruction file の作成に関する詳しい説明は、このワークショップの範囲外です。サンプルプロジェクトには、一般的な方法を示す例が含まれています。

- `copilot-instructions.md` には、開発中の内容、プロジェクト構造、全体に適用するコーディング標準など、プロジェクトレベルのガイダンスを記載します。
- `*.instructions.md` は、ユニットテスト、Astro コンポーネント、データレイヤーなどのファイル種別や、特定のタスク向けの指示に使います。
- 自然な言葉で明確に記述し、望ましいコードと望ましくないコードの例を示します。

Instruction file の作成方法も AI の利用方法も、ひとつに決まっているわけではありません。試しながら、プロジェクトに合った方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うすべてのプロジェクトに、充実した instruction file を用意しましょう。このプロジェクトには、[UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けのファイルがあります。
>
> Copilot に instruction file の生成を依頼することもできます。VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init` など、利用する環境によって方法が異なります。詳しくは各環境のレッスンを参照してください。
>
> テンプレートや出発点を探している場合は、instruction file、カスタムエージェントなどを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトの custom instruction file を確認する

このリポジトリに用意されている instruction file を確認します。中心となる `copilot-instructions.md` と、タスク別の `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開きます。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの概要と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** 内の **GitHub Actions Workflows** の指示にも注目してください。これらの指示は、Copilot とのすべてのやり取りに適用されます。
3. `.github/instructions` フォルダーを開き、Astro ファイル、Drizzle データレイヤー、テストなどの指示を確認します。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドには、適用対象を決める glob パターンが設定されています。この例では `**/*.test.ts` に一致するような TypeScript のテストファイルが対象です。
5. このプロジェクトのユニットテスト作成に特化した指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、一番下までスクロールします。別の instruction file（`unit-tests.instructions.md` など）やプロジェクト内の既存ファイルへのリンクを確認します。大きな指示を小さく再利用しやすいファイルに分け、Copilot が参考にできる例を示せます。（記載されているパスはリポジトリルートではなく、instruction file からの相対パスです。）

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントはまだ必須ではありません。次の手順で、TSDoc の doc comment とファイルコメントヘッダーのルールを追加します。

## 変更用ブランチを作成する

ベストプラクティスに沿って、変更用ブランチを作成します。

1. 前の演習で使用した codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する*前に* Copilot Chat を使う

custom instruction の効果を見るため、まず現在の指示を使ってプロンプトを送信します。その後、指示を更新して同じプロンプトを再送信し、違いを確認します。

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。該当する手順で説明します。

1. Copilot が必要なコンテキストだけを使うよう、前の演習で開いたエディタータブを閉じます。
2. `src/lib/publishers.ts` を開き、helper を置く場所を Copilot に示します。
3. Chat ビューのエージェントのドロップダウンから **Agent** を選択し、Copilot がファイルを変更できるようにします。

   ![Chat ビューのエージェント選択を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot がプロジェクトを調べてコードを更新します。helper ファイルとテストなど、複数のファイルが変更されることがあります。
6. 提案された helper が、最初の引数に `db` client を受け取り、型付きの publishers 配列を返す関数であることを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータレイヤー規約に基づいています。
7. 提案されたコードに TSDoc の doc comment とファイルレベルのコメントヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくても doc comment を追加することがあります。その場合も問題ありません。重要なのは指示を更新した後の一貫性の改善です。

## リポジトリ全体に適用する標準を追加する

`.github/copilot-instructions.md` は、Copilot にプロジェクトレベルの情報を提供するファイルです。コーディング標準を記載し、提案の質を高めます。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。現在の標準は記載されていますが、コード内ドキュメントのルールはありません。そのため、先ほど生成された helper に doc comment がありませんでした。
3. 既存の標準の直下に、次の Markdown 行を追加し、ファイルコメントヘッダーと TSDoc の doc comment を追加するよう Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、instruction file は全般的なガイダンスを記載するリポジトリレベル (`.github/copilot-instructions.md`) にも、特定の言語、ファイル種別、タスク向けの `*.instructions.md` にも作成できます。今回追加した doc comment のルールのように、プロジェクト全体に適用する標準にはリポジトリレベルのファイルが適しています。

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択してバッファーをクリアします。
2. `src/lib/publishers.ts` を選択し、Copilot が対象ファイルに注目するようにします。
3. 先ほどと**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメントブロックがあることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが含まれていることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Copilot を新しいプロジェクト標準に従わせ、それを次の演習で利用する実際のコードに適用できました。

## 指示を commit してブランチを push する

Instruction file もリポジトリのほかのファイルと同様に、ソース管理で管理します。変更を commit してブランチを push します。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
2. ターミナルで次を実行し、指示と helper の変更が含まれていることを確認します。

   ```bash
   git status
   ```

3. ターミナルで指示の変更と helper の土台に関する Copilot の変更を stage して commit します。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ターミナルからブランチをリポジトリに push します。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチを push できたら、バックログにあるドキュメント標準の Issue に関連付けてプルリクエストを作成します。手動で行うこともできますが、プロジェクトに接続済みの GitHub ツールを使い、Copilot に Issue を探して PR を作成し、マージするよう依頼します。

> [!NOTE]
> Copilot が GitHub 上の操作を行うのは、この演習が初めてです。プロジェクトテンプレートでは GitHub のツールがワークスペースにあらかじめ接続されています。Copilot が初めてツールを使う際、VS Code で GitHub へのサインインが求められることがあります。画面の指示に従って許可してください。この接続方法 (Model Context Protocol) は、後の演習で学びます。

1. codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント選択で **Agent** が選択されていることを確認します。
3. instruction file の更新に関する Issue を探し、現在のブランチから PR を作成するよう Copilot に依頼します。指示の更新と、今後のフィルター機能のための publishers helper の土台の両方を説明に含めます。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が Issue を探して PR を作成します。
5. GitHub の操作を許可するよう求められたら、コマンドを確認して、適切な場合に **Allow** を選択します。
6. PR が作成されたら、次のプロンプトでマージし、ローカルのブランチを `main` に戻すよう依頼します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. GitHub の操作とシェルコマンドを許可するよう求められたら、コマンドを確認して、適切な場合に **Allow** を選択します。

GitHub Copilot の支援を受けてプルリクエストを作成し、マージできました。

## まとめと次のステップ

このプロジェクトの instruction file から Copilot がコンテキストを取得する方法を確認し、VS Code の Copilot Chat を使って次の作業を行いました。

- 既存の指示を使ってコード生成のプロンプトを送り、Copilot の出力を確認した。
- `.github/copilot-instructions.md` にリポジトリ全体の標準を追加した。
- 同じプロンプトを再実行し、提案されたコードが新しい標準に従うことを確認した。
- 指示の変更と helper の土台を `main` に commit し、次の演習で利用できるようにした。

次は、Copilot がコードベース全体に機能を追加するときに、[エージェントモードでこの指示を使います][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズ用 instruction file][instruction-files]
- [Custom instruction 作成のベストプラクティス][instructions-best-practices]
- [Copilot 向け custom instruction 作成の5つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用 custom instruction][personal-instructions]
- [Awesome Copilot — instruction file などのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
