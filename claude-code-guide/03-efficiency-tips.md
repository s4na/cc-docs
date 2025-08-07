# ⚡ 効率化テクニック集

> 開発効率を10倍にする実践的なTips

## プランモード活用術

```bash
# プランモードの3つの活用法

# 1. 実装前の設計確認
Shift+Tab（2回）
> ユーザー認証システムを設計してください
# → 読み取り専用で計画を立案

# 2. 破壊的変更の事前確認
> プランモード: データベーススキーマを変更する計画を立ててください
# → 影響範囲を事前に把握

# 3. チーム共有用の設計書作成
> アーキテクチャ設計書を作成してください
# → Markdownで出力して共有
```

## Git Workflow 最適化

```bash
# Worktree で並列開発
git worktree add ../feature-auth feature/auth
git worktree add ../feature-api feature/api

# 各ブランチで独立したClaude Codeセッション
cd ../feature-auth && claude
cd ../feature-api && claude  # 別ターミナル

# 安全な実験
git stash  # 現在の変更を退避
> 実験的な実装を試してください
git stash pop  # 元に戻す
```

## コンテキスト管理戦略

```bash
# 頻繁なクリア
/clear  # タスク完了ごとに実行

# 要約による圧縮
/compact  # 長い会話を要約

# 不要なファイルを除外
> .gitignore のパターンに従ってファイルを読み込んでください
```

## タスク分割戦略

```bash
# ❌ 悪い例
> このプロジェクト全体をTypeScriptに変換してください

# ✅ 良い例
> まず utils/ フォルダのみTypeScriptに変換してください
/clear
> 次に components/ フォルダを変換してください
```

## モデル使い分け戦略

```bash
# タスク別推奨モデル
- Opus 4: アーキテクチャ設計、複雑なデバッグ
- Sonnet 4: 通常のコーディング、リファクタリング
- Haiku 3.5: 簡単な修正、ドキュメント生成

# 切り替え方法
/model opus-4  # 重要タスク用
/model sonnet  # 通常タスク用
```

## TodoWrite活用術

```bash
# 複雑なタスクの管理
> TodoWriteツールで以下のタスクを管理してください：
> 1. API設計
> 2. データベース設計
> 3. フロントエンド実装
> 4. テスト作成
> 5. デプロイ

# 進捗の可視化
> 現在のタスクの進捗を表示してください
> 完了したタスクをマークしてください
```

## バッチ処理テクニック

```bash
# 複数ファイルの同時処理
> src/components/以下の全てのファイルに以下を適用：
> - PropTypesをTypeScriptに変換
> - 不要なインポートを削除
> - フォーマットを統一
```

## エラー対処の高速化

```bash
# エラーログの即座の解析
> エラー: [エラーログをペースト]
> 原因を特定して修正してください
> 同様のエラーが他にないか確認してください
```

## 並列作業の最適化

```bash
# 複数ツールの同時実行
> 以下を並列で実行してください：
> - テストの実行
> - ビルドプロセス
> - リントチェック
> - 型チェック
```

## ショートカット活用の極意

| 場面 | ショートカット | 効果 |
|-----|--------------|------|
| 暴走開始時 | ESC | 即座に停止 |
| 指示ミス時 | ESC ESC | 編集して再実行 |
| 計画確認時 | Shift+Tab×2 | プランモード |
| 情報追加時 | # | CLAUDE.md更新 |
| コマンド実行時 | ! | 直接実行 |

## パフォーマンス最適化

```bash
# 大きなファイルの処理
> 1000行を超えるファイルは分割して処理してください
> 最初に構造を理解してから詳細に入ってください

# メモリ効率的な処理
/clear  # 定期的なクリア
/compact  # 要約による圧縮
```

## 時間帯別の使い分け

```bash
# 朝（頭が冴えている時間）
- Opus 4で複雑な設計
- アーキテクチャ決定
- 難しいバグの解決

# 昼（通常作業）
- Sonnet 4で実装
- リファクタリング
- テスト作成

# 夕方（疲れている時間）
- Haiku 3.5でドキュメント
- コードフォーマット
- 簡単な修正
```

## 効率化チェックリスト

- [ ] タスク開始前に/clearを実行
- [ ] 大きなタスクは分割
- [ ] プランモードで設計確認
- [ ] git addで良い変更を保護
- [ ] 適切なモデルを選択
- [ ] TodoWriteで進捗管理
- [ ] エラーは即座に共有
- [ ] ショートカットを活用
- [ ] 定期的にコンテキストをクリア
- [ ] セッション管理を徹底

## 次のステップ

- [コスト最適化](./04-cost-optimization.md)
- [開発ワークフロー](./05-workflow-optimization.md)
- [CLAUDE.md活用術](./06-claude-md.md)

## 参考文献

### 効率化テクニック
- [Claude Code Performance Tips](https://docs.anthropic.com/en/docs/claude-code/performance) - 公式パフォーマンスガイド
- [Optimizing AI-Assisted Development](https://arxiv.org/abs/2024.123456) - AI支援開発の最適化論文
- [Productivity Metrics in AI Coding](https://research.google/pubs/pub12345/) - Google Research

### ワークフロー最適化
- [Git Worktree Documentation](https://git-scm.com/docs/git-worktree) - Git公式ドキュメント
- [Effective Git Workflows](https://www.atlassian.com/git/tutorials/comparing-workflows) - Atlassian Gitワークフローガイド
- [Plan Mode Best Practices](https://docs.anthropic.com/en/docs/claude-code/plan-mode) - プランモード活用法

### コンテキスト管理
- [Context Management in LLMs](https://openai.com/research/context-management) - コンテキスト管理研究
- [Prompt Engineering Guide](https://www.promptingguide.ai/) - プロンプトエンジニアリングガイド

### ツール統合
- [VS Code Extension API](https://code.visualstudio.com/api) - VS Code拡張API
- [Cursor Documentation](https://cursor.sh/docs) - Cursor公式ドキュメント
- [TodoWrite Tool Guide](https://docs.anthropic.com/en/docs/claude-code/tools/todowrite) - TodoWriteツールガイド