---
title: "Modifier les indicateurs dans la barre"
linkTitle: "Modifier les indicateurs dans la barre"
description: >
  Pour faire quelques changements dans les indicateurs de statut dans la barre
---

La session X11 (i3) et la session Wayland (sway) utilisent des programmes différents pour la barre d'état :

- **X11 :** [`i3xrocks`]({{< ref path="/docs/reference/i3xrocks.md" lang="en" >}}). Chaque indicateur de statut est un paquet `i3xrocks-*` distinct.
- **Wayland :** [`i3status-rs`](https://github.com/greshake/i3status-rust), configuré par un seul fichier. Les paquets `i3xrocks-*` n'y ont aucun effet ; voir [Wayland (sway)](#wayland) plus bas.

## Ajouter ou retirer des indicateurs de statut

### Voir les indicateurs de statut disponibles

```bash
apt search i3xrocks-
```

### Installer un nouvel indicateur de statut

```bash
sudo apt install i3xrocks-memory
regolith-look refresh
```

Pour retirer un indicateur, désinstallez son paquet (par exemple `sudo apt remove i3xrocks-memory`) et lancez `regolith-look refresh`.

## Personnaliser la barre i3 {#customize-bar}

Chaque indicateur de statut dans la barre est géré par un fichier.
Le nom du fichier correspond à la position de l'indicateur dans la barre. Par exemple, un fichier avec le nom commençant par `10_` signifie que l'indicateur de statut correspondant sera placer avant un indicateur qui a un fichier de configuration commençant par `20_`.

Dans chaque fichier, on peut trouver des informations utilisées par `i3xrocks` pour executer un script et récupérer les données qui sont affichées dans la barre.
Certains des indicateurs ont des paramètres qui peuvent être ajustés à votre convenance.
Par exemple, si vous préférez que le statut de la batterie change plus vouent, vous pouvez modifier l'interval de polling.

La première chose à faire pour personnaliser la barre est de recopier les indicateurs que vous souhaitez voir à partir de `/usr/share/i3xrocks/conf.d/` vers `~/.config/regolith3/i3xrocks/conf.d`.
Chaque fichier dans `~/.config/regolith3/i3xrocks/conf.d`. peut être modifier selon vos besoins.
Pour changer l'ordre des indicateurs, changez simplement le nom des fichiers.

Une fois que vous avez fait des changements, il est nécessaire de rafraîchir la session pour que la barre se mette à jour en fonction de la nouvelle configuration.
Par exemple, pour afficher l'indicateur du processeur avant celui du trafic réseau et ne pas afficher l'indicateur de mémoire, suivez ces étapes (les fichiers présents dans `/usr/share/i3xrocks/conf.d/` dépendent des paquets d'indicateurs installés) :

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

Ces étapes effectuent les actions suivantes :

-   Les modules `i3xrocks` présents dans le dossier créé par Regolith sont listés
-   Le dossier de configuration utilisateur `~/.config/regolith3/i3xrocks/conf.d` est créé
-   Quatre fichiers sont copiés du dossier de l'application vers le dossier utilisateur. `01_setup` contient les réglages communs à tous les indicateurs, il faut donc le garder. Renommer `40_cpu-usage` en `20_cpu-usage` place l'indicateur du processeur devant celui du trafic réseau, et l'indicateur de mémoire n'est pas copié, il n'est donc pas affiché
-   La commande `regolith-look refresh` rafraîchit les **indicateurs de statut de la barre**

**Note** : Si le dossier `~/.config/regolith3/i3xrocks/conf.d` existe, la configuration par défaut dans `/usr/share/i3xrocks/conf.d/` est ignorée, même si le dossier est vide. Copiez tous les indicateurs que vous voulez garder, et supprimez le dossier pour revenir à la configuration par défaut. Les paquets d'indicateurs installés par la suite n'apparaissent dans la barre qu'une fois leur fichier copié lui aussi.

## Wayland (sway) {#wayland}

La barre de sway lance `i3status-rs` avec `/etc/regolith/i3status-rust/config.toml`. Pour changer les indicateurs, copiez ce fichier, modifiez la copie et indiquez son chemin dans l'Xresource `wm.bar.status_config` :

```bash
mkdir -p ~/.config/regolith3/i3status-rust
cp /etc/regolith/i3status-rust/config.toml ~/.config/regolith3/i3status-rust/config.toml
echo "wm.bar.status_config: ~/.config/regolith3/i3status-rust/config.toml" >> ~/.config/regolith3/Xresources
```

Chaque section `[[block]]` du fichier est un indicateur, affiché dans l'ordre du fichier. Supprimez une section pour masquer un indicateur, déplacez-la pour changer l'ordre, ou ajoutez un bloc, par exemple l'indicateur du processeur :

```toml {filename="~/.config/regolith3/i3status-rust/config.toml"}
[[block]]
block = "cpu"
```

Lancez ensuite `regolith-look refresh`. Les blocs disponibles et leurs options sont décrits dans la [documentation d'i3status-rust](https://docs.rs/i3status-rs/latest/i3status_rs/blocks/index.html).

## Informations complémentaires

La [page de référence des configuration]({{< ref "/docs/Reference/configurations.md" >}}) donne plus de détails à propres des fichiers de configuration utilisés par Regolith Linux.
