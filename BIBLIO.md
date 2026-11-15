# BIBLIO — Recherche bibliographique : 4 modes

> Prérequis : `SOCLE-clinique.md` (cadrage allégé, échelle de preuve, références, vérification, extraction RT).
> Apparence des sorties HTML : `DESIGN.md`, gabarit **R** (document de revue), référence `pneumo/RTA_CBNPC_revue.html`.
> Relecture d'un manuscrit : `RULES-peer-review.md` (hors de ces modes).

## Choisir le mode

| Mode | Objet | Déclencheurs | Exemples existants |
|---|---|---|---|
| **1. PICO** | une **question** clinique ciblée | « faut-il… », « apport de X », « X vs Y », « place de X dans Y », « quels critères pour… » | `pneumo/BIBLIO_LCT_CBNPC_M_consolidation.html`, `pneumo/RTA_CBNPC_revue.html`, `neuro/CSI_metastases_leptomeningees_CLB.html`, `pedia/ORN_dentaire_postRT_pediatrique.html`, `pedia/TSC_malin_RT_sauvetage.html` |
| **2. Fiche indication RT** | une **indication** déjà retenue | « situation thérapeutique : … », « fiche indication pour… », « justification / efficacité / toxicité / suivi pour… » | — |
| **3. Pathologie rare** | une **maladie** dans son ensemble | entité rare (< 1/2 000 ou < 200 cas publiés) + « prise en charge », « conduite à tenir », « fiche sur… » | `pedia/TSC_pediatrie_revue.html` |
| **4. État de l'art** | un **domaine** de connaissances | « que sait-on sur… », « grands travaux sur… », sujet large ou non clinique | — |

Cas limites : technique sans comparateur → PICO (C facultatif) · pathologie rare + question RT précise → PICO · pathologie fréquente → PICO ou Fiche, jamais Pathologie rare.

---

## Mode 1 — PICO

**Cadrage spécifique** (en plus du socle, seulement si utile) : histologie et stade ; contexte (synchrone / métachrone / oligoprogression) ; traitement systémique de référence ; ère thérapeutique (données contemporaines seules ou contexte historique).

