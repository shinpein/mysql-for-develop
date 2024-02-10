### docker や docker-compose はインストール済みで進めます。

# MySql ローカル開発環境の構築
## 1.ディレクトリの作成
適当なディレクトリ（ここではmysql_test）を作成し、その下にmysql と phpmyadmin を作成します。

```
mysql_test/
　├ mysql/
　└ phpmyadmin/
```

## 2.docker-compose.yml の作成
mysql_test 配下に docker-compose.yml を作成します。

※ volumes の部分でデータの永続化をしています。
※ rootパスワードやポート番号は適宜変えてください。

```docker-compose
version: '3.8'

services:
  mysql:
    image: mysql:latest
    container_name: "dev-mysql"
    environment:
      - MYSQL_ROOT_PASSWORD=root
      - TZ=Asia/Tokyo
    ports: 
      - 3306:3306
    volumes:
      - ./mysql:/var/lib/mysql

  phpmyadmin:
    image: phpmyadmin/phpmyadmin
    container_name: "dev-phpmyadmin"
    environment:
      - PMA_ARBITRARY=1
      - PMA_HOST=mysql
      - PMA_USER=root
      - PMA_PASSWORD=root
    links:
      - mysql
    ports:
      - 8080:80
    volumes:
      - "./phpmyadmin/sessions:/sessions"
```

## 3.docker-compose で起動
※初回はイメージをフェッチしてコンテナを作り上げるので多少時間がかかります。
```bash
$ docker-compose up -d
```

## 4.画面表示
https://localhost:8080 で画面を表示します。

※ポート番号は docker-compose.yml で設定したものになります。
