---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で進める必要がある場合や、Copilot に把握しておいてほしい背景情報がある場合は、そのコンテキストを利用できるようにします。[Instruction file][instruction-files] を使うと、何を実行するかだけでなく、どのように実行するかも Copilot に伝えられます。

この演習では、次の内容を学習します。

- リポジトリの custom instruction とパス単位の instruction file を通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot に渡る方法を確認する。
- `.github/copilot-instructions.md` に、リポジトリ全体に適用する標準を追加する。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、ここではプロンプトの変更前後を比較しません。Copilot cloud agent は GitHub Issue に対して非同期で作業するため、リアルタイムで並べて効果を示すのは困難です。このハーネスの後半で、cloud agent が作成したプルリクエストをレビューし、instruction file の影響を確認します。

## Instruction file

### シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。たとえば、次のような内容です。

- データレイヤーには必ずユニットテストを追加する。
- UI はダークモードにし、現代的な見た目にする。
- コードのドキュメントには TSDoc の doc comment を使う。
- 各ファイルの先頭に、ファイルの役割を説明するコメントブロックを追加する。

instruction file を使って、Copilot がこれらのプラクティスに沿って作業するために必要な情報を提供します。

### Custom instruction

Custom instruction を使うと、コンテキストや好みを Copilot に伝え、コーディングスタイルや要件を理解させられます。コーディング規約、ライブラリ、追加したいコメントの種類などを指定でき、より関連性の高い提案やコードスニペットを得られます。リポジトリ全体に適用する指示も、特定のファイル種別に適用するタスクレベルの指示も作成できます。

Instruction file には次の2種類があります。

- `.github/copilot-instructions.md` は、リポジトリへの**すべて**のリクエストで Copilot に送られる単一の instruction file です。使用する技術スタック、開発中のものの概要、ベストプラクティス、その他の全般的なガイダンスなど、ほとんどのチャットや CLI リクエストに関係するプロジェクト情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイル種別向けに作成します。TypeScript や Astro などの言語、UI コンポーネントの作成、ユニットテストの追加といったタスクのガイドラインを記載できます。

> [!NOTE]
> IDE では、instruction file は Copilot Chat によるコード生成で使われます。コード補完や next-edit suggestion には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリレベルの指示と `applyTo` front matter を持つ `*.instructions.md` ファイルの両方を使います。
>
> Copilot は、AGENTS.md や CLAUDE.md など、[ほかの標準に基づく instruction file][custom-instructions-support]もサポートしています。

### Instruction file を管理するためのベストプラクティス

instruction file の作成に関する詳しい説明は、このワークショップの範囲外です。サンプルプロジェクトには、一般的な方法を示す例が含まれています。

- `copilot-instructions.md` には、開発中の内容、プロジェクト構造、全体に適用するコーディング標準など、プロジェクトレベルのガイダンスを記載します。
- `*.instructions.md` は、ユニットテスト、Astro コンポーネント、データレイヤーなどのファイル種別や、特定のタスク向けの指示に使います。
- 自然な言葉で明確に記述します。望ましいコードと望ましくないコードの例を示します。

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
2. プロジェクトの概要と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の **GitHub Actions Workflows** に関するガイダンスにも注目してください。これらの指示は、Copilot とのすべてのやり取りに適用されます。
3. `.github/instructions` フォルダーを開き、Astro ファイル、Drizzle データレイヤー、テストなどの指示を確認します。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドには、適用対象を決める glob パターンが設定されています。この例では、`**/*.test.ts` に一致するような TypeScript のテストファイルが対象です。
5. このプロジェクトのユニットテスト作成に特化した指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、一番下までスクロールします。`unit-tests.instructions.md` などの別の instruction file や、プロジェクト内の既存ファイルへのリンクを確認します。指示を小さな再利用可能なファイルに分け、Copilot が参考にできる例を示せます。（記載されているパスはリポジトリルートではなく、instruction file からの相対パスです。）

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントはまだ必須ではありません。次の手順で、TSDoc の doc comment とファイルコメントヘッダーのルールを追加します。

## リポジトリ全体に適用する標準を追加する

ここで編集するのは、コード内に TSDoc の doc comment とファイルレベルのコメントヘッダーを追加するというプロジェクト全体のルールです。後続の演習で Issue を割り当てると、cloud agent はこの指示を参照します。

編集の前に作業ブランチを準備します。cloud agent は Issue の対象ブランチにある指示を読み取りますが、変更を分けて commit します。

1. codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` は Copilot にプロジェクトレベルの情報を提供します。コーディング標準を明記して、提案の質を高めます。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。現在の標準は記載されていますが、コード内ドキュメントのルールはありません。そのため、生成された helper には doc comment がありませんでした。
3. 既存の標準の直下に、次の Markdown 行を追加します。これにより、Copilot にファイルコメントヘッダーと TSDoc の doc comment を追加するよう指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、instruction file は全般的なガイダンスを記載するリポジトリレベル (`.github/copilot-instructions.md`) にも、特定の言語、ファイル種別、タスク向けの `*.instructions.md` にも作成できます。今回追加した doc comment のルールのように、プロジェクト全体に適用する標準にはリポジトリレベルのファイルが適しています。

## 指示の変更を commit、push、マージする

Cloud agent は Issue の対象ブランチにある instruction file を読み取ります。次の演習で Copilot に Issue を割り当てると、エージェントは `main` からブランチを作成します。そのため、cloud agent が変更した指示を読み取れるよう、`main` にマージする必要があります。

1. 変更を stage して commit します。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチを push します。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` に向けたプルリクエストを開き、マージします。次の演習で Issue に取り組むとき、cloud agent はこの指示を読み取ります。

> [!TIP]
> このワークショップで `main` に直接作業する場合は、ブランチの作成を省略して `main` に直接 commit できます。ここでは、実際のプロジェクトで一般的な手順に沿うためにブランチを使っています。

## まとめと次のステップ

このプロジェクトの instruction file から Copilot がコンテキストを取得する方法を確認し、`.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。残りのハーネスで cloud agent が作成するプルリクエストを通じて、この標準が適用される様子を確認します。

次は、[最初の Issue を Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズ用 instruction file][instruction-files]
- [Custom instruction 作成のベストプラクティス][instructions-best-practices]
- [Copilot 向け custom instruction 作成の5つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — instruction file などのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
