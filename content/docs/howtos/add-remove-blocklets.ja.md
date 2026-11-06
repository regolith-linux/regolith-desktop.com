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

バーにある各インジケーターはファイルで管理されます。バーのステータスインジケーターの位置に対応しています。例として、ファイル名が`10_`で始まるものは、 `20_`で始まる設定ファイルのインジケーターより前に配置されることを意味します。各ファイルの中には、`i3xrocks`がスクリプトを実行するために使用する情報があり、最終的にバーに表示されるデータを返します。いくつかのステータスインジケーターは設定パラメーターが存在し、ユーザーの好みに合わせることができます。例えば、バッテリーの状態を頻繁に変更するのがよい場合、調べる間隔を更新することができます。

{{< callout type="info" >}}
このセクションはRegolith 3.6以降について説明しています。Regolith 3.5までは、バーはユーザーのファイルかパッケージのファイルのどちらか一方だけを読み込みます。[Regolith 3.5以前](#customize-bar-3-5)を参照してください。
{{< /callout >}}

`i3xrocks`は2つのディレクトリを合わせて読み込みます。`/usr/share/i3xrocks/conf.d/`にあるパッケージのファイルと、`~/.config/regolith3/i3xrocks/conf.d`にあるユーザー自身のファイルです。両方のディレクトリのファイルを名前順に並べるので、ユーザーのファイルはパッケージのファイルの間に入ります。ファイルを作成する必要があるのは、変更したい部分だけです。

-   **インジケーターを追加する：** 新しい名前のファイルを作成します。例えば`50_hello`は`40_cpu-usage`と`90_time`の間に表示されます。
-   **インジケーターを置き換える：** パッケージのファイルと同じ名前のファイルを作成します。パッケージのファイルの代わりに、そのファイルが読み込まれます。
-   **インジケーターを非表示にする：** パッケージのファイルと同じ名前の空のファイルを作成します。
-   **インジケーターの設定を一部だけ変更する：** パッケージのブロックと同じ名前（`instance`がある場合は同じ`instance`）のセクションを書きます。そのセクションに書いたプロパティだけが変更され、インジケーターは残りのパッケージの設定を保ちます。インジケーターはユーザーのファイルの位置に表示されるため、ファイルを別の番号でコピーすると、インジケーターは2つ表示されるのではなく移動します。

`01_setup`のグローバルプロパティ（コマンドや色）はユーザーのファイルにも適用されます。後からインストールしたインジケーターパッケージは、ユーザーのファイルを変更しなくてもバーに追加されます。

例えば、CPUインジケーターをネットワークトラフィックインジケーターの前に表示し、メモリーインジケーターを非表示にし、時計にラベルを付けたいとき、以下の操作を行います（`/usr/share/i3xrocks/conf.d/`にあるファイルは、インストールしたインジケーターパッケージによって異なります）。

```console
$ ls /usr/share/i3xrocks/conf.d/
01_setup
30_net-traffic
40_cpu-usage
50_memory
90_time
$ mkdir -p ~/.config/regolith3/i3xrocks/conf.d
$ cd ~/.config/regolith3/i3xrocks/conf.d
$ cp /usr/share/i3xrocks/conf.d/40_cpu-usage 20_cpu-usage
$ touch 50_memory
$ printf '[time]\nlabel=TIME \n' > 95_time-label
$ pkill -HUP i3xrocks
```

上記の操作では、以下の操作が実行されます。

-   Regolithが作成したディレクトリに存在する現在の`i3xrocks`モジュールが一覧表示されます
-   ユーザー設定が存在するディレクトリに`~/.config/regolith3/i3xrocks/conf.d`が作成されます
-   `40_cpu-usage`を`20_cpu-usage`としてコピーすると、CPUインジケーターがネットワークトラフィックインジケーターの前に移動します
-   空の`50_memory`によって、メモリーインジケーターが非表示になります
-   `95_time-label`で時計にラベルが付きます。時計のコマンドと間隔はそのままです
-   `pkill -HUP i3xrocks`で、i3を再起動せずにバーが設定を読み込み直します。`regolith-look refresh`でも同じです

既定に戻すには、ユーザーのファイルまたはディレクトリ全体を削除し、もう一度`pkill -HUP i3xrocks`を実行します。

**注意**：Regolith 3.5以前で、残したいインジケーターをすべてコピーしてバーをカスタマイズしていた場合、ユーザーのファイルが同じ名前のパッケージのファイルを置き換えるので、バーは変わりません。ただし、コピーしなかったインジケーターは再び表示されます。もう一度非表示にするには、その名前の空のファイルを作成します。例：`touch ~/.config/regolith3/i3xrocks/conf.d/50_memory`

### Regolith 3.5以前 {#customize-bar-3-5}

バーをカスタマイズするために最初にするべきことは、表示したいインジケーターを`/usr/share/i3xrocks/conf.d/`から`~/.config/regolith3/i3xrocks/conf.d`にコピーすることです。 `~/.config/regolith3/i3xrocks/conf.d`内の各ファイルは好きなように変更できます。バーのステータスインジケーターを変更するには、ファイルの名前を好きな並び順に変更するだけです。変更を加えたあとは、セッションを再読み込みすることで、新しい設定ファイルに基づいてバーが更新されるはずです。

上記と同じ例（時計のラベルなし）：

```console
$ mkdir -p ~/.config/regolith3/i3xrocks/conf.d
$ cp /usr/share/i3xrocks/conf.d/01_setup ~/.config/regolith3/i3xrocks/conf.d/01_setup
$ cp /usr/share/i3xrocks/conf.d/40_cpu-usage ~/.config/regolith3/i3xrocks/conf.d/20_cpu-usage
$ cp /usr/share/i3xrocks/conf.d/30_net-traffic ~/.config/regolith3/i3xrocks/conf.d/30_net-traffic
$ cp /usr/share/i3xrocks/conf.d/90_time ~/.config/regolith3/i3xrocks/conf.d/90_time
$ regolith-look refresh
```

`01_setup`にはすべてのインジケーターに共通する設定が含まれているので、残しておきます。`40_cpu-usage`を`20_cpu-usage`に名前変更すると、CPUインジケーターがネットワークトラフィックインジケーターの前に移動します。メモリーインジケーターはコピーされないため、表示されません。

**注意**：`~/.config/regolith3/i3xrocks/conf.d`ディレクトリが存在する場合、ディレクトリが空であっても、既定である`/usr/share/i3xrocks/conf.d/`は無視されます。残したいインジケーターはすべてコピーしてください。既定に戻すには、このディレクトリを削除します。後からインストールしたインジケーターパッケージは、そのファイルもコピーするまでバーに追加されません。`i3xrocks`に`SIGHUP`を送ると、バーは再読み込みされずに終了するので、`regolith-look refresh`を使ってください。

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
