> **Réservé Chiverse, toute copie est interdite.**
> **Reserved to Chiverse, all copying is prohibited.**

## Liens Chiverse / Chiverse links

| Page | Français | English |
|---|---|---|
| Site | [chiverse.eu](https://www.chiverse.eu/) | [chiverse.eu](https://www.chiverse.eu/) |
| Documentation | [Ouvrir](https://www.chiverse.eu/docs) | [Open](https://www.chiverse.eu/docs#en-requirements) |
| Installation | [Installer](https://www.chiverse.eu/docs#fr-installation) | [Install](https://www.chiverse.eu/docs#en-install) |
| Téléphones et inventaires / Phones and inventories | [Compatibilité](https://www.chiverse.eu/docs#fr-compat) | [Compatibility](https://www.chiverse.eu/docs#en-compat) |
| Configuration | [Réglages](https://www.chiverse.eu/docs#fr-config) | [Settings](https://www.chiverse.eu/docs#en-config) |
| Commandes / Commands | [Commandes](https://www.chiverse.eu/docs#fr-commandes) | [Commands](https://www.chiverse.eu/docs#en-commands) |
| Aide / Troubleshooting | [Solutions](https://www.chiverse.eu/docs#fr-souci) | [Solutions](https://www.chiverse.eu/docs#en-trouble) |
| Chidex | [Découvrir les chi](https://www.chiverse.eu/chidex) | [Discover the chi](https://www.chiverse.eu/chidex) |
| Serveurs / Servers | [Serveurs](https://www.chiverse.eu/serveurs) | [Servers](https://www.chiverse.eu/serveurs) |
| Nouveautés / News | [Nouveautés](https://www.chiverse.eu/nouveautes) | [News](https://www.chiverse.eu/nouveautes) |
| Compte / Account | [Mon compte](https://www.chiverse.eu/compte) | [My account](https://www.chiverse.eu/compte) |
| Panel | [Espace serveur](https://www.chiverse.eu/panel/) | [Server area](https://www.chiverse.eu/panel/) |

Sur le site, le bouton de langue affiche la documentation en français ou en anglais. / On the website, use the language button to switch the documentation.

[Français](#francais) | [English](#english)

<a id="francais"></a>

# Chiverse — installation sur un serveur FiveM

Chiverse est un jeu de capture, d'élevage et de combats de créatures, les chi. Cette ressource est la porte d'entrée du jeu sur votre serveur. Les règles et les données vivent sur le serveur central Chiverse : la progression d'un joueur le suit sur tous les serveurs partenaires.

## Ce qu'il vous faut

| Élément | Obligatoire | Remarque |
|---|---|---|
| Un serveur FiveM récent (Lua 5.4) | oui | |
| Un accès à Internet depuis le serveur | oui | Vers `https://api.chiverse.eu`. |
| Un compte administrateur en jeu | oui | Pour lier le serveur. |
| ESX, QBCore ou QBox | non | Sans framework, le jeu s'ouvre avec la commande `/chi`. |
| Un inventaire | non | ox_inventory, qs-inventory, codem-inventory ou celui de votre framework, pour l'objet « Console Chiverse ». |
| Un téléphone | non | Application complète sur lb-phone et lb-tablet, icône sur Quasar et 17Movement. |
| ox_target ou qb-target | non | Pour viser un chi « à l'œil », en plus de la touche E. |
| ox_lib | non | Pour les notifications ; sinon celles du jeu sont utilisées. |

## Installation

1. Copiez le dossier `chiverse` dans vos ressources, **sans le renommer**.
2. Dans `config.lua`, mettez votre licence FiveM dans `Config.Admins`, par exemple `{ 'license:abcdef0123...' }`. Les groupes de `Config.TestGroups` et l'ACE `chiverse.test` comptent aussi comme administrateurs.
3. Ajoutez `ensure chiverse` à votre `server.cfg`, après votre framework, votre inventaire et votre téléphone.
4. Redémarrez le serveur et connectez-vous avec votre compte administrateur.
5. Tapez `/chi`, choisissez le nom du serveur et sa langue, puis validez. Votre serveur reçoit un code public du type `CHV-AB23CD`.

Une fois le serveur lié, vos joueurs ont accès au jeu. Pour faire vos essais avant, mettez `Config.TestMode = true` dans `config.lua`, puis tapez `restart chiverse` : seuls les administrateurs verront Chiverse.

Tant que le serveur n'est pas lié, les joueurs voient un message indiquant que l'administrateur doit le lier. Laissez `ApiKey` vide dans `config_server.lua` : la clé est créée à la liaison et reste sur votre serveur.

Pour afficher cette clé, tapez `chi_key` dans la **console du serveur**. Elle sert à ouvrir le panel. Ne la partagez jamais.

## Après la liaison

Votre serveur est d'abord **en attente de validation** par Chiverse. Vos nouveaux joueurs peuvent déjà capturer, élever et jouer. Les échanges, le classement, les tournois, les chi géants et les arènes officielles ouvrent après la validation.

Dans l'application, **Plus > Mon serveur** montre vos statistiques, les classements, les objets disponibles et les tickets de support. Vous pouvez y couper Chiverse pour vos joueurs.

## Comptes des joueurs

À partir de 3 chi, le joueur est invité à créer son compte :

1. Dans **Plus > Mon compte**, il obtient un code valable 10 minutes.
2. Il l'entre sur **chiverse.eu/compte** et choisit un identifiant et un mot de passe.
3. Sur un autre serveur, il obtient un nouveau code et autorise ce serveur depuis le site.

Le mot de passe ne se tape jamais dans le jeu. Un joueur sans compte ne peut pas emmener sa progression sur un autre serveur.

## Ramassage

Certains chi peuvent trouver un objet après une victoire. Vérifiez que les objets de `Config.Pickup` existent dans votre inventaire, puis passez `enabled` à `true`. Ce sont des objets de votre serveur uniquement.

## L'objet « Console Chiverse »

Le joueur utilise l'objet depuis son inventaire et la console s'ouvre, comme avec `/chi`. Les fichiers prêts à copier sont dans le dossier `install/`.

**ox_inventory** — collez ce bloc dans `ox_inventory/data/items.lua` (il est aussi dans `install/ox_inventory_item.lua`) :

```lua
['chi_console'] = {
    label = 'Console Chiverse',
    description = 'Une console portable pour capturer, élever et faire combattre vos chi.',
    weight = 300,
    stack = false,
    close = true,
    client = {
        image = 'chi_console.png',
        export = 'chiverse.useConsole',
    },
},
```

Copiez ensuite `install/chi_console.png` dans `ox_inventory/web/images/`, puis redémarrez ox_inventory (ou le serveur).

**ESX sans ox_inventory** — table `items` :

```sql
INSERT INTO items (name, label, weight) VALUES ('chi_console', 'Console Chiverse', 1);
```

**QBCore** — dans `qb-core/shared/items.lua` :

```lua
chi_console = { name = 'chi_console', label = 'Console Chiverse', weight = 300, type = 'item', image = 'chi_console.png', unique = true, useable = true, shouldClose = true, description = 'Une console portable Chiverse.' },
```

**Autres inventaires** — ajoutez un objet nommé `chi_console` à leur liste d'objets et copiez `install/chi_console.png` dans leur dossier d'images.

Réglages liés, dans `config.lua` :

- `Config.Item` : nom de l'objet, si vous voulez l'appeler autrement.
- `Config.RequireItem = true` : la commande `/chi` et la touche demandent alors de posséder l'objet. L'application du téléphone reste accessible sans lui.

Pour donner l'objet : vendez-le dans une boutique de votre serveur, ou donnez-le avec la commande de votre inventaire (par exemple `/giveitem <id> chi_console 1`).

## Comment les joueurs y accèdent

| Accès | Détail |
|---|---|
| Commande | `/chi` ouvre la console. Une touche peut être associée dans les réglages du jeu. |
| Objet | « Console Chiverse », voir plus haut. |
| Téléphone ou tablette | Application lb-phone et lb-tablet (`Config.Phone`, `Config.Tablet`), icône sur Quasar et 17Movement. |
| Dans le monde | Touche E près d'un chi ou d'un lieu, ou visée « à l'œil » (`Config.Target`). |

### Discret par défaut

Un joueur qui n'a jamais ouvert Chiverse ne voit rien : ni repères sur la carte, ni chi, ni notifications. Le jeu s'active pour lui à sa première ouverture (commande, objet, téléphone ou tablette). Ensuite :

- le bouton lune de l'application, ou la commande `/chiveille`, masque tout et le réaffiche ;
- **Plus > Affichage en jeu** règle chaque élément un par un : zones, arènes, sources, refuges, ranchs, chi sur le radar, lunettes, chi dans le monde, compagnon, compagnons des autres, notifications, sons.

### Les lunettes Chiverse

En activant le jeu, le joueur met ses lunettes : un petit geste de la main, des lunettes sur le visage et un filtre léger à l'écran. C'est à travers elles qu'il voit les chi, les repères et les compagnons des autres dresseurs. En veille, il les retire et ses lunettes d'avant reviennent.

Réglages dans `Config.Glasses` :

- `male` / `female` : le modèle de lunettes posé sur les personnages « freemode ». **Choisissez-les dans votre menu de vêtements** (accessoire « lunettes ») : les numéros par défaut sont un exemple. `drawable = -1` garde les lunettes du personnage.
- `filter` : le filtre de l'écran (un modificateur « timecycle » du jeu), `''` pour aucun ; `strength` règle sa force.
- `anim`, `sound` : le geste et le petit son.
- `enabled = false` retire les lunettes pour tout le monde (le jeu marche pareil). Chaque joueur peut aussi les couper dans **Plus > Affichage en jeu**.

Ces choix sont gardés sur l'ordinateur du joueur. Pour tout afficher dès la connexion, à tout le monde, mettez `Config.StartAsleep = false`.

## Ce que le joueur trouve dans le jeu

- **Chi sauvages** : ils apparaissent dans les zones, avec leur sprite animé, et se promènent. Les joueurs partent les chasser à pied.
- **Lieux** : sources (objets), Refuges (soins contre de l'or, PC et Link), donjons (un gardien, un badge, une seule victoire) et arènes de joueurs (le vainqueur devient champion et son équipe défend l'arène).
- **PC et Link** : changer d'équipe ou relâcher un chi se fait au PC d'un Refuge ; les échanges se font à son Link.
- **Sceaux** : les plus puissants se débloquent avec le niveau du dresseur. Le Sceau parfait n'est jamais vendu.
- **Badges** : **Plus > Badges** montre les badges gagnés et le prochain défi. Un badge peut ouvrir une région ou une zone.
- **Dresseurs** : ils apparaissent dans les zones et rapportent de l'or. Certains dresseurs fixes ne se battent qu'une fois par joueur.
- **Œufs** : ils éclosent en marchant.
- **Compagnon** : le premier chi de l'équipe marche à côté de son dresseur et gagne un peu d'expérience. Les dresseurs qui portent leurs lunettes voient aussi les compagnons des autres dresseurs proches (`Config.Companion.others`). C'est le serveur qui lit le compagnon auprès du central : un joueur ne peut pas en afficher un autre.
- **Ranch des chi** : un enclos sur la carte. En s'approchant, on voit se promener les chi que les dresseurs ont mis en élevage : les siens d'abord, puis ceux des joueurs de votre serveur, puis ceux d'autres serveurs. L'élevage se gère sur place (`Config.Pen`).
- **Écran « Monde »** de l'application :
  - le ciel des chi (jour, nuit, pluie, orage, vent), qui change les chi les plus fréquents ;
  - les quêtes du jour et de la semaine ;
  - le chi errant du jour, un chi rare qui change de zone toutes les heures ;
  - le chi géant : un raid commun à tous les serveurs. Son niveau est tiré au sort et il se régénère, si bien qu'un joueur seul ne le bat pas. Pendant le combat, chacun voit les chi des autres dresseurs qui l'affrontent au même moment, de n'importe quel serveur, et la même barre de PV ; plus ils sont nombreux en même temps, plus leurs coups portent (`Config.Giant`) ;
  - le tournoi du week-end et la saison de classement du mois.

- **Combat** : le 1 contre 1 classé (recherche d'un adversaire sur tous les serveurs), le défi entre amis (amical, il ne compte pas pour le classement) et le **2 contre 2 classé** : deux amis forment un duo, cherchent un autre duo, et chacun engage ses trois premiers chi, choisit son attaque et sa cible. Le 2 contre 2 a son propre classement.

Tout cela est décidé par le central et vaut pour tous les serveurs. Le ciel des chi est propre au jeu : il ne change pas la météo de votre serveur.

## Ce que vous réglez, et ce que vous ne réglez pas

Vous vous connectez au panel sur `https://www.chiverse.eu/panel/` avec la clé de votre serveur (commande `chi_key` dans la console).

| Vous pouvez | Détail |
|---|---|
| Masquer du contenu officiel | Page **Contenu officiel** : décochez une zone ou un lieu, il disparaît chez vous seulement. |
| Créer vos zones | Emplacement, rayon, nombre de chi. Choisissez un **modèle** : la zone officielle dont la vôtre reprend les chi et les niveaux. Sans modèle, c'est la zone officielle la plus proche. |
| Poser des sources, des refuges et des ranchs | 20 lieux au plus. Un ranch montre les chi en élevage, et sa taille se règle dans ses réglages. |
| Choisir le mode des zones | **Automatique** : zones officielles plus les vôtres. **Personnalisées** : uniquement les vôtres. |
| Vous retirer d'un événement | Page **Événements**. |

En jeu, `/chipos` (administrateurs) affiche vos coordonnées pour créer une zone ou un lieu.

Pour que le jeu reste équitable entre les serveurs, vous ne choisissez pas les chi ni les niveaux d'une zone, vous ne créez ni donjon ni arène, et vous ne réglez ni les quêtes, ni le ciel, ni le chi géant, ni le tournoi. La distance de marche est mesurée par votre serveur mais plafonnée par le central, et les sources sont limitées par heure et par joueur.

Les zones officielles suivent le décor et montent en niveau du sud au nord. Il n'y a pas de niveau imposé : chaque chi combat avec son vrai niveau.

## Boutique

Les packs se vendent sur votre boutique Tebex. Votre stock de packs est prépayé auprès de Chiverse : chaque livraison le décompte, et une livraison sans stock est refusée.

Commande à mettre dans le paquet Tebex (exécution « quand le joueur est en ligne ») :

```
chi_grant {sid} pack_dresseur {transaction}
```

- 1er argument : l'identifiant du joueur en jeu, ou sa licence (`license:xxxx`) ;
- 2e argument : l'identifiant du pack (visible sur le panel) ;
- 3e argument : l'identifiant de transaction, pour qu'un même achat ne soit jamais livré deux fois.

La commande ne s'exécute que depuis la console du serveur. Les objets et les chi achetés sont liés au compte du joueur : ils ne s'échangent pas.

Votre boutique doit respecter les règles de la plateforme FiveM : pas de monnaie virtuelle vendue contre de l'argent réel, pas de contenu aléatoire payant.

## En cas de souci

| Ce que vous voyez | Ce qu'il faut regarder |
|---|---|
| Les joueurs voient « l'administrateur doit lier ce serveur » | Connectez-vous en administrateur et tapez `/chi`. |
| `/chi` n'ouvre pas le menu de liaison | Votre licence n'est pas dans `Config.Admins` et vous n'avez pas de groupe administrateur. |
| Un joueur doit autoriser ce serveur | Il a déjà un compte Chiverse : **Plus > Mon compte**, puis chiverse.eu/compte. |
| `le serveur central ne répond pas (401)` | La clé est fausse, remplacée, ou le serveur a été bloqué. Contactez le support. |
| `le serveur central ne répond pas (0)` | L'adresse `ApiUrl` est fausse, ou le central est injoignable depuis votre machine. |
| `/chi` répond « pas disponible » | Chiverse est coupé pour les joueurs dans **Mon serveur**, ou `Config.TestMode` est à `true`. |
| Un joueur ne voit ni repères ni chi | Il n'a jamais ouvert le jeu, ou il est en veille (`/chiveille`). |
| Les chi apparaissent la tête en bas | `Config.WorldSprites.flipV = true`. |
| Les chi se voient à travers les murs | `Config.WorldSprites.mode` doit valoir `'world'`. |
| Baisse de performance près des zones | Baissez `Config.WorldSprites.max`, ou passez `enabled` à `false` (orbes lumineux). |
| L'objet n'ouvre rien | Vérifiez le nom `chi_console` et la ligne `export = 'chiverse.useConsole'`. |
| Les lunettes ne vont pas avec vos tenues | Changez `Config.Glasses.male` / `female` (numéro choisi dans votre menu de vêtements), ou mettez `drawable = -1`. |
| Le filtre de l'écran gêne | `Config.Glasses.filter = ''`, ou baissez `strength`. |
| On ne voit pas les compagnons des autres | Les deux joueurs doivent avoir activé le jeu, avec « Compagnon » et « Compagnons des autres » allumés. |

## Mise à jour

Les nouveaux chi, lieux, quêtes, événements et réglages arrivent par le central : vous n'avez rien à faire. Seule une nouvelle version de la ressource demande de remplacer le dossier ; gardez alors votre `config.lua` et ne renommez pas le dossier : la liaison est conservée.

## Fichiers

| Fichier | Rôle |
|---|---|
| `config.lua` | Réglages visibles : langue, mode test, accès, carte, ranchs, compagnon, lunettes, chi géant, animation. |
| `config_server.lua` | Adresse du central. La clé reste vide. |
| `install/` | Objet pour ox_inventory et son image. |
| `locales/` | Textes en jeu (invites, noms des points sur la carte). |
| `ui/` | Interface du jeu, commune au téléphone, à la tablette et à la console. |

## Téléphones et inventaires

Chiverse ne dépend d'aucun téléphone ni inventaire : `/chi`, la touche choisie et l'objet ouvrent toujours le jeu.

| Téléphone | Intégration |
|---|---|
| lb-phone, lb-tablet | Application complète dans le téléphone |
| qs-smartphone, qs-smartphone-pro, 17mov_Phone | Icône qui ouvre la console Chiverse |
| NPWD, GKS Phone et les autres | Pas d'icône : `/chi`, la touche ou l'objet |

| Inventaire | Objet « Console Chiverse » |
|---|---|
| ox_inventory | Pris en charge directement |
| qs-inventory, codem-inventory | Pris en charge directement |
| Inventaire ESX, QBCore ou QBox | Pris en charge par le framework |
| Autres inventaires | Fonctionnent s'ils respectent les objets utilisables de votre framework |

Ajoutez l'objet `chi_console` à la liste d'objets de votre inventaire, avec l'image `install/chi_console.png`. Pour couper les icônes Quasar et 17Movement : `Config.PhoneBridges = false`.

**Réservé Chiverse, toute copie est interdite.**

---

<a id="english"></a>

# Chiverse — install on a FiveM server

Chiverse is a game about catching, raising and battling creatures called chi. This resource is the game's entry point on your server. The rules and data live on the Chiverse central server, so a player's progress follows them across every partner server.

## What you need

| Item | Required | Note |
|---|---|---|
| A recent FiveM server (Lua 5.4) | yes | |
| Internet access from the server | yes | To `https://api.chiverse.eu`. |
| An administrator account in game | yes | To link the server. |
| ESX, QBCore or QBox | no | Without a framework, the game opens with the `/chi` command. |
| An inventory | no | ox_inventory, qs-inventory, codem-inventory or your framework's, for the "Chiverse Console" item. |
| A phone | no | Full app on lb-phone and lb-tablet, icon on Quasar and 17Movement. |
| ox_target or qb-target | no | To aim at a chi with eye targeting, in addition to the E key. |
| ox_lib | no | For notifications; otherwise the game's own are used. |

## Installation

1. Copy the `chiverse` folder into your resources, **without renaming it**.
2. In `config.lua`, put your FiveM license in `Config.Admins`, for example `{ 'license:abcdef0123...' }`. Groups in `Config.TestGroups` and the `chiverse.test` ACE also count as administrators.
3. Add `ensure chiverse` to your `server.cfg`, after your framework, inventory and phone.
4. Restart the server and connect with your administrator account.
5. Type `/chi`, choose the server name and language, then confirm. Your server receives a public code such as `CHV-AB23CD`.

Until the server is linked, players see a message saying the administrator must link it. Leave `ApiKey` empty in `config_server.lua`: the key is created when you link and stays on your server.

To display this key, type `chi_key` in the **server console**. It is used to open the panel. Never share it.

Once linked, your players can access the game. To test first, set `Config.TestMode = true` in `config.lua`, then type `restart chiverse`: only administrators will see Chiverse.

## After linking

Your server is first **awaiting approval** by Chiverse. Your new players can already catch, raise and play. Trades, leaderboards, tournaments, giant chi and official arenas open after approval.

In the app, **More > My server** shows your statistics, rankings, available items and support tickets. You can also turn Chiverse off for your players there.

## Player accounts

From 3 chi onwards, players are invited to create an account:

1. In **More > My account**, they get a code valid for 10 minutes.
2. They enter it on **chiverse.eu/compte** and choose a username and password.
3. On another server, they get a new code and authorize that server from the website.

The password is never typed in game. A player without an account cannot take their progress to another server.

## Pickup

Some chi can find an item after a win. Check that the items in `Config.Pickup` exist in your inventory, then set `enabled` to `true`. These are items from your server only.

## The "Chiverse Console" item

Players use the item from their inventory and the console opens, just like with `/chi`. Ready-to-copy files are in the `install/` folder.

**ox_inventory** — paste this block into `ox_inventory/data/items.lua`:

```lua
['chi_console'] = {
    label = 'Chiverse Console',
    description = 'A portable console to catch, raise and battle your chi.',
    weight = 300,
    stack = false,
    close = true,
    client = {
        image = 'chi_console.png',
        export = 'chiverse.useConsole',
    },
},
```

Then copy `install/chi_console.png` to `ox_inventory/web/images/` and restart ox_inventory (or the server).

**ESX without ox_inventory** — `items` table:

```sql
INSERT INTO items (name, label, weight) VALUES ('chi_console', 'Chiverse Console', 1);
```

**QBCore** — in `qb-core/shared/items.lua`:

```lua
chi_console = { name = 'chi_console', label = 'Chiverse Console', weight = 300, type = 'item', image = 'chi_console.png', unique = true, useable = true, shouldClose = true, description = 'A portable Chiverse console.' },
```

**Other inventories** — add an item named `chi_console` to their item list and copy `install/chi_console.png` to their images folder.

Related settings in `config.lua`:

- `Config.Item`: item name, if you want to call it something else.
- `Config.RequireItem = true`: the `/chi` command and the key then require owning the item. The phone app stays available without it.

To hand out the item, sell it in one of your server's shops or give it with your inventory's command (for example `/giveitem <id> chi_console 1`).

## How players get in

| Access | Details |
|---|---|
| Command | `/chi` opens the console. A key can be bound in the game settings. |
| Item | "Chiverse Console", see above. |
| Phone or tablet | lb-phone and lb-tablet app (`Config.Phone`, `Config.Tablet`), icon on Quasar and 17Movement. |
| In the world | E key near a chi or place, or eye targeting (`Config.Target`). |

### Quiet by default

A player who has never opened Chiverse sees nothing: no map markers, no chi, no notifications. The game activates for them the first time they open it (command, item, phone or tablet). After that:

- the moon button in the app, or the `/chiveille` command, hides everything and shows it again;
- **More > In-game display** controls each element individually: zones, arenas, springs, sanctuaries, ranches, chi on the radar, glasses, chi in the world, companion, other trainers' companions, notifications, sounds.

### Chiverse glasses

When the game is activated, the player puts on their glasses: a small hand gesture, glasses on the face and a light screen filter. Through them they see chi, markers and other trainers' companions. In sleep mode, they take them off and their previous glasses come back.

Settings in `Config.Glasses`:

- `male` / `female`: the glasses model placed on freemode characters. **Choose them in your clothing menu** (glasses accessory): the default numbers are only an example. `drawable = -1` keeps the character's own glasses.
- `filter`: the screen filter (a game "timecycle" modifier), `''` for none; `strength` sets how strong it is.
- `anim`, `sound`: the gesture and the small sound.
- `enabled = false` removes the glasses for everyone (the game works the same). Each player can also turn them off in **More > In-game display**.

These choices are stored on the player's computer. To show everything from login for everyone, set `Config.StartAsleep = false`.

## What players find in the game

- **Wild chi**: they appear in zones with their animated sprite and wander around. Players hunt them on foot.
- **Places**: springs (items), Sanctuaries (healing for gold, PC and Link), dungeons (a keeper, a badge, one win only) and player arenas (the winner becomes champion and their team defends the arena).
- **PC and Link**: changing team or releasing a chi happens at a Sanctuary PC; trades happen at a Sanctuary Link.
- **Seals**: stronger seals unlock with trainer level. The Perfect Seal is never sold.
- **Badges**: **More > Badges** shows the badges earned and the next challenge. A badge can open a region or a zone.
- **Trainers**: they appear in zones and reward gold. Some fixed trainers can only be beaten once per player.
- **Eggs**: they hatch as you walk.
- **Companion**: the first chi in the team walks beside its trainer and gains a little experience. Trainers wearing their glasses also see the companions of nearby trainers (`Config.Companion.others`). The server reads the companion from the central server, so a player cannot display a different one.
- **Chi Ranch**: an enclosure on the map. Breeding is managed there. Nearby, you can watch chi that trainers have placed in breeding: your own first, then those of players on your server, then those from other servers (`Config.Pen`).
- **"World" screen** in the app:
  - the chi sky (day, night, rain, storm, wind), which changes the most common chi;
  - daily and weekly quests;
  - the roaming chi of the day, a rare chi that changes zone every hour;
  - the giant chi: a raid shared by every server. Its level is drawn at random and it regenerates, so a lone player cannot beat it. During the battle, everyone sees the chi of other trainers fighting it at that moment, from any server, and the same HP bar; the more of them there are at once, the harder their hits land (`Config.Giant`);
  - the weekend tournament and the monthly ranked season.

- **Battles**: ranked 1v1 (matchmaking across all servers), friendly challenges (they do not count for the ranking) and **ranked 2v2**: two friends form a duo, search for another duo, and each commits their first three chi and chooses their move and target. 2v2 has its own ranking.

All of this is decided by the central server and applies to every server. The chi sky belongs to the game: it does not change your server's weather.

## What you control, and what you do not

Log in to the panel at `https://www.chiverse.eu/panel/` with your server key (`chi_key` command in the console).

| You can | Details |
|---|---|
| Hide official content | **Official content** page: untick a zone or place and it disappears on your server only. |
| Create your zones | Location, radius, number of chi. Choose a **model**: the official zone whose chi and levels yours reuses. Without a model, the nearest official zone is used. |
| Place springs, Sanctuaries and ranches | 20 places at most. A ranch shows chi being bred, and you can set its size in its settings. |
| Choose the zone mode | **Automatic**: official zones plus yours. **Custom**: yours only. |
| Opt out of an event | **Events** page. |

In game, `/chipos` (administrators) shows your coordinates to create a zone or place.

To keep the game fair across servers, you do not choose the chi or levels of a zone, you do not create dungeons or arenas, and you do not set quests, the sky, the giant chi or the tournament. Walking distance is measured by your server but capped by the central server, and springs are limited per hour and per player.

Official zones follow the scenery and rise in level from south to north. There is no forced level: each chi fights at its real level.

## Store

Packs are sold on your Tebex store. Your pack stock is prepaid to Chiverse: each delivery deducts from it, and a delivery without stock is refused.

Command to put in the Tebex package (run "when the player is online"):

```
chi_grant {sid} pack_dresseur {transaction}
```

- 1st argument: the player's in-game ID, or their license (`license:xxxx`);
- 2nd argument: the pack ID (visible on the panel);
- 3rd argument: the transaction ID, so the same purchase is never delivered twice.

The command only runs from the server console. Purchased items and chi are bound to the player's account and cannot be traded.

Your store must follow the FiveM platform rules: no virtual currency sold for real money and no paid random content.

## Troubleshooting

| What you see | What to check |
|---|---|
| Players see "the administrator must link this server" | Connect as an administrator and type `/chi`. |
| `/chi` does not open the link menu | Your license is not in `Config.Admins` and you have no administrator group. |
| A player must authorize this server | They already have a Chiverse account: **More > My account**, then chiverse.eu/compte. |
| `le serveur central ne répond pas (401)` (central server not responding) | The key is wrong, replaced, or the server was blocked. Contact support. |
| `le serveur central ne répond pas (0)` (central server not responding) | The `ApiUrl` address is wrong, or the central server cannot be reached from your machine. |
| `/chi` says "not available" | Chiverse is turned off for players in **My server**, or `Config.TestMode` is `true`. |
| A player sees no markers or chi | They never opened the game, or they are in sleep mode (`/chiveille`). |
| No icon on my phone | Check it is in the compatible list and that Chiverse starts after it. Otherwise use `/chi`, the key or the item. |
| Chi appear upside down | `Config.WorldSprites.flipV = true`. |
| Chi are visible through walls | `Config.WorldSprites.mode` must be `'world'`. |
| Performance drops near zones | Lower `Config.WorldSprites.max`, or set `enabled` to `false` (glowing orbs). |
| The item does nothing | Check the name `chi_console` and the line `export = 'chiverse.useConsole'`. |
| The glasses do not fit your outfits | Change `Config.Glasses.male` / `female` (number chosen in your clothing menu), or set `drawable = -1`. |
| The screen filter is annoying | `Config.Glasses.filter = ''`, or lower `strength`. |
| Other trainers' companions are not visible | Both players must have activated the game, with "Companion" and "Other companions" on. |

## Updates

New chi, places, quests, events and settings come through the central server: you have nothing to do. Only a new version of the resource requires replacing the folder; keep your `config.lua` and do not rename the folder, so the link is preserved.

## Files

| File | Purpose |
|---|---|
| `config.lua` | Visible settings: language, test mode, access, map, ranches, companion, glasses, giant chi, animation. |
| `config_server.lua` | Central server address. The key stays empty. |
| `install/` | ox_inventory item and its image. |
| `locales/` | In-game texts (prompts, map point names). |
| `ui/` | Game interface, shared by the phone, tablet and console. |

## Phones and inventories

Chiverse does not depend on any phone or inventory: `/chi`, the chosen key and the item always open the game.

| Phone | Integration |
|---|---|
| lb-phone, lb-tablet | Full in-phone app |
| qs-smartphone, qs-smartphone-pro, 17mov_Phone | Icon that opens the Chiverse console |
| NPWD, GKS Phone and others | No icon: `/chi`, the key or the item |

| Inventory | "Chiverse Console" item |
|---|---|
| ox_inventory | Supported directly |
| qs-inventory, codem-inventory | Supported directly |
| ESX, QBCore or QBox inventory | Supported through the framework |
| Other inventories | Work if they follow your framework's usable items |

Add the `chi_console` item to your inventory's item list, with the `install/chi_console.png` image. To remove the Quasar and 17Movement icons: `Config.PhoneBridges = false`.

**Reserved to Chiverse, all copying is prohibited.**
