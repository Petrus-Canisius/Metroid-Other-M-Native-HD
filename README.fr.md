# Metroid: Other M Native HD

Je développe une **adaptation PC native et indépendante de *Metroid: Other M***, sorti à l'origine sur Nintendo Wii. Mon objectif est de préserver le jeu original tout en le rendant utilisable sur les ordinateurs modernes, avec des commandes adaptées et, à terme, des améliorations graphiques facultatives.

**État actuel : le prototype privé Linux atteint environ 0,978× dans un banc d'entraînement ; la fluidité et la compatibilité du jeu complet ne sont pas validées. Aucune version jouable publique n'est distribuée.**

[English](README.md) | **Français**

[Dernier avancement](docs/BUILD_STATUS.md) · [Feuille de route](docs/ROADMAP.md) · [Journal de développement](docs/DEVELOPMENT_LOG.md) · [Fonctionnement du projet](docs/ARCHITECTURE.md) · [FAQ](docs/FAQ.md)

## Le projet en quelques mots

Il ne s'agit **pas d'un émulateur Dolphin simplement fourni avec le jeu**. Le projet traduit le programme PowerPC de la Wii pour l'exécuter sur PC et recrée les services de la console nécessaires aux graphismes, au son et aux commandes. Dolphin sert de **référence pour vérifier le comportement**. Certains de ses composants libres ont aussi été adaptés au moteur privé.

Je souhaite proposer à terme :

- **Des versions natives Linux et Windows**, avec un lanceur dédié.
- **Le clavier-souris et les manettes Xbox/PlayStation**, y compris l'adaptation des gestes, du pointeur et des changements d'orientation de la Wiimote. Une Wiimote physique ne devra pas être obligatoire pour terminer le jeu.
- **La possibilité de conserver les commandes Wii originales**, notamment avec une vraie Wiimote et une Mayflash DolphinBar.
- **Une image fidèle avant tout**, puis des options 1080p, 1440p, 4K, 16:9, 21:9 adapté, anticrénelage réglable, HDR10 et rafraîchissement variable lorsque cela sera réalisable.
- **Une exécution fluide et un gameplay respecté**, sans que la fréquence de l'écran modifie la vitesse du jeu.

Ce sont des **objectifs**, et non des fonctionnalités déjà disponibles dans une version publique.

## Dernières avancées — 3 novembre 2026

**Le prototype privé sous Ubuntu affiche une fenêtre SDL3/Vulkan, produit du son et reconnaît une manette Xbox Series X physique.** Le déplacement et le tir ont été démontrés par enregistrement puis rejeu de commandes dans la salle d'entraînement. Toutes les commandes et l'intégralité du jeu ne sont pas encore validées.

Le **dernier rapport privé** indique une vitesse de simulation d'environ **0,978×** sur le banc de la salle d'entraînement (**environ 16,8–17,2 ms/trame**), après une optimisation mesurée des soumissions Vulkan. Cela **ne démontre pas encore 1,0× stable dans l'ensemble du jeu**, et **aucune version jouable publique n'est distribuée**.

| Domaine | Derniers résultats rapportés |
| --- | --- |
| Recompilation native | **32 282 fonctions PowerPC** traduites ; **753/753 tests** du traducteur |
| Expérience Linux | Fenêtre SDL3/Vulkan, sortie audio, manette Xbox réelle ; déplacement et tir démontrés par rejeu |
| Performances graphiques | Passage de **4 à 8 soumissions Vulkan simultanées** : environ **−1,28 ms/trame** sur six paires A/B ; trois paires supplémentaires confirment **−1,20 ms/trame** |
| Compilation | **ThinLTO avec Clang/LLD** sur le rendu : gain antérieur d'environ **−0,58 ms/trame** ; PGO testé puis rejeté car plus lent |
| Coût mémoire | Environ **1 041 Mo de RAM** avec huit emplacements, contre **863 Mo** avec quatre, sur la machine de développement |
| Fidélité | **28 167 trames internes** identiques ; **753/753** tests du traducteur et **3/3** des commandes ; **156,73 s** d'audio conformes |
| Copies GPU | **4 671 copies contrôlées**, aucune différence lors de la dernière validation concernée |
| Écarts connus | **29 trames de fondu** différentes de la référence logicielle ; **135 et 387 octets** d'écart mémoire avec Dolphin |
| Sauvegarde PAL | Export privé européen **R3OP** identifié, trois emplacements chiffrés ; progression « 100 % » et chargement non vérifiés |
| Windows | Base multiplateforme ; **aucune compilation ni validation Windows** |

