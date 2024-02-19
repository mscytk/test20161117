https://a1026302.hatenablog.com/entry/2023/02/20/170514

Ubuntu 21.04から、サーバー版に「needrestart」というパッケージが最初からインストールされるようになったとのこと。
「needrestart」はパッケージの更新時に再起動が必要なデーモンを通知してくれる仕組みのようです。

https://github.com/liske/needrestart/blob/master/ex/needrestart.conf#L33

/etc/needrestart/needrestart.conf
## 再起動モードの変更

- 再起動が必要なサービスを表示するだけ（l）
- サービスごとに再起動が必要かどうかを通知する（i）
- 必要なサービスはすべて自動的に再起動する（a）
