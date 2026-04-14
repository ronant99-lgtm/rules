# RULES — Pathologies rares : synthèse clinique

> Fichier de règles pour standardiser les réponses aux requêtes de type
> « prise en charge d'une pathologie rare » (onco-radiothérapie, neuro-oncologie,
> oncologie pédiatrique et domaines adjacents).

---

## 1. Déclenchement

Ce workflow s'applique dès que la requête contient :
- une pathologie nommée **rare** (prévalence < 1/2 000 ou < 200 cas publiés)
- ou les mots-clés : « prise en charge », « algo », « conduite à tenir », « protocole »,
  « que faire avec… », « note sur… », « fiche sur… »

---

## 2. Séquence d'exécution obligatoire

### Étape 0 — Cadrage préalable (obligatoire avant toute recherche)

Avant de lancer quoi que ce soit, poser les questions de cadrage suivantes de façon
conversationnelle (pas de widget systématique, sauf ambiguïté majeure) :

1. **Contexte clinique** : pédiatrique ou adulte ? primitif ou récidive ? stade/extension ?
2. **Objectif d'utilisation** : RCP / note personnelle Obsidian / préparation présentation ?
3. **Références en main** : l'utilisateur dispose-t-il déjà d'articles, guidelines ou
   protocoles institutionnels pertinents ? Si oui, demander à les transmettre en pièce jointe
   ou en copie.

> ⛔ Ne pas lancer la recherche avant d'avoir reçu ces précisions si la question
> est ambiguë sur l'un de ces trois points. Si la question est suffisamment claire,
> le cadrage peut être allégé à 1–2 points seulement.

---

### Étape A — Vérification des connaissances internes
Avant toute recherche web, évaluer :
- La pathologie est-elle connue avec un niveau de confiance suffisant ?
- Les données sont-elles susceptibles d'avoir évolué dans les **5 dernières années** ?
- Existe-t-il une classification OMS récente (≥ 2021) à vérifier ?

---

### Étape B — Recherche web systématique
Effectuer **au minimum 3 recherches web** dans l'ordre suivant :

1. **PubMed / PMC** — séries rétrospectives et reviews (filtre : 2020–présent)
   - Requête type : `"[pathologie]" treatment series 2020 2021 2022 2023 2024 2025`
   - Cibler : séries ≥ 10 cas, méta-analyses, reviews systématiques

2. **Guidelines sociétés savantes** — ESMO, ASCO, NCCN, SIOPE, EANO, SFRO selon domaine
   - Requête type : `"[pathologie]" guidelines ESMO OR NCCN OR SIOPE 2023 2024 2025`
   - Si aucune guideline spécifique : noter explicitement l'absence

3. **ClinicalTrials.gov** — essais ouverts ou récemment complétés
   - Requête type : `"[pathologie]" clinical trial recruiting 2024 2025 site:clinicaltrials.gov`
   - Mentionner les essais phase II/III pertinents si trouvés

4. **Demande de références complémentaires**
   Après les 3 recherches, si une guideline majeure a été identifiée mais est inaccessible
   en texte intégral (ex. NCCN derrière paywall, protocole SIOPE non public) :
   - **Signaler explicitement** : nom de la guideline, date, URL, et ce qui manque
   - Formuler la demande : *« J'ai identifié [nom guideline] ([année]) qui pourrait
     être pertinente mais je n'ai pas pu y accéder. Si tu en disposes, transmets-la
     pour que je l'intègre à la note. »*
   - Ne jamais synthétiser une guideline sur la seule base de son titre ou résumé
     sans le signaler explicitement.

---

### Étape C — Évaluation critique des sources
Pour chaque donnée retenue, qualifier :
- **Niveau de preuve** : utiliser la terminologie qualitative suivante
  - `Pas de standard` — absence de données prospectives
  - `Données de cas` — < 10 cas publiés
  - `Série rétrospective` — ≥ 10 cas, non randomisée
  - `Guideline de société savante` — recommandation formelle
  - `Essai prospectif` — phase I/II/III
- **Date des données** : signaler si la référence la plus récente date de > 5 ans
- **Conflit entre sources** : mentionner explicitement si deux séries majeures divergent

---

## 3. Structure de la réponse conversationnelle

Produire une **note structurée complète** dans le chat, organisée comme suit.
Chaque section doit être présente ; indiquer « Données insuffisantes » si vide.

```
### [NOM PATHOLOGIE] — Note clinique [ANNÉE]

#### 1. Définition & contexte
- Entité OMS (grade, classification, année de reconnaissance)
- Cellule d'origine présumée
- Incidence / prévalence (ordre de grandeur)
- Âge de survenue typique

#### 2. Bilan initial & diagnostic
- Imagerie recommandée
- Biologie / marqueurs
- Anatomopathologie / IHC / biologie moléculaire
- Diagnostic différentiel principal

#### 3. Protocoles existants
- Protocoles nationaux/internationaux identifiés (nom, promoteur, statut)
- Essais cliniques ouverts (NCT, phase, critères principaux)
- Mention si aucun protocole spécifique disponible

#### 4. Traitement
- Chirurgie
- Radiothérapie (dose, volume, fractionnement)
- Chimiothérapie / thérapies ciblées / immunothérapie
- Séquence et combinaisons
- ⚠ Signaler systématiquement les traitements sans preuve solide

#### 5. Pronostic & survie
- Survie globale (5 ans si disponible)
- Facteurs pronostiques identifiés
- Taux de récidive

#### 6. Algorithme décisionnel
- Présenter sous forme de tableau ou de schéma textuel étape par étape
- Inclure les points de décision clés (résidu, extension, grade)

#### 7. Références clés
- Format : Auteur et al. Titre abrégé. Journal Année;vol:pages.
- Limiter à 5–8 références les plus impactantes
- Toujours inclure la référence OMS si pertinente
- Indiquer le niveau de preuve entre crochets : [série rétrospective], [guideline], etc.
```

---

## 4. Honnêteté sur les limites

Règles impératives :
- **Ne jamais inventer de références.** Si une donnée est issue d'une analogie
  avec une pathologie voisine, le dire explicitement.
- **Signaler l'incertitude** sur les schémas de chimiothérapie si non confirmés
  par une recherche web récente.
- Utiliser la formule : *« Ces données reposent sur [n] cas publiés —
  niveau de preuve faible. Une RCP nationale/internationale est recommandée. »*
- Pour les pathologies avec < 50 cas publiés dans la littérature mondiale :
  ajouter un encadré d'avertissement en tête de note.

---

## 5. Outil HTML — Fiche de synthèse statique stylisée

Après la note conversationnelle, **demander confirmation à l'utilisateur** avant
de générer la fiche HTML. Ne jamais générer la fiche automatiquement.
Formule suggérée : *« Souhaites-tu que je génère la fiche HTML statique correspondante ? »*

Si la pathologie est trop peu documentée (< 20 cas publiés) : le signaler et ne
pas proposer la fiche.

### Structure HTML — layout et ordre des sections

**Layout général :**
- Grille 2 colonnes sur desktop, 1 colonne sur mobile (CSS grid, `grid-template-columns: 1fr 1fr`)
- Colonne gauche : Définition → Bilan → Protocoles/Essais
- Colonne droite : Traitement → Algorithme → Pronostic
- Références en pleine largeur en bas de fiche
- Alertes visuelles regroupées en bandeau **en haut de fiche**, avant toute section

**Ordre fixe des sections :**
1. *(Bandeau alertes si applicable)*
2. Définition & épidémiologie
3. Bilan diagnostique
4. Traitement par modalité *(tableaux)*
5. Algorithme décisionnel *(étapes numérotées)*
6. Pronostic *(pills / chiffres clés)*
7. Protocoles & guidelines
8. Références *(liste numérotée, pleine largeur)*

**Alertes visuelles** — bandeau en haut, icône + texte court :
- `⚠ Pas de consensus` — absence de standard
- `🏥 RCP recommandée` — orientation nationale conseillée
- `📋 Données limitées` — données de cas uniquement

**Badges niveau de preuve sur les références** (colorés) :
- 🟢 Guideline / essai prospectif
- 🟡 Série rétrospective
- 🔴 Données de cas / < 10 cas

**En-tête de fiche :**
- Nom de la pathologie (DM Serif Display, grand)
- OMS grade + année de classification
- Date de la note + mention « Confirmation utilisateur requise avant mise à jour »

### Design system

- Typographie : **DM Serif Display** (titres) / **DM Sans** (corps) / **DM Mono** (doses, chiffres clés)
- Palette : navy/slate sombre par défaut, warm parchment en light mode
- Toggle dark/light : `◑` / `☀`, persistance `localStorage`
- Viewport : `maximum-scale=5.0`
- **Avant de générer le HTML** : tenter de récupérer le RULES-clinical-tools en vigueur
  (`https://raw.githubusercontent.com/ronant99-lgtm/rules/main/RULES-clinical-tools.md`)
  pour appliquer les conventions exactes du design system. Si inaccessible, appliquer
  les conventions ci-dessus par défaut.

### À NE PAS faire
- Ne pas générer la fiche sans confirmation explicite de l'utilisateur
- Ne pas inclure de formulaires ou d'entrées utilisateur dans la fiche statique
- Ne pas placer les alertes en bas de fiche ou à l'intérieur des sections

---

## 6. Langue

- Réponse en **français** par défaut
- Références bibliographiques dans leur langue originale (anglais)
- Termes anatomopathologiques et noms de protocoles : conserver la version
  anglaise entre parenthèses si la traduction française est ambiguë

---

## 7. RCP et orientation nationale

Pour toute pathologie rare en oncologie (France) :
- Mentionner systématiquement le réseau national pertinent si connu :
  - Neuro-oncologie : **RENOCLIP-LOC**
  - Tumeurs rares pédiatriques : **SFCE / réseau FRACTAL**
  - Sarcomes : **NetSarc / RRePS**
  - Autres : mentionner l'existence d'un recours national sans inventer le nom
- Rappeler que la RCP nationale est recommandée pour tout cas hors situation standard

---

## 8. Ce workflow ne s'applique PAS à

- Questions portant sur une pathologie fréquente (ex. cancer du sein, NSCLC)
- Demandes de calcul dosimétrique pur
- Requêtes de modification d'outils HTML existants
- Revues de littérature ciblées sur une question précise (ex. « quel BED pour… »)

---

*v0.3 — Avril 2026. Ajouts : cadrage préalable (Étape 0), demande de références
complémentaires (Étape B4), structure HTML détaillée (layout, ordre, badges, alertes).*
