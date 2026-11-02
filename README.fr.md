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

**Le projet produit désormais ses premières images natives.** Le moteur graphique logiciel adapté a affiché l'avertissement de la dragonne, avec une image XFB **identique à Dolphin sur 614 400 octets** dans des conditions de filtrage équivalentes. Le runtime atteint aussi le menu **OPTIONS**, réagit aux commandes Wiimote scriptées et exécute la cinématique d'introduction. **Aucune séquence de gameplay n'a encore été validée. Le jeu n'est pas jouable.**

| Domaine | Derniers résultats rapportés |
| --- | --- |
| Traduction PowerPC | **32 282 fonctions** traduites, dont sept nouvelles entrées indirectes ; **732/732 tests** du traducteur réussis |
| Runtime Wii | Démarrage, IOS, NAND, lecteur RVZ intégré, ordonnancement et Wiimote émulée exercés |
| Graphismes | Véritable rendu logiciel GX ; image XFB de la dragonne identique à Dolphin sur **614 400/614 400 octets** |
| Menus et commandes | Navigation dans OPTIONS, modification des réglages, menu HOME et pointeur IR, puis START et introduction observés **en natif** |
| Audio | **17,09 secondes** de son actif identiques à Dolphin, échantillon par échantillon après synchronisation |
| Prochaines étapes | Horloge Wii déterministe, validation reproductible des menus, puis rendu accéléré GPU (Vulkan direct recommandé, non implémenté) |

Ce bilan provient du compte rendu de Claude Code du **2 novembre 2026**. La première image et certaines trames de l'introduction correspondent à Dolphin, mais **toutes les interactions des menus n'ont pas encore été reproduites dans Dolphin**. Le moteur logiciel de référence est lent, environ 28 images/s lors du test rapporté. Il ne constitue **pas le futur moteur graphique définitif**. Je n'ai pas exécuté à nouveau les tests privés pour cette mise à jour documentaire. Voir l'[état technique](docs/BUILD_STATUS.md).


## Principe technique

La chaîne de développement traduit les instructions PowerPC de la Wii en C++, puis les compile pour le processeur du PC. Un runtime dédié prend en charge la mémoire, les interfaces matérielles et les services système Wii.

Ce projet **n'est pas un lanceur Dolphin**. Dolphin sert de référence pour les comparaisons de comportement et de mémoire. Le moteur logiciel adapté de Dolphin produit désormais les images natives de référence. **Vulkan direct** est recommandé pour le futur rendu accéléré par GPU, mais ce backend n'est pas encore implémenté.

## Fonctionnalités prévues

Il s'agit d'objectifs, pas de fonctionnalités déjà disponibles.

- **Systèmes et commandes :** Windows et Linux, clavier et souris, manettes Xbox et PlayStation, puis Wiimote via Mayflash DolphinBar.
- **Graphismes :** prise en charge prévue des formats 16:9 (écran large) et 21:9 (ultrawide), avec un champ de vision adapté plutôt qu'une image étirée ; affichage 1080p, 1440p et 4K, améliorations HD facultatives et fréquences pouvant atteindre 144 FPS si cela est techniquement réalisable. Le HDR10, le VRR, G-SYNC et AMD FreeSync sont aussi prévus comme options facultatives, avec retour au SDR classique. Aucune de ces technologies d'affichage n'est encore implémentée ni validée.
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
