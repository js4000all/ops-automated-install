---
title: OS Automated Installation
---

# OS 自動インストール設定

このページでは実験的に、各種自動インストール用設定ファイルをホストしています。
インストール用 USB メモリから起動したインストーラーがネットワーク経由で、各設定ファイルを取得します。


## NixOS

WIP


## Debian — Preseed

Debian Installer の Automated Install 機能を使用します。

### インストール手順

1. Debian のインストール ISO を書き込んだ USB メモリを対象 PC に接続する。
2. USB メモリから起動する。
3. 起動メニューで **Advanced options** を選択する。
4. **Automated install** を選択する。
5. ネットワークインターフェースを選択し、ネットワーク接続を確立する。
6. Preseed ファイルの URL 入力画面が表示されたら、以下を指定する。

   ```text
   https://<GitHubユーザー名>.github.io/<リポジトリ名>/debian/preseed.cfg
   ```

7. インストーラーが `preseed.cfg` を取得し、設定内容に従って自動インストールを開始する。
8. インストールが完了したら、USB メモリを取り外し、インストール先の OS を起動する。

### 動作確認

上記の手順で、GitHub Pages 上の `preseed.cfg` が正常に取得され、自動構成が開始されることを確認済み。
GRUB の起動引数を手動編集する必要はなく、標準のインストールメニューから実行できる。

なお、現状は設定項目が足りないので、自動では完走しません。

### Preseed 設定ファイル

[preseed.cfg](./debian/preseed.cfg)


## Ubuntu Server — Autoinstall

** 動作確認してません **

Ubuntu Server では Subiquity の autoinstall 機能と cloud-init の NoCloud データソースを使用する。

### インストール手順

1. Ubuntu Server のインストール ISO を書き込んだ USB メモリから起動する。
2. GRUB メニューでインストール項目を選択し、`e` キーを押す。
3. `linux` で始まる行の `---` より前に、以下の起動引数を追加する。

   ```text
   autoinstall ds=nocloud-net\;s=https://<GitHubユーザー名>.github.io/<リポジトリ名>/ubuntu/
   ```

4. `Ctrl + X` または `F10` で起動する。
5. インストーラーが GitHub Pages から `user-data` と `meta-data` を取得し、自動インストールを開始する。

### 設定ファイル

- [user-data](./ubuntu/user-data)
- [meta-data](./ubuntu/meta-data)

## 注意事項

- インストール対象 PC がネットワークに接続できること。
- GitHub Pages の設定ファイルは公開されるため、パスワードや秘密鍵などの機密情報を平文で保存しないこと。
- 自動インストールではディスクが初期化される可能性があるため、対象ディスクの指定を事前に確認すること。
- 設定ファイルを変更すると、次回のインストールから変更内容が反映される。

## 参考資料

- [Debian Installation Guide](https://www.debian.org/releases/stable/amd64/install.en.html)
- [Debian Preseed Example](https://www.debian.org/releases/stable/example-preseed.txt)
- [Ubuntu Autoinstall Documentation](https://canonical-subiquity.readthedocs-hosted.com/en/latest/)
- [GitHub Pages Documentation](https://docs.github.com/en/pages)
