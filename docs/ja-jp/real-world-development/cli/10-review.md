---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2つのプルリクエスト、再利用可能なカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使い、Tailspin Toys で一連の開発ワークフローを進めました。次のことを行いました。

- Codespace を準備し、Copilot CLI をインストールしてプロジェクトを確認し、あらかじめ登録されたフィルタリング issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージしました。
- フィルタリング issue から始め、Plan モードで方針を決め、Autopilot モードで実装し、Interactive モードでレビューしました。
- カスタム指示でエージェントを導き、既存の `quality-checks` スキルをカスタマイズして、ユニットテスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルタリング機能を試しました。
- 要件、テストカバレッジ、スキルの結果、ブラウザー上の根拠を評価するため、品質保証 (QA) カスタムエージェントを作成して選択しました。
- フィルタリングの変更全体をレビューし、フィルタリング PR に対して Agent Merge を承認しました。
- コンテキスト、モデル、共有、任意の cloud agent への委任に使うスラッシュコマンドを確認しました。

## リリースしたもの

このワークショップには、それぞれ更新済みの `main` から作成したブランチで進める、2つの PR の節目があります。

1. **星評価:** 既存の `starRating` と未評価の状態をゲームカードに表示します。
2. **フィルタリングと品質ワークフロー:** フィルタリングを実装し、指示を更新して機能に適用し、`quality-checks` のレポートをカスタマイズして QA プロファイルを作成し、関連するテストを追加します。

フィルタリングの計画から PR 作成まで、同じ会話とブランチを使いました。ワークショップを簡潔にするため、これらの作業を1つの PR にまとめました。

## さまざまな検証方法

自動テスト、自分で行うブラウザー確認、MCP 経由で Copilot が行うブラウザー確認という複数の方法でコードを検証しました。`quality-checks` スキルでユニットテスト、lint、型チェックを実行し、新しい形式で報告しました。QA は PR の前に、それらの結果を要件とテストカバレッジのレビューと組み合わせました。

追加するテストは、実際に存在する不足を補うものでなければなりません。QA の実行で新しいテストが不要と判断されることもあります。マージを承認する前にコードと根拠を確認し、変更後は該当する根拠を更新してください。

## ベストプラクティス

Copilot に与えるコンテキストとツールが、作業の結果に影響します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを設定しました。会話をまたいでこれらのカスタマイズを再利用し、チームのニーズに合わせて調整できます。指示は規約を定め、スキルは繰り返し実行するタスクを説明し、カスタムエージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果をレビューしてください。

タスクに合わせて**モードとモデル**を選びます。実装前に方針を検討するときは **Plan**、範囲を絞った変更を自分で確認しながら進めるときは **Interactive**、範囲が明確なタスクには **Autopilot** を使います。定型的な編集には高速なモデルを、複雑な作業にはより高性能なモデルを選択します。

インフラストラクチャと同じように、コンテキストも重要です。何を、なぜ、どのように作るのかを明確に説明すると、結果が大きく変わります。

## さらに試す

主要なワークフローを学びました。次の CLI 機能も確認してみてください。

- `/review` でコードレビューエージェントに変更の分析を依頼します。
- `/rubber-duck` で問題を相談し、別の視点を得ます。
- `/fleet` で独立したサブタスクを並行して調整します。
- `/worktree` で別のタスクを分離します。
- `/delegate` で Copilot cloud agent にタスクを送ります。

## 次のステップ

どのツールも、使い続けることで上達します。プロダクションコード、趣味のコード、ずっと考えていたけれど作り始められなかった小さなアプリなどで活用してください。チームと学んだことを共有し、互いの経験から学びましょう。ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys を拡張したい場合は、省略可能な[Foundry Backer Concierge シリーズ][foundry]に進みます。他の環境も比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI のカスタマイズ][customize-cli]
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
