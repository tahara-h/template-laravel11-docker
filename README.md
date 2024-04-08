# laravel11環境

Laravel11の実行環境をDockerのコンテナ内に作成するテンプレートリポジトリです。

## 構築する内容

- nginxコンテナ
  - nginx
- appコンテナ
  - php8.2
  - composer
- dbコンテナ
  - mySQL5.7

## 親環境に必要なツール類

- Git (GitHub)
- Docker Desktop
- ターミナル (Windowsの場合、「PowerShell」, 「GitBash」等)
- VSCode (任意)
- SQLクライアント (任意 「A5:SQL Mk-2」、「DBeaver」等)

※各種必要に最初にインストールしておいてください。

## 構築手順
### 1.任意のディレクトリに今回のプロジェクトディレクトリをcloneする

```bash
#ターミナルで実行
git clone https://github.com/tahara-h/template-laravel11-docker.git
```

#### 2.Dockerコンテナを起動する
docker-compose.ymlファイルを元に構築されます。

```bash
# ターミナルで実行(docker-compose.ymlファイルがある場所で実行)
docker-compose up -d
```

上記コマンドでエラーがなければ環境構築が完了しています。

### Laravelのセットアップ
Dockerコンテナを起動したら、構築した環境内にコマンドで入ります。

```bash
# ターミナルで実行
docker exec -it app bash
```

無事は入れたら下記のような表示になります。
root@--------:/var/www# 

ディレクトリを移動しcomposer.jsonがある位置に移動します。

```bash
# WEBサーバーで入力
cd　./laravel
```

#### composer install

composer.lockを元に各種コンポーネントをインストールします。

```bash
# ■ WEBサーバーで入力
# 「composer.json」、「composer.lock」に記載されているパッケージをvendorディレクトリにインストール
#   ※ 時間がかかるので注意。
composer install
```

`composer install` 実行後に「`Exception`」が出ていると失敗しているので  
[root/vendor/](./root/vendor/)ディレクトリを削除して、再実行してみましょう。  
「`successfully`」が出ていれば成功です。

#### Laravel初期設定

```bash
# ■ WEBサーバーで入力
# storage ディレクトリに読み取り・書き込み権限を与える（bootstrap, storage内に書き込み（ログ出力時等）に「Permission denied」のエラーが発生する）
chmod -R 777 bootstrap/cache/
chmod -R 777 storage/
```

### 確認
  - <http://localhost:8000/> （デフォルト設定のURL）  
    [routes/web.php](./root/routes/web.php)のURI「`'/'`」の実行結果が画面に表示されます。
    laravelの画面が表示されれば導入完了です。

 - DBの情報
   MySQLのルートユーザーのパスワードは　root　です。
   また初回起動時にユーザー名が　db-user　でパスワードが　db-pass　のアカウントを作成して
   laravelがDBを利用する際に使用するように設定しています。

MySQLのターミナルに入れるか確認
```bash
# ターミナルで実行
docker exec -it db mysql -u db-user -p
```
Enter password:root

mysql>と表示されればOK。

