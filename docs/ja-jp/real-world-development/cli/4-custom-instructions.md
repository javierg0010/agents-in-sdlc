---
title: "レッスン 4 - カスタム指示で Copilot を導く"
description: "リポジトリの指示を確認し、ドキュメント標準を追加して、フィルター機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使って作業するときは、コンテキストが重要です。特定の方法でタスクを進める必要がある場合、そのガイダンスを Copilot が利用できるようにします。[Instruction file][instruction-files] では、必要なコードだけでなく、その構成方法も説明できます。フィルター機能を実装できたので、Copilot が参照した指示を確認し、ドキュメント標準を追加して、コードに適用します。

このレッスンでは、次の内容を学習します。

- リポジトリの指示とパス単位の instruction file がエージェントにどう渡されるかを確認する。
- コーディング標準に従うよう instruction file を更新する。
- instruction file がコードに与える影響を確認する。

## シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。たとえば、次のような内容です。

- コメントでは、コードの内容を繰り返すのではなく、意図や自明でない判断を説明する。
- `db/` と `src/lib/` の公開関数では、`db` 引数を注入可能な場合はその点も含め、目的、パラメーター、戻り値を TSDoc/JSDoc で説明する。
- 再利用可能な Astro コンポーネントでは `Props` の契約を文書化し、関連するコードの変更時にコメントも最新に保つ。
- 既存のフォーマットと lint のガイダンスを維持する。

instruction file を使って、Copilot がこれらのプラクティスに沿って作業できるようにします。

## Instruction file

Custom instruction を使うと、コーディングスタイルや要件などのコンテキストを Copilot に渡せます。これにより、より関連性の高い提案やコードスニペットを得られます。リポジトリ全体に適用する指示も、特定のファイル種別に適用するタスクレベルの指示も作成できます。

Instruction file には次の2種類があります。

- `.github/copilot-instructions.md` は、リポジトリへの**すべて**のリクエストで Copilot に送られる単一の instruction file です。ほとんどのリクエストに関係するプロジェクトレベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイル種別、タスクに関するガイドラインを記載します。

> [!NOTE]
> ほかの instruction 形式への対応状況は環境によって異なります。特定の形式に依存する前に、[custom instructions のサポートに関するリファレンス][custom-instructions-support]を確認してください。

## このプロジェクトの custom instruction file を確認する

スターター プロジェクトには instruction file が用意されています。変更を加える前に、既存のファイルを確認します。

1. Codespace に戻ります。
2. Codespaces のエディターで（ターミナルではなく）`.github/copilot-instructions.md` を開きます。
3. プロジェクトの概要とコーディングガイダンスを確認します。この指示は、リポジトリでの Copilot とのすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開き、Astro ファイル、Drizzle データレイヤー、テストなどに関する指示を確認します。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、指示の適用対象を決める glob を設定しています。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの instruction file や既存のプロジェクトファイルへのリンクを確認します。大きな指示を小さく再利用しやすいファイルに分け、Copilot が参考にできる例を示せます。

## チームのガイダンスに合わせて instruction file を更新する

既存のファイルは出発点として十分ですが、まだ改善できます。中心となる `copilot-instructions.md` を変更し、新しく生成される TypeScript に TSDoc コメントが追加されるようにします。

1. `.github/copilot-instructions.md` で **Code formatting guidance** セクションを探します。35 行目付近にあります。
2. 最後の箇条書きとして、次を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを適用する

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。指示を保存したら、フィルター機能の会話を終了して再開します。これにより、機能のコンテキストを保ったまま新しい指示を読み込めます。

1. フィルター機能の会話が開いている場合は、`/exit` を入力して終了します。Copilot CLI は会話を自動的に保存します。
2. `game-filters-cli` ブランチのリポジトリルートから、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   選択を求められた場合は、フィルター機能の会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. 新しいガイダンスを適用するよう Copilot に依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。新しい TSDoc コメントが追加され、コードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が instruction file からコンテキストを取得する方法を確認し、新しい標準を機能に適用しました。このレッスンでは、次の作業を行いました。

- リポジトリの指示とパス単位の instruction file がエージェントにどう渡されるかを確認した。
- コーディング標準に従うよう instruction file を更新した。
- instruction file がコードに与える影響を確認した。

次は、[再利用可能な quality-checks スキルをカスタマイズして実行し][next-lesson]、lint とテストを一貫して実行します。

## リソース

- [Copilot CLI 用の custom instruction を追加する][instruction-files]
- [Custom instruction のサポート][custom-instructions-support]
- [Custom instruction 作成のベストプラクティス][instructions-best-practices]
- [Awesome Copilot — instruction file などのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
