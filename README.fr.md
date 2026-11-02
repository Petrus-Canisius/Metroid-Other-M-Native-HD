# Metroid: Other M Native HD

Je développe une **adaptation PC native et indépendante de *Metroid: Other M***, sorti à l'origine sur Nintendo Wii. Mon objectif est de préserver le jeu original tout en le rendant utilisable sur les ordinateurs modernes, avec des commandes adaptées et, à terme, des améliorations graphiques facultatives.

**État actuel : un prototype interactif fonctionne sous Linux, mais il reste plus lent que le jeu original. Aucune version jouable n'est distribuée.**

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

## Dernières avancées — rapports jusqu'au 2 novembre 2026

**Le prototype privé sous Ubuntu s'affiche dans une véritable fenêtre SDL3/Vulkan, produit du son et reconnaît une manette Xbox Series X physique.** Une séance de commandes enregistrée a été rejouée et comparée à une exécution sans entrée : Samus **se déplace et tire** dans la salle d'entraînement. Ce prototype interactif **n'est pas encore un jeu PC complet, fluide et distribué**.

Les dernières sessions visent surtout **l'optimisation mesurable sans perte de fidélité** :

| Domaine | Derniers résultats rapportés |
| --- | --- |
| Recompilation | **32 282 fonctions PowerPC** traduites ; **753/753 tests** du traducteur réussis |
| Expérience Linux | Fenêtre SDL3/Vulkan, sortie audio, manette physique ; déplacement et tir démontrés par rejeu comparatif |
| Rendu | Copies GPU vers textures, shader TEV optimisé, deux fils de transformation, assemblage/découpage parallèles des triangles et réutilisation bornée des images Vulkan |
| Performances | Dernière session, code inchangé : **0,879–0,917× (0,897× de moyenne sur cinq exécutions)** dans la salle d'entraînement ; **18,19–18,97 ms/trame** |
| Gain directement imputable au code | Une comparaison A/B précédente a confirmé **environ 0,54 ms/trame** économisée grâce à l'assemblage parallèle ; dans cette session, la vitesse était de **0,835–0,848×** |
| Fidélité | **28 167 trames internes** inchangées lors des contrôles de non-régression ; des écarts avec Dolphin demeurent |
| Son | **156,73 secondes** identiques à une capture de référence testée, sans validation de tout le jeu |
| Mémoire | Environ **857 Mo de RAM** et **78 Mo de VRAM** lors du dernier banc ; ce ne sont pas des besoins minimaux |
| Sauvegarde PAL | Export Wii SD européen **R3OP** reconnu avec **trois emplacements** ; progression « 100 % » et chargement natif non vérifiés |
| Windows | Base multiplateforme présente ; **version Windows non compilée et non testée** |

### Ce que signifient les dernières mesures

La dernière session **n'a conservé aucune nouvelle optimisation**. Claude a déplacé temporairement le calcul des pentes géométriques vers les fils de transformation : les images restaient conformes, mais le temps moyen augmentait **d'environ 0,21 ms/trame**. L'expérience a été annulée.

Le même code s'est montré sensiblement plus rapide que lors de la session précédente. Par conséquent, **la moyenne de 0,897× est une observation dépendant des conditions du poste, et non un gain attribuable à une modification de code**. La prochaine priorité est de maîtriser cette variabilité, puis d'optimiser le chargement des sommets et les fonctions du runtime natif.

Sur la séquence testée à 60 Hz, il faut environ **16,67 ms/trame** pour atteindre la vitesse normale, contre **18,58 ms/trame** en moyenne dans la dernière session. Les paliers **0,90×, 0,95× et 1,0× stable ne sont pas encore validés**. La vitesse de simulation n'est pas le pourcentage d'achèvement du projet.

### Origine des saccades au démarrage

Avec des caches isolés, la création des pipelines Vulkan a demandé environ **436 ms à froid**, contre **1 ms à chaud**. La première image a pris **514 ms à froid**, contre **82 ms à chaud**. Cela explique le pic initial d'environ 520 ms observé dans ce test, sans garantir la disparition de toutes les saccades pendant le jeu. La préparation anticipée des pipelines est à étudier.

### Ce qui reste à faire

- Atteindre **1,0× en continu**, maîtriser les temps de trame et mesurer plusieurs scènes.
- Compléter les effets GX/Vulkan : certains traitements de brouillard, textures indirectes, mipmaps et mélange des couleurs ; **29 trames de fondu diffèrent toujours** de la référence logicielle.
- Examiner les **écarts mémoire de 135 et 387 octets avec Dolphin** aux checkpoints observés.
- Vérifier le saut, la Morph Ball, les esquives, la visée subjective, les missiles, la concentration et les autres commandes.
- Vérifier réellement le contenu de la sauvegarde PAL annoncée à 100 % ; **aucune importation dans Dolphin ni lecture native n'a encore été démontrée**.
- Valider la Wiimote physique avec DolphinBar, compiler Windows et développer les options graphiques HD/HDR/VRR et d'anticrénelage.

### Priorités immédiates

1. Rendre **les benchmarks reproductibles** : un même binaire varie de plus d'une milliseconde/trame entre sessions.
2. Profiler et alléger le **Vertex Loader** et les fonctions d'assistance au code PowerPC recompilé.
3. Réduire les coûts Vulkan et de synchronisation sans modifier les résultats.
4. Préparer les pipelines GPU pour limiter les saccades à cache froid.
5. Valider un gameplay à vitesse normale avec une vraie manette, puis élargir les tests aux autres scènes et aux sauvegardes.

L'[état de développement](docs/BUILD_STATUS.md) et le [journal de développement](docs/DEVELOPMENT_LOG.md) détaillent les preuves, les expériences conservées ou rejetées et les problèmes ouverts.

## Suivi et publication

Ces informations proviennent de mes **rapports privés Claude Code du 2 novembre 2026**. Les programmes et leurs tests n'ont pas été réexécutés indépendamment lors de cette mise à jour publique. Je distingue les résultats rapportés des fonctionnalités effectivement disponibles pour les joueurs.

Je prévois de publier un lanceur sur [GitHub Releases](https://github.com/Petrus-Canisius/Metroid-Other-M-Native-HD/releases) lorsqu'il sera prêt. **Aucune date de sortie, aucun exécutable public et aucune configuration minimale validée** ne sont annoncés. La future installation devra utiliser **une copie personnelle et locale du jeu**.

**Ce dépôt contient uniquement de la documentation** : ni image du jeu, ni code Nintendo traduit, ni ressource originale, ni enregistrement privé, ni clé de chiffrement, ni exécutable. Le projet est indépendant et non affilié à Nintendo. Les composants libres adaptés de Dolphin et d'autres projets devront respecter leurs licences et leurs obligations de publication du code source lors d'une éventuelle distribution. Voir la [FAQ](docs/FAQ.md#licensing).
