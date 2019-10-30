# [AWS] ほぼ全自動Redmine構築

## 概要

qiitaに概要を載せています。

https://qiita.com/Hiro-AGU/items/86446af898334a7dbc23

## 構築手順

* AWS環境にRedmine用の環境を作成する

Cloudformationを使用し、環境を自動で作成します。  
テンプレート用のファイルは、`create_redmine_env.yml`です。

* ファイルの編集

以下ファイルの各自の環境に合わせ、編集してください。

```
roles/redmine/template/docker-compose.yml


version: '3'
services:
  redmine:
    image: redmine:4.0.5
    restart: always
    container_name: redmine_container
    environment:
      - REDMINE_DB_POSTGRES=db
      - REDMINE_DB_USERNAME=postgres
      - REDMINE_DB_PASSWORD=postgres
      - VIRTUAL_HOST=xxx.xxx.xxx.xxx  <=各自のPublicIP
```

```
roles/redmine/template/nginx.conf

server{

  server_name xxx.xxx.xxx.xxx; <= 各自のPublicIP

  location / {
    proxy_pass http://redmine_container:3000;
  }
}
```

```
hosts


[ec2Instance]
xxx.xxx.xxx.xxx   <= 各自のPublicIP
```

## Ansibleの実行

以下コマンドを使用し、Ansibleの実行を行います。

```
$ ansible-playbook -i hosts playbook.yml --private-key=~/.ssh/AWS秘密鍵.pem -u ec2-user
```

---
以上