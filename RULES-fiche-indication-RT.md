# RULES-fiche-indication-RT.md
# Fiche d'indication en radiothérapie — synthèse ancrée sur les sociétés savantes
# Ronan Tanguy — Oncologue radiothérapeute, Institut Léon Bérard, Lyon
# Spécialités : neuro-oncologie, SRS/SBRT, métastases, pédiatrie
# Version 1.0 — Septembre 2026

---

## Contexte et usage

Ce fichier gouverne un type de recherche bibliographique précis : **je donne une situation
thérapeutique, Claude produit une fiche structurée en 4 items, ancrée en priorité sur les
recommandations de sociétés savantes.** La fiche alimente des templates par pathologie
(« un autre travail »), donc la sortie doit être compacte, sourcée et directement réutilisable.

**Ce que Claude produit, à chaque requête :**
1. Les **références bibliographiques de référence**, axées sociétés savantes (voir §Sources).
2. Les **4 items**, tirés de cette bibliographie et centrés radiothérapie :
   - **Justification** (bibliographique et/ou sociétés savantes)
   - **Efficacité attendue**
   - **Toxicité**
   - **Suivi en radiothérapie**
3. **Deux versions du texte produit** : une version médicale détaillée et une version patient
   courte, destinée au compte rendu remis au patient (voir §Deux versions de sortie).

**Déclenché par :** *« situation thérapeutique : … »*, *« fais-moi la fiche indication RT pour … »*,
*« justification / efficacité / toxicité / suivi pour l'indication … »*, ou toute demande de
synthèse d'une indication de RT selon les 4 items ci-dessus.

> ⛔ **Ne s'applique pas à** (voir §Délimitation en fin de fichier) :
> - une question clinique comparative PICO (I vs C) → `RULES-pico-search.md`
> - un état de l'art large sans indication délimitée → `RULES-academique-gen-consensus.md`
> - la revue critique d'un manuscrit → `RULES-peer-review.md`
> - la construction d'un outil HTML → `RULES-clinical-tools-v2.md`

---

## Les 4 items — définition et contenu attendu

> La fiche est **centrée radiothérapie**. Chaque item privilégie ce qui relève de la RT
> (schéma, timing, technique, volumes, OAR, suivi RT-spécifique). Les autres modalités
> ne sont mentionnées que pour situer l'indication de RT.

### Item 1 — Justification
- **Recommandation de société savante** citée nominativement (organisme, intitulé, année, version).
- **Niveau de preuve et grade** tels qu'énoncés par la source (ex. LoE, grade A-B ; niveau HAS ;
  catégorie NCCN). Ne jamais inventer un grade : reprendre celui de la source, ou signaler son absence.
- **Synthèses de preuves** de rang supérieur (revue systématique, méta-analyse), de préférence
  publiées dans une revue de société (PRO, IJROBP, Radiother Oncol).
- ⚠️ **Signaler explicitement tout décalage de périmètre** entre la source et la situation demandée
  (cf. exemple OH orthopédique vs POAN neurogène : la reco existe pour l'une, pas pour l'autre).
  Graduer la transposabilité au lieu de l'ignorer.

### Item 2 — Efficacité attendue
- **Chiffres d'efficacité** de la source (contrôle local, taux de prévention, SSP/SG selon le cas),
  avec la métrique et l'effectif de référence.
- **Schéma RT de référence** : dose totale · nombre de fractions · dose/fraction · timing.
  BED/EQD2 (α/β précisé) si utile à la comparaison ou à la décision.
- Distinguer honnêtement **signal clinique** et **preuve statistique** (ex. OR cliniquement
  pertinent mais non significatif → le dire).

### Item 3 — Toxicité
- **Profil de toxicité** rapporté par la source (aiguë / tardive), fréquences si disponibles.
- **Points de vigilance** propres à l'indication et au site (OAR critiques, cicatrisation, etc.).
- **Risques tardifs / radio-induits** (second cancer, effets fonctionnels) avec l'incidence si connue —
  particulièrement chez le sujet jeune ou en pédiatrie.
- Échelle de cotation utilisée (CTCAE version, RTOG/EORTC) quand la source la précise.

### Item 4 — Suivi en radiothérapie
- **Rythme et modalités** de surveillance clinique et d'imagerie recommandés (ou pratique usuelle si
  la source est muette — le préciser).
