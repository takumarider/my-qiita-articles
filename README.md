# Qiita記事投稿フロー (Qiita CLI & GitHub連携)
このノートは、Qiita CLI を利用してローカルで作成・編集した記事を、GitHub を経由して Qiita に自動投稿・更新するための手順をまとめたものです。

## 前提
* Qiita CLI のインストールと初期設定 (`npx qiita init`) が完了している。
* Qiita CLI のログイン (`npx qiita login`) が完了している。
* GitHub リポジトリ (`takumarider/my-qiita-articles`) との連携設定 (GitHub Actions、`QIITA_TOKEN` シークレットなど) が完了している。

---

## 1. Qiita CLI 作業ディレクトリの確認

Qiita CLI の設定ファイル（`qiita.config.json` や `.github/workflows/publish.yml` など）や、記事ファイルを格納する `public` ディレクトリがある場所が、あなたの**メインの作業ディレクトリ**となります。

このノートでは、そのディレクトリを `my-qiita-articles` と仮定して説明します。

## 重要なポイント
>**`my-qiita-articles`**: ここがGitリポジトリのルートであり、Qiita CLI のコマンド (`npx qiita new`, `npx qiita preview` など) やGitコマンド (`git add`, `git commit`, `git push` など) を実行する場所です。

>my-qiita-articles/public: このディレクトリの中に、Qiitaに投稿するMarkdown形式の記事ファイル (`.md`) が保存されます。このディレクトリに直接 `cd` して作業することは通常ありません。

### 以下のコマンドで、このメインの作業ディレクトリに移動してから、以降の作業を開始してください。

```bash
cd my-qiita-articles
```

## 記事の作成と編集
### 1. 新しい記事ファイルの作成

```Bash
npx qiita new [記事のファイル名]
```
>このコマンドを実行すると、public/[記事のファイル名].md のように、public ディレクトリの中に新しいMarkdownファイルが生成されます。

### 2. 記事の編集 (エディタ作業)
>生成された Markdown ファイル (public/[記事のファイル名].md) をお好みのエディタ（VS Codeなど）で開いて、記事を執筆します。

>ファイル上部の --- で囲まれた部分はFront Matterと呼ばれ、記事のタイトルやタグ、公開設定などを YAML 形式で記述します。# 以降はコメントなので、削除しても問題ありません。
```
Markdown

title: ここに記事のタイトルを記述 # 記事のタイトル
tags:
  - "タグ1"🟢🟢" # タグは複数設定できます（ダブルクォーテーションで囲む）
  - "タグ2"🟡🟡"
private: false # true: 限定共有記事 / false: 公開記事
updated_at: "" # Qiita投稿時に自動で更新されるため、空のままでOK
id: null # Qiita投稿時に自動で生成されるため、空のままでOK
organization_url_name: null # Organizationに関連付ける場合のみ使用
slide: false # true: スライドモードON / false: スライドモードOFF
ignorePublish: false # true: この記事を`publish`コマンドで無視する / false: `publish`コマンドで処理する


# ここに記事本文の見出しを書く
ここにMarkdown記法で記事の具体的な内容を記述していきます。
画像も`![代替テキスト](画像URL)`の形式で貼り付けられます。

## コードブロックの例

\`\`\`python
def hello_qiita():
    print("Hello, Qiita CLI!")
\`\`\`
```

# ↑ここまでがMarkdownファイルの例です
## 3. ローカルでのプレビュー
記事を執筆しながら、ブラウザでリアルタイムのプレビュー表示を確認できます。

メインの作業ディレクトリ (cd my-qiita-articles) で以下のコマンドを実行します。

```Bash
npx qiita preview

または、VSコード上で確認するのであれば、[command + shift + v]
```
>コマンド実行後、ターミナルに表示されるURL（例: http://localhost:8888 や http://[::1]:8888）をブラウザで開いてください。public ディレクトリ内の記事ファイルに変更を加えるたびに、プレビューが自動的に更新されます。

>プレビューを終了するには、ターミナルで Ctrl + C を押します。

## 4. 記事の投稿・更新 (GitHubへのプッシュ)
記事の執筆が完了し、内容に問題がなければ、Gitを使ってGitHubリポジトリにプッシュします。GitHub Actionsが連携されているため、プッシュが完了すると自動的にQiitaに記事が投稿または更新されます。

以下のコマンドは、必ずメインの作業ディレクトリ (my-qiita-articles) で実行してください。

### 1,変更をステージングする
新しく作成した記事ファイルや編集したファイルなど、Gitに含めたい全ての変更をステージングエリアに追加します。

```
git add .
```
### 2,変更をコミットする
>行った変更をGitの履歴に記録します。コミットメッセージは、何の変更を行ったのかが分かりやすく具体的に記述しましょう。

```
git commit -m "feat: [記事タイトル] を新規投稿"
```

```
git commit -m "fix: [記事タイトル] の誤字を修正"
# または
git commit -m "update: [記事タイトル] に新しいセクションを追加"
```
### 3,GitHubにプッシュする
>ローカルのコミットをGitHubリポジトリにアップロードします。
```
git push
(リポジトリ設定の初回プッシュ時に -u origin main を実行済みであれば、2回目以降は git push だけでOKです。Gitが自動で追跡ブランチを認識します。)
```
プッシュ後の確認
>プッシュが完了したら、ブラウザであなたのGitHubリポジトリにアクセスし、「Actions」タブをクリックしてください。

今行ったプッシュに対応するワークフローが自動的に実行されているはずです。
ワークフローが緑色のチェックマークで完了すれば、Qiitaへの投稿・更新が成功しています。
Qiitaのあなたの記事一覧 (https://qiita.com/takumarider/items) で、記事が反映されているか確認してください。
4. その他のよく使うコマンド
全ての記事を同期する (Qiitaからローカルへ)

Qiita上で直接記事を更新した場合など、ローカルのファイルと同期を取りたい場合に使用します。
注意: --force を付けると、ローカルの未コミットの変更を上書きしてQiitaの最新に合わせます。必要な場合以外は使用を避け、使用する際はローカルの変更を失わないよう十分注意してください。
```Bash
cd my-qiita-articles
npx qiita pull --force
Qiita CLI のヘルプを表示する
```
```Bash
cd my-qiita-articles
npx qiita help
```