# Pagora Core

Document version: `README-ja-v0.1.3`

この文書は日本語向けガイドです。英語版 `README.md` を正本とします。

Pagora Core は、ローカル蔵書を高速に閲覧するためのローカルファースト漫画ライブラリサーバーです。

Pagora Core は 1 台の Linux マシン上で service として動作します。閲覧は Web ブラウザから行います。同じ PC のブラウザだけでなく、同じ LAN または VPN 上の別 PC、タブレット、スマートフォンのブラウザからもアクセスできます。

本リリースは Linux x86_64 向けの初回公開バイナリ配布です。ソースコードは同梱していません。利用者は `tar.gz` を展開し、同梱された公開用スクリプトでインストール、更新、アンインストールを行います。

## 目的

Pagora Core は次を優先します。

- 高速なページめくり
- Linux マシン上で動作するローカル蔵書サーバー
- Web ブラウザからの閲覧
- 同じ PC、LAN 内の別端末、または VPN 経由でのアクセス
- ローカルファイル中心の運用
- クラウド非依存
- オフライン利用
- Core 単体で成立する読書体験

アカウント管理、クラウド同期、書籍ストア、SNS 機能は Core の対象外です。

## 動作環境

Pagora Core のサーバー本体の初回公開版は次の環境で動作します。

```text
Linux x86_64
systemd が利用できる Linux 環境
```

主な想定環境は、Debian、Ubuntu、Linux Mint、MX Linux などの Debian 系 Linux です。

Pagora の閲覧 UI は Web ブラウザで利用します。同じ PC のブラウザだけでなく、同じ LAN または VPN 上の別端末のブラウザからもアクセスできます。

## 対応フォーマット

Pagora Core の正式対応フォーマットは次です。

```text
zip
cbz
```

PDF、rar、7z、画像フォルダ直読みは、この Core 初回公開版には含めません。

## 配布物

公開 tar.gz には、主に次の内容が含まれます。

```text
bin/pagora-server
web/
scripts/install_public.sh
scripts/update_public.sh
scripts/uninstall_public.sh
scripts/public_common.sh
config/pagora.service
build_info.json
README.md
README.ja.md
CHANGELOG.md
LICENSE
```

## インストール

公開ページから Linux x86_64 用 tar.gz を取得し、展開します。

```bash
tar -xzf pagora-core-linux-x86_64-v0.1.2-xxxxxxx.tar.gz
cd pagora-core-linux-x86_64-v0.1.2-xxxxxxx
sudo bash scripts/install_public.sh
```

既定ポートは `32117` です。

別のポートを使う場合は、初回インストール時に指定します。

```bash
sudo bash scripts/install_public.sh --port 32118
```

## インストール後の配置

インストール後、Pagora Core の実行本体は OS 上の正式配置先へコピーされます。

```text
実行本体:
  /opt/pagora

systemd service:
  /etc/systemd/system/pagora.service

runtime 設定:
  /var/lib/pagora/config/public.env

runtime data:
  /var/lib/pagora

既定の蔵書置き場:
  /srv/manga
```

インストール後、元の `tar.gz` と展開ディレクトリは削除しても Pagora Core は動作します。

## アクセス方法

インストールに成功すると、Pagora service が起動します。

インストールした PC 上で開く場合:

```text
http://127.0.0.1:32117
```

同じ LAN または VPN 上の別 PC、タブレット、スマートフォンから開く場合:

```text
http://<PagoraをインストールしたPCのIPアドレス>:32117
```

別端末からアクセスする場合は、Pagora Core をインストールした PC 側で、使用する TCP ポートへの接続を許可する必要がある場合があります。

既定ポートは TCP `32117` です。`--port` で別のポートを指定した場合は、その TCP ポートが対象です。

firewall、ルーター、VPN の設定方法は環境によって異なります。Pagora Core は、公開インターネットへ直接露出する構成ではなく、同じ LAN または VPN 経由のローカルアクセスを想定しています。

## 接続できない場合

