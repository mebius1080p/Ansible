# docker サーバー構築 ansible

## メモ
- ansible-playbook -i production site.yml --tags common などで特定のタグを付けられたタスクを実行可能
- ansible target_host_name -i production -m ping で接続確認
- ansible target_host_name -i production -m command -a '/sbin/reboot' で再起動
- role を作るには、roles ディレクトリで ansible-galaxy init xxxxxxxx

## ファイル
- production, staging というファイルを設置しておく
- ファイルの中身は下記のような感じ
	```
	[webservers]
	mmm
	ggg

	[mmm]
	mmm

	[ggg]
	ggg
	```
- site.yml を設置しておく
- ファイルの中身は下記のような感じ
	```yml
	- import_playbook: webservers.yml
	- import_playbook: mmm.yml
	- import_playbook: ggg.yml
	```

## 実行コマンド
```
ansible-playbook -i staging site.yml --ask-become-pass
ansible-playbook -i production site.yml --ask-become-pass
```

## ローカルの開発環境用オレオレ証明書作成手順
```
openssl genrsa 2048 > hoge.net.key
openssl req -new -key hoge.net.key > hoge.net.csr
JP
TOKYO
x_city
...
hoge.net

openssl x509 -in hoge.net.csr -days 36500 -req -signkey hoge.net.key > server.crt
```