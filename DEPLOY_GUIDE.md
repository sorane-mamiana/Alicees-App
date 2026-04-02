# Alicees App - Google OAuth 公開手順

新しいアプリを追加するときの手順まとめ。

---

## 1. GitHub Pages の準備（初回のみ）

リポジトリ: https://github.com/sorane-mamiana/Alicees-App
公開URL: https://sorane-mamiana.github.io/Alicees-App/

### ファイル構成
| ファイル | URL |
|---|---|
| index.html | https://sorane-mamiana.github.io/Alicees-App/ |
| privacy.html | https://sorane-mamiana.github.io/Alicees-App/privacy.html |
| terms.html | https://sorane-mamiana.github.io/Alicees-App/terms.html |

新しいアプリを追加するときは `index.html` の Apps セクションにカードを追加するだけ。

---

## 2. Google Cloud Console の設定

プロジェクト: **Alicees App**
Console URL: https://console.cloud.google.com/?project=alicees-app（プロジェクトIDは要確認）

### 2-1. APIの有効化
1. 「APIとサービス」→「APIライブラリ」
2. 以下を有効化：
   - **Google Sheets API**
   - **Google Drive API**（Drive連携が必要な場合）

### 2-2. OAuth同意画面（ブランディング）
「Google Auth Platform」→「ブランディング」

| 項目 | 値 |
|---|---|
| アプリ名 | Alicees App |
| ホームページURL | https://sorane-mamiana.github.io/Alicees-App/ |
| プライバシーポリシー | https://sorane-mamiana.github.io/Alicees-App/privacy.html |
| 利用規約 | https://sorane-mamiana.github.io/Alicees-App/terms.html |
| 開発者メール | e.fujii262@gmail.com |

### 2-3. スコープの追加
「Google Auth Platform」→「データアクセス」

| スコープ | 用途 |
|---|---|
| `.../auth/spreadsheets` | スプレッドシートの読み書き |
| `.../auth/drive.metadata.readonly` | ドライブのファイル一覧取得 |

### 2-4. 公開ステータスを「本番」に変更
「Google Auth Platform」→「対象」→「アプリを公開」

---

## 3. ホームページの所有権確認（初回のみ）

Google Search Console: https://search.google.com/search-console

1. 「プロパティを追加」→ URLプレフィックスに `https://sorane-mamiana.github.io/Alicees-App/` を入力
2. 確認方法：**HTMLファイル**を選択
3. ダウンロードしたHTMLファイル（例: `google1234abcd.html`）をリポジトリに追加してpush
4. Search Consoleで「確認」を押す

```bash
cd /tmp/Alicees-App
cp ~/Downloads/google〇〇〇〇.html .
git add google〇〇〇〇.html
git commit -m "Add Google Search Console verification"
git push
```

---

## 4. OAuthクライアントID の作成（アプリごと）

「Google Auth Platform」→「クライアント」→「クライアントを作成」

| 項目 | 値 |
|---|---|
| アプリケーションの種類 | iOS |
| 名前 | アプリ名（例: seisan） |
| バンドルID | アプリのBundle ID（例: kry.seisan） |

作成後、クライアントIDを `project.yml` に設定：

```yaml
GID_CLIENT_ID: "〇〇〇.apps.googleusercontent.com"
REVERSED_CLIENT_ID: "com.googleusercontent.apps.〇〇〇"
```

設定後に XcodeGen を実行：

```bash
cd /Users/ponta-x-esk/精算アプリ
xcodegen generate
```

---

## 5. TestFlight への配布

1. Xcode のデバイス選択を「Any iOS Device (arm64)」に変更
2. **Product → Archive**
3. Organizer が開いたら **Distribute App → TestFlight & App Store → Upload**
4. App Store Connect（https://appstoreconnect.apple.com）でテスターのメールアドレスを登録

---

## 現在のアプリ一覧

| アプリ | Bundle ID | クライアントID |
|---|---|---|
| seisan（精算アプリ） | kry.seisan | 733159391856-4jigkf8s8qio8b7so7m8tk18kcu3v9g3.apps.googleusercontent.com |
