---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使う際はコンテキストが重要です。特定の方法でタスクを実行する必要がある場合や、Copilot に知っておいてほしい背景情報がある場合、そのコンテキストを参照できるようにします。[指示ファイル][instruction-files]を使うと、何を実行するかだけでなく、どのように実行するかも Copilot に伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパス別の指示ファイルを使って、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメントの規約が Copilot に伝わる仕組みを確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する新しい規約を追加します。

> [!NOTE]
> VS Code および CLI のハーネスとは異なり、この演習では変更前後のプロンプトを比較しません。Copilot cloud agent は GitHub issue をもとに非同期で動作するため、影響をリアルタイムで並べて示すのが難しいためです。このハーネスの後半で cloud agent が作成したプルリクエストをレビューするときに、指示ファイルの効果を確認できます。

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
2. プロジェクトの概要と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の **GitHub Actions Workflows** の指示も確認してください。これらの指示は、Copilot を使うすべてのやりとりに適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ層、テストなどの指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、リポジトリのルートからの glob を指定します。たとえば `**/*.test.ts` に一致する TypeScript のテストファイルが対象になります。
5. このプロジェクトでユニットテストを作成する際の指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開いて末尾までスクロールします。`unit-tests.instructions.md` などの他の指示ファイルや、プロジェクト内の既存ファイルへのリンクを確認します。これにより、大きな指示セットを小さく再利用可能なファイルに分け、Copilot が参照できる例を示せます (リンクのパスはリポジトリのルートではなく、指示ファイルの場所からの相対パスです)。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング規約が記載されていますが、コード内のドキュメント化はまだ求めていません。次の手順で TSDoc コメントとファイルヘッダーコメントの規則を追加します。

## リポジトリの新しい規約を追加する

この演習で行う編集は、コード内のドキュメントとして TSDoc コメントとファイルレベルのコメントヘッダーを追加する、プロジェクト全体の規則です。Cloud agent は、このハーネスの後半で issue に取り組む際に規則を読み取ります。

編集する前に、作業用のブランチを作成します (cloud agent は issue の対象ブランチから指示を読み取りますが、ここでは変更をきちんとコミットします)。

1. codespace のターミナルからブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

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

## 指示の更新をコミット、プッシュ、マージする

Cloud agent は、issue の対象ブランチにある指示ファイルを読み取ります。次の演習で issue を Copilot に割り当てると、Copilot は `main` からブランチを作成します。そのため、cloud agent が更新した指示を読み取れるよう、`main` にマージする必要があります。

1. 変更をステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` に向けたプルリクエストを開き、マージします。次の演習で issue を割り当てると、cloud agent がこの指示を読み取ります。

> [!TIP]
> このワークショップで `main` に直接作業したい場合は、ブランチを作成せずに `main` にコミットしてもかまいません。実際のプロジェクトに近い手順にするため、ここではブランチを作成しています。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを取得する仕組みを確認し、`.github/copilot-instructions.md` にリポジトリ全体の規約を追加しました。このハーネスの後半で、cloud agent が作成したプルリクエストにその規約が反映されます。

次は[最初の issue を Copilot cloud agent に割り当て][next-lesson]ます。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot によりよいカスタム指示を書くための5つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://awesome-copilot.github.com/
