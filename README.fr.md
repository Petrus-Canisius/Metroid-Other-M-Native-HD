# Metroid: Other M Native HD

![Bannière fan art du projet Metroid: Other M Native HD](assets/metroid-other-m-native-hd-banner.webp)

*Illustration de fan non officielle. Il ne s'agit pas d'une capture du jeu.*

Un projet indépendant et expérimental pour exécuter Metroid: Other M nativement sur PC par recompilation statique du code PowerPC de la Wii.

![In Active Development](https://img.shields.io/badge/status-In%20Active%20Development-blue)
![Not Playable Yet](https://img.shields.io/badge/gameplay-Not%20Playable%20Yet-orange)
![No Public Release](https://img.shields.io/badge/release-No%20Public%20Release-lightgrey)

[English](README.md) | **Français**

[Architecture](docs/ARCHITECTURE.md) · [État technique](docs/BUILD_STATUS.md) · [Feuille de route](docs/ROADMAP.md) · [Journal](docs/DEVELOPMENT_LOG.md) · [FAQ](docs/FAQ.md)

## État actuel

**In Active Development · Not Playable Yet · No Public Release**

Le début du démarrage s'exécute nativement dans un banc de développement. Aucune version jouable ni aucun téléchargement public ne sont disponibles. Ce dépôt documente le projet. Il ne contient ni les sources de développement ni les données du jeu.

Éléments examinés le **1er novembre 2026** :

| Domaine | État vérifié |
| --- | --- |
| Traduction PowerPC | Le dernier journal consulté indique 32 275 fonctions traduites |
| Tests du traducteur | Le dernier essai consulté indique 732 réussites, aucun échec ni test ignoré |
| Compilation native | Génération antérieure : 30 045 fonctions compilées avec succès |
| Validation du démarrage | MEM1 comparée à Dolphin au premier accès VI, avec quatre octets de base de temps différents |
| Interruptions et IOS | Délivrance initiale des interruptions et communication STM exercées |
| Threads et NAND | Implémentations partielles présentes, validation plus large à poursuivre |
| Accès disque | Commandes disque IOS encore non implémentées dans le code examiné |
| Rendu et gameplay | Aucun rendu natif du jeu vérifié ni version jouable |

Ces jalons sont distincts. Le nombre de fonctions traduites ne mesure pas l'achèvement du jeu. L'[état technique](docs/BUILD_STATUS.md) précise les limites des preuves et les écarts non résolus.

## Principe de la recompilation native

La chaîne de développement traduit les instructions PowerPC en C++, puis compile ce code pour le processeur du PC. Un runtime dédié fournit les services Wii, la gestion de la mémoire et les interfaces matérielles.

Le projet n'est pas un lanceur Dolphin. Dolphin sert de référence de développement pour comparer le comportement et la mémoire. L'exécution native exige encore un travail de compatibilité avec le matériel Wii, notamment pour les graphismes, l'audio, le stockage et les entrées.

La base technique comprend des outils d'analyse C++20, des parseurs DOL et REL, la relocation statique, un traducteur WiiCompiled adapté et un runtime Other M. Le backend graphique reste à choisir.

## Fonctionnalités prévues

Les éléments suivants sont des **objectifs**, pas des fonctionnalités disponibles :

- Exécution native sous Windows et Linux.
- Affichage en 1080p, 1440p et 4K, avec améliorations graphiques HD.
- Fréquences élevées jusqu'à 144 FPS si cela est techniquement réalisable.
- Clavier et souris, manettes Xbox et PlayStation.
- Support futur de la Wiimote avec la Mayflash DolphinBar.
- Optimisation CPU, GPU, RAM et VRAM, avec tests sur des PC modestes.
- Précompilation des shaders et comparaisons graphiques automatisées.
- Installation depuis une copie personnelle du jeu, lanceur et mises à jour.

La [feuille de route](docs/ROADMAP.md) précise les dépendances. Aucune date de sortie ni configuration minimale n'ont été établies.

## Lanceur et téléchargements

**Le lanceur sera publié sur ce dépôt dans la section [GitHub Releases](https://github.com/Petrus-Canisius/Metroid-Other-M-Native-HD/releases).** Ce sera le point de téléchargement officiel lorsqu'il sera prêt. L'installation devra utiliser une copie personnelle de *Metroid: Other M*.

Je développe ce projet sur mon temps libre. Sa réalisation pourra prendre beaucoup de temps et aucune date de sortie n'est annoncée. **Je reste déterminé à publier le lanceur ici.** Aucun lanceur ni jeu jouable n'est disponible pour le moment.

## Suivre le développement

Le [journal](docs/DEVELOPMENT_LOG.md) présente les jalons vérifiés. La [FAQ](docs/FAQ.md) aborde les téléchargements, les copies du jeu et les contributions. Les mises à jour publiques suivent une [procédure de maintenance](docs/MAINTENANCE.md) avec revue manuelle. Les documents techniques détaillés sont en anglais.

## Indépendance et droits

Ce projet n'est ni affilié à Nintendo, ni approuvé ou soutenu par Nintendo. Metroid et les noms associés appartiennent à leurs détenteurs respectifs. Aucun code de jeu Nintendo, ressource, image disque, clé ou donnée NAND privée n'est distribué ici.

La [FAQ](docs/FAQ.md#licensing) précise les licences tierces et les conditions d'une éventuelle publication des sources.
