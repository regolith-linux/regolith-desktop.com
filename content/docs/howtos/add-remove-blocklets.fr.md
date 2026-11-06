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

{{< callout type="info" >}}
Cette section décrit Regolith 3.6 et les versions suivantes. Jusqu'à Regolith 3.5, la barre lit soit vos fichiers, soit ceux des paquets, jamais les deux : voir [Regolith 3.5 et versions antérieures](#customize-bar-3-5).
{{< /callout >}}

`i3xrocks` lit deux dossiers ensemble : les fichiers des paquets dans `/usr/share/i3xrocks/conf.d/` et vos propres fichiers dans `~/.config/regolith3/i3xrocks/conf.d`. Il trie les fichiers des deux dossiers par nom, si bien que vos fichiers prennent place parmi ceux des paquets. Il vous suffit de créer des fichiers pour ce que vous voulez changer :

-   **Ajouter un indicateur :** créez un fichier avec un nouveau nom. Par exemple, `50_hello` apparaît entre `40_cpu-usage` et `90_time`.
-   **Remplacer un indicateur :** créez un fichier portant le même nom que le fichier du paquet. Votre fichier est lu à la place de celui du paquet.
-   **Masquer un indicateur :** créez un fichier vide portant le même nom que le fichier du paquet.
-   **Modifier certains réglages d'un indicateur :** écrivez une section portant le même nom (et la même `instance`, s'il en a une) que le bloc du paquet. Votre section ne définit que les propriétés que vous y écrivez, et l'indicateur garde le reste de ses réglages. L'indicateur est affiché à la position de votre fichier : copier un fichier sous un autre numéro déplace donc l'indicateur au lieu de l'afficher deux fois.

Les propriétés globales de `01_setup` (la commande, les couleurs) s'appliquent aussi à vos fichiers. Les paquets d'indicateurs installés par la suite apparaissent dans la barre sans modifier vos fichiers.

Par exemple, pour afficher l'indicateur du processeur avant celui du trafic réseau, ne pas afficher l'indicateur de mémoire et ajouter une étiquette à l'horloge, suivez ces étapes (les fichiers présents dans `/usr/share/i3xrocks/conf.d/` dépendent des paquets d'indicateurs installés) :

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

Ces étapes effectuent les actions suivantes :

-   Les modules `i3xrocks` présents dans le dossier créé par Regolith sont listés
-   Le dossier de configuration utilisateur `~/.config/regolith3/i3xrocks/conf.d` est créé
-   `40_cpu-usage` est copié sous le nom `20_cpu-usage`, ce qui place l'indicateur du processeur devant celui du trafic réseau
-   Le fichier vide `50_memory` masque l'indicateur de mémoire
-   `95_time-label` ajoute une étiquette à l'horloge, qui garde sa commande et son intervalle
-   `pkill -HUP i3xrocks` fait relire sa configuration à la barre, sans redémarrer i3. `regolith-look refresh` fonctionne aussi

Pour revenir à la configuration par défaut, supprimez vos fichiers, ou tout le dossier, puis relancez `pkill -HUP i3xrocks`.

**Note** : Si vous avez personnalisé la barre avec Regolith 3.5 ou une version antérieure en copiant tous les indicateurs à garder, votre barre reste la même, car vos fichiers remplacent les fichiers des paquets portant le même nom. Les indicateurs que vous n'aviez pas copiés réapparaissent toutefois. Pour en masquer un à nouveau, créez un fichier vide portant son nom, par exemple `touch ~/.config/regolith3/i3xrocks/conf.d/50_memory`.

### Regolith 3.5 et versions antérieures {#customize-bar-3-5}

La première chose à faire pour personnaliser la barre est de recopier les indicateurs que vous souhaitez voir à partir de `/usr/share/i3xrocks/conf.d/` vers `~/.config/regolith3/i3xrocks/conf.d`.
Chaque fichier dans `~/.config/regolith3/i3xrocks/conf.d` peut être modifié selon vos besoins.
Pour changer l'ordre des indicateurs, changez simplement le nom des fichiers.
Une fois que vous avez fait des changements, il est nécessaire de rafraîchir la session pour que la barre se mette à jour en fonction de la nouvelle configuration.

Le même exemple que ci-dessus, sans l'étiquette de l'horloge :

```console
$ mkdir -p ~/.config/regolith3/i3xrocks/conf.d
$ cp /usr/share/i3xrocks/conf.d/01_setup ~/.config/regolith3/i3xrocks/conf.d/01_setup
$ cp /usr/share/i3xrocks/conf.d/40_cpu-usage ~/.config/regolith3/i3xrocks/conf.d/20_cpu-usage
$ cp /usr/share/i3xrocks/conf.d/30_net-traffic ~/.config/regolith3/i3xrocks/conf.d/30_net-traffic
$ cp /usr/share/i3xrocks/conf.d/90_time ~/.config/regolith3/i3xrocks/conf.d/90_time
$ regolith-look refresh
```

`01_setup` contient les réglages communs à tous les indicateurs, il faut donc le garder. Renommer `40_cpu-usage` en `20_cpu-usage` place l'indicateur du processeur devant celui du trafic réseau, et l'indicateur de mémoire n'est pas copié, il n'est donc pas affiché.

**Note** : Si le dossier `~/.config/regolith3/i3xrocks/conf.d` existe, la configuration par défaut dans `/usr/share/i3xrocks/conf.d/` est ignorée, même si le dossier est vide. Copiez tous les indicateurs que vous voulez garder, et supprimez le dossier pour revenir à la configuration par défaut. Les paquets d'indicateurs installés par la suite n'apparaissent dans la barre qu'une fois leur fichier copié lui aussi. Envoyer `SIGHUP` à `i3xrocks` arrête la barre au lieu de la recharger : utilisez `regolith-look refresh`.

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
