# 📚 CLAUDE.md 活用術

> プロジェクトコンテキストを最大限に活用する方法

## 効果的なCLAUDE.md構成

```markdown
# プロジェクト概要
Eコマースプラットフォーム - Next.js 14, TypeScript, Prisma, PostgreSQL

# アーキテクチャ
- フロントエンド: Next.js App Router
- バックエンド: API Routes
- データベース: PostgreSQL (Supabase)
- 認証: NextAuth.js

# 開発コマンド
- `npm run dev`: 開発サーバー起動 (http://localhost:3000)
- `npm run build`: プロダクションビルド
- `npm run test`: テスト実行（単体テストを優先）
- `npm run lint`: ESLint実行
- `npm run typecheck`: TypeScript型チェック

# コーディング規約
## 命名規則
- コンポーネント: PascalCase (例: UserProfile.tsx)
- 関数: camelCase (例: getUserById)
- 定数: UPPER_SNAKE_CASE (例: MAX_RETRY_COUNT)
- ファイル: kebab-case (例: user-service.ts)

## インポート順序
1. React/Next.js
2. 外部ライブラリ
3. 内部モジュール
4. 型定義
5. スタイル

## エラーハンドリング
- 全ての非同期処理でtry-catchを使用
- カスタムエラークラスを使用: `@/lib/errors`
- エラーログは構造化: `logger.error({ error, context })`

# Git ワークフロー
1. feature/[ticket-number]-[brief-description] ブランチを作成
2. コミットメッセージ: type(scope): message
   - feat: 新機能
   - fix: バグ修正
   - refactor: リファクタリング
   - test: テスト追加
   - docs: ドキュメント更新
3. PR作成前に必ずテストとlintを実行
4. レビュー承認後にsquash merge

# テスト戦略
- 単体テスト: Jest + React Testing Library
- E2Eテスト: Playwright（重要フローのみ）
- カバレッジ目標: 80%以上
- TDD推奨: テストファーストで実装

# パフォーマンス要件
- Lighthouse Score: 90以上
- First Contentful Paint: 1.5秒以内
- Time to Interactive: 3秒以内

# セキュリティ
- 環境変数は.env.localで管理（絶対にコミットしない）
- SQLインジェクション対策: Prismaのプリペアドステートメント使用
- XSS対策: React のデフォルトエスケープを信頼
- CSRF対策: NextAuth.jsのCSRFトークン使用

# デプロイ
- ステージング: Vercel Preview (PRごと自動)
- 本番: Vercel Production (mainブランチ自動)

# 外部サービス
- 決済: Stripe
- メール: SendGrid
- 画像最適化: Cloudinary
- 監視: Sentry

# トラブルシューティング
- ビルドエラー時: `rm -rf .next node_modules && npm install`
- 型エラー時: `npm run typecheck -- --noEmit false`
- Prismaエラー: `npx prisma generate && npx prisma db push`

# 既知の問題
- Safari でのCSS Grid表示崩れ → flexboxで代替
- iOS でのinput要素のズーム → meta viewportで制御

@package.json  # 依存関係の詳細
@.env.example  # 環境変数のテンプレート
@docs/api.md   # API仕様書
```

## インポート機能の活用

### ファイル分割による管理

```markdown
# メインのCLAUDE.md
プロジェクト設定 @config/project.md
API仕様 @docs/api-spec.md
デザインシステム @docs/design-system.md

# チーム別設定
フロントエンドチーム @team/frontend.md
バックエンドチーム @team/backend.md
```

### 設定ファイルの例

```markdown
# config/project.md
## 技術スタック
- Framework: Next.js 14.2.5
- Language: TypeScript 5.3
- Styling: Tailwind CSS 3.4
- Database: PostgreSQL 15
- ORM: Prisma 5.10

## 環境設定
- Node.js: 20.x LTS
- Package Manager: npm 10.x
- IDE: VS Code推奨
```

## ローカルとグローバルの使い分け

```bash
# プロジェクト共有（Git管理）
./CLAUDE.md  # チーム全体の規約

# 個人設定（Git無視）
~/.claude/CLAUDE.md  # 個人の作業スタイル

# プロジェクト個人設定（Git無視）
./CLAUDE.local.md  # プロジェクト固有の個人設定
```

### CLAUDE.local.md の例

```markdown
# 個人的なメモ
- UserServiceのバグ: line 234で型エラーが発生しやすい
- APIテスト時は必ずVPNを切る
- ステージング環境: https://staging.example.com

# よく使うコマンドエイリアス
- quick-test: npm test -- --watch
- full-check: npm run lint && npm run typecheck && npm test
```

## 動的な情報の管理

### タスク管理

```markdown
# 現在のスプリント
## Sprint 23 (2024-01-15 - 2024-01-28)
- [ ] ユーザー認証の実装
- [ ] 決済システムの統合
- [x] ダッシュボードUI
```

### 学習事項の蓄積

```markdown
# 解決済みの問題
## 2024-01-20: ビルドが遅い問題
- 原因: node_modulesのキャッシュ不足
- 解決: turbopackを有効化、CI/CDでキャッシュ設定
- 結果: ビルド時間が5分→1分に短縮

## 2024-01-18: 型エラーの多発
- 原因: tsconfig.jsonの設定不備
- 解決: strictモードを有効化、型定義を整備
```

## カスタムコマンドの定義

```markdown
# カスタムコマンド
## /review
コードレビューを実行:
1. 変更されたファイルを特定
2. セキュリティチェック
3. パフォーマンス分析
4. コード品質評価
5. 改善提案を生成

## /deploy-check
デプロイ前チェック:
1. 全テスト実行
2. ビルド成功確認
3. 環境変数チェック
4. マイグレーション確認
5. ロールバック計画確認
```

## ベストプラクティス

### DO's ✅

1. **具体的な例を含める** - コード例、コマンド例を記載
2. **最新の状態を保つ** - 変更があれば即座に更新
3. **構造化する** - 見出しとリストで整理
4. **参照を活用** - @記法で他ファイルを参照
5. **チーム全体で共有** - Git管理で同期

### DON'Ts ❌

1. **機密情報を含めない** - APIキー、パスワード等
2. **冗長にしない** - 必要最小限の情報に絞る
3. **古い情報を残さない** - 定期的にクリーンアップ
4. **個人的な設定を混ぜない** - local.mdを使い分ける
5. **フォーマットを崩さない** - Markdown構文を守る

## 運用のコツ

### 初期セットアップ

```bash
# プロジェクト開始時
claude
> プロジェクトの構造を分析してCLAUDE.mdの雛形を作成してください
```

### 定期メンテナンス

```bash
# 週次で実行
> CLAUDE.mdの内容を確認して、古い情報や不要な項目を削除してください
> 最近の変更を反映してCLAUDE.mdを更新してください
```

### チーム共有

```bash
# PRレビュー時
> このPRの内容をCLAUDE.mdに反映する必要があるか確認してください
```

## 活用効果の測定

| 指標 | 導入前 | 導入後 | 改善率 |
|-----|--------|--------|--------|
| コンテキスト理解時間 | 30分 | 5分 | 83% |
| 誤った実装の頻度 | 週5回 | 週1回 | 80% |
| 新メンバーの立ち上げ | 2週間 | 3日 | 78% |

## 次のステップ

- [トラブルシューティング](./07-troubleshooting.md)
- [自動化テクニック](./08-automation.md)
- [チーム開発](./09-team-development.md)