**Étapes**
1. PICO reformulé ; **périmètre et exclusions** : lister les essais à risque de confusion (raison d'exclusion, apport indirect éventuel).
2. Recherche : PubMed (mots-clés population / intervention / outcome, filtres de type d'étude et de dates), ClinicalTrials.gov, protocoles complets si détails RT nécessaires, revues systématiques existantes (date de fin de recherche).
3. Analyse critique : données positives et négatives avec leur badge de preuve, hypothèses expliquant les discordances, lacunes.

**Plan de sortie**
1. **Synthèse** (encadré) : réponse en 3–5 points, verdict par contexte, message à retenir en 1 phrase.
2. **PICO** et contextes couverts.
3. **Tableau des essais randomisés** (chronologique) : essai, année, N, phase, population, systémique associé, schéma et cibles RT, critère principal, résultat clé (HR / médianes), données RT publiées ou abstract seul, limites.
4. **Données non randomisées** : référence, type, N, résultat, biais.
5. **Détails dosimétriques** par site si la question l'exige (dose, fractions, dose/fr, BED₁₀, technique, contraintes OAR).
6. **Signaux et analyse critique** : positifs, négatifs, discordances, lacunes.
7. **Essais en cours** : NCT, phase, randomisé, critère principal, résultats attendus.
8. **Conclusion** : niveau global pour la pratique (socle §4.2), sous-groupes bénéficiaires.
9. **Références** (Vancouver, liens, 📖).

Proposer une illustration quand elle clarifie : chronologie des preuves, arbre de sélection des patients, paysage des essais en cours, patients-types pour une RCP.

---

## Mode 2 — Fiche indication RT

**Logique « recommandations d'abord »** : on part des sociétés savantes pour une indication donnée ; sortie compacte, destinée à des modèles par pathologie et au compte rendu patient.

**Cadrage spécifique** : cible / site irradié ; réirradiation ; périmètre des recommandations (européennes + françaises par défaut, ouverture nord-américaine ?) ; dernière version seule ou évolution.

**Hiérarchie des sources**

| Rang | Sources |
|---|---|
| 1a | Sociétés **européennes** : DEGRO (affections bénignes ++), ESTRO / ESTRO-ACROP, ESMO, EANO — à chercher d'emblée |
| 1b | **Françaises** : SFRO, ANOCEF, SFCE, HAS, INCa |
| 1c | **Nord-américaines** (complément) : ASTRO, NCCN, ACR Appropriateness Criteria, SIOP-E / COG |
| 1d | Standards de prescription : ICRU |
| 2 | Revues systématiques / méta-analyses (PRO, IJROBP, Radiother Oncol, JCO) |
| 3 | Essais pivots, seulement pour combler une lacune |

ESTRO-ACROP, ESMO et EANO couvrent des indications **oncologiques** ; pour une indication bénigne, c'est souvent la DEGRO. Vérifier qui couvre réellement l'indication. Au moins une source de rang 1, ou dire explicitement qu'il n'y en a pas.

**Les 4 items (version médicale)** — centrés RT
1. **Justification** : recommandation nommée (organisme, intitulé, année, version), grade repris de la source ; synthèses de rang 2 ; **décalage de périmètre** source ↔ situation signalé et gradué.
2. **Efficacité attendue** : chiffres de la source (métrique, effectif) ; schéma de référence (dose, fractions, dose/fr, timing ; BED/EQD2 si utile) ; distinguer signal clinique et preuve statistique.
3. **Toxicité** : aiguë / tardive avec fréquences ; OAR et points de vigilance du site ; risques tardifs et radio-induits (surtout sujet jeune) ; échelle de cotation si précisée.
4. **Suivi en radiothérapie** : rythme et modalités ; échelles standardisées (Brooker, RANO, RECIST…) ; pratique usuelle signalée si la source est muette.

**Version patient** (produite avec la version médicale, pour le compte rendu)
- Mêmes 4 items (« Toxicité » devient « Tolérance / effets indésirables »), 1–2 phrases chacun, ≈ 6–8 lignes au total.
- Sans chiffres, sans dosimétrie, sans nombre de séances, sans jargon ni acronyme, sans références.
- Honnête et mesurée (« réduit le risque », pas « supprime ») ; risques rares renvoyés à la consultation.
- Formulation impersonnelle de compte rendu (ni « vous » ni « je »).
- **Dérivée de la version médicale validée** : aucune affirmation qui n'y figure pas.

**Format** : bloc Markdown compact et copiable (4 items + références liées), puis la version patient ; HTML gabarit R sur demande. Une à deux pages.

---

## Mode 3 — Pathologie rare

**Cadrage spécifique** : primitif ou récidive, extension ; documents déjà en main.

**Étapes**
1. **Connaissances internes** : niveau de confiance ; données susceptibles d'avoir changé depuis 5 ans ; classification OMS ≥ 2021 à vérifier.
2. **Repérage** : une recherche large (`"[pathologie]" overview OR review OR management`) pour la terminologie (noms anciens / actuels), les sous-groupes moléculaires, les groupes coopérateurs. Moins de 5 publications pertinentes : le dire d'emblée.
3. **Au moins 3 recherches** : PubMed (séries ≥ 10 cas, revues, depuis 2020) ; recommandations (ESMO, NCCN, SIOPE, EANO, SFRO… ou absence notée) ; ClinicalTrials.gov. Recommandation majeure inaccessible : la nommer et demander le document.
4. Suivi des références et auteurs récurrents (🔁), signal de citations élevé par an.

**Plan de la note** (chaque section présente, « Données insuffisantes » si vide)
1. Bandeau d'alertes en tête si applicable : `⚠ Pas de consensus` · `🏥 RCP recommandée` · `📋 Données limitées` (< 50 cas publiés : encadré d'avertissement).
2. Définition et contexte : entité OMS (grade, année), cellule d'origine, incidence, âge.
3. Bilan initial et diagnostic : imagerie, marqueurs, anatomopathologie / moléculaire, diagnostic différentiel.
4. Protocoles existants et essais ouverts (ou absence).
5. Traitement par modalité (chirurgie, RT avec dose / volume / fractionnement, systémique, séquence) ; traitements sans preuve solide signalés.
6. Pronostic : survie, facteurs pronostiques, récidive.
7. Algorithme décisionnel par étapes (résidu, extension, grade).
8. Références clés (5–8, OMS si pertinent, badge de preuve, 🔁).
9. Recours national (socle §9).

**Sortie** : note dans la conversation, puis **proposer** la version HTML (gabarit R) ; ne pas la proposer si < 20 cas publiés. Le format tableau de bord (Type D, `OUTILS-cliniques.md`) est réservé aux synthèses **multi-entités**.

---

## Mode 4 — État de l'art (outil Consensus)

Pour un domaine large, souvent non clinique (santé, comportement, éducation, sciences sociales, technologie).

**Étapes**
1. **Repérage** : une recherche large non structurée (terminologie, sous-domaines, groupes, références très citées). Pas de synthèse à ce stade.
2. **Plan** : choisir un cadre de découpage — PICO (intervention identifiable), SPIDER (qualitatif, expérience vécue), ou Mécanisme · Applications · Limites · Comparaisons (technologie) — et 4–5 sous-domaines. Présenter le plan et le budget (rapide : 5 requêtes · standard : 10, par défaut). Attendre seulement si le découpage est discutable (cadrage allégé).
3. **Recherches** séquentielles (Consensus : 1 requête/s). Budget standard : 1 par sous-domaine + 2 « systematic review / meta-analysis » sur les plus actifs + 2 datées sur le sous-domaine central (`year_max: 2015`, `year_min: 2021`) + 1 suivi de la référence la plus citée. Filtres utiles : `year_min/max`, `human`, `sample_size_min`, `sjr_max: 1`.

**Plan de sortie**
1. Vue d'ensemble : sujet, cadre utilisé, où la preuve est solide ou lacunaire.
2. Ordre de lecture (5–7 articles : meilleure revue, fondateurs, front actuel, controverse), avec pour chacun l'apport et le point d'attention.
3. Histoire du domaine : récit court + tableau chronologique (5–8 lignes), glissements terminologiques.
4. Un guide par sous-domaine : ce que montre la recherche, 3–5 articles clés, 6–10 termes de recherche, 2–3 requêtes booléennes prêtes à coller.
5. Groupes de recherche clés (3–5).
6. Lacunes : méthodologiques, de population / contexte, conceptuelles — avec « pourquoi ça compte ».
7. Bibliographie alphabétique, URL Consensus complètes ; références hors Consensus labellisées `[hors Consensus]`.

---

## Checklist commune avant livraison

- [ ] Mode choisi conforme à l'aiguillage ; cadrage allégé fait (socle §2)
- [ ] Au moins un tableau de synthèse ; badge de preuve sur chaque étude ; niveau global énoncé
- [ ] Essais négatifs recherchés ; essais en cours vérifiés sur ClinicalTrials.gov
- [ ] Données RT au niveau de détail demandé ; données manquantes (abstract seul) signalées
- [ ] Liens DOI / PubMed / NCT / société pour chaque référence ; vérification socle §7 faite
- [ ] Format conforme à l'objectif ; HTML au gabarit R
- [ ] Mode 2 : source de rang 1 ou absence signalée ; version patient dérivée, sans chiffres ni jargon
