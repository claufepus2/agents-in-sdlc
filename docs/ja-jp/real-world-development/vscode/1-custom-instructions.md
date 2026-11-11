---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェントモード →][next-lesson]

生成 AI を使う際はコンテキストが重要です。特定の方法でタスクを実行する必要がある場合や、Copilot に知っておいてほしい背景情報がある場合、そのコンテキストを参照できるようにします。Copilot に具体的なコンテキストを伝える方法はいくつかあります。その中心となるのが[指示ファイル][instruction-files]です。指示ファイルを使うと、コード生成の指針を伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパス別の指示ファイルを使って、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメントの規約が Copilot に伝わる仕組みを確認します。
- 現在の指示のもとでフィルタリング機能の最初のデータ部分 (販売元ヘルパー) を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する新しい規約を追加します。
- 同じプロンプトを再実行し、生成コードに新しい規約が反映される様子を確認します。
- 指示の更新とフィルタリング機能の一部を `main` にコミットし、次の演習で Copilot が最新のガイダンスを使えるようにします。

> [!CAUTION]
> 生成コードが設定した規約から外れる場合があります。Copilot は確率的に動作するためです。この演習では、出力の文字列を完全に一致させるのではなく、指示の更新後に動作の傾向が変わることを確認します。

## 指示ファイル

### シナリオ

適切な開発チームである Tailspin Toys には、開発手法に関するガイドラインと要件があります。具体的には、次のとおりです。

- データ層には必ずユニットテストを用意します。
- UI はダークモードにし、現代的な見た目にします。
- コードのドキュメントには TSDoc のドキュメントコメントを使います。
- 各ファイルの先頭に、ファイルの役割を説明するコメントブロックを追加します。

指示ファイルを使い、これらの開発慣行に沿ってタスクを実行するためのコンテキストを Copilot に与えます。

### カスタム指示

カスタム指示を使うと、コーディングスタイルや要件を Copilot に伝えられます。好みのコーディング規約、ライブラリ、コメントの方針などを指定でき、提案やコードスニペットをより適切なものにできます。リポジトリ全体に適用する指示も、特定のファイル種別に適用する指示も作成できます。

指示ファイルには次の2種類があります。

- `.github/copilot-instructions.md` は、リポジトリ内のすべてのリクエストで Copilot に送られる単一の指示ファイルです。技術スタック、開発中の内容の概要、ベストプラクティスなど、Copilot に送る多くのリクエストに関係するプロジェクト全体の情報を記述します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイル種別に利用できます。TypeScript や Astro などの言語、UI コンポーネントの作成やユニットテストの作成などのタスクに関するガイドラインを記述できます。

> [!NOTE]
> IDE で使う場合、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリレベルの指示と `applyTo` frontmatter を持つ `*.instructions.md` ファイルの両方をコード生成時に使います。
>
> Copilot は AGENTS.md や CLAUDE.md など、他の標準に沿った指示ファイルにも対応しています。詳細は[カスタム指示のサポート][custom-instructions-support]を参照してください。

### 指示ファイルの管理に関するベストプラクティス

指示ファイルの作成について詳しく扱うのはこのワークショップの範囲外ですが、サンプルプロジェクトには代表的な方法が含まれています。基本的には次のようにします。

- `copilot-instructions.md` には、開発中の内容、プロジェクトの構造、全体的なコーディング規約など、プロジェクトレベルのガイダンスを記述します。
- `*.instructions.md` は、ファイル種別 (ユニットテスト、Astro コンポーネント、データ層など) や特定のタスクに関する指示に使います。
- 自然な言葉を使い、指示を明確にします。コードのよい例と悪い例を示します。

指示ファイルの作成方法はひとつではありません。AI の使い方と同様に、プロジェクトに合った方法を試しながら見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、十分な指示ファイルを用意しましょう。このプロジェクトの指示ファイルを確認すると、[UI の更新][ui-instructions]や [Astro][astro-instructions] など、さまざまなタスク用のファイルがあります。
>
> Copilot に指示ファイルを作成してもらうこともできます。VS Code の **Configure Chat → Generate Agent Instructions** や Copilot CLI の `/init` など、各環境で方法は異なります。このワークショップでは、該当する手順で説明します。
>
> テンプレートや出発点を探している場合は、指示ファイル、カスタムエージェント、その他のリソースを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに含まれる指示ファイルを確認します。中心となる `copilot-instructions.md` と、タスク別の `*.instructions.md` ファイルがあります。エディターまたは GitHub Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの概要と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。これらの指示は、Copilot を使うすべてのやりとりに適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ層、テストなどの指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、リポジトリのルートからの glob を指定します。たとえば `**/*.test.ts` に一致する TypeScript のテストファイルが対象になります。
5. このプロジェクトでユニットテストを作成する際の指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などの他の指示ファイルや、プロジェクト内の既存ファイルへのリンクを確認します。これにより、大きな指示セットを小さく再利用可能なファイルに分け、コード生成の参考例を Copilot に示せます (リンクのパスはリポジトリのルートではなく、指示ファイルの場所からの相対パスです)。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング規約が記載されていますが、コード内のドキュメント化はまだ求めていません。次の手順で TSDoc コメントとファイルヘッダーコメントの規則を追加します。

