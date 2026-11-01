# Metroid: Other M Native HD

![Bannière fan art du projet Metroid: Other M Native HD](assets/metroid-other-m-native-hd-banner.webp)

*Illustration de fan non officielle. Il ne s'agit pas d'une capture du jeu.*

Je développe un projet indépendant visant à faire fonctionner *Metroid: Other M* nativement sur PC grâce à la recompilation statique du code PowerPC de la Wii.

![In Active Development](https://img.shields.io/badge/status-In%20Active%20Development-blue)
![Not Playable Yet](https://img.shields.io/badge/gameplay-Not%20Playable%20Yet-orange)
![No Public Release](https://img.shields.io/badge/release-No%20Public%20Release-lightgrey)

[English](README.md) | **Français**

[Architecture](docs/ARCHITECTURE.md) · [État technique](docs/BUILD_STATUS.md) · [Feuille de route](docs/ROADMAP.md) · [Journal](docs/DEVELOPMENT_LOG.md) · [FAQ](docs/FAQ.md)

## État du développement

Les premières étapes du démarrage Wii s'exécutent dans mon environnement de développement natif. Je n'ai pas encore vérifié d'image de gameplay native ni obtenu de version jouable.

| Domaine | Derniers éléments documentés |
| --- | --- |
| Traduction PowerPC | 32 275 fonctions générées, dont certaines issues d'une détection exploratoire |
| Tests du traducteur | 732 tests réussis dans la dernière suite examinée |
| Compilation native | 30 045 fonctions compilées dans une génération antérieure |
| Services Wii | Interruptions et IOS ; mes dernières notes de travail indiquent la lecture de `SYSCONF` depuis la NAND |
| Ordonnancement | Fibres natives en développement ; 55 boucles d'attente détectées pour instrumentation |
| Disque et rendu | Lectures disque à l'étude ; aucun rendu de gameplay natif vérifié |

J'ai consigné les dernières observations sur la NAND et les boucles d'attente dans mes journaux du **1er novembre 2026**. Je ne les ai pas reproduites indépendamment lors de cette mise à jour. Ces chiffres représentent des jalons distincts, pas un pourcentage d'achèvement. Voir l'[état technique](docs/BUILD_STATUS.md).

## Principe technique

La chaîne de développement traduit les instructions PowerPC de la Wii en C++, puis les compile pour le processeur du PC. Un runtime dédié prend en charge la mémoire, les interfaces matérielles et les services système Wii.

Ce projet **n'est pas un lanceur Dolphin**. Dolphin sert de référence pour les comparaisons de comportement et de mémoire. Le choix du backend graphique reste ouvert entre Aurora/Dawn et une éventuelle implémentation Vulkan directe.

## Fonctionnalités prévues

Il s'agit d'objectifs, pas de fonctionnalités déjà disponibles.

- **Systèmes et commandes :** Windows et Linux, clavier et souris, manettes Xbox et PlayStation, puis Wiimote via Mayflash DolphinBar.
- **Graphismes :** affichage 1080p, 1440p et 4K, améliorations HD facultatives et fréquences pouvant atteindre 144 FPS si cela est techniquement réalisable.
- **Optimisation :** maîtrise du CPU, du GPU, de la RAM et de la VRAM, compatibilité avec les PC modestes, préparation des shaders et tests automatisés.
- **Installation :** importation locale d'une copie personnelle du jeu, lanceur dédié et futur système de mises à jour.

La [feuille de route](docs/ROADMAP.md) présente les étapes et leurs limites.

## Lanceur et téléchargements

Je **publierai le lanceur prévu sur ce dépôt via [GitHub Releases](https://github.com/Petrus-Canisius/Metroid-Other-M-Native-HD/releases)** lorsqu'il sera prêt. Les joueurs devront posséder leur propre copie de *Metroid: Other M*. Aucun téléchargement ni date de sortie ne sont annoncés pour le moment.

Je développe ce projet sur mon temps libre. Le développement peut prendre du temps, mais je compte publier le lanceur ici.

## Suivre le développement

Je présente les jalons importants dans mon [journal](docs/DEVELOPMENT_LOG.md) et je réponds aux questions techniques et de distribution dans la [FAQ](docs/FAQ.md). Je révise manuellement les informations publiques à l'aide de ma [procédure de maintenance](docs/MAINTENANCE.md). La documentation technique détaillée est en anglais.

## Indépendance et droits

Je mène ce projet de fan de manière indépendante, sans affiliation, soutien ni approbation de Nintendo. Metroid et les noms associés appartiennent à leurs détenteurs respectifs. Ce dépôt ne distribue aucun code de jeu Nintendo, ressource du jeu, image disque, donnée NAND privée ou clé de chiffrement.

La [FAQ](docs/FAQ.md#licensing) présente les questions relatives aux licences tierces.
