# Template — Recherche bibliographique clinique ciblée
*Usage personnel — Ronan T.*

---

## 0. Métadonnées de la session

| Champ | Valeur |
|---|---|
| **Date** | |
| **Thème général** | |
| **Durée estimée** | |
| **Statut** | ☐ En cours  ☐ Complété  ☐ À reprendre |

---

## A. Instructions pour Claude — À lire avant chaque session

*Cette section définit le comportement attendu de Claude pour toute recherche bibliographique conduite avec ce template.*

### A.1 Phase de cadrage (avant de lancer la recherche)

> ⛔ **RÈGLE ABSOLUE : Claude ne lance aucune recherche bibliographique avant d'avoir reçu les réponses aux questions de cadrage et avoir soumis le PICO reformulé à validation explicite. Cette règle s'applique même si la question semble claire en apparence.**

**Séquence obligatoire :**
1. Lire la question posée
2. Poser les questions de cadrage via widget (voir exemples ci-dessous)
3. **Attendre les réponses**
4. Reformuler le PICO et le soumettre à validation ("Voici comment je comprends la question — je peux lancer la recherche ?")
5. Lancer la recherche seulement après confirmation explicite

**Questions de cadrage systématiques :**
- Histologie et stade : "S'agit-il d'un CBNPC ou d'un CBPC ?"
- Contexte oligométastatique : "Synchrone / métachrone / oligoprogression ?"
- Systémique de référence : "ICI seul / chimio-ICI / TKI / tous ?"
- Ère thérapeutique : "Données contemporaines (post-2015) uniquement, ou contexte historique aussi ?"
- Objectif d'utilisation (voir A.5) : "RCP / enseignement / manuscrit ?"
- Focus RT : "Détails dosimétriques complets ou grands principes ?"

### A.2 Pendant la recherche

**Claude doit systématiquement :**
- **Signaler les articles inaccessibles** en texte intégral et proposer les alternatives (abstract, protocole NCT, version auteur sur PubMed Central). Ne jamais synthétiser un article sur la seule base du titre ou du résumé sans le signaler explicitement.
- **Maintenir le focus sur les détails RT** : pour toute étude impliquant une radiothérapie, rechercher et rapporter systématiquement — dose totale, nombre de fractions, dose par fraction, BED₁₀ (α/β = 10), technique, cibles irradiées, contraintes OAR si disponibles. Si ces données sont absentes de la publication principale (ex. abstract de congrès), le signaler et proposer d'accéder au protocole complet via ClinicalTrials.gov.
- **Distinguer abstract de congrès et publication complète** : signaler explicitement quand les données citées proviennent d'un abstract non encore publié en revue à comité de lecture.
- **Fournir systématiquement les liens** pour chaque essai ou publication cité :
  - Essais cliniques → lien direct `https://clinicaltrials.gov/study/NCT[XXXXXXXX]`
  - Publications → DOI (`https://doi.org/...`) ou lien PubMed (`https://pubmed.ncbi.nlm.nih.gov/[PMID]/`)
  - Protocoles complets → `https://cdn.clinicaltrials.gov/large-docs/[XX]/NCT[XXXXXXXX]/Prot_SAP_000.pdf`
  - En HTML : les liens sont intégrés directement dans les tableaux et les références
  - En Markdown : les liens sont ajoutés entre crochets à côté de chaque référence
- **Identifier les essais négatifs** avec la même rigueur que les essais positifs.

### A.3 Recommandations de lecture

Quand un article constitue une **référence incontournable** pour la question posée — essai pivot, analyse poolée influente, revue de synthèse de haute qualité — Claude le signale explicitement avec la mention :

> 📖 **Lecture recommandée :** [référence complète] — [1 phrase justifiant pourquoi]

### A.4 Illustrations

La synthèse doit privilégier les représentations visuelles quand le sujet s'y prête :

- **Toujours :** tableau comparatif des essais (section 4), tableau dosimétrique RT (section 6)
- **Si pertinent selon la question :**
  - Diagramme chronologique de l'évolution des preuves
  - Schéma de décision / arbre de sélection des patients
  - Tableau comparatif des schémas RT par site anatomique
  - Figure "landscape" des essais en cours (NCT, phase, critère principal, résultats attendus)
  - Carte conceptuelle des mécanismes biologiques (ex. rationnel radio-immunologique)

Claude propose spontanément ces illustrations si elles ajoutent de la clarté, sans attendre d'être sollicité.

### A.5 Objectif d'utilisation et format de sortie

