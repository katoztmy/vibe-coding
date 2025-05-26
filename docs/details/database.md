# データベース設計

## テーブル一覧

| テーブル名 | 説明                                                     |
| ---------- | -------------------------------------------------------- |
| users      | ユーザー情報を管理するテーブル                           |
| posts      | ムカつき投稿を管理するテーブル                           |
| reactions  | 投稿に対するリアクション（スッキリ度）を管理するテーブル |

## テーブル詳細

### users テーブル

| カラム名      | データ型     | 制約                    | 説明                       |
| ------------- | ------------ | ----------------------- | -------------------------- |
| id            | UUID         | PRIMARY KEY             | ユーザー ID                |
| email         | VARCHAR(100) | UNIQUE, NOT NULL        | メールアドレス             |
| password_hash | VARCHAR(255) | NOT NULL                | ハッシュ化されたパスワード |
| created_at    | TIMESTAMP    | NOT NULL, DEFAULT now() | 作成日時                   |
| updated_at    | TIMESTAMP    | NOT NULL, DEFAULT now() | 更新日時                   |

### posts テーブル

| カラム名          | データ型    | 制約                             | 説明                                     |
| ----------------- | ----------- | -------------------------------- | ---------------------------------------- |
| id                | UUID        | PRIMARY KEY                      | 投稿 ID                                  |
| user_id           | UUID        | NOT NULL, FOREIGN KEY (users.id) | 投稿者のユーザー ID                      |
| content           | TEXT        | NOT NULL                         | 元のムカつき内容                         |
| converted_content | TEXT        | NOT NULL                         | AI 変換後の内容                          |
| style             | VARCHAR(20) | NOT NULL                         | 変換スタイル（"daiogiri"または"senryu"） |
| created_at        | TIMESTAMP   | NOT NULL, DEFAULT now()          | 作成日時                                 |
| updated_at        | TIMESTAMP   | NOT NULL, DEFAULT now()          | 更新日時                                 |

### reactions テーブル

| カラム名   | データ型    | 制約                                     | 説明                                             |
| ---------- | ----------- | ---------------------------------------- | ------------------------------------------------ |
| id         | UUID        | PRIMARY KEY                              | リアクション ID                                  |
| post_id    | UUID        | NOT NULL, UNIQUE, FOREIGN KEY (posts.id) | 対象投稿 ID                                      |
| type       | VARCHAR(20) | NOT NULL                                 | リアクションタイプ（"positive"または"negative"） |
| created_at | TIMESTAMP   | NOT NULL, DEFAULT now()                  | 作成日時                                         |
| updated_at | TIMESTAMP   | NOT NULL, DEFAULT now()                  | 更新日時                                         |

## インデックス

### users テーブル

- `email`に`UNIQUE`インデックス

### posts テーブル

- `user_id`にインデックス
- `created_at`にインデックス（日付検索の高速化）

### reactions テーブル

- `post_id`に`UNIQUE`インデックス

## リレーションシップ

### users テーブル

- users.id → posts.user_id (1:N)

### posts テーブル

- posts.user_id → users.id (N:1)
- posts.id → reactions.post_id (1:1)

### reactions テーブル

- reactions.post_id → posts.id (1:1)

## データベース設計の説明

### users テーブル

- ユーザー認証に必要な最低限の情報のみを保持
- パスワードは平文ではなくハッシュ化して保存
- Supabase Auth を利用する場合は、このテーブルは自動生成されるため実装不要

### posts テーブル

- `content`: ユーザーが入力したムカつき内容（元の文章）
- `converted_content`: AI によって変換された内容（大喜利または川柳）
- `style`: 変換スタイルを示す文字列。"daiogiri"（大喜利形式）または"senryu"（川柳形式）

### reactions テーブル

- 1 つの投稿に対して 1 つのリアクションのみを許可（UNIQUE 制約）
- `type`: "positive"（👍）または"negative"（👎）

## 機能要件との対応

### 投稿履歴

- posts テーブルから`user_id`でフィルタリングし、`created_at`で降順ソートして取得

### スッキリ度の記録

- reactions テーブルに保存
- 統計情報取得時には集計クエリを使用

### 統計情報

- posts テーブルと reactions テーブルを結合し、期間やスタイルごとに集計
- 特定期間の投稿数やポジティブリアクション率などを算出可能

## 将来的な拡張性

### カテゴリ分類

- posts テーブルに`category`カラムを追加
- または別途 categories テーブルと post_categories テーブルを作成して多対多関係を表現

### 他ユーザーの投稿閲覧

- posts テーブルに`visibility`カラムを追加（"private"または"public"）
- 共感ボタン用に新たに likes テーブルを作成

### バズりランキング

- 外部 SNS 連携のために posts テーブルに`share_count`、`like_count`などのカラムを追加
