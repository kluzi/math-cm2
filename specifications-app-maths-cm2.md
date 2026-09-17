# Spécifications fonctionnelles — App Maths CM2

2026-09-16 · @Someone

Application web autonome (HTML, iPad/Safari) pour l'entraînement quotidien en maths, basée sur le manuel *Maths au CM2* (Accès éditions), en suivant la progression de la classe.

## 1. Vue d'ensemble et principes

- Appli web autonome en un seul fichier HTML (pas de serveur, pas de compte), fonctionnant hors ligne
- Accès via une icône en favori sur l'écran d'accueil de l'iPad (Safari)
- Un seul profil utilisateur (pas de gestion multi-enfants en v1)
- Contenu basé sur le manuel *Maths au CM2* (Accès éditions), construit chapitre par chapitre à partir de photos envoyées par le parent
- Objectif : entraînement quotidien court et ludique, aligné sur la progression réelle de la classe

## 2. Structure de navigation et progression

- **Périodes** : suivent la découpe du manuel (Période 1 à 5), affichées comme des sections successives
- **Chapitres** : 35 au total, numérotés comme dans le manuel, regroupés par domaine (Nombres et Calculs, Grandeurs et Mesures, Espace et Géométrie)
- **Déblocage progressif** : un chapitre n'est jouable qu'une fois le(s) précédent(s) de la période validé(s), pour suivre l'ordre du manuel et donc de la classe
- **Validation d'un chapitre** : score minimum de 80% atteint sur chacun des 4 blocs (voir section 3)
- **Accès libre** : une fois un chapitre débloqué, il reste accessible à tout moment, sans limite de session ni horaire imposé
- **Rejouer** : un bloc déjà validé peut être refait à volonté pour s'entraîner ; le meilleur score est conservé

## 3. Structure d'un chapitre : les 4 blocs

Chaque chapitre contient 4 blocs, jouables dans l'ordre choisi par l'enfant :

