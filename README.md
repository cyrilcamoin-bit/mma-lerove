# MMA Interclubs — Le Rove 🥊

Application web de gestion d'interclubs MMA pour l'événement du **18 avril 2026** au gymnase du Rove (13740).

## Fonctionnalités

### Espace public (spectateurs / parents)
- Compte à rebours avant l'événement
- Formulaire d'inscription des combattants (nom, âge, poids, club, photo)
- **Suivi live** des combats en temps réel (filtrage par table ou recherche)
- Consultation des résultats et podiums par catégorie

### Espace administration
- Gestion complète des inscriptions (ajout, modification, suppression)
- Import / export CSV des combattants
- **Pesée** : vérification du poids et de la présence par catégorie
- **Génération automatique des poules** par tranche de poids avec algorithme anti-même-club
- Attribution des poules aux 4 tables d'arbitrage
- Monitoring live des 4 tables (combats en cours, chronomètre, progression)
- Gestion des mots de passe par rôle
- Consultation des résultats et podiums

### Espace table (arbitres)
- **Appel** des combattants (présent / absent)
- Liste des combats assignés à la table
- **Arbitrage** : chronomètre 3-4 min, sélection du vainqueur, validation du résultat
- Déblocage automatique des combats dépendants (brackets éliminatoires)

## Catégories d'âge

| Catégorie | Âge |
|-----------|-----|
| Poussin | 6 – 9 ans |
| Benjamin | 10 – 11 ans |
| Minime | 12 – 13 ans |
| Cadet | 14 – 15 ans |
| Junior | 16 – 17 ans |

## Formats de combat

- **Bye** — 1 combattant (exempt)
- **Aller-Retour** — 2 combattants, best of 3
- **Robin3** — 3 combattants, round-robin
- **Nordique4** — 4 combattants, 6 combats + barrage si égalité
- **Élim** — 5+ combattants, bracket éliminatoire + match pour la 3ᵉ place

## Stack technique

| Composant | Technologie |
|-----------|-------------|
| Frontend | HTML / CSS / JavaScript vanilla |
| Base de données | Firebase Realtime Database |
| Stockage local | localStorage |
| Polices | Bebas Neue, Barlow Condensed, Share Tech Mono |

## Lancer en local

```bash
# Serveur HTTP simple
python3 -m http.server 8080
```

Puis ouvrir **http://localhost:8080** dans le navigateur.

## Rôles & accès

| Rôle | Description |
|------|-------------|
| **Administration** | Accès complet (inscriptions, pesée, poules, monitoring, config) |
| **Table 1 – 4** | Interface d'arbitrage pour la table assignée |

## Structure du projet

```
index.html        # Application complète (HTML + CSS + JS)
README.md         # Ce fichier
PROPOSITIONS.md   # Propositions d'améliorations futures
```

## Firebase

- **Projet** : `mix-martial-academy`
- **Région** : `europe-west1`
- Synchronisation en temps réel entre toutes les tables et l'administration

## Licence

Projet privé — Mix Martial Academy.