- **Échelles / critères** de suivi standardisés pertinents (ex. Brooker, RANO, RECIST, gradation
  spécifique au site).
- Surveillance RT-spécifique éventuelle (cicatrice si RT périopératoire, effets tardifs à dépister).

---

## Deux versions de sortie — médicale + patient

> **Finalité : ces textes sont destinés au compte rendu du patient.** Chaque fiche est donc produite
> en **deux versions**, les deux ensemble par défaut.

### Version médicale (défaut)
Les 4 items complets tels que définis ci-dessus : sourcée, chiffrée, avec niveaux de preuve, schéma
dosimétrique et références liées. Destinée au dossier, à la RCP, au staff, au courrier confraternel.

### Version patient — pour le compte rendu remis au patient
Un texte court, en langage clair, directement collable dans le compte rendu. Règles :
- **Structurée selon les 4 mêmes items** (Justification · Efficacité · Tolérance · Suivi), chaque
  item réduit à **1-2 phrases courtes** en langage clair. En version patient, « Toxicité » est
  intitulée « Tolérance / Effets indésirables ». Total très court (≈ 6-8 lignes).
- **Peu ou pas de chiffres** : pas de pourcentages, pas de niveaux de preuve, pas de dosimétrie.
  **Rester au-dessus du niveau du nombre de séances** : décrire le traitement globalement
  (ex. « une radiothérapie préventive réalisée autour de l'intervention »), sans détailler séances ni dose.
- **Sans jargon** : pas de LoE, grade, OR, BED ni acronyme technique non explicité. Termes simples
  (ex. « formation d'os anormal autour de l'articulation » plutôt que « ossification hétérotopique » seul).
- **Honnête et mesuré** : ne jamais transformer un bénéfice non démontré en certitude ; ne pas
  sur-dramatiser ni sur-rassurer (« réduit le risque » plutôt que « supprime »).
- **Risques rares** : mentionnés sobrement, avec renvoi à la consultation pour le détail — ne pas
  énumérer de chiffres de risque radio-induit dans le compte rendu patient.
- **Pas de références bibliographiques** dans la version patient (elles restent en version médicale).
- **Formulation impersonnelle de compte rendu** : pas de « vous » ni de « je » ; tournures neutres
  (« Une radiothérapie préventive est proposée… », « Ce traitement est habituellement bien toléré… »), prête à coller.

> ⚠️ **Règle d'intégrité** : la version patient est **dérivée de la version médicale validée**,
> jamais l'inverse. Toute affirmation de la version patient doit être soutenue par la version
> médicale ; la simplification ne doit créer aucun écart factuel.

---

## Sources — priorité aux sociétés savantes

**Hiérarchie à respecter dans l'item Justification :**

| Rang | Type de source | Exemples |
|---|---|---|
| **1a** | Sociétés savantes **européennes** (souvent les mieux construites pour la RT — à rechercher d'emblée) | **DEGRO** (RT des affections bénignes ++), **ESTRO / ESTRO-ACROP** (ex-Advisory Committee for Radiation Oncology Practice — guidelines par indication), **ESMO** (indications oncologiques), **EANO** (neuro-oncologie) |
| **1b** | Sociétés savantes / agences **françaises** | SFRO, ANOCEF (neuro), SFCE (pédiatrie), HAS, INCa |
| **1c** | Sociétés savantes **nord-américaines** (en complément) | ASTRO (+ revues PRO / IJROBP), NCCN, ACR Appropriateness Criteria, SIOP-E / COG (pédiatrie) |
| **1d** | Standards de prescription / reporting | ICRU |
| **2** | Revues systématiques / méta-analyses | de préférence PRO, IJROBP, Radiother Oncol, JCO |
| **3** | Essais pivots / grandes cohortes | pour combler une lacune quand aucun rang 1-2 ne couvre l'indication |

> ⚠️ **Attention au périmètre des sociétés européennes** : ESTRO-ACROP, ESMO et EANO produisent des
> recos par **indication oncologique** (métastases, PMRT, gliomes…). Pour une indication bénigne ou
> hors champ oncologique, elles peuvent ne rien couvrir — c'est alors la **DEGRO** qui fait référence
> (affections bénignes). Toujours vérifier laquelle couvre réellement l'indication et le signaler.

**Règles de sourçage :**
- L'item Justification **doit** citer au moins une source de rang 1 si elle existe.
  Si aucune recommandation de société ne couvre l'indication, **le dire explicitement** et remonter
  au rang 2 en le signalant.
- **Ne citer que des références réellement retrouvées dans la session.** Toute référence issue des
  connaissances internes est labellisée `[hors recherche]` et ne compte pas comme source de référence.
- **Signaler les textes intégraux inaccessibles** et distinguer abstract de congrès vs publication complète.
- **Périmètre par défaut : sociétés européennes et françaises au premier plan.** DEGRO,
  ESTRO / ESTRO-ACROP, ESMO et EANO sont souvent les recommandations les mieux construites pour la RT
  et sont recherchées d'emblée ; SFRO / HAS / INCa pour l'ancrage français ; ASTRO / NCCN en complément.
  N'élargir ou restreindre que selon la réponse de cadrage.

**Liens obligatoires pour chaque référence :**
- Reco de société → URL de la page/PDF officiel
- Publication → DOI (`https://doi.org/…`) ou PubMed (`https://pubmed.ncbi.nlm.nih.gov/[PMID]/`)
- Essai → `https://clinicaltrials.gov/study/NCT[XXXXXXXX]`
- Protocole complet si détails dosimétriques → `https://cdn.clinicaltrials.gov/large-docs/[XX]/NCT[XXXXXXXX]/Prot_SAP_000.pdf`

---

## Workflow — de la requête au résultat

### Phase 0 — CADRAGE (obligatoire, bloquant)

> ⛔ **RÈGLE ABSOLUE : Claude ne lance aucune recherche avant d'avoir (1) posé les questions de
> cadrage, (2) reçu les réponses, (3) reformulé la situation thérapeutique et obtenu une validation
> explicite. Cette règle s'applique même si la situation paraît claire.**

**Séquence :**
1. Lire la situation donnée.
2. Poser les questions de cadrage **via widget de choix** (options cliquables), une passe suffit.
3. **Attendre les réponses.**
4. Reformuler : *« Voici la situation telle que je la comprends — je lance la recherche ? »*
5. Rechercher seulement après confirmation explicite.

**Questions de cadrage systématiques (adapter, ne pas toutes poser si déjà répondues) :**
- **Pathologie et situation précise** : histologie / stade, et intention — curatif / adjuvant /
  néoadjuvant / palliatif / prophylactique ?
- **Cible / site irradié** : quel volume, quelle localisation ?
- **Population** : adulte ou pédiatrique ? Réirradiation ?
- **Objectif de la fiche** : RCP / protocole de service / enseignement / information patient /
  publication ? *(conditionne le niveau de détail et le format)*
- **Périmètre des recommandations** : européennes + françaises par défaut (DEGRO / ESTRO-ACROP /
  ESMO / EANO / SFRO / HAS), ou ouverture explicite aux nord-américaines (ASTRO / NCCN) ?
- **Niveau de détail technique RT** : dosimétrie complète (dose/fractionnement/technique/contraintes OAR)
  ou grands principes ?
- **Horizon des sources** : dernière version de reco seulement, ou évolution historique aussi ?

### Phase 1 — Recherche sociétés savantes (guideline-first)
- Chercher d'abord la ou les **recommandations de rang 1** couvrant l'indication (site de la société,
  PubMed pour la citation, recherche web pour le PDF/landing page).
- Relever : organisme, intitulé exact, année/version, niveau de preuve et grade, schéma RT recommandé.

### Phase 2 — Complément de preuves
- Compléter avec **revues systématiques / méta-analyses** récentes (rang 2), puis **essais pivots**
  (rang 3) uniquement pour combler une lacune.
- Extraire systématiquement les **détails RT** (voir §Extraction RT).

### Phase 3 — Rédaction des 4 items et des deux versions
- Remplir Justification / Efficacité / Toxicité / Suivi (version médicale).
- Chaque donnée chiffrée sourcée ; chaque source liée.
- Signaler les décalages de périmètre et les lacunes de preuve.
- **Dériver la version patient** de la version médicale (voir §Deux versions de sortie).

### Phase 4 — Contrôle qualité
- Passer la checklist avant livraison.

---

## Extraction RT — à relever systématiquement

Pour toute indication de RT, quand la source le fournit :
- **Dose totale · nombre de fractions · dose par fraction · timing** (jour/semaine/cycle de référence)
- **BED / EQD2** (préciser α/β) si utile
- **Technique** (3D-CRT, IMRT/VMAT, SRS, SBRT, protons)
- **Volumes cibles** (GTV → CTV → PTV, marges) si la fiche est de niveau dosimétrique complet
- **Contraintes OAR** notables
- Signaler quand ces données sont absentes de la publication principale et proposer le protocole complet (NCT).

---

## Format de sortie

- **Deux versions par défaut** : version médicale (4 items) puis version patient courte (voir §Deux versions de sortie).
- **Par défaut : bloc Markdown compact et copiable**, structuré selon les 4 items + une mini-liste
  de références liées, pensé pour être collé dans les templates par pathologie.
- **HTML sur demande** (ou si l'objectif de la fiche est RCP / enseignement / information patient) —
  lisible à l'écran, imprimable, partageable.
- Liens intégrés (DOI / PubMed / NCT / URL société) inline dans le bloc et dans les références.
- Longueur : synthétique. Une fiche tient sur une à deux pages ; le détail dosimétrique complet
  n'est déployé que si demandé au cadrage.

---

## Langue
- Réponse et fiche en **français** par défaut.
- Acronymes, intitulés de recommandations et de protocoles : conserver la langue originale
  (souvent l'anglais).
- Termes techniques : version anglaise entre parenthèses si la traduction française est ambiguë.

---

## Checklist avant livraison

**Cadrage**
- [ ] Questions de cadrage posées **et** réponses reçues avant tout lancement
- [ ] Situation reformulée et validée explicitement
- [ ] Objectif de la fiche et périmètre géographique définis

**Sources**
- [ ] Au moins une source de rang 1 (société savante) citée, ou absence signalée explicitement
- [ ] Références réellement retrouvées en session ; les connaissances internes labellisées `[hors recherche]`
- [ ] Lien fourni pour chaque référence (URL société / DOI / PubMed / NCT)
- [ ] Abstract de congrès vs publication complète distingués ; textes inaccessibles signalés

**4 items**
- [ ] Justification : reco nommée + niveau de preuve/grade repris de la source (non inventé)
- [ ] Décalage de périmètre source ↔ situation signalé et gradé si présent
- [ ] Efficacité : chiffres + schéma RT (dose/fractions/dose-fr/timing) ; signal vs significativité distingués
- [ ] Toxicité : profil aigu/tardif, risques radio-induits, échelle de cotation si précisée
- [ ] Suivi RT : rythme + échelles/critères (Brooker, RANO, RECIST…) ; pratique usuelle signalée si source muette

**Deux versions**
- [ ] Version médicale complète (4 items sourcés, chiffrés, liés)
- [ ] Version patient courte, **structurée selon les 4 items**, sans jargon, peu chiffrée, sans références
- [ ] Version patient honnête (pas de sur-promesse ni sur-dramatisation) ; risques rares renvoyés à la consultation
- [ ] Aucune affirmation de la version patient non soutenue par la version médicale

**Mise en forme**
- [ ] Format conforme à l'objectif (Markdown par défaut, HTML si demandé)
- [ ] Détail dosimétrique déployé seulement si demandé au cadrage
- [ ] Bloc directement collable dans un template par pathologie

---

## Délimitation vis-à-vis des autres RULES

| Situation | Fichier |
|---|---|
| **Situation thérapeutique → fiche 4 items ancrée sociétés savantes** | **ce fichier** |
| Question clinique comparative avec PICO (I vs C), synthèse d'essais | `RULES-pico-search.md` |
| État de l'art large sans indication délimitée (via Consensus) | `RULES-academique-gen-consensus.md` |
| Revue critique / peer review d'un manuscrit | `RULES-peer-review.md` |
| Construction d'un outil clinique HTML | `RULES-clinical-tools-v2.md` |

> Différence clé avec `RULES-pico-search.md` : ici la logique est **guideline-first** (on part de la
> recommandation de société pour une indication donnée), et la sortie est une **fiche à 4 items fixes**
> destinée à des templates — non une synthèse comparative d'essais autour d'une question PICO.

---

*Fichier à stocker dans le repo `rules`. Compatible avec les workflows définis dans les autres RULES.*
*v1.0 — Septembre 2026 (usage personnel R.T.)*
