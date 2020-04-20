# Docker Database

`docker-compose up -d --build` するだけで一通りのデータベースが構築できるよ！やったね

## 環境構築
- とりあえずdockerは入れてください
  - Mac OS(High Sierra以降)
    - とりあえずしたのファイルダウンロードすればどうにかなる。ユーザー登録は少し面倒だけど、Dockerさえ入れば諸々の面倒が省けるので頑張って。High Sierraよりも前のMac OS使ってるやつは知らん。
      - https://hub.docker.com/editions/community/docker-ce-desktop-mac/
      - https://docs.docker.com/docker-for-mac/
  - Windows 10 Pro
    - Hyper-vを有効にしてからインストールしてください。コンテナをGUIで操作できるのでとても便利です。
      - https://hub.docker.com/editions/community/docker-ce-desktop-windows/
      - https://docs.docker.com/docker-for-windows/
      - Settings > Resources > FILE SHARING から適当なDriveを選択してチェックしてください。
      ![image](https://user-images.githubusercontent.com/43576650/75431538-5259c900-5990-11ea-808d-ce9d196b4e46.png)


  - それ以外のWindows
    - Docker toolboxを入れてください。レガシーだけど安定はしています。VMなども勝手に入れてくれます。
      - https://github.com/docker/toolbox/releases
      - https://docs.docker.com/toolbox/overview/
- docker-composeする
  ```bash
  git clone git@github.com:Tsutomu-Ikeda/docker-database.git
  cd docker-database
  docker-compose up -d --build
  ```

  たった3行だけでデータベース環境が構築できる！すごい！！！

  しっかりと起動しているか確認するために以下のコマンドで確認してみよう。StateがUpになっていれば大成功！
  ```bash
  $ docker-compose ps
     Name                   Command               State                 Ports
  -------------------------------------------------------------------------------------------
  docker_db_1      docker-entrypoint.sh mysqld      Up      0.0.0.0:3306->3306/tcp, 33060/tcp
  docker_redis_1   docker-entrypoint.sh redis ...   Up      0.0.0.0:6379->6379/tcp
  ```

  ホストコンピュータ側からMySQLにアクセスするには以下のコマンド
  ```bash
  # Macユーザーの場合のインストール手順
  # インストールは初回だけで大丈夫！
  # WindowsユーザーもMySQLクライアントだけを入れる方法があるのだけどここでは割愛します。
  brew install mysql-client
  echo 'export PATH="/usr/local/opt/mysql-client/bin:$PATH"' >> ~/.bash_profile
  source ~/.bash_profile

  mysql --host=127.0.0.1 -u user -p
  Enter password: pass
  ```
  127.0.0.1でホストを指定しないと動かないから注意してね！

  毎回 `mysql --host=127.0.0.1` と打つのが面倒な場合はエイリアスを `~/.bashrc` に登録しておこう。下のコマンドを実行するか、直接 `~/.bashrc` を編集すれば登録されるよ。
  ```bash
  cat alias mysql='mysql --host=127.0.0.1' >> ~/.bashrc
  ```
