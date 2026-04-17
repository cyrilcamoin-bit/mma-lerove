# Propositions d'amélioration — MMA Interclubs Le Rove

## 🔴 Priorité Haute — Sécurité

### 1. Authentification côté serveur
Les mots de passe sont hashés et vérifiés côté client (SHA-256 dans le navigateur). N'importe qui peut lire les hashes dans le code source ou les contourner via la console.
- **Proposition** : Déplacer l'authentification vers Firebase Authentication (email/password ou anonymous + custom claims pour les rôles admin/T1-T4).
- **Impact** : Empêche tout accès non autorisé, même en inspectant le code.

### 2. Règles de sécurité Firebase
Actuellement, la base Firebase semble accessible en lecture/écriture sans restriction. Un utilisateur malveillant peut modifier les résultats directement.
- **Proposition** : Configurer des Firebase Security Rules strictes :
  - Lecture publique limitée aux données de suivi live
  - Écriture sur les combats réservée aux tables authentifiées
  - Écriture sur la config réservée à l'admin

### 3. Protection des données personnelles (RGPD)
Photos, noms, âges et clubs sont stockés sans mécanisme de consentement explicite ni politique de suppression.
- **Proposition** : Ajouter une case de consentement RGPD lors de l'inscription, permettre la suppression des données après l'événement, limiter la durée de stockage des photos.

### 4. Validation des données d'entrée
L'import CSV et le formulaire d'inscription n'ont pas de validation robuste (doublons, formats malformés, taille des photos).
- **Proposition** : Vérifier les doublons (nom + prénom + club), limiter la taille des photos (ex : 500 Ko), valider les champs CSV à l'import.

---

## 🟠 Priorité Moyenne — Fiabilité & Données

### 5. Sauvegarde et restauration
Un reset accidentel de la compétition est irréversible. Aucun système de backup automatique.
- **Proposition** : Sauvegarde automatique horodatée dans Firebase (snapshot toutes les 15 min), bouton de restauration avec sélection du point de sauvegarde.

### 6. Journal d'audit
Aucune traçabilité sur qui a fait quoi (validation de résultat, modification d'inscription, reset).
- **Proposition** : Logger les actions critiques avec timestamp + rôle (ex : `T2 a validé combat #14 à 15:32`). Afficher un historique dans l'admin.

### 7. Mode hors-ligne (PWA)
Si le WiFi du gymnase tombe, les tables perdent la synchronisation.
- **Proposition** : Transformer l'app en Progressive Web App avec Service Worker. Les actions se mettent en file d'attente et se synchronisent au retour du réseau.

### 8. Gestion des forfaits et matchs nuls
Seuls victoire/défaite sont gérés. Pas de forfait, abandon ou match nul.
- **Proposition** : Ajouter des options "Forfait", "Abandon", "Match nul" dans l'interface d'arbitrage.

---

## 🟡 Priorité Moyenne — UX / Fonctionnalités

### 9. Détection de doublons à l'inscription
Un même combattant peut s'inscrire plusieurs fois.
- **Proposition** : Vérification automatique par nom + prénom + date de naissance. Alerte si un combattant similaire existe déjà.

### 10. Notifications aux arbitres
Quand l'admin assigne de nouvelles poules à une table, l'arbitre ne reçoit aucune notification.
- **Proposition** : Notification sonore + visuelle sur l'interface table quand une nouvelle poule est assignée.

### 11. Visualisation des brackets en arbre
Les combats éliminatoires (5+ combattants) sont affichés en liste. Difficile de suivre la progression.
- **Proposition** : Afficher un arbre de bracket visuel (type tournoi) pour les poules éliminatoires.

### 12. Mode affichage spectateur / écran externe
Pas de vue optimisée pour un grand écran dans le gymnase.
- **Proposition** : Créer un mode "Scoreboard" plein écran avec les combats en cours, chronomètre géant, et résultats récents. Adapté pour vidéoprojecteur.

### 13. Statistiques admin enrichies
Pas de vue d'ensemble sur l'avancement global (taux d'inscription, progression par catégorie, temps moyen par combat).
- **Proposition** : Dashboard avec graphiques : répartition par catégorie/genre, progression des combats, temps moyen, taux de présence à la pesée.

---

## 🔵 Priorité Basse — Qualité du code

### 14. Découpage du code
Tout le code (HTML + CSS + JS, ~4200 lignes) est dans un seul fichier `index.html`.
- **Proposition** : Séparer en fichiers distincts :
  - `styles.css` — styles
  - `app.js` — logique principale
  - `firebase.js` — sync Firebase
  - `modules/inscriptions.js`, `modules/pesee.js`, `modules/combats.js`, etc.
- Utiliser un bundler léger (Vite) pour le build.

### 15. Variables globales
30+ variables globales (`COMBATTANTS`, `FIGHTS`, `POOLS`, `PM`, `PD`, etc.) polluent le scope.
- **Proposition** : Encapsuler l'état dans un objet unique `AppState` ou utiliser un store simple.

### 16. Paramétrage de l'événement
La date, le lieu, les horaires et les catégories d'âge sont codés en dur.
- **Proposition** : Fichier de configuration `config.json` ou section admin pour paramétrer l'événement (nom, date, lieu, catégories, durées de combat) sans toucher au code.

### 17. Tests
Aucun test unitaire ou d'intégration.
- **Proposition** : Ajouter des tests pour les fonctions critiques : génération des poules, algorithme anti-même-club, calcul des brackets éliminatoires, classement nordique.

---

## 💡 Idées bonus

| Idée | Description |
|------|-------------|
| **QR Code d'accès** | Générer un QR code par table/rôle pour connexion rapide le jour J |
| **Export PDF résultats** | Générer un PDF des résultats et podiums pour chaque club |
| **Multi-événements** | Gérer plusieurs compétitions depuis la même app |
| **Thème clair** | Option de basculer en thème clair pour lisibilité en extérieur |
