# Metroid: Other M Native HD

Je développe une **adaptation PC native et indépendante de *Metroid: Other M***, sorti à l'origine sur Nintendo Wii. Mon objectif est de préserver le jeu original tout en le rendant utilisable sur les ordinateurs modernes, avec des commandes adaptées et, à terme, des améliorations graphiques facultatives.

**État actuel : le prototype privé Linux a franchi 1,0× de vitesse moyenne dans un benchmark d'entraînement (1,013×) ; la stabilité et la compatibilité du jeu complet restent à valider. Aucune version jouable publique n'est distribuée.**

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

## Dernières avancées — 3 novembre 2026 : premier benchmark supérieur à 1,0×

**Pour la première fois, le prototype PC natif privé dépasse la vitesse normale dans le banc de référence de la salle d'entraînement : 1,013× en moyenne selon le dernier rapport.** Ce résultat provient d'une optimisation ciblée du fragment shader Vulkan, vérifiée sur **six comparaisons A/B alternées**, avec un gain moyen de **0,52 ms par trame**, un P99 amélioré et des trames de référence identiques. Claude indique également que la **suite complète de non-régression a réussi**.

**Cette étape ne démontre pas encore que l'ensemble du jeu fonctionne à 1,0× de façon stable. Aucune version jouable publique n'est disponible.**

Le prototype privé Ubuntu affiche une fenêtre SDL3/Vulkan, produit du son et reconnaît une manette Xbox Series X physique. Des commandes enregistrées puis rejouées ont déjà permis de démontrer le déplacement et le tir de Samus. Les autres commandes et le jeu complet restent à valider.

| Domaine | Dernières données du développement privé |
| --- | --- |
| **Jalon de performance** | **1,013× de vitesse moyenne** dans la salle d'entraînement : premier résultat mesuré dépassant 1,0× |
| **Nouvelle optimisation Vulkan** | Shader du test alpha sans `discard` **uniquement lorsque la configuration du test garantit que tous les fragments passent** : **−0,52 ms/trame** sur six paires A/B ; trames de référence identiques |
| Optimisations précédentes | Huit soumissions Vulkan en vol : **−1,28 ms/trame** sur six paires ; **ThinLTO : −0,58 ms/trame** sur six paires |
| Recompilation et tests | **32 282 fonctions PowerPC** traduites ; **753/753 tests** du traducteur dans les validations documentées ; dernière suite complète annoncée réussie |
| Graphismes et audio | **28 167 trames internes** dans la suite de référence ; **156,73 secondes d'audio** vérifiées précédemment ; divergences avec Dolphin non résolues |
| Commandes | Manette Xbox reconnue, déplacements et tirs démontrés par rejeu ; commandes complètes encore non validées |
| Diagnostic NVIDIA | Modification PowerMizer **non effectuée** : extension NV-CONTROL inaccessible sous Wayland/Xwayland. Pilote laissé inchangé |
| Expérience en cours | Spécialisation du shader **TEV** par constantes Vulkan, évaluée sur **six paires A/B** ; **résultat encore inconnu** |
| Compatibilité | Cible CPU **x86-64-v3**, objectif de neutralité Vulkan ; **aucune version Windows testée ni distribution publique** |

### Pourquoi le moteur dépasse désormais 1,0×

L'instrumentation a montré que le travail des fragments TEV occupait une grande partie du temps des passes GPU. Une expérience supprimant `discard` partout réduisait ce coût, mais n'était pas adaptée à une version définitive car elle aurait pu produire des images incorrectes.

La solution retenue est **conditionnelle** : lorsque les règles du test alpha Wii garantissent que tous les fragments passent, le moteur utilise un shader Vulkan qui n'exécute pas `discard`. Dans les autres cas, il conserve le comportement nécessaire.

Après préchauffage des pipelines, **six comparaisons A/B** ont montré **−0,52 ms/trame en moyenne**, une vitesse de **1,013×**, un meilleur P99 et des références graphiques identiques. La non-régression complète a ensuite été annoncée réussie. Le rapport partagé ne donne pas de nouvelle plage chiffrée précise pour le P99.

### Portée réelle du résultat

À 60 Hz, la vitesse normale correspond à environ **16,67 ms/trame** pour le scénario de référence. Dépasser **1,0× en moyenne dans cette salle** ne garantit ni des temps de trame réguliers partout, ni tous les combats, cinématiques, chargements ou commandes. Les **29 trames de fondu** différentes de la référence logicielle et les écarts de **135/387 octets** par rapport à Dolphin restent à examiner.

Cette optimisation appartient au **moteur Vulkan commun**, et non à une salle particulière. Les autres zones devront être testées de façon représentative, sans supposer qu'il faudra les optimiser une par une.

### Étapes suivantes

Terminer les mesures A/B de la spécialisation **TEV** avant d'en annoncer les résultats. Consolider ensuite une marge de performance supérieure à 1,0×, contrôler P95/P99 et les saccades à froid, élargir les tests de gameplay, résoudre les écarts restants et valider les sauvegardes puis Windows. Le pilote NVIDIA n'a pas été modifié durant le diagnostic Wayland.

Détails : [état de développement](docs/BUILD_STATUS.md), [journal](docs/DEVELOPMENT_LOG.md), [feuille de route](docs/ROADMAP.md).

## Suivi et publication

Ces informations proviennent de mes **rapports privés Claude Code jusqu'au 3 novembre 2026**. Les programmes et leurs tests n'ont pas été réexécutés indépendamment lors de cette mise à jour publique. Je distingue les résultats rapportés des fonctionnalités effectivement disponibles pour les joueurs.

Je prévois de publier un lanceur sur [GitHub Releases](https://github.com/Petrus-Canisius/Metroid-Other-M-Native-HD/releases) lorsqu'il sera prêt. **Aucune date de sortie, aucun exécutable public et aucune configuration minimale validée** ne sont annoncés. La future installation devra utiliser **une copie personnelle et locale du jeu**.

**Ce dépôt contient uniquement de la documentation** : ni image du jeu, ni code Nintendo traduit, ni ressource originale, ni enregistrement privé, ni clé de chiffrement, ni exécutable. Le projet est indépendant et non affilié à Nintendo. Les composants libres adaptés de Dolphin et d'autres projets devront respecter leurs licences et leurs obligations de publication du code source lors d'une éventuelle distribution. Voir la [FAQ](docs/FAQ.md#licensing).
