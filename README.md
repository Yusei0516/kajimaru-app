## かじまる
2025年　ハッカソン秋の陣（チーム開発/期間：2か月/メンバー：4人）

Djangoベースで家事管理アプリを開発。Web3層構成で本番運用を想定したインフラ設計しました。
ローカル環境ではDocker composeにより同様の構成を再現可能です。

---

## 🚀使用技術
- **フロントエンド**：HTML, CSS, JavaScript
- **バックエンド**：Python（Django）, MySQL
- **インフラ**：AWS, Docker
- **Webサーバー**：Nginx＋Gunicorn
- **開発管理**：GitHub

---

## 👤自分の担当
**認証機能**
- 新規登録
- ログイン/ログアウト
- 家族セッションを確立
- ワンタイムパスワードによるセッション招待機能

**買い物リスト**
- 家族内で共有できる買い物リストを実装
- 買い物リスト追加/編集/削除機能

**天気レコメンド**
- 天気APIを取得し、天候に適した家事をおすすめする機能

---

## 🗂️機能概要
**認証機能**
- 管理者が新規登録を行い、一般ユーザーをワンタイムパスワードで招待
- 管理者が新規登録を行うことで、セッションを確立
- その他一般的なログイン/ログアウト機能

**買い物リスト**
- 買い物リストを家族内で共有でき、チェックボックスで購入済みと未購入を管理
- 商品一つ一つにメモや数量を登録できる

**天気レコメンド**
- ダッシュボード上に用意されたボタンを押下すると天気とおすすめの家事をモーダルで表示

---

## 📂ディレクトリ・ファイル構成
本プロジェクトのディレクトリ・ファイル構成を以下のとおり示す。
```
<pre>
.
└── プロジェクト名
    ├── src                                   # ソースディレクトリ
    │   ├── apps                              # バックエンドディレクトリ
    │   │   ├── (core)                        # 任意(共通動作ディレクトリ)
    │   │   └── (your_app)                    # 任意
    │   ├── templates                         # テンプレートディレクトリ
    │   │   └── (your_html)                   # 任意
    │   ├── static                            # 静的ファイルディレクトリ
    │   │   ├── img                           # 画像ディレクトリ
    │   │   │   └── (your_img)                # 任意
    │   │   ├── css                           # CSSディレクトリ
    │   │   │   └── (your_css)                # 任意
    │   │   └── js                            # JavaScriptディレクトリ
    │   │       └── (your_js)                 # 任意
    │   ├── config                            # プロジェクト設定ディレクトリ
    │   │   ├── __init__.py                   # パッケージ化のための空ファイル
    │   │   ├── urls.py                       # プロジェクト全体のURL設定
    │   │   ├── asgi.py                       # 非同期通信のための設定ファイル
    │   │   ├── wsgi.py                       # 同期通信のための設定ファイル
    │   │   └── settings                      # 環境設定ディレクトリ
    │   │       ├── __init__.py               # パッケージ化のための空ファイル
    │   │       ├── base.py                   # 全環境共通の設定
    │   │       ├── dev.py                    # 開発環境専用の設定（例: DEBUG=True）
    │   │       └── prod.py                   # 本番環境専用の設定（例: DEBUG=False, セキュリティ強化）
    │   └── manage.py                         # Django管理ファイル
    ├── docker                                # Dockerディレクトリ
    │   ├── Dockerfile                        # Python(Django)のDockerイメージ
    │   └── wait-for-it.sh                    # DB起動後にDjangoを開始するためのシェルスクリプト
    ├── .dockerignore                         # Dockerビルド時の無視ファイルの設定
    ├── .gitignore                            # GitHubにpushしないファイルの設定
    ├── docker-compose.yml                    # Python(django)とDBの構成ファイル
    ├── Makeflie                              # コマンド省略のための設定ファイル
    ├── .env                                  # 開発用環境設定ファイル（Gitに含めない）
    ├── .env.exapmle                          # 環境変数のテンプレート（ダミー値で共有）
    ├── README.md                             # プロジェクトの説明ファイル
    └── requirements.txt                      # 依存関係ファイル
</pre>
```

## ▶️ 実行方法（ローカル）
```bash
# 環境変数ファイルの作成
cp .env.sample .env

# 🔑SECRET_KEY生成方法
python -c "import secrets; print(secrets.token_urlsafe(50))"

# .envの中身は以下のように設定してください(サンプル):
# ======= ✅ チームで共通にする設定 =======
MYSQL_DATABASE=django_db                    # 開発環境で使うデータベース名（チームで共通・固定）
MYSQL_USER=dev_user                         # 開発用のデータベースユーザ名（チーム共通）
MYSQL_HOST=db                               # Django(web)コンテナが接続するDBコンテナ名(db)（チーム共通）
DJANGO_PORT=8000                            # Djangoのポート番号（チーム共通）
DJANGO_LANGUAGE_CODE=ja                     # 言語コード設定(チーム共通)
DJANGO_ALLOWED_HOSTS=localhost,127.0.0.1    # アプリにアクセスできるホスト・ドメイン名（チーム共通）
DJANGO_SETTINGS_MODULE=config.settings.dev  # 開発環境ファイルの参照（チーム共通）
TZ=Asia/Tokyo                               # タイムゾーン設定（チーム共通）
USERNAME=appuser                            # コンテナ内のユーザーネーム（チーム共通）
GROUPNAME=appgroup                          # コンテナ内のグループネーム（チーム共通）

# ======= 🔧 各自で変更する設定 =======
MYSQL_PASSWORD=your_own_password            # 各自がローカル環境で設定するDBユーザーパスワード(環境開発で各自設定、非公開)
MYSQL_ROOT_PASSWORD=your_root_pw            # MYSQLのrootパスワード(開発環境で各自設定、非公開)
DJANGO_SECRET_KEY=your_secret_key           # Djangoのセキュリティキー(必ず各自で生成すること)
DJANGO_DEBUG=True                           # デバッグモード設定。開発中はTrue、本番や検証環境はFalse推奨
UID=your_uid                                # 各自のuidを指定（id -uコマンドで確認）
GID=your_gid                                # 各自のgidを指定（id -gコマンドで確認）
```

# 開発環境立ち上げ
```
docker compose up --build
```
もしくは
```
make build
```

2回目以降は既にイメージがビルドされているため、以下のコマンドで起動してもよい。  
必要に応じて、 **-d** をupの後に付けて、バックグラウンドで起動してもよい。  
```
docker compose up -d
```  
もしくは  
```
make up
```

終了時のdockerコマンド
終了時は以下のコマンドで終了する。  
必要に応じて、ボリューム（db_data）を削除する場合は、 **-v** をdownのあとに付ける。  
```
docker compose down
```  
もしくは  
```
make down
```

## アクセス先
```
http://localhost:8000/
```  
もしくは  
```
http://127.0.0.1:8000/
```

## 📸起動イメージ
<img width="715" height="393" alt="image" src="https://github.com/user-attachments/assets/16afcd2c-dd21-4a02-abde-8018723252c6" />