## 変更用ブランチを作成する

ベストプラクティスに従い、変更用のブランチを作成します。

1. 前の演習で使った codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. ブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に、codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

カスタム指示の影響を確認するため、まず現在の指示ファイルのままでプロンプトを送信します。その後、指示を更新して同じプロンプトを再送信し、違いを確認します。

1. 前の演習で開いたエディターのタブを閉じ、Copilot に必要なコンテキストだけを与えます。
2. `src/lib/publishers.ts` を開き、ヘルパーの配置場所を Copilot に伝えます。
3. Chat ビューのエージェントドロップダウンから **Agent** を選択し、ファイルの変更を適用できるようにします。

   ![Chat ビューでエージェントを選択するドロップダウンのスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot はプロジェクトを調べてコードを更新します。多くの場合、ヘルパーファイルとテストが変更されます。
6. 提案されたヘルパーが、`db` クライアントを最初の引数として受け取り、販売元の型付き配列を返す関数であることを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ層の規約によるものです。
7. 提案されたコードに TSDoc コメントとファイルレベルのコメントヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくてもドキュメントコメントを追加する可能性があります。その場合も問題ありません。重要なのは、指示を更新した後に一貫性が高まることです。

## リポジトリの新しい規約を追加する

前述のとおり、`.github/copilot-instructions.md` はプロジェクト全体の情報を Copilot に提供します。コード提案の質を高めるため、リポジトリのコーディング規約を記述しましょう。

1. `.github/copilot-instructions.md` を開き直します。
2. 27行目付近にある **Code formatting requirements** セクションを探します。プロジェクトのコーディング規約が記載されていますが、コード内ドキュメントの規則はありません。そのため、先ほどのヘルパーにはドキュメントコメントが含まれていません。
3. 既存の規約のすぐ下に次の Markdown を追加し、ファイルヘッダーと TSDoc コメントを Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、指示ファイルはリポジトリレベル (`.github/copilot-instructions.md`) に作成して全体に適用することも、特定の言語、ファイル種別、タスク向けに `*.instructions.md` として作成することもできます。今回追加したドキュメントコメントの規則のようなプロジェクト全体の規約には、リポジトリレベルのファイルが適しています。

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択してコンテキストをクリアします。
2. `src/lib/publishers.ts` を選択して、Copilot が正しいファイルに注目するようにします。
3. 先ほどと**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの冒頭に、次のようなコメントブロックが追加されていることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが含まれることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Copilot に新しいプロジェクト規約を適用させ、次の演習で使う実際のコードに反映できました。

## 指示とブランチをコミットしてプッシュする

指示ファイルも他のリソースと同様に、ソース管理で管理します。変更をコミットしてリポジトリにブランチをプッシュしましょう。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルウィンドウを開きます。
2. ターミナルで次のコマンドを実行し、指示とヘルパーの変更があることを確認します。

   ```bash
   git status
   ```

3. ターミナルで指示の変更と、ヘルパーの基礎部分に関して Copilot が変更したすべてのファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ターミナルからリポジトリにブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、バックログにあるドキュメント規約の issue に関連付けてプルリクエストを作成します。手動で行う代わりに、プロジェクトに接続済みの GitHub ツールを使って Copilot に依頼できます。issue を見つけて、issue をクローズする PR を作成し、マージするようにプロンプトを送信します。

> [!NOTE]
> Copilot に GitHub での操作を依頼するのはこの演習が初めてです。プロジェクトテンプレートには GitHub ツールがすでに設定されているため、追加のセットアップは必要ありません。Copilot が初めてツールを使うとき、VS Code から GitHub へのサインインを求められることがあります。画面の指示に従ってください。後の演習では、この接続の仕組みである Model Context Protocol (MCP) を学びます。

1. codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント選択で **Agent** が選択されていることを確認します。
3. 指示ファイルの更新に関する issue を探し、現在のブランチから PR を作成して、指示の更新と今後のフィルタリング作業に向けた販売元ヘルパーの基礎部分の両方を説明するよう Copilot に依頼します。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が issue を探して PR を作成します。
5. GitHub の操作を許可するよう Copilot に求められたら、コマンドを確認して、適切な場合は **Allow** を選択します。
6. PR が作成されたら、次のプロンプトで PR のマージと `main` への切り替えを依頼します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. GitHub の操作とシェルコマンドの実行を許可するよう求められたら、コマンドを確認して、適切な場合は **Allow** を選択します。

GitHub Copilot の支援でプルリクエストを作成してマージできました。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを取得する仕組みを確認し、VS Code の Copilot Chat を使って次のことを行いました。

- 既存の指示を使ってコード生成のプロンプトを送り、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体の規約を追加しました。
- 同じプロンプトを再実行し、提案されたコードに新しい規約が反映される様子を確認しました。
- 指示の更新とヘルパーの基礎部分を `main` にコミットし、次の演習で使えるようにしました。

次は[エージェントモード][next-lesson]を使い、コードベース全体にわたる新機能を追加します。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot によりよいカスタム指示を書くための5つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
