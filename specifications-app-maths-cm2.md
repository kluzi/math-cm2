# Spécifications fonctionnelles — App Maths CM2

2026-09-16 · @Someone · mis à jour le 2026-09-18 (reflète l'état réel de l'app après développement du Chapitre 1) · mis à jour le 2026-09-24 (refonte du barème d'étoiles et des récompenses) · mis à jour le 2026-09-24 (validation/refus/annulation des demandes de récompenses, limite hebdomadaire par récompense) · mis à jour le 2026-09-24 (Chapitre 2 "Aires" jouable, nouveaux types d'exercices géométriques)

Application web autonome (HTML, iPad/Safari) pour l'entraînement quotidien en maths, basée sur le manuel *Maths au CM2* (Accès éditions), en suivant la progression de la classe.

> **État actuel** : le moteur complet est développé. Le **Chapitre 1** ("Nombres entiers jusqu'à 999 999") et le **Chapitre 2** ("Aires") sont entièrement jouables. Les chapitres 3 à 35 existent dans la navigation mais affichent "Contenu à venir" tant que leurs photos n'ont pas été envoyées. L'enfant s'appelle **Nola** (prénom codé en dur dans l'app, cf. §1).

## 1. Vue d'ensemble et principes

- Appli web **autonome en un seul fichier HTML** (`index.html`, pas de serveur, pas de compte), fonctionnant hors ligne — CSS et JS inline, aucune dépendance externe (pas de police ou script chargé depuis internet)
- Accès via une icône en favori sur l'écran d'accueil de l'iPad (Safari → Partager → Sur l'écran d'accueil) ; l'icône (`icon.png`) reproduit le badge blanc à la règle de l'écran de démarrage sur fond diagonal bleu/or
- Un seul profil utilisateur, prénom **Nola** codé en dur (pas de gestion multi-enfants, pas d'écran de réglage de prénom en v1)
- Contenu basé sur le manuel *Maths au CM2* (Accès éditions), construit chapitre par chapitre à partir de photos envoyées par le parent
- Objectif : entraînement quotidien court et ludique, aligné sur la progression réelle de la classe
- Dépôt Git dédié au projet (dossier `Maths CM2`), distinct du dépôt racine de l'utilisateur

## 2. Structure de navigation et progression

- **Périodes** : suivent la découpe du manuel (Période 1 à 5)
- **Accueil en accordéon** : seule la période **en cours** (celle du premier chapitre non maîtrisé) est dépliée par défaut, avec un badge doré "EN COURS" ; les autres périodes sont repliées (chevron cliquable pour les ouvrir/fermer). Une période entièrement maîtrisée affiche un badge vert **"ACQUIS"** à la place
- **Chapitres** : 35 au total, numérotés comme dans le manuel, regroupés par domaine (Nombres et Calculs, Grandeurs et Mesures, Espace et Géométrie)
- **Déblocage progressif** : un chapitre n'est jouable qu'une fois le(s) précédent(s) de la période validé(s)
- **Validation d'un chapitre** : score minimum de 80% atteint sur chacun des 4 blocs (voir section 3)
- **Accès libre** : une fois un chapitre débloqué, il reste accessible à tout moment, sans limite de session ni horaire imposé
- **Rejouer** : un bloc déjà validé peut être refait à volonté pour s'entraîner ; le meilleur score est conservé
- **Affichage du statut d'un chapitre** sur l'accueil :
  - non commencé et débloqué → nombre de blocs jouables (ex. "4 exercices disponibles")
  - entamé mais non maîtrisé → % de réussite moyen des blocs déjà joués (ex. "59% de réussite")
  - maîtrisé → "Maîtrisé" + icône trophée
  - verrouillé → icône cadenas fermé

## 3. Structure d'un chapitre : les 4 blocs

Chaque chapitre contient 4 blocs, jouables dans l'ordre choisi par l'enfant :

| Bloc | Contenu | Chrono |
| --- | --- | --- |
| Leçon | Exercices d'entraînement de la notion + QCM récapitulatif "Je sais" | Non |
| Flash Maths | Notions transversales rapides (le Chapitre 1 est entièrement composé de droite graduée ; les autres sous-types du manuel — calcul astucieux, vrai/faux, lecture de l'heure, solides — seront ajoutés si un chapitre futur les contient) | Oui |
| Calcul | Calcul mental et instrumenté, repris **séance par séance** telles que découpées dans le manuel (ex. Chapitre 1 : Séances 1 à 4, puis une partie "Poser les opérations" pour les additions/soustractions posées) | Selon l'exercice, via un chrono par question dont la durée est fixée par bloc |
| Atelier problèmes | Énoncés contextualisés (problèmes "vraie vie"), avec des valeurs numériques modifiées par rapport au manuel (voir §9) | Non |

- Un chapitre est marqué "maîtrisé" quand les 4 blocs atteignent 80%
- Le libellé "Séance X" affiché pendant une session Calcul correspond au découpage du manuel, pas à une action à effectuer par l'enfant — c'est juste un repère visuel
- **Longueur de session plafonnée à 20 questions** par bloc, tirées aléatoirement dans la banque d'exercices du chapitre si elle en contient davantage (ex. Calcul, Chapitre 1 : 63 exercices dans la banque → 20 tirés par session) — pour que toutes les sessions durent à peu près le même temps, quel que soit le bloc

## 4. Types d'exercices et interactions

Types réellement implémentés dans le moteur (le Chapitre 1 n'a pas eu besoin de tous les types envisagés initialement — l'horloge analogique et l'identification visuelle seront ajoutés si un futur chapitre en a besoin) :

- **Saisie numérique libre** (`numeric`) : réponse tapée au clavier numérique — utilisé aussi pour les problèmes avec énoncé
- **Encadrement à un trou** (`range`) : ex. "69 999 < ? < 70 001"
- **Encadrement à deux trous** (`compound2`) : ex. "Encadre 20 490 par deux milliers consécutifs"
- **QCM classique** (`mcq`) : choix parmi plusieurs réponses
- **QCM multi-réponses** (`mcq_multi`) : plusieurs bonnes réponses possibles par question — utilisé pour le tableau récapitulatif "Je sais"
- **Vrai / Faux** (`true_false`)
- **Décomposition par étiquettes** (`decomposition`) : assembler des étiquettes (milliers/centaines/dizaines/unités) pour représenter un nombre — remplace les exercices à représentation visuelle 3D (cubes) du manuel
- **Rangement** (`order`) : toucher des nombres dans l'ordre croissant/décroissant (type ajouté en cours de développement, non prévu initialement)
- **Droite graduée** (`number_line`) : lire la valeur indiquée par une flèche sur un axe gradué ; seuls les repères de début et de fin affichent leur valeur, les graduations intermédiaires restent muettes pour ne pas trivialiser l'exercice
- **Rangement de figures** (`order_label`, ajouté pour le Chapitre 2) : variante de `order` où l'on range des figures (identifiées par une lettre) au lieu de nombres, en s'appuyant sur une aire cachée associée à chaque figure
- **Figure géométrique jointe** (`figure`, ajouté pour le Chapitre 2) : champ optionnel disponible sur `numeric`, `mcq`, `mcq_multi` et `order_label`, affichant une ou plusieurs figures dessinées en SVG sur un quadrillage (unité de mesure, légende, grille secondaire pour changer d'unité) au-dessus de la question — utilisé pour tous les exercices d'aires du Chapitre 2

Chaque exercice affiche un feedback immédiat (icône + couleur : vert/coche pour une bonne réponse, rouge/croix sinon) après la réponse, avec la bonne réponse et une courte explication en cas d'erreur.

**Variation des nombres à chaque partie** (ajouté après la première version) : pour éviter que Nola mémorise une réponse plutôt que la méthode, la plupart des exercices à réponse numérique (saisie libre, encadrements, droite graduée, décomposition) régénèrent des nombres aléatoires différents à chaque lancement du bloc — que ce soit la première fois ou en rejouant. La structure et le niveau de difficulté restent identiques, seuls les nombres changent. Sur le Chapitre 1, 93 des 110 exercices en bénéficient ; les QCM, le tri par ordre et le tableau récapitulatif "Je sais" restent fixes pour l'instant (risque de générer des mauvaises réponses incohérentes si randomisés). Sur le Chapitre 2, les exercices géométriques (figures sur quadrillage, comparaisons d'aires, tris de figures) sont fixes pour la même raison — seuls le Flash Maths (droite graduée décimale), le Calcul et l'Atelier problèmes varient à chaque partie.

**Figures géométriques du Chapitre 2** : les figures ne sont pas des reproductions pixel par pixel du manuel (dont on ne dispose pas du corrigé officiel) mais des figures originales, redessinées en SVG à partir de carrés unité assemblés (aires et disposition contrôlées avec précision à la conception), de même structure et niveau de difficulté que celles du manuel — cohérent avec le principe déjà appliqué aux problèmes contextualisés (§9).

## 5. Système de score, validation et récompenses

- **Score par bloc** : pourcentage de bonnes réponses, converti en étoiles (0 à 3, affichées sur les cartes de bloc et à l'écran de résultat) — seuils : <50% = 0★, 50-69% = 1★, 70-89% = 2★, 90-100% = 3★
- **Seuil de déblocage de chapitre** : 80% minimum sur les 4 blocs pour débloquer le chapitre suivant (seuil indépendant du barème d'étoiles ci-dessus, non modifié par la refonte du 2026-09-24)
- **Meilleur score conservé** : en cas de replay, seul le meilleur essai est retenu pour la progression
- **Série de jours (streak)** : compteur de jours consécutifs avec au moins une session jouée
- **Mode révision** : possibilité de rejouer uniquement les questions ratées d'un bloc ("Rejouer les erreurs")
- **Objectif hebdomadaire** : **40 minutes** cumulées sur la semaine *(changé depuis la v1 initiale, qui prévoyait 70 minutes)*, avec une jauge sur l'écran d'accueil dont la **couleur change selon la progression** : rouge (<25%), or (25-59%), bleu (60-99%), vert (100% atteint)
- **Réinitialisation hebdomadaire automatique** : le compteur (temps + %) repart à 0 chaque début de semaine (calcul par semaine ISO, lundi 00:00 — équivalent à quelques minutes près au "dimanche 23:59" prévu initialement)
- **Écran Progression** : affiche en plus le temps d'entraînement cumulé sur la semaine et le **temps d'entraînement total** (depuis le début, toutes semaines confondues)

### Monnaie virtuelle (étoiles)

*(Refonte du 2026-09-24 — remplace l'ancien système de bonus à la maîtrise de chapitre/période, jugé trop confus : l'enfant ne voyait pas le lien entre bien réussir un exercice et voir son solde augmenter.)*

Les étoiles gagnées à un exercice (voir barème ci-dessus) alimentent **directement** le solde utilisé pour les récompenses — un seul mécanisme, plus simple à comprendre pour l'enfant :

- Le solde n'augmente que lorsqu'un bloc atteint un **nouveau meilleur score** : le gain correspond à la différence entre les étoiles obtenues et les étoiles précédemment enregistrées pour ce bloc (ex. bloc jamais tenté → 70% obtenu → +2★ ; rejoué plus tard → 92% obtenu → +1★ de plus, car on passe de 2★ à 3★ pour ce bloc)
- Rejouer un bloc sans améliorer son record ne redonne pas d'étoile (pas de possibilité de farming)
- Le mode révision ("Rejouer les erreurs") ne compte jamais pour le record ni pour les étoiles
- **Calibrage** : visé pour environ 10 à 18 étoiles par semaine à 40 minutes de pratique hebdomadaire, une fois plusieurs chapitres disponibles. Avec un seul chapitre actuellement jouable (§10), le total gagnable est aujourd'hui plafonné à 12★ (4 blocs × 3★ max) jusqu'à l'ajout de nouveaux chapitres

### Catalogue de récompenses (barème du 2026-09-24)

| Coût | Récompense | Cadence visée |
| --- | --- | --- |
| 5 ★ | Choisir le film du vendredi soir | chaque semaine |
| 7 ★ | Un magazine | chaque semaine |
| 20 ★ | Un squishy | toutes les 2 semaines |
| 25 ★ | Un petit jouet | toutes les 2 semaines |
| 55 ★ | Un vêtement | chaque mois |
| 80 ★ | Une sortie spéciale (cinéma, parc...) | tous les 1 à 2 mois |

- Catalogue entièrement **éditable par le parent** (ajout, suppression, et **réordonnancement par glisser-déposer** via une poignée dédiée — au doigt sur iPad ou à la souris)
- **Demande** : l'enfant consulte son solde et peut "demander" une récompense une fois le seuil atteint (le coût est débité immédiatement) ; la demande apparaît dans une section "En attente de validation" en haut de l'écran, et la récompense disparaît du catalogue tant qu'elle est en attente (voir limite hebdomadaire ci-dessous)
- **Validation / refus par le parent** (protégé par le code parent, voir "Accès parent") : le parent peut **valider** une demande en attente (la récompense est considérée comme remise, les étoiles restent débitées) ou la **refuser** (les étoiles sont automatiquement recréditées à l'enfant et la demande annulée)
- **Annulation par l'enfant** : tant qu'une demande n'a pas été traitée par le parent, l'enfant peut l'annuler lui-même via un lien "Annuler ma demande" (sans code parent) — les étoiles sont recréditées, pour lui permettre de changer d'avis et choisir une autre récompense
- **Limite hebdomadaire** : une même récompense ne peut pas être redemandée plus d'une fois par semaine (semaine ISO, lundi 00:00), même si l'enfant a assez d'étoiles ; elle réapparaît dans le catalogue dès que la demande est annulée, refusée, ou dès la semaine suivante

### Accès parent (ajouté, non prévu initialement)

- La gestion du catalogue, ainsi que la **validation ou le refus d'une demande de récompense**, sont protégés par un **code parent à 4 chiffres** (écran de saisie façon code de vérification, 4 cases séparées, validation automatique à la 4ᵉ saisie, code erroné = animation + message)
- Dans cette même zone protégée, un bouton **"Réinitialiser le score et le temps"** permet au parent de tout remettre à zéro (progression, streak, temps de jeu, étoiles) avec une double confirmation ; le catalogue de récompenses configuré n'est pas affecté par cette réinitialisation

## 6. Charte graphique

- **Écran de démarrage** : visuel original inspiré de la couverture du manuel (bandeau diagonal bleu/or, badge blanc arrondi avec une icône de règle) ; pas de reproduction de l'illustration protégée de l'éditeur
- **Couleur dominante par chapitre** : suit la matière, comme dans le sommaire "par domaine mathématique" du manuel :
  - Rouge/rose = Nombres et Calculs
  - Bleu = Grandeurs et Mesures
  - Vert = Espace et Géométrie
- Cette couleur est constante sur les 4 blocs d'un même chapitre
- **Icônes** : toutes les icônes de l'app (navigation, statuts de chapitre, blocs, feedback, récompenses…) sont des icônes plates (flat design) dessinées sur mesure en SVG, dans le style et les couleurs de l'app — aucun emoji système utilisé, pour un rendu plus cohérent et adapté à un usage enfant
- **Typographie** : la police manuscrite/ronde envisagée initialement a été abandonnée après retours utilisateur ("trop ancienne") au profit d'une police système arrondie et moderne (SF Pro Rounded sur iPad, avec repli propre sur les autres navigateurs) pour les titres clés (accueil, "Récompenses", "Progression", titre de chapitre, badges de période)
- Style général enfant/ludique : cartes arrondies, feedback animé (icônes vertes/rouges, légère vibration sur mauvais code parent)
- **Mode paysage iPad** : au-delà de 820px de large en orientation paysage, la mise en page s'élargit et la liste des chapitres passe en grille à 2 colonnes

## 7. Écrans de l'application

1. **Démarrage / splash** : visuel d'accueil au chargement (badge à la règle sur fond bleu/or)
2. **Accueil** : carte "aujourd'hui" avec date complète (ex. "Jeudi 17 septembre 2026") et message de bienvenue personnalisé, adapté à **4 créneaux horaires** (matin <12h, midi 12h-14h, après-midi 14h-18h, soir 18h+) et au type de jour (semaine/week-end), plusieurs variantes tournantes par créneau ; + streak (icône calendrier) + solde d'étoiles + jauge hebdomadaire colorée ; périodes en accordéon avec état des chapitres (verrouillé / exercices disponibles / % de réussite / maîtrisé) et code couleur par matière
3. **Chapitre** : accès aux 4 blocs (Leçon, Flash Maths, Calcul, Atelier problèmes) avec leur meilleur score et leurs étoiles
4. **Session d'exercices** : déroulé des questions d'un bloc avec compteur "x / y", jauge de progression et minuteur (largeur fixe pour ne pas décaler la jauge pendant le décompte) ; bouton **Valider** (sombre) visuellement distinct du bouton **Continuer** (vert, avec flèche) affiché après la correction ; feedback immédiat par question
5. **Résultat** : score final, étoiles, icône contextuelle (trophée ≥80%, pouce levé 70-79%, pousse verte "continue de t'entraîner" <70%), récapitulatif des erreurs, option "Rejouer les erreurs"
6. **Récompenses** : solde d'étoiles, section "En attente de validation" (avec valider/refuser côté parent, et "Annuler ma demande" côté enfant), catalogue réordonnable par glisser-déposer et filtré des récompenses déjà demandées dans la semaine, bouton "Demander" une récompense, accès parent protégé par code (gestion du catalogue + réinitialisation + validation/refus des demandes)
7. **Progression** : chapitres maîtrisés, streak, temps cette semaine, temps d'entraînement total, taux de réussite par domaine, vue d'ensemble des 35 chapitres

## 8. Données et persistance

- Toutes les données (progression, scores, streak, historique des erreurs, étoiles, catalogue de récompenses) stockées en local sur l'iPad (localStorage)
- Aucune synchronisation entre appareils, aucun compte en ligne
- Aucune donnée envoyée à un serveur externe

## 9. Workflow de contenu (photos du livre)

- Le parent envoie les photos des pages d'un chapitre (format libre, au cas par cas selon le chapitre)
- Les exercices sont extraits et adaptés au format interactif de l'appli (voir types d'exercices, section 4)
- **Les valeurs numériques des énoncés (notamment les problèmes contextualisés) sont volontairement modifiées** par rapport à celles imprimées dans le manuel, pour que Nola ne puisse pas répondre de mémoire à un exercice déjà vu en classe — la structure et la difficulté du problème sont conservées à l'identique
- Le contenu s'enrichit progressivement, chapitre après chapitre, au fil des envois

## 10. Points ouverts / hors périmètre v1

- Pas de multi-enfants, pas de compte ni de synchronisation cloud
- Pas de vue "reporting" parent distincte, mais un accès parent protégé par code existe désormais pour la gestion du catalogue et la réinitialisation des données
- Notifications ou rappels quotidiens : non implémenté
- Contenu disponible : Chapitres 1 et 2 de la Période 1 ; les chapitres 3 à 35 sont en attente des photos correspondantes (structure de navigation déjà en place, affichage "Contenu à venir")
- Horloge analogique et identification visuelle : types d'exercices prévus au §4 mais pas encore implémentés dans le moteur, faute de contenu du Chapitre 1 en ayant besoin — à construire au moment où un chapitre futur les requiert
