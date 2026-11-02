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

## Dernières avancées — 2 novembre 2026

**Le jeu s'affiche désormais dans une véritable fenêtre SDL3 sous Ubuntu grâce à Vulkan.** Le prototype privé produit du son et reconnaît également une véritable manette Xbox Series X. Une séance enregistrée avec cette manette a été rejouée et comparée à une exécution sans commandes : **Samus se déplace et tire dans la salle d'entraînement** lorsque les commandes sont présentes.

C'est une étape importante pour le gameplay. Toutefois, **d'autres actions restent à tester, la vitesse normale n'est pas encore atteinte et aucun exécutable n'est disponible au téléchargement**.

| Domaine | Dernier résultat rapporté |
| --- | --- |
| Programme natif | **32 282 fonctions PowerPC traduites** ; **753/753 tests** du traducteur réussis |
| Affichage Linux | **Fenêtre SDL3 avec Vulkan**, redimensionnement et plein écran ; pas encore de version Windows |
| Manette physique | Entrées Xbox enregistrées et rejouées ; **déplacement et tir** démontrés dans la salle d'entraînement |
| Graphismes | Rendu Vulkan par lots et copies de textures sur GPU ; **28 167 trames** identiques à la référence interne de non-régression |
| Son | Sortie sonore sous Linux ; **156,7 secondes** identiques à une capture de référence dans la dernière comparaison rapportée |
| Reproductibilité | Plus de dix exécutions avec images identiques aux points testés ; **7 686 résultats de textures GPU** conformes à la référence CPU |
| Performances | Dans la salle d'entraînement : **0,369× → 0,576× de la vitesse normale**, soit environ 22 à 35 images simulées/s sur la séquence testée à 60 Hz |
| Windows | Base multiplateforme commencée, mais **code Windows non compilé et non testé** |

### Pourquoi les optimisations sont importantes

Le moteur traite maintenant une image en **28,98 ms en moyenne**, contre **45,27 ms** avant cette phase. Sur la séquence mesurée à 60 Hz, il faudrait environ **16,67 ms par image** pour maintenir la vitesse originale.

Claude a gagné du temps en évitant des recalculs de sommets, en transférant certaines conversions de textures sur le GPU, en supprimant des traitements logiciels inutiles et en répartissant la conversion de l'image finale sur plusieurs cœurs CPU. La puissance brute de la RTX 4090 ne suffit pas : le traitement graphique côté processeur et les synchronisations nécessitent encore du travail.

**0,576× ne signifie pas que le développement du projet est terminé à 57,6 %.** C'est seulement une mesure de vitesse sur une scène donnée.

### Ce qui n'est pas encore validé

- Une **session de jeu prolongée à vitesse normale**.
- Toutes les commandes Xbox : saut, Morph Ball, esquives, visée subjective, missiles et concentration.
- Le fonctionnement d'une **véritable Wiimote avec DolphinBar**.
- La progression intégrale, les sauvegardes et toutes les scènes.
- Certains effets Vulkan : brouillard, textures indirectes, mipmaps et fondus. **29 trames de fondu diffèrent encore** du moteur logiciel dans une comparaison ciblée.
- Le portage Windows compilé et les options 4K, 21:9, HDR, VRR et anticrénelage.

Les **28 167 trames identiques** démontrent une absence de régression dans un test interne, **pas** une correspondance intégrale avec Dolphin. Deux comparaisons mémoire présentent toujours **135 et 387 octets de différence** avec Dolphin.

## Prochaines priorités

1. **Atteindre une vitesse stable de 1,0×** grâce aux optimisations du traitement des sommets, de l'éclairage et des copies graphiques.
2. **Compléter la fidélité du rendu Vulkan** et poursuivre les comparaisons image par image.
3. **Valider le gameplay avec une vraie manette Xbox** : déplacements, tirs, saut, Morph Ball, missiles, visée et actions spécifiques.
4. **Adapter l'ensemble des gestes indispensables de la Wiimote** au clavier-souris et aux manettes classiques, sans abandonner la DolphinBar.
5. **Tester les sauvegardes et d'autres scènes**, puis préparer Windows et les améliorations graphiques facultatives.

## Suivi et publication

Ces informations proviennent de mes **rapports privés Claude Code du 2 novembre 2026**. Les programmes et leurs tests n'ont pas été réexécutés indépendamment lors de cette mise à jour publique. Je distingue les résultats rapportés des fonctionnalités effectivement disponibles pour les joueurs.

Je prévois de publier un lanceur sur [GitHub Releases](https://github.com/Petrus-Canisius/Metroid-Other-M-Native-HD/releases) lorsqu'il sera prêt. **Aucune date de sortie, aucun exécutable public et aucune configuration minimale validée** ne sont annoncés. La future installation devra utiliser **une copie personnelle et locale du jeu**.

**Ce dépôt contient uniquement de la documentation** : ni image du jeu, ni code Nintendo traduit, ni ressource originale, ni enregistrement privé, ni clé de chiffrement, ni exécutable. Le projet est indépendant et non affilié à Nintendo. Les composants libres adaptés de Dolphin et d'autres projets devront respecter leurs licences et leurs obligations de publication du code source lors d'une éventuelle distribution. Voir la [FAQ](docs/FAQ.md#licensing).
