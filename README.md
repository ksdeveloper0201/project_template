<h2 id="repository">各種リポジトリのリンク</h2>

-   [サーバーサイド (API)](サーバー側のリポジトリのリンク)
-   [モックデータ関連](モック関連のリポジトリのリンク)

---

<br>

## 使用技術

### node バージョン

-   node v19.5.0
-   npm v9.3.1

### フロント

-   [Next.js](https://nextjs.org/blog/next-13) 12.3.4
-   [React](https://ja.reactjs.org/) 18.0.26
-   [typescript](https://www.typescriptlang.org/) 4.9.4
-   [Storybook](https://storybook.js.org/)
-   [Recoil](https://recoiljs.org/)

### スタイル

-   [styled-components](https://styled-components.com/)

### ホスティング

-   Vercel

### テスト関連

-   [React Testing Library](https://testing-library.com/docs/react-testing-library/intro/)
-   [Jest](https://jestjs.io/ja/)

---

<br>

## 環境構築の手順

#### 環境変数ファイルの作成 (.env の中の値は管理者に確認)

```
$ cp .env.example .env
```

#### パッケージのインストール

```
$ npm install
```

#### ローカル環境の立ち上げ

```
$ npm run dev
```

下記のローカル環境にアクセスできれば OK<br>
http://localhost:3000/<br>

#### フロント用のモックサーバーの立ち上げ(Docker を起動した状態で)

```
$ npm run mockapi
```

起動して下記にリクエストを送りレスポンスが返ってくれば OK
http://localhost:8080/api/xxx

※ npm install 時にエラーが出る場合

-   node のバージョンが異なる可能性があるので下記を参考にバージョンを変えてください。

[node.js のバージョンを変更する](https://qiita.com/k3ntar0/items/322e668468716641aa5c)

---

<br>

## ディレクトリ構造

```
本プロジェクトはpages配下をルーティングとしてbulletproof-reactに基づいて設計されています。

project-name/
    ├─.github         # プルリクテンプレートや自動デプロイの設定(基本いじらない)
    ├─public/
    |   ├─images/      # 画像を格納
    ├─src/
    |   ├─pages/        # ルーティングファイル
    |     ├─todo
    |        ├─index.tsx  # /todoのパスで表示される画面
    |   ├─components    # ボタンやフォームなど汎用的なコンポーネントを格納する
    |     ├─page.tsx    # ルートファイル
    |   ├─const         # 汎用的な変数を格納
    |   ├─features/     # 機能ごとにロジックやコンポーネント型定義やstore等を管理するディレクトリ(基本ここで作業)
    |     ├─todo/       # todoという機能を追加する場合(以下todoという機能を追加する想定の参考例)
    |        ├─components  # todoで利用するコンポーネントを格納(TodoList, TodoPage等)
    |        ├─pages  # /pagesで読み込ませるページの実体
    |        ├─const  # todo機能で利用する汎用的な変数(stateの固定の初期値など)
    |        ├─hooks  # todo機能で利用するカスタムフック(API通信ロジックやstateを管理する責務を持つ)
    |        ├─stores # todo機能で利用するグローバルなstateを管理 (Recoil)
    |        ├─types  # todo機能で利用する型定義
    ├─hooks             # 汎用的なカスタムフック (トークン等の扱いなど)
    ├─lib               # ライブラリや設定済みのインスタンスをエクスポートするファイルを設置(axiosやreactQueryなど)
    ├─models/           # API通信時のデータ整合性を合わせる型定義ファイル
    ├─repository/       # API通信ロジック(/model配下の型定義を必ず参照する)
    | utils/            # 汎用的な関数を格納(時間の整形ロジックなどを格納)
    ├─stores            # 汎用的グローバルstateを管理 (Recoil)
    ├─.eslintrc.json    # ESlintの設定ファイル(基本いじらない)
    ├─.gitignore        # githubの差分に含まないものを格納
    ├─.prettierrc       # コード整形ツールprettierrcの設定ファイル (基本いじらない)
    ├─next.config.js    # Nextアプリの設定ファイル (基本いじらない)
    ├─package.json      # 設定ファイル (基本いじらない)
    ├─tsconfig.json     # TypeScript設定ファイル
```

---

<br>

<h2 id="component-design">コンポーネントの責務早見表</h2>

各コンポーネントに関する責務は下記の通りです。(SOLID 原則を参考に設計)

|                     | API 通信                 | グローバル state (Recoil) | style |
| ------------------- | ------------------------ | ------------------------- | ----- |
| /pages              | ○ (基本カスタムフックで) | ×                         | ×     |
| /feature/pages      | △                        | ○                         | △     |
| /feature/components | ×                        | ○                         | ○     |
| /components         | ×                        | ×                         | ○     |

---

<br>

## Git と GitHub の運用方法

### ブランチについて

devlop ブランチが開発環境で main が本番環境です。

| ブランチ名 | 役割                               | 派生元  | マージ先        |
| ---------- | ---------------------------------- | ------- | --------------- |
| master     | 公開するものを置くブランチ         |         |                 |
| develop    | 開発中のものを置くブランチ         | master  | master          |
| release    | 次にリリースするものを置くブランチ | develop | develop, master |
| feature-\* | 新機能開発中に使うブランチ         | develop | develop         |
| hotfix-\*  | 公開中のもののバグ修正用ブランチ   | master  | develop, master |

### コミットメッセージの記法

fix：バグ修正
hotfix：クリティカルなバグ修正
add：新規（ファイル）機能追加
update：機能修正（バグではない）
change：仕様変更
clean：整理（リファクタリング等）
disable：無効化（コメントアウト等）
remove：削除（ファイル）
upgrade：バージョンアップ
revert：変更取り消し

---

<br>

## プロジェクトを進める上での参考記事
