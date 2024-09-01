# 起動方法

## Docker スタート
   ```
   docker compose up
   ```
## Vite スタート
- 通常
   ```
   docker exec wordpress-docker-theme-web-1 bash -c "cd /var/www/html/wp-content/themes/case-series && source /root/.nvm/nvm.sh && npm run dev"
   ```
- npm エラーが出る場合
   ```
   docker exec wordpress-docker-theme-web-1 bash -c "cd /var/www/html/wp-content/themes/case-series && rm -rf node_modules && source /root/.nvm/nvm.sh && npm install && npm run dev"
   ```

# インストール方法

## リポジトリをクローンする
```
git clone https://github.com/candypopbeat/wordpress-docker-theme.git
```
## Docker 環境構築する
1. Docker Desktop を起動させる
2. docker-compose.yml を調整する（PHPのバージョンなど）
3. クローンしたリポジトリフォルダ内でコンテナ構築をする
    ```
    docker compose up
    ```
## Wordpress のインストール
### 共通のデータベース情報
- ホスト: db
- データベース名: wordpress
- ユーザー: user
- パスワード: pass

### 新規
1. 公式サイトから Wordpress をダウンロードする
1. ダウンロードした zip ファイルを一度解凍して wordpress ディレクトリができないようにまた圧縮する
1. 上記の zip ファイルを wp-data ディレクトリに置く
1. コンテナ内に入れ込む
    ```
    docker cp ./wp_data/. wordpress-docker-theme-web-1:/var/www/html/
    ```
1. 解凍する
    ```
    docker exec wordpress-docker-theme-web-1 unzip {Wordpressファイル名}.zip -d ./
    ```
1. パーミッションを調整する
    ```bash
    docker exec wordpress-docker-theme-web-1 chown -R www-data:www-data ./
    ```
1. ブラウザからアクセスしてインストールする
   http://localhost:2000

### Duplicatorから 復元する
1. コピー元から Duplicator バックアップファイルを取得する（installer.php と zip ファイル）
1. Duplicator バックアップファイルを wp-data ディレクトリに置く
1. コンテナ内に入れ込む
    ```
    docker cp ./wp_data/. wordpress-docker-theme-web-1:/var/www/html/
    ```
1. パーミッションを調整する
    ```bash
    docker exec wordpress-docker-theme-web-1 chown -R www-data:www-data ./
    ```
1. ブラウザからアクセスしてインストールする
   http://localhost:2000

### All-in-One WP Migration から復元する
1. 公式サイトから Wordpress をダウンロードする
1. ダウンロードした zip ファイルを wp-data ディレクトリに置く
1. コンテナ内に入れ込む
    ```
    docker cp ./wp_data/. wordpress-docker-theme-web-1:/var/www/html/
    ```
1. コンテナに入って余計なファイルがあったら削除する
    ```
    docker exec -it wordpress-docker-theme-web-1 bash
    ```
1. 解凍する
    ```
    docker exec wordpress-docker-theme-web-1 unzip {Wordpressファイル名}.zip -d ./
    ```
1. パーミッションを調整する
    ```bash
    docker exec wordpress-docker-theme-web-1 chown -R www-data:www-data ./
    ```
1. ブラウザからアクセスしてインストールする
   http://localhost:2000
1. 新規プラグインインストールで All-in-One WP Migration をインストールする
2. wpress ファイルをインポートする

# Docker コマンド
- 起動中コンテナ情報
   ```bash=
   docker ps
   ```
- コンテナに入る
   ```bash=
   docker exec -it {コンテナ名} bash
   ```
- ホストからコンテナ内へコピー
   ```bash=
   docker cp {対象ファイルパス} {コンテナID}:{パス}
   ```
- コンテナを一括停止
   ```bash=
   docker stop $(docker ps -q)
   ```

# Docker Compose コマンド
- コンテナ起動
   ```bash=
   docker-compose up
   ```
- 再ビルドしながらのコンテナ起動
   ```bash=
   docker compose up --build
   ```
- コンテナ削除 コンポーズファイル指定なし
   ```bash=
   docker-compose down
   ```
- キャッシュを使わないでビルド
   ```bash=
   docker-compose build --no-cache
   ```
- コンテナとそれに関連したイメージとボリューム削除
   ```bash=
   docker-compose down --rmi all --volumes --remove-orphans
   ```
- コンテナに入る
    ```
    docker exec -it wordpress-docker-theme-web-1 bash
    ```