| Bloc | Contenu | Chrono |
| --- | --- | --- |
| Leçon | Exercices d'entraînement de la notion + QCM récapitulatif "Je sais" | Non |
| Flash Maths | Notions transversales rapides (droite graduée, calcul astucieux, vrai/faux, lecture de l'heure, solides) | Oui |
| Calcul | Calcul mental et instrumenté, en séances | Selon l'exercice |
| Atelier problèmes | Énoncés contextualisés (problèmes "vraie vie") | Non |

- Chrono variable selon l'objectif pédagogique de l'exercice (réflexe rapide vs réflexion posée), configuré exercice par exercice lors de la création du contenu
- Un chapitre est marqué "maîtrisé" quand les 4 blocs atteignent 80%

## 4. Types d'exercices et interactions

- **Saisie numérique libre** : réponse tapée au clavier numérique
- **QCM classique** : choix parmi plusieurs réponses
- **QCM tableau** : grille à choix multiples (plusieurs questions × plusieurs colonnes de réponses, comme la page "Je sais")
- **Vrai / Faux**
- **Droite graduée** : positionner ou lire une valeur sur un axe gradué
- **Horloge analogique** : lire ou régler un cadran interactif
- **Identification visuelle** : reconnaître une forme, un solide, une image parmi plusieurs
- **Problème avec énoncé** : texte + éventuelle illustration, réponse numérique ou QCM
- **Décomposition par étiquettes** : assembler des étiquettes (centaines/dizaines/unités, etc.) pour représenter un nombre — remplace les exercices à représentation visuelle 3D (cubes) du manuel, jugés trop complexes à reproduire fidèlement

Chaque exercice affiche un feedback immédiat (couleur/animation/son léger) après la réponse, avec la bonne réponse et une courte explication en cas d'erreur.

## 5. Système de score, validation et récompenses

- **Score par bloc** : pourcentage de bonnes réponses, converti en étoiles (1 à 3)
- **Seuil de déblocage** : 80% minimum sur les 4 blocs d'un chapitre pour débloquer le suivant
- **Meilleur score conservé** : en cas de replay, seul le meilleur essai est retenu pour la progression
- **Série de jours (streak)** : compteur de jours consécutifs avec au moins une session jouée
- **Mode révision** : possibilité de rejouer uniquement les questions ratées d'un bloc
- **Temps de jeu quotidien** : chrono cumulé du temps passé sur l'appli, objectif minimum de 10 minutes par jour
- **Objectif hebdomadaire** : 70 minutes cumulées sur la semaine, avec une barre/pourcentage de complétion affiché sur l'écran d'accueil
- **Réinitialisation** : le compteur hebdomadaire (temps + % de complétion) revient à 0 chaque dimanche à 23:59
- **Monnaie virtuelle** : les étoiles gagnées sur les blocs sont converties en une monnaie virtuelle cumulable
- **Catalogue de récompenses configurable par le parent** : chaque récompense a un coût en étoiles (ex. 3 étoiles = un magazine, 5 étoiles = un squishy), liste éditable et évolutive
- **Échange** : l'enfant consulte son solde et peut "demander" une récompense une fois le seuil atteint ; la remise de la récompense reste validée par le parent hors appli (pas d'achat réel géré par l'app)

## 6. Charte graphique

- **Écran de démarrage** : visuel original inspiré de la couverture du manuel (bandeau bleu, ambiance artisanale/manuelle — papier, crayon, horloge, ardoise) ; pas de reproduction de l'illustration protégée de l'éditeur
- **Couleur dominante par chapitre** : suit la matière, comme dans le sommaire "par domaine mathématique" du manuel :
  - Rouge/rose = Nombres et Calculs
  - Bleu = Grandeurs et Mesures
  - Vert = Espace et Géométrie
- Cette couleur est constante sur les 4 blocs d'un même chapitre
- Style général enfant/ludique : cartes arrondies, police manuscrite/ronde pour les titres, feedback animé

## 7. Écrans de l'application

1. **Démarrage / splash** : visuel d'accueil au chargement
2. **Accueil** : carte "aujourd'hui" avec message de bienvenue personnalisé au prénom de l'enfant, chaleureux et contextualisé (jour de la semaine, encouragement — ex. "Bonjour Nola, j'espère que ta journée d'école t'a appris plein de choses. On attaque la session du lundi ?"), adapté au jour de la semaine et à l'heure de connexion (formule différente le matin/midi/soir, semaine/week-end), plusieurs variantes tournantes par créneau pour éviter la répétition, + streak + suggestion + barre de progression hebdomadaire en minutes/%, liste des chapitres par période avec état (verrouillé / en cours / maîtrisé) et code couleur par matière
3. **Chapitre** : accès aux 4 blocs (Leçon, Flash Maths, Calcul, Atelier problèmes) avec leur score actuel
4. **Session d'exercices** : déroulé des questions d'un bloc, feedback immédiat par question
5. **Résultat** : score final, étoiles, récapitulatif des erreurs, option "rejouer les erreurs"
6. **Récompenses** : solde de monnaie virtuelle, catalogue des récompenses disponibles et leur coût, bouton "demander" une récompense
7. **Progression** : vue d'ensemble des 35 chapitres, statistiques (streak, taux de réussite par domaine)

## 8. Données et persistance

- Toutes les données (progression, scores, streak, historique des erreurs) stockées en local sur l'iPad (localStorage)
- Aucune synchronisation entre appareils, aucun compte en ligne
- Aucune donnée envoyée à un serveur externe

## 9. Workflow de contenu (photos du livre)

- Le parent envoie les photos des pages d'un chapitre (format libre, au cas par cas selon le chapitre)
- Les exercices sont extraits et adaptés au format interactif de l'appli (voir types d'exercices, section 4)
- Le contenu s'enrichit progressivement, chapitre après chapitre, au fil des envois

## 10. Points ouverts / hors périmètre v1

- Pas de multi-enfants, pas de compte ni de synchronisation cloud
- Pas de vue parent/reporting distincte prévue à ce stade
- Notifications ou rappels quotidiens : non spécifié, à discuter si souhaité
- Contenu limité à la Période 1 pour le lancement ; les périodes suivantes seront ajoutées au même rythme que l'envoi des photos
