# RULES — Revue académique généraliste via Consensus

> Fichier de règles pour les revues de littérature à large spectre,
> indépendantes d'une question clinique PICO précise.
> Outillage : outil Consensus (corpus > 200 millions de papers peer-reviewed).
> Domaines couverts : sciences de la santé, sciences comportementales,
> sciences de l'éducation, sciences sociales, technologie.

---

## 1. Déclenchement

Ce workflow s'applique aux requêtes de type :
- « Fais-moi une revue de littérature sur [sujet] »
- « Que sait-on sur [sujet] ? »
- « Quels sont les grands travaux sur [domaine] ? »
- Toute demande d'état de l'art sans PICO clairement délimité

> ⛔ Ce workflow **ne s'applique pas** aux revues cliniques ciblées avec PICO
> (→ utiliser `RULES-literature-review.md`) ni aux fiches de pathologie rare
> (→ utiliser `RULES-pathology_brief.md`).

---

## 2. Séquence d'exécution

### Phase 1 — RECON

Avant toute chose, lancer **une recherche large non structurée** sur le sujet :
- Objectif : apprendre la terminologie en usage, les sous-domaines actifs,
  les distinctions méthodologiques importantes, les groupes de recherche visibles
- Repérer les papers à haute citation dans les résultats — signal de références fondatrices
- Ne pas synthétiser à ce stade : cette passe informe uniquement la Phase 2

---

### Phase 2 — PLAN

Sur la base de la RECON, choisir un **framework de décomposition** et découper
le sujet en **4–5 sous-domaines** à explorer :

| Framework | Quand l'utiliser |
|---|---|
| **PICO** | Questions de santé, comportement, éducation, sciences sociales avec intervention identifiable |
| **SPIDER** | Questions qualitatives / expérience vécue, sans intervention clairement définie |
| **Décomposition** | Sujets technologiques — Mécanisme · Applications · Limites · Comparaisons |

**Présenter à l'utilisateur :**
1. Le framework retenu et la justification en une ligne
2. Les 4–5 sous-domaines identifiés
3. La question : *« Budget de recherche — scan rapide (5 requêtes) ou standard (10) ? »*

> ⛔ **Attendre la confirmation de l'utilisateur avant de lancer les recherches.**

---

### Phase 3 — SEARCH

Lancer les recherches **séquentiellement** (Consensus : limite 1 requête/sec).

**Allocation du budget :**

| Budget | Répartition |
|---|---|
| **Rapide (5)** | 1 recherche par sous-domaine |
| **Standard (10)** | 5 sous-domaines + 2 "systematic review / meta-analysis [sous-domaine]" sur les sous-domaines les plus actifs + 2 recherches datées sur le sous-domaine central (1 avec `year_max: 2015` pour le contexte historique, 1 avec `year_min: 2021` pour le front actif) + 1 suivi sur le paper le plus cité identifié jusqu'ici |

**Filtres à utiliser stratégiquement :**
- `year_min` / `year_max` — pour borner les ères
- `human` — pour les études cliniques
- `sample_size_min` — pour écarter les petits effectifs non représentatifs
- `sjr_max: 1` — pour cibler les revues de premier rang

**Tracking transversal — maintenir en continu sur l'ensemble des recherches :**

- **Papers récurrents** : un paper apparaissant dans 3+ recherches de sous-domaines distincts
  est probablement fondateur → le marquer 🔁
- **Auteurs récurrents** : identifier les groupes dominants (affiliation, sous-domaines couverts)
- **Signal citation** : ratio citations/an depuis publication — les papers anciens très cités
  sont souvent les références de fond même si datés

---

### Phase 4 — SYNTHÈSE

Produire la revue directement dans le chat, structurée en 6 sections obligatoires :

---

#### Section 1 — Vue d'ensemble du sujet

Un paragraphe dense :
- Ce qu'est le sujet
- Le framework utilisé et pourquoi
- La forme du paysage de preuves : où la littérature est robuste, où elle est lacunaire

---

#### Section 2 — Par où commencer — Ordre de lecture prioritaire

**5–7 papers ordonnés pour un lecteur qui découvre le domaine :**

1. Meilleure revue récente / méta-analyse (orientation la plus large)
2. Paper(s) fondateur(s) / séminal(aux)
3. 2–3 papers au front actuel de la recherche
4. Un paper mettant en évidence une lacune clé ou une controverse

**Pour chaque paper :**
- Titre (lien URL Consensus complet)
- Auteurs + année
- Une phrase sur ce qu'il apporte
- Une phrase sur ce à quoi être attentif en le lisant

---

#### Section 3 — Comment le domaine en est arrivé là

Court récit narratif + **tableau chronologique 5–8 lignes** :

| Année | Événement / Publication | Signification |
|---|---|---|
| | | |

Inclure les **glissements terminologiques** importants (ex. "gut flora" → "gut microbiome")
pour ne pas manquer la littérature ancienne.

---

#### Section 4 — Guides par sous-domaine

Une section par sous-domaine identifié en Phase 2, contenant chacune :

- **Ce que la recherche montre** (2–3 phrases avec citations inline)
- **3–5 papers clés** (lien Consensus, nb citations, année, une phrase sur leur apport)
- **6–10 termes de recherche** (synonymes, termes MeSH, termes historiques)
- **2–3 chaînes booléennes prêtes à coller**

---

#### Section 5 — Groupes de recherche clés

**Top 3–5 auteurs récurrents.** Pour chacun :
- Affiliation principale
- Sous-domaines couverts
- Un paper représentatif

---

#### Section 6 — Questions ouvertes et lacunes

Trois catégories, avec une ligne « pourquoi ça compte » pour chacune :

- **Lacunes méthodologiques** — designs faibles, échantillons insuffisants, mesures hétérogènes
- **Lacunes de population / contexte** — qui / quoi n'a pas été étudié
- **Lacunes conceptuelles / théoriques** — contradictions non résolues, mécanismes non testés,
  champs adjacents non intégrés

---

#### Bibliographie

Tous les papers cités, ordre alphabétique par premier auteur,
chacun avec son URL Consensus complète et cliquable.

---

## 3. Règles impératives

- **Ne citer que les papers réellement retournés par Consensus dans la session.**
  Toute référence issue des connaissances internes doit être explicitement labellisée
  `[hors Consensus]` et ne compte pas dans les décomptes.
- **Si une recherche retourne peu de résultats**, le signaler explicitement —
  ne pas combler silencieusement la lacune.
- **URLs Consensus complètes et non tronquées** pour chaque référence.
- **Signaler les papers récurrents** (🔁) — ils sont probablement incontournables.
- **Ne pas lancer Phase 3 sans confirmation explicite** de l'utilisateur sur le budget.

---

## 4. Langue

- Réponse en **français** par défaut
- Titres de papers et citations : conserver la langue originale (généralement anglais)
- Termes techniques : conserver la version anglaise entre parenthèses si la traduction
  française est ambiguë ou non standardisée

---

## 5. Ce workflow ne s'applique PAS à

- Revues cliniques avec PICO délimité (→ `RULES-literature-review.md`)
- Fiches de prise en charge d'une pathologie rare (→ `RULES-pathology_brief.md`)
- Calculs dosimétriques ou questions techniques pointues
- Modifications d'outils HTML existants

---

*v1.0 — Mai 2026. Adapté du prompt `literature-review-prompt.txt` (usage personnel R.T.)
Ajouts par rapport au prompt source : déclenchement explicite, règle d'attente de
confirmation Phase 2→3, section langue, délimitation par rapport aux autres RULES.*