### Signification de ces performances

Le test couvre la salle d'entraînement entre **438 et 470 secondes simulées**. Pour une simulation à 60 Hz, **1,0× correspond à environ 16,67 ms/trame**. La vitesse actuelle de **0,978×** est un résultat de benchmark, **pas un pourcentage d'achèvement** ni la preuve de performances identiques dans les autres zones. Le **P99 reste autour de 20,2–20,5 ms**.

Le gain provient d'une **modification démontrée** : huit emplacements de soumission Vulkan au lieu de quatre. Six emplacements n'apportaient pas de gain confirmé ; douze augmentaient la mémoire sans bénéfice clair. Une autre correction a supprimé **8 038 attentes EFB/texture artificielles** causées par un calcul de plages mémoire trop large : elle améliore la précision, mais **aucun gain moyen n'a été démontré**.

ThinLTO a également apporté un gain mesuré, tandis que PGO a été rejeté. La comparaison de la compilation des fonctions PowerPC avec `-ffp-contract=off` n'a pas modifié leur code machine ; le runtime applique désormais un garde-fou contre certaines contractions flottantes. La cible CPU officielle est **x86-64-v3**, sans que cela définisse à lui seul la configuration minimale complète.

### Étapes suivantes

La trace indique notamment **4,65 ms/trame d'exécution GPU**, **2,43 ms/trame de relecture/encodage** et **2,26 ms/trame de délai avant le GPU** dans le chemin étudié. La RTX 4090 utilisée pour le développement reste souvent en état d'économie **P8**. **L'influence réelle de cet état n'est pas encore prouvée** : un test temporaire et réversible a été autorisé, sans résultat disponible à ce stade. Le moteur final ne devra pas exiger un réglage NVIDIA.

Les prochaines priorités sont : obtenir **1,0× reproductible**, améliorer la régularité des trames, vérifier plusieurs scènes et les commandes, corriger les écarts graphiques et mémoire, puis valider les sauvegardes et Windows. Les optimisations portent sur le moteur commun ; chaque zone devra être testée, pas optimisée individuellement par principe.

Détails : [état de développement](docs/BUILD_STATUS.md), [journal](docs/DEVELOPMENT_LOG.md), [feuille de route](docs/ROADMAP.md).

## Suivi et publication

Ces informations proviennent de mes **rapports privés Claude Code jusqu'au 3 novembre 2026**. Les programmes et leurs tests n'ont pas été réexécutés indépendamment lors de cette mise à jour publique. Je distingue les résultats rapportés des fonctionnalités effectivement disponibles pour les joueurs.

Je prévois de publier un lanceur sur [GitHub Releases](https://github.com/Petrus-Canisius/Metroid-Other-M-Native-HD/releases) lorsqu'il sera prêt. **Aucune date de sortie, aucun exécutable public et aucune configuration minimale validée** ne sont annoncés. La future installation devra utiliser **une copie personnelle et locale du jeu**.

**Ce dépôt contient uniquement de la documentation** : ni image du jeu, ni code Nintendo traduit, ni ressource originale, ni enregistrement privé, ni clé de chiffrement, ni exécutable. Le projet est indépendant et non affilié à Nintendo. Les composants libres adaptés de Dolphin et d'autres projets devront respecter leurs licences et leurs obligations de publication du code source lors d'une éventuelle distribution. Voir la [FAQ](docs/FAQ.md#licensing).