**L'objectif d'utilisation conditionne directement le niveau de détail et le format :**

| Objectif | Niveau de détail | Sections prioritaires | Format de sortie |
|---|---|---|---|
| **RCP** | Synthétique — messages clés, 1–2 pages | Section 4 (tableau), 9.4 (message), 📖 lectures | **HTML** — lisible sur écran, imprimable |
| **Enseignement / présentation** | Panoramique — chronologie, contexte historique | Sections 4, 6, 7, 8 | **HTML** — partageable facilement |
| **Préparation de manuscrit** | Complet — tous détails dosimétriques, références Vancouver | Toutes sections + section 6 étendue | **Markdown** — éditable, versionnable |
| **Mise à jour personnelle** | Intermédiaire — signal principal + lacunes | Sections 4, 7, 9 | Markdown ou HTML selon préférence |

**Sur le format de sortie :**
- **HTML** : rendu immédiat dans tout navigateur, imprimable en PDF, partageable sans outil. Privilégier pour RCP, enseignement, transmission à un collègue.
- **Markdown** : source éditable, versionnable, convertible à la demande. Privilégier pour les manuscrits et l'archivage personnel.
- **Stratégie recommandée** : Markdown comme source de vérité → HTML généré à la demande pour les sorties finales.

> Claude génère par défaut la sortie en **HTML** sauf si l'objectif est "manuscrit" ou demande explicite du contraire.

---

## 1. Question clinique

### 1.1 Formulation libre
> *Écrire ici la question telle qu'elle se pose en pratique clinique.*

```
[QUESTION]
```

### 1.2 Décomposition PICO

| Élément | Précision |
|---|---|
| **P** — Population | Histologie : / Stade : / Ligne : / Biomarqueur : |
| **I** — Intervention | |
| **C** — Comparateur | |
| **O** — Outcome principal | |
| **O** — Outcomes secondaires | |

### 1.3 Précisions de contexte (cocher ce qui s'applique)

**Stade / contexte :**
- ☐ Localisé / précoce
- ☐ Localement avancé (non résécable)
- ☐ Métastatique d'emblée — synchrone
- ☐ Métastatique métachrone / récidive
- ☐ Oligoprogression sous systémique

**Ère thérapeutique pertinente :**
- ☐ Pré-immunothérapie (avant 2015) — contexte historique
- ☐ Post-immunothérapie (après 2015)
- ☐ Les deux (évolution historique recherchée)

**Type de question :**
- ☐ Efficacité (SSP / SG)
- ☐ Schéma technique — détails complets (dose / fraction / technique / cibles / BED₁₀)
- ☐ Schéma technique — grands principes seulement
- ☐ Toxicité
- ☐ Sélection de patients / biomarqueurs
- ☐ Timing d'association de traitements

**Objectif d'utilisation :**
- ☐ RCP → sortie courte, HTML
- ☐ Enseignement / présentation → sortie panoramique, HTML
- ☐ Préparation de manuscrit → sortie complète, Markdown
- ☐ Mise à jour personnelle → sortie intermédiaire, format au choix

---

## 2. Périmètre et exclusions à vérifier

*Lister ici les erreurs de périmètre typiques à éviter pour cette question précise.*

| Essai / donnée à risque de confusion | Raison d'exclusion | Apport indirect éventuel |
|---|---|---|
| | | |
| | | |

---

## 3. Stratégie de recherche

### 3.1 Mots-clés PubMed

```
Population :

Intervention :

Outcome :

Opérateurs booléens combinés :
```

**Filtres appliqués :**
- Types d'articles : ☐ RCT  ☐ Phase II  ☐ Systematic review  ☐ Meta-analysis  ☐ Retrospective
- Dates : de ______ à ______
- Langues : ☐ Anglais  ☐ Français

### 3.2 ClinicalTrials.gov

| NCT | Titre abrégé | Phase | Statut | Notes |
|---|---|---|---|---|
| | | | | |
| | | | | |

### 3.3 Protocoles complets NCI/NRG/RTOG (si détails dosimétriques nécessaires)

*URL d'accès : `https://cdn.clinicaltrials.gov/large-docs/[XX]/NCT[XXXXXXXX]/Prot_SAP_000.pdf`*

| NCT | Section RT | Données récupérées | Données manquantes |
|---|---|---|---|
| | | | |

### 3.4 Revues systématiques / méta-analyses identifiées

| Référence (auteur, année, revue) | Recherche couverte jusqu'à | Points clés | Limites |
|---|---|---|---|
| | | | |

