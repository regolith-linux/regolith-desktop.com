---
title: "バーステータスインジケーターの変更"
linkTitle: "バーステータスインジケーターの変更"
description: >
  バーにあるステータスインジケーターを変更する。
---

X11セッション（i3）とWaylandセッション（sway）では、異なるステータスバープログラムを使用します。

- **X11：** [`i3xrocks`]({{< ref path="/docs/reference/i3xrocks.md" lang="en" >}})。各ステータスインジケーターは個別の`i3xrocks-*`パッケージです。
- **Wayland：** [`i3status-rs`](https://github.com/greshake/i3status-rust)。1つのファイルで設定します。`i3xrocks-*`パッケージはWaylandでは効果がありません。下記の[Wayland (sway)](#wayland)を参照してください。

## ステータスインジケーターの追加と削除

### ステータスインジケーターの検索

```bash
apt search i3xrocks-
```

### ステータスインジケーターのインストール

```bash
sudo apt install i3xrocks-memory
regolith-look refresh
```

インジケーターを削除するには、そのパッケージを削除し（例：`sudo apt remove i3xrocks-memory`）、`regolith-look refresh`を実行します。

## i3バーのカスタマイズ {#customize-bar}

バーにある各インジケーターはファイルで管理されます。バーのステータスインジケーターの位置に対応しています。例として、ファイル名が`10_`で始まるものは、 `20_`で始まる設定ファイルのインジケーターより前に配置されることを意味します。各ファイルの中には、`i3xrocks`がスクリプトを実行するために使用する情報があり、最終的にバーに表示されるデータを返します。いくつかのステータスインジケーターは設定パラメーターが存在し、ユーザーの好みに合わせることができます。例えば、バッテリーの状態を頻繁に変更するのがよい場合、調べる間隔を更新することができます。バーをカスタマイズするために最初にするべきことは、表示したいインジケーターを`/usr/share/i3xrocks/conf.d/`から`~/.config/regolith3/i3xrocks/conf.d`にコピーすることです。 `~/.config/regolith3/i3xrocks/conf.d`内の各ファイルは好きなように変更できます。バーのステータスインジケーターを変更するには、ファイルの名前を好きな並び順に変更するだけです。

変更を加えたあとは、セッションを再読み込みすることで、新しい設定ファイルに基づいてバーが更新されるはずです。

例えば、CPUインジケーターをネットワークトラフィックインジケーターの前に表示し、メモリーインジケーターを非表示にしたいとき、以下の操作を行います（`/usr/share/i3xrocks/conf.d/`にあるファイルは、インストールしたインジケーターパッケージによって異なります）。

```console
$ ls /usr/share/i3xrocks/conf.d/
01_setup
30_net-traffic
40_cpu-usage
50_memory
90_time
$ mkdir -p ~/.config/regolith3/i3xrocks/conf.d
$ cp /usr/share/i3xrocks/conf.d/01_setup ~/.config/regolith3/i3xrocks/conf.d/01_setup
$ cp /usr/share/i3xrocks/conf.d/40_cpu-usage ~/.config/regolith3/i3xrocks/conf.d/20_cpu-usage
$ cp /usr/share/i3xrocks/conf.d/30_net-traffic ~/.config/regolith3/i3xrocks/conf.d/30_net-traffic
$ cp /usr/share/i3xrocks/conf.d/90_time ~/.config/regolith3/i3xrocks/conf.d/90_time
$ regolith-look refresh
```

上記の操作では、以下の操作が実行されます。

-   Regolithが作成したディレクトリに存在する現在の`i3xrocks`モジュールが一覧表示されます
-   ユーザー設定が存在するディレクトリに`~/.config/regolith3/i3xrocks/conf.d`が作成されます
-   4つのファイルが、アプリケーションが作成したディレクトリからユーザーが作成したディレクトリにコピーされます。`01_setup`にはすべてのインジケーターに共通する設定が含まれているので、残しておきます。`40_cpu-usage`を`20_cpu-usage`に名前変更すると、CPUインジケーターがネットワークトラフィックインジケーターの前に移動します。メモリーインジケーターはコピーされないため、表示されません
-   `regolith-look refresh`コマンドで**バーステータスインジケーター**の再読み込みが行われます

**注意**：`~/.config/regolith3/i3xrocks/conf.d`ディレクトリが存在する場合、ディレクトリが空であっても、既定である`/usr/share/i3xrocks/conf.d/`は無視されます。残したいインジケーターはすべてコピーしてください。既定に戻すには、このディレクトリを削除します。後からインストールしたインジケーターパッケージは、そのファイルもコピーするまでバーに追加されません。

## Wayland (sway) {#wayland}

swayのバーは`/etc/regolith/i3status-rust/config.toml`を使って`i3status-rs`を実行します。インジケーターを変更するには、このファイルをコピーしてコピーを編集し、Xresourceの`wm.bar.status_config`でそのコピーを指定します。

```bash
mkdir -p ~/.config/regolith3/i3status-rust
cp /etc/regolith/i3status-rust/config.toml ~/.config/regolith3/i3status-rust/config.toml
echo "wm.bar.status_config: ~/.config/regolith3/i3status-rust/config.toml" >> ~/.config/regolith3/Xresources
```

ファイル内の各`[[block]]`セクションが1つのインジケーターで、ファイル内の順番で表示されます。セクションを削除するとインジケーターが非表示になり、セクションを移動すると順番が変わります。ブロックを追加することもできます。例えばCPUインジケーターを追加するには：

```toml {filename="~/.config/regolith3/i3status-rust/config.toml"}
[[block]]
block = "cpu"
```

その後、`regolith-look refresh`を実行します。利用できるブロックとそのオプションについては、[i3status-rustのドキュメント](https://docs.rs/i3status-rs/latest/i3status_rs/blocks/index.html)を参照してください。

## さらに深く読み込む

[設定のリファレンスページ]({{< ref "/docs/Reference/configurations.md" >}})には、Regolith Linuxを使うときの設定ファイルについての詳細な項目があります。