まず、Pagora Core をインストールした PC 上で次を開けるか確認してください。

```text
http://127.0.0.1:32117
```

これが開ける場合、Pagora Core は起動しています。別端末から開けない場合は、次を確認してください。

- 別端末が同じ LAN または VPN 上にあるか
- 接続先 IP アドレスが正しいか
- Pagora Core をインストールした PC 側で、使用する TCP ポートへの接続が許可されているか
- `--port` で別のポートを指定した場合、その TCP ポートを使っているか

## 蔵書の置き場所

既定の蔵書置き場は次です。

```text
/srv/manga
```

`zip` または `cbz` ファイルを `/srv/manga` に置いたあと、Pagora の Settings > Library で `Rescan library` を実行すると、Pagora Core から読み込めます。

別の場所を使いたい場合は、Settings > Library の `Library folders` で `Add folder` を選び、フォルダを追加して `Save changes` を実行します。そのあと `Rescan library` を実行してください。

新しい蔵書ファイルを追加した場合も、一覧に反映するには Settings > Library で `Rescan library` を実行してください。

Settings > Library には `Full rebuild` もあります。通常の蔵書追加やフォルダ変更後の反映には、まず `Rescan library` を使ってください。`Full rebuild` はライブラリ全体を作り直す重い管理操作です。

更新、アンインストール、再インストールでは、次の蔵書フォルダは削除されません。

```text
/srv/manga
Settings > Library で追加した蔵書フォルダ
```

蔵書ファイルは Pagora Core の更新やアンインストールで削除されません。

## 更新

新しい tar.gz を取得し、展開したディレクトリ内で更新スクリプトを実行します。

```bash
tar -xzf pagora-core-linux-x86_64-vX.Y.Z-xxxxxxx.tar.gz
cd pagora-core-linux-x86_64-vX.Y.Z-xxxxxxx
sudo bash scripts/update_public.sh
```

更新では `/opt/pagora` のアプリ本体を置き換えます。置き換え前の本体は `/var/lib/pagora/backups/` に保存されます。

次は変更しません。

```text
/var/lib/pagora
/srv/manga
Settings > Library で追加した蔵書フォルダ
/var/lib/pagora/config/public.env
```

既存のポート設定と蔵書設定は維持されます。

## アンインストール

アンインストールは、インストール済み環境のスクリプトから行います。

```bash
sudo bash /opt/pagora/scripts/uninstall_public.sh
```

アンインストールで削除するものは次です。

```text
/opt/pagora
/etc/systemd/system/pagora.service
```

次は削除しません。

```text
/var/lib/pagora
/srv/manga
Settings > Library で追加した蔵書フォルダ
```

蔵書ファイルは削除されません。

## 再インストール

アンインストール後に再度インストールしても、残っている `/var/lib/pagora` の設定と既存の library folder 設定は尊重されます。

既存設定がある場合、Pagora Core は勝手に蔵書置き場を `/srv/manga` だけに戻しません。

## サービスの確認

状態確認:

```bash
systemctl --no-pager status pagora
```

直近ログ:

```bash
journalctl -u pagora -n 100 --no-pager
```

追跡表示:

```bash
journalctl -u pagora -f
```

## health check

公開スクリプトは次の health check を使用します。

```text
http://127.0.0.1:<port>/api/v2/health
```

期待される応答例:

```json
{
  "app": "pagora",
  "ok": true,
  "apiVersion": "v2"
}
```

## 既知の制限

- Core 初回公開版は Linux x86_64 のみ対象です。
- Windows / macOS / Linux arm64 の公開バイナリは本リリース対象外です。
- PDF、rar、7z、画像フォルダ直読みは対象外です。
- 公開インターネットへ直接露出する構成は推奨しません。
- LAN または VPN 経由のローカルアクセスを想定します。

## ライセンス

利用条件は同梱の `LICENSE` を確認してください。

## 変更履歴

変更履歴は同梱の `CHANGELOG.md` を確認してください。