---

## 4. Essais randomisés — Tableau de synthèse

*Un essai par ligne. Remplir dans l'ordre chronologique.*

| Essai | Année pub. | N | Phase | Population | Systémique concomitant | Schéma RT | Cibles RT | Critère principal | Résultat clé (HR / médiane) | Données RT publiées ? | Limites majeures |
|---|---|---|---|---|---|---|---|---|---|---|---|
| | | | | | | | | | | ☐ Oui ☐ Abstract seul | |
| | | | | | | | | | | ☐ Oui ☐ Abstract seul | |
| | | | | | | | | | | ☐ Oui ☐ Abstract seul | |

---

## 5. Données non randomisées pertinentes

| Référence | Type | N | Population | Intervention | Résultat principal | Biais identifiés |
|---|---|---|---|---|---|---|
| | Rétrospective / Cohorte / Phase I | | | | | |

---

## 6. Détails dosimétriques RT (si applicable)

*À compléter pour chaque essai où les schémas RT sont la question centrale.*

| Essai | Site cible | Dose totale | Nb fractions | Dose/fraction | BED₁₀ | Technique | Contraintes OAR notables |
|---|---|---|---|---|---|---|---|
| | Primitif pulmonaire | | | | | | |
| | Métastases osseuses | | | | | | |
| | Ganglions médiastinaux | | | | | | |
| | Foie | | | | | | |
| | Autre : | | | | | | |

---

## 7. Signaux clés et analyse critique

### 7.1 Données positives (avec niveau de preuve)

```
- 
- 
- 
```

### 7.2 Données négatives (essais et analyses)

```
- 
- 
```

### 7.3 Hypothèses explicatives des discordances

```
- 
- 
```

### 7.4 Lacunes identifiées

- ☐ Absence d'essai de phase III
- ☐ Données dosimétriques non publiées (abstract seulement)
- ☐ Ère pré-ICI non transposable
- ☐ Hétérogénéité des populations entre essais
- ☐ Absence de biomarqueur prédictif validé
- ☐ Autre : ______________________

---

## 8. Essais en cours

| NCT | Titre | Phase | Randomisé ? | Critère principal | Résultats attendus |
|---|---|---|---|---|---|
| | | | ☐ Oui ☐ Non | | |

---

## 9. Conclusions et implications pratiques

### 9.1 Réponse synthétique à la question clinique

```
[SYNTHÈSE EN 3-5 LIGNES MAXIMUM]
```

### 9.2 Niveau de preuve global pour la pratique

- ☐ Standard validé (phase III positif)
- ☐ Option raisonnable (phase II randomisé)
- ☐ Expérimental (phase I/II ou rétrospectif)
- ☐ Contre-indiqué (essai négatif de haut niveau)
- ☐ Insuffisant pour conclure

### 9.3 Sous-groupe(s) bénéficiaire(s) potentiel(s)

```
- 
- 
```

### 9.4 Message à retenir pour la pratique

```
[1 PHRASE]
```

---

## 10. Références à citer (format Vancouver)

```
1. 
2. 
3. 
```

---

## 11. Checklist finale avant clôture

**Cadrage :**
- [ ] Les questions de cadrage ont été posées ET les réponses reçues avant tout lancement
- [ ] Le PICO a été soumis à validation explicite avant la recherche
- [ ] L'objectif d'utilisation a été défini (RCP / enseignement / manuscrit / mise à jour)
- [ ] Les essais de périmètre incorrect ont été exclus et justifiés

**Contenu :**
- [ ] Les données pré-ICI et post-ICI sont clairement distinguées
- [ ] Au moins un essai négatif a été recherché et analysé
- [ ] Les essais en cours ont été vérifiés sur ClinicalTrials.gov
- [ ] Le niveau de preuve global est explicitement énoncé

**Radiothérapie :**
- [ ] Les données dosimétriques sont renseignées au niveau de détail demandé (complet ou grands principes)
- [ ] Les données manquantes (abstract seul) sont explicitement signalées
- [ ] Les protocoles complets NCT consultés si données RT absentes

**Mise en forme :**
- [ ] Format de sortie conforme à l'objectif (HTML ou Markdown)
- [ ] Au moins un tableau de synthèse est présent
- [ ] Les lectures recommandées sont identifiées (mention 📖)
- [ ] Liens NCT, DOI et PubMed fournis pour chaque essai et publication
- [ ] Des illustrations complémentaires ont été proposées si pertinent

---

*Template v1.3 — 18/03/2026*
