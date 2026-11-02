# Metroid: Other M Native HD

Je développe une **adaptation native sur PC de *Metroid: Other M***, jeu sorti à l'origine sur Wii. Mon objectif est de retrouver l'aventure originale sur un ordinateur moderne, avec des commandes adaptées au PC et, à terme, des améliorations graphiques facultatives.

C'est un projet de fan indépendant, **sans affiliation avec Nintendo**. Il est **en cours de développement, n'est pas encore jouable comme un jeu PC classique et n'est pas disponible au téléchargement**.

[English](README.md) | **Français**

[Avancement actuel](docs/BUILD_STATUS.md) · [Feuille de route](docs/ROADMAP.md) · [Journal de développement](docs/DEVELOPMENT_LOG.md) · [Fonctionnement](docs/ARCHITECTURE.md) · [FAQ](docs/FAQ.md)

## Ce que je veux réaliser

Je souhaite proposer une véritable version PC de *Metroid: Other M*, et non simplement distribuer un émulateur Wii avec le jeu. Le projet traduit le programme d'origine pour qu'un processeur PC puisse l'exécuter et recrée les fonctions de la Wii dont le jeu a besoin.

À terme, je veux proposer :

- **Une version native sous Linux et Windows**, avec un lanceur dédié et l'utilisation locale d'une copie personnelle du jeu.
- **Le clavier et la souris ainsi que les manettes classiques**, notamment Xbox et PlayStation. Les gestes de la Wiimote, son pointeur et ses changements d'orientation devront être adaptés aux commandes PC pour que **la Wiimote physique ne soit pas obligatoire**.
- **Un affichage adapté aux écrans modernes**, avec une image fidèle en priorité, puis du 1080p, 1440p, 4K, du 16:9 et, si les scènes s'y prêtent, du 21:9 sans étirement.
- **Des améliorations visuelles optionnelles**, dont un anticrénelage réglable et désactivable, le HDR10 et le rafraîchissement variable (G-SYNC/FreeSync sur matériel compatible).
- **Le respect du jeu original** : rythme, maniabilité, son, ambiance et logique de gameplay. Les fréquences d'affichage élevées ne seront envisagées que si elles ne perturbent pas le jeu.

Il s'agit **d'objectifs de développement**, pas de fonctionnalités déjà présentes dans une version distribuée.

## Où en est le projet ? — 2 novembre 2026

**Samus apparaît désormais dans la salle d'entraînement et réagit à des commandes envoyées par des scripts dans le programme natif.** Claude a comparé deux exécutions : les images étaient identiques jusqu'à l'envoi de la première commande, puis Samus se déplaçait uniquement dans l'exécution avec commandes.

C'est une étape importante vers le gameplay, mais **ce n'est pas encore un jeu PC jouable** : il manque **une vraie fenêtre d'affichage et des commandes clavier/manette en direct**.

| Domaine | Résultat actuel |
| --- | --- |
| Reconstruction du jeu | **32 282 fonctions traduites** ; **753 tests du traducteur réussis sur 753** lors du dernier rapport |
| Démarrage et menus | La Wiimote émulée permet de passer les avertissements, d'atteindre les menus, le chargement et la cinématique d'introduction |
| Première scène de gameplay | Samus et l'interface du jeu apparaissent dans la salle d'entraînement ; un déplacement causé par des **commandes scriptées** a été démontré |
| Graphismes | Un moteur de référence produit de vraies images, dont certaines identiques à Dolphin. Un **premier moteur GPU Vulkan** fonctionne sur la machine de développement |
| Son | Après synchronisation, **20,58 secondes** de son actif correspondent exactement à Dolphin, échantillon par échantillon |
| Fiabilité | Une nouvelle horloge simulée rend les répétitions natives identiques aux points mémoire testés ; des écarts avec Dolphin restent à résoudre |
| Windows | Une base multiplateforme et une implémentation spécifique à Windows ont été écrites, mais **la version Windows n'a pas encore été compilée ni testée** |

### Ce que signifie l'avancée Vulkan

Un premier rendu Vulkan fonctionne sur une NVIDIA RTX 4090. Sur l'écran de la dragonne utilisé comme référence, **767 images sur 796 sont identiques au rendu de référence** ; 29 images du fondu présentent encore des différences. Le GPU dépend toujours de certaines opérations du moteur logiciel, plus lent. Il ne s'agit donc **pas encore d'un moteur graphique autonome prêt pour jouer en temps réel**.

Les images et les essais de gameplay restent dans **l'environnement de développement privé**. Je ne publie volontairement aucune capture du jeu ni ressource Nintendo sur ce dépôt.

### Les prochaines grandes étapes

1. **Rendre Vulkan autonome et suffisamment rapide**, sans dépendre de la rastérisation logicielle.
2. **Créer une vraie fenêtre Linux**, avec clavier, souris et manette utilisables en direct.
3. **Adapter toutes les actions indispensables de la Wiimote** — orientation horizontale ou verticale, visée, pointeur, concentration et gestes — aux périphériques PC, avec des touches personnalisables.
4. **Valider une véritable séquence de jeu contrôlable**, avec graphismes, son et réactions immédiates aux commandes.
5. Poursuivre les comparaisons avec Dolphin, améliorer la compatibilité générale et préparer le portage Windows.

## Comment je vérifie les avancées

J'utilise **Dolphin comme référence de comparaison**, et non comme moteur final. La correspondance de certaines images, de certains extraits sonores et des données internes permet de vérifier progressivement le fonctionnement natif. Des scripts reproductibles servent aussi à distinguer un mouvement causé par une commande d'une animation automatique.

Les résultats ci-dessus proviennent du **rapport Claude Code du 2 novembre 2026**. Je ne les ai pas reproduits indépendamment pour cette mise à jour de la documentation. Des tests réussis et un déplacement de Samus sous script **ne prouvent pas encore que tout le jeu fonctionne**.

## Téléchargement et sortie

Je prévois de publier un lanceur sur la page [GitHub Releases du projet](https://github.com/Petrus-Canisius/Metroid-Other-M-Native-HD/releases) lorsqu'il sera prêt. La future installation nécessitera **une copie personnelle de *Metroid: Other M***. Aucune date de sortie ni configuration minimale n'est annoncée.

Le [journal de développement](docs/DEVELOPMENT_LOG.md), la [feuille de route](docs/ROADMAP.md) et la [FAQ](docs/FAQ.md) présentent l'avancement et les limites connues.

## Indépendance et droits

*Metroid: Other M Native HD* est un projet de fan indépendant, non soutenu ni approuvé par Nintendo. Les droits sur le jeu et les marques concernées appartiennent à leurs détenteurs.

**Ce dépôt public contient uniquement de la documentation.** Il ne distribue ni image du jeu, ni code traduit, ni ressources d'origine, ni clés, ni captures privées, ni exécutable jouable. Certains composants du runtime privé reprennent du code de projets libres, dont Dolphin ; une future distribution devra respecter leurs licences et les obligations correspondantes de mise à disposition du code source. Voir les [informations sur les licences](docs/FAQ.md#licensing).
