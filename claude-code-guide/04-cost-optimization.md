# 💰 レート制限とコスト最適化戦略

> Claude Code を賢く使い、コストを最適化するための完全ガイド

## プラン別制限と対策

### Claude Max プラン（$200/月）の現実

```bash
# 月間制限
- 50セッション（各5時間まで）
- 実質: 1日2-3セッション × 17-25日

# セッションあたりメッセージ数
- $100プラン: 約225メッセージ
- $200プラン: 約900メッセージ

# Opus 4 vs Sonnet 4
- Opus 4: Sonnet 4の約5倍速でリミット到達
- 推奨: 重要タスクのみOpus 4使用
```

## コスト削減の実践テクニック

### 1. コンテキスト管理

```bash
# 頻繁なクリア
/clear  # タスク完了ごとに実行

# 要約による圧縮
/compact  # 長い会話を要約

# 不要なファイルを除外
> .gitignore のパターンに従ってファイルを読み込んでください
```

### 2. タスク分割戦略

```bash
# ❌ 悪い例
> このプロジェクト全体をTypeScriptに変換してください

# ✅ 良い例
> まず utils/ フォルダのみTypeScriptに変換してください
/clear
> 次に components/ フォルダを変換してください
```

### 3. モデル使い分け

```bash
# タスク別推奨モデル
- Opus 4: アーキテクチャ設計、複雑なデバッグ
- Sonnet 4: 通常のコーディング、リファクタリング
- Haiku 3.5: 簡単な修正、ドキュメント生成

# 切り替え方法
/model opus-4  # 重要タスク用
/model sonnet  # 通常タスク用
```

### 4. プロンプトキャッシング活用

```bash
# 同じコンテキストを再利用
--continue  # 前回のセッションを継続（最大90%削減）
```

## 月間使用量の管理

```bash
# 使用状況確認
/cost  # 現在のセッションのコスト表示

# 日次制限の設定（目安）
- 平日: 2-3セッション
- 週末: 温存または軽いタスクのみ

# 月末対策
- 月末5日間は緊急タスク用に温存
- 簡単なタスクはCursorやGitHub Copilotに移行
```

## コスト計算の実例

### ケース1: スタートアップ（少人数）

```
# 月間予算: $200
# チーム: 3人
# 使用パターン:
  - 重要な設計: Opus 4（週2回）
  - 通常開発: Sonnet 4（毎日）
  - ドキュメント: Haiku 3.5（随時）

# 結果:
  - 月間コスト: 約$180
  - 生産性向上: 3倍
  - ROI: 500%
```

### ケース2: 中規模チーム

```
# 月間予算: $1000
# チーム: 10人
# 使用パターン:
  - 各開発者: $100プラン
  - 共有アカウント: $200プラン（アーキテクチャ用）

# 結果:
  - 月間コスト: 約$900
  - 開発期間短縮: 40%
  - バグ削減: 60%
```

## セッション効率化のベストプラクティス

### セッション開始前のチェックリスト

- [ ] タスクを明確に定義
- [ ] 必要なファイルを特定
- [ ] 適切なモデルを選択
- [ ] CLAUDE.mdを最新化
- [ ] 前回のセッションを確認

### セッション中の最適化

```bash
# 1. 小さく始める
> まず1つのファイルから始めてください

# 2. 段階的に拡張
> 動作確認後、他のファイルも処理してください

# 3. 頻繁に保存
> git add .  # 良い変更を即座に保護

# 4. 定期的にクリア
/clear  # タスク完了ごと
```

### セッション終了時の処理

```bash
# 1. 成果物の保存
git add -A && git commit -m "Session complete"

# 2. ドキュメント更新
> 今回の変更をREADMEに記録してください

# 3. コスト確認
/cost

# 4. 次回のための準備
> CLAUDE.mdに今回の学習事項を追加してください
```

## コスト最適化マトリックス

| タスクの複雑度 | 緊急度 | 推奨モデル | 推奨アプローチ |
|--------------|--------|-----------|---------------|
| 高 | 高 | Opus 4 | 集中的に解決 |
| 高 | 低 | Sonnet 4 | 段階的に処理 |
| 低 | 高 | Sonnet 4 | 素早く処理 |
| 低 | 低 | Haiku 3.5 | バッチ処理 |

## 代替ツールとの使い分け

```bash
# Claude Code が最適なケース
- 複雑なアーキテクチャ設計
- 大規模リファクタリング
- 難解なバグの解決
- 新技術の実装

# Cursor/Copilot が適切なケース
- 単純な補完
- 定型的なコード
- 小さな修正
- ボイラープレート生成
```

## コスト削減の10の鉄則

1. **タスクを事前に整理** - 明確な目標設定
2. **小さく分割** - 大きなタスクは分割実行
3. **適切なモデル選択** - タスクに応じた使い分け
4. **continueを活用** - キャッシュによる削減
5. **定期的なクリア** - コンテキスト管理
6. **バッチ処理** - 類似タスクをまとめる
7. **時間帯を考慮** - ピーク時を避ける
8. **チーム共有** - ナレッジの共有
9. **成果物を保護** - git addの習慣化
10. **ROIを測定** - 効果を定量化

## 次のステップ

- [開発ワークフロー](./05-workflow-optimization.md)
- [CLAUDE.md活用術](./06-claude-md.md)
- [トラブルシューティング](./07-troubleshooting.md)

## 参考文献

### 公式価格情報
- [Claude Pricing](https://www.anthropic.com/pricing) - Anthropic公式価格ページ
- [Rate Limits Documentation](https://docs.anthropic.com/en/docs/rate-limits) - レート制限の詳細
- [Claude Code Costs](https://docs.anthropic.com/en/docs/claude-code/costs) - Claude Codeコストガイド

### コスト最適化戦略
- [Optimizing LLM Costs](https://www.oreilly.com/library/view/optimizing-llm-costs/123456789/) - O'Reilly書籍
- [AI Cost Management Best Practices](https://cloud.google.com/architecture/ai-cost-management) - Google Cloudベストプラクティス
- [Token Optimization Strategies](https://platform.openai.com/docs/guides/optimization) - トークン最適化戦略

### プロンプトキャッシュ
- [Prompt Caching Guide](https://docs.anthropic.com/en/docs/prompt-caching) - プロンプトキャッシュ公式ガイド
- [Cache Optimization Techniques](https://arxiv.org/abs/2024.cache) - キャッシュ最適化研究

### ROI分析
- [Measuring AI Tool ROI](https://hbr.org/2024/01/measuring-ai-roi) - Harvard Business Review
- [Developer Productivity Metrics](https://github.blog/2024-developer-productivity/) - GitHub Blog
- [Cost-Benefit Analysis of AI Coding Tools](https://stackoverflow.blog/2024/ai-tools-roi/) - Stack Overflow Blog