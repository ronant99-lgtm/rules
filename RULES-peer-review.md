# RULES-peer-review.md
# Règles personnelles pour la revue critique d'articles scientifiques
# Ronan — Oncologue radiothérapeute, Institut Léon Bérard, Lyon
# Spécialités : neuro-oncologie, SRS/SBRT, métastases, pédiatrie
# Version 1.0 — Avril 2026

---

## Contexte et usage

Ce fichier gouverne les sessions de peer review assistées par Claude. Il s'applique lorsque :
- Je soumets un article (PDF ou texte) pour review en tant que referee pour un journal
- Je demande une lecture critique structurée d'un manuscrit

**Discipline cible :** radio-oncologie clinique et translationnelle — essais cliniques de phase II/III, études rétrospectives et de cohorte. Les études dosimétrique pures ou de planification ne relèvent pas de ce workflow (règles spécifiques à définir).

**Langue de travail :** la review peut être produite en français ou en anglais selon le journal cible. Par défaut : français pour les échanges, anglais pour le rapport final exportable.

---

## Workflow en 5 étapes

### Étape 1 — Lecture initiale et cadrage (rapid scan)

Avant toute analyse détaillée, répondre à ces questions de cadrage :

- Quelle est la question clinique centrale ? Est-elle clairement formulée ?
- Quel est le design de l'étude (RCT, cohorte prospective, rétrospective, registre) ?
- Quelle est la guideline de reporting applicable ? (→ voir §Reporting Standards)
- Les conclusions principales sont-elles en accord avec les données présentées ?
- Y a-t-il un défaut majeur rédhibitoire (design fondamentalement inadapté à la question, endpoint primaire non atteint mais conclusions positives, n insuffisant sans discussion) ?

**Sortie :** 3–4 phrases de synthèse initiale + signal de gravité globale (Minor revisions / Major revisions / Reject).

---

### Étape 2 — Analyse section par section

#### Titre et Abstract
- Le titre est-il précis, non sensationnaliste, et reflète-t-il fidèlement les résultats ?
- L'abstract contient-il tous les éléments PICO (Population, Intervention, Comparaison, Outcome) ?
- Les conclusions de l'abstract sont-elles strictement supportées par les résultats ?
- ⚠️ Red flag : abstract positif avec résultats borderline (ex : p=0.04 sur endpoint secondaire)

#### Introduction
- La rationale est-elle bien construite et ancrée dans la littérature récente ?
- L'hypothèse est-elle explicitement formulée (et non rétro-construite) ?
- Les limites des études antérieures justifient-elles cette nouvelle étude ?

#### Méthodes
Points à vérifier systématiquement :

**Design général**
- [ ] Design clairement défini et justifié pour la question posée
- [ ] Période d'inclusion et de suivi précisées
- [ ] Centre(s) impliqué(s), caractère mono/multicentrique
- [ ] Approbation éthique / IRB mentionnée
- [ ] Enregistrement sur ClinicalTrials.gov ou équivalent (obligatoire pour essais)

**Population**
- [ ] Critères d'inclusion/exclusion explicites
- [ ] Caractéristiques de base (Table 1) disponibles
- [ ] Flux de patients (flowchart CONSORT ou équivalent)
- [ ] Potentiel biais de sélection discuté

**Intervention / Traitement**
- ⚡ **Spécifique radio-oncologie :** technique de RT précisée (3D-CRT, IMRT, VMAT, SRS, SBRT), dose totale, fractionnement, volume cible (GTV/CTV/PTV), contraintes OAR — ces éléments doivent être suffisamment détaillés pour être reproductibles
- Traitements concomitants ou séquentiels décrits
- Critères de modification/arrêt de traitement

**Endpoints**
- [ ] Endpoint primaire clairement défini a priori (et pas post-hoc)
- [ ] Définitions précises (ex : OS, PFS, LC — à partir de quand ? par quelle méthode ?)
- [ ] Endpoints secondaires hiérarchisés
- [ ] Toxicité : échelle utilisée (CTCAE version ?), fenêtre de collection, méthode de recueil

**Statistiques**
- [ ] Calcul de puissance ou justification de taille d'échantillon
- [ ] Test statistique principal pré-spécifié
- [ ] Méthode d'estimation des courbes de survie (Kaplan-Meier standard)
- [ ] Test de comparaison (log-rank, Cox) avec hypothèses vérifiées
- [ ] Gestion des données manquantes explicitée
- [ ] Analyses de sous-groupes : pré-spécifiées ou exploratoires ? Correction pour tests multiples ?
- [ ] Logiciel statistique et version mentionnés

#### Résultats
- [ ] Tous les endpoints pré-spécifiés rapportés (pas de selective reporting)
- [ ] IC 95% systématiquement fournis avec les estimations ponctuelles
- [ ] Médiane de suivi précisée avec méthode (inverse KM préférable)
- [ ] Taille des groupes cohérente entre méthodes et résultats
- [ ] Toxicités rapportées exhaustivement (y compris tardives si applicable)
- ⚠️ Red flag : p-values regroupées en "p<0.05" sans valeurs exactes ; courbes de survie sans table des sujets à risque

#### Discussion
- [ ] Conclusions strictement supportées par les données (pas d'overreach causal depuis données rétrospectives)
- [ ] Limites discutées honnêtement et spécifiquement (pas de section "limitations" générique)
- [ ] Positionnement par rapport à la littérature pertinente (études similaires citées)
- [ ] Pertinence clinique discutée au-delà de la significativité statistique
- ⚠️ Red flag : claims de supériorité depuis un design single-arm ou non-comparatif

#### Références
- Références clés présentes et récentes
- Auto-citation excessive ou déséquilibre flagrant ?

---

### Étape 3 — Checklist reporting standards

Identifier la guideline applicable et vérifier sa compliance :

| Type d'étude | Guideline | Éléments critiques |
|---|---|---|
| RCT | **CONSORT** | Flowchart participants, allocation concealment, ITT analysis, randomisation |
| Cohorte / Cas-témoins / Cross-sectionnel | **STROBE** | Définition des variables, gestion des biais, données manquantes |
| Revue systématique / Méta-analyse | **PRISMA** | Flowchart sélection, hétérogénéité (I²), biais de publication |
| Protocole d'essai | **SPIRIT** | Calendrier des évaluations, plan statistique complet |

Vérifier si les auteurs :
1. Mentionnent explicitement la guideline suivie
2. Ont soumis un checklist (souvent en supplément)
3. Respectent les items clés (flowchart, baseline table, etc.)

---

### Étape 4 — Détection des problèmes méthodologiques fréquents

#### En radio-oncologie spécifiquement

**Biais de sélection et de traitement**
- Sélection des patients pour SRS/SBRT vs. traitement standard : critères de sélection reflétés dans les analyses ?
- Dans les études rétrospectives : évolution des techniques dans le temps (learning curve, changement de technique) — effet temporel contrôlé ?
- Biais d'indication dans les comparaisons RT ± systémique

**Endpoints et définitions**
- Local control (LC) : définition précise (critères RANO, RECIST, critères spécifiques SRS) ? Imagerie de suivi standardisée ?
- Différenciation progression tumorale vs. radionécrose : critères, méthode (imagerie fonctionnelle, biopsie) ?
- OS censuré à la date de dernières nouvelles ou à une date fixe ?

**Statistiques de survie**
- Competing risks ignorés ? (ex : décès sans récidive locale dans un contexte métastatique → Fine-Gray plus approprié que KM standard)
- Landmark analysis pour éviter immortal time bias dans les études avec traitement séquentiel ?
- Confusion entre suivi médian et survie médiane

**Toxicité**
- Toxicité rapportée seulement pour les patients ayant terminé le traitement (survivorship bias) ?
- Fenêtre de suivi pour les toxicités tardives suffisante ?

#### Issues statistiques génériques prioritaires

1. **P-hacking / selective reporting** — proportion suspecte de p juste sous 0.05 ; endpoints secondaires "significatifs" promus sans correction
2. **Test inapproprié** — données appariées analysées en non-apparié ; données de survie analysées en Student
3. **Pseudoréplication** — plusieurs lésions par patient comptées comme observations indépendantes (très fréquent en SRS multi-lésions)
4. **Analyses de sous-groupes non pré-spécifiées** — présentées comme exploratoires mais conclues comme définitives
5. **Données manquantes non gérées** — exclusions implicites sans discussion de l'impact potentiel
6. **Visualisation trompeuse** — courbes KM sans table at-risk ; axe Y tronqué exagérant les différences

---

### Étape 5 — Rédaction du rapport de review

#### Structure du rapport (format journal referee)

```
SUMMARY
[3–5 phrases : question, design, résultats principaux, impression générale,
 recommandation : Accept / Minor revision / Major revision / Reject]

MAJOR CONCERNS
[Numérotés, justifiés, avec référence à la section concernée.
 Chaque point = 1 problème + 1 demande précise et actionnable.
 Ordre : validité interne > reporting > interprétation]

MINOR CONCERNS
[Idem, pour les points qui n'affectent pas la validité mais la clarté,
 la complétude ou la rigueur formelle]

COMMENTS TO THE AUTHORS
[Optionnel : remarques constructives non bloquantes, suggestions
 d'analyses complémentaires, points de discussion clinique]
```

#### Principes de rédaction
- Ton : rigoureux mais constructif. Pas de formulations agressives.
- Chaque critique majeure doit être actionnable : "les auteurs devraient..." plutôt que "c'est incorrect"
- Distinguer ce qui est **obligatoire** (Major) de ce qui est **souhaitable** (Minor)
- Ne pas demander des expériences supplémentaires si l'étude répond à sa propre question dans ses limites
- La confidentialité : ne pas identifier le reviewer dans le texte

#### Calibrage de la recommandation

| Recommandation | Critères |
|---|---|
| **Accept** | Pas de problème majeur, mineurs corrigeables à la marge |
| **Minor revision** | Quelques problèmes de clarté/reporting, pas de remise en cause des conclusions |
| **Major revision** | Problèmes méthodologiques ou de présentation substantiels mais corrigeables ; conclusions potentiellement valides |
| **Reject** | Design fondamentalement inadapté, biais non corrigibles, conclusions non supportées, ou question déjà répondue |

---

## Modes de travail

### Mode REVIEW COMPLÈTE
Déclenché par : *"review complète de cet article"*, *"fais-moi un rapport de review"*

→ Appliquer le workflow complet en 5 étapes, produire le rapport structuré complet en anglais.

### Mode LECTURE CIBLÉE
Déclenché par une question spécifique : *"analyse la méthode statistique"*, *"regarde les critères d'inclusion"*, *"est-ce que les conclusions sont justifiées ?"*

→ Répondre spécifiquement à la question posée, en mode "day science" (critique, factuel, sans extrapolation).

### Mode RÉSUMÉ CRITIQUE
Déclenché par : *"fiche de lecture"*, *"résumé critique"*

→ Produire une fiche Obsidian-compatible avec : Question clinique / Design / Résultats clés / Forces / Limites / Applicabilité ILB.

---

## Spécificités journal / calibrage

Avant de rédiger le rapport, si le journal cible est connu, adapter le niveau d'exigence :
- **Journals à fort impact** (JCO, IJROBP, Lancet Oncology, NEJM, Radiotherapy & Oncology) : exigence maximale sur design, stats, reproductibilité
- **Journals spécialisés de second rang** (Strahlentherapie, Clinical Oncology, Acta Oncologica) : exigence adaptée, focus sur la valeur incrémentale
- **Si journal non précisé** : appliquer les standards IJROBP comme référence de la spécialité

---

## Checklist finale avant envoi du rapport

- [ ] La recommendation est cohérente avec les concerns identifiés
- [ ] Chaque Major concern est justifié et actionnable
- [ ] Les Standards de reporting ont été vérifiés
- [ ] Les spécificités radio-oncologie (technique RT, endpoints, compétition des risques) ont été évaluées
- [ ] Le ton est constructif et professionnel
- [ ] Le rapport ne révèle pas l'identité du reviewer
- [ ] Le rapport est rédigé en anglais (sauf demande contraire)

---

*Fichier à stocker dans le vault Obsidian sous `/Rules/RULES-peer-review.md`*
*Compatible avec les workflows "day science" définis dans RULES-day-science.md*
