# OUTILS CLINIQUES — Outils HTML interactifs

> Prérequis : `SOCLE-clinique.md` (vérification des sources §7, règles de modification §8).
> Apparence : `DESIGN.md`, gabarit **O** (DM, sombre / clair). Le code CSS/JS n'est plus recopié ici : partir du fichier de référence de chaque type.
> Ex-`RULES-clinical-tools.md` v2.3 ; numérotation des anciennes règles (6-A, 6-B…) conservée entre parenthèses.

---

## 1. Choisir le type

| Type | Usage | Référence | Exemples |
|---|---|---|---|
| **AB** — Questionnaire + logigramme | Stratification d'un patient selon un protocole, un fichier, deux onglets | `pedia/cog-wilms-fusion.html`, `pedia/us-rms-cog.html` | UMBRELLA, COG AREN, COG NB, NB rechute, FaR-RMS, HR-NBL2, US-RMS, ARIA, SIOP-MB, SFCE-MB, MB6, ATRT01 |
| **C** — Consultation | Filtrage pas à pas d'un référentiel (région → OAR → fractionnement) ; consultation directe au poste de dosimétrie | `pedia/COG_LTFU_RadiationLateEffects.html` | COG LTFU, PENTEC |
| **D** — Tableau de bord de revue | Synthèse **multi-entités** d'une pathologie : protocoles RT, essais actifs, signal clinique | `pedia/WilmsTumor_Revue.html` | Wilms, NeuroOnco, RareNeuroOnco, ACNS2021, ACNS2321, SIOP CNS GCT II, NGGCT rechute |
| **E** — Atlas par situations | Plusieurs entités, une fiche homogène par situation clinique finale | `neuro/neuro-onco-atlas.html` (généré par `build/`) | Atlas neuro-oncologie pédiatrique |
| **M** — Mode opératoire du service | Protocole de service d'une localisation : doses, contraintes filtrables, calculateurs | `neuro/MO_neuro_2026.html`, `pneumo/MO_poumon_2026.html` | MO neuro, MO poumon |

- **Comparateur** (`neuro/mb-comparator.html`) : variante du type AB — un profil patient, plusieurs référentiels côte à côte, code couleur de concordance.
- **Document de revue d'une seule question** : ce n'est pas un outil → `BIBLIO.md`, gabarit R.
- **Anciens types A et B séparés** (logigrammes Ewing FR / US, comparatif FaR-RMS vs COG) : restent valides tels quels. Tout **nouvel** outil de stratification suit le type AB.

---

## 2. Spécifications par type

### Type AB — Questionnaire + logigramme
- En-tête collant, puis barre d'onglets collante : « ⬡ Stratification pas-à-pas » (`#panel-wizard`, affiché par défaut) · « ⬡ Logigramme interactif » (`#panel-flow`).
- Hauteur du logigramme calculée en JS (`fenêtre − en-tête − onglets`), recalculée au redimensionnement.
- **Logigramme D3 construit au premier affichage de l'onglet** (jamais au chargement : un SVG invisible a une largeur nulle). D3 7 importé quand même dans `<head>`.
- Questionnaire : barre de progression (`Étape X / N` + nom de l'étape), une carte par question, options en boutons, bouton « ← Retour » si historique non vide.
- Historique de navigation dans **`navStack`** (jamais `history`, qui masque `window.history`).
- Données séparées du rendu, en tête de script : `STEPS` (étapes et `next()`), `RES` (résultats), `TT` (info-bulles).
- Nœuds du logigramme : ellipses (arbres > 15 nœuds, positions manuelles) ou rectangles hiérarchiques `d3.tree()` (< 15 nœuds). Décision = petit nœud neutre ; résultat = nœud large coloré selon le risque. Chaque nœud a un `id` unique ; libellés sur les arêtes significatives.
- **L'info-bulle d'un résultat contient autant d'informations que la carte résultat** du questionnaire (outcomes, RT : timing, volumes, doses).
- Zoom ajusté automatiquement aux dimensions réelles ; bouton « ⊙ Recentrer » en haut à droite ; légende en bas à gauche couvrant tous les types de nœuds.
- Nom de fichier : `[protocole]-[pathologie]-fusion.html` ; titre `PROTOCOLE · Pathologie — Stratification & Logigramme`.

### Type C — Consultation
- Étapes de filtrage successives, résultat en tableau ou en cartes de contraintes.
- Bascule de thème comme les autres types.

### Type D — Tableau de bord de revue
- Accueil : grille de cartes (une par entité ou scénario : numéro, nom, dose clé, nombre d'essais, étiquette colorée), puis tableau comparatif transversal.
- Un panneau par entité, ouvert par `show(id)` / fermé par `back()` ; pas de questionnaire ni de D3.
- Chaque panneau : tableau des protocoles RT · grille d'essais actifs (NCT cliquable, badge de statut RECRUITING / ACTIVE · NR / NOT YET RECRUITING, modalités, promoteur) · encadré « signal clinique » (3–5 phrases) · références avec DOI et note chiffrée.
- Clé de thème `theme-[pathologie]-revue` ; avertissement dans l'en-tête ; `lang="en"` si contenu anglophone.
- Nom de fichier : `[pathologie]-revue.html` ou `[Pathologie]_Revue.html`.

### Type E — Atlas par situations cliniques
- Fichier unique, routage par `location.hash` : `#/` (accueil : entités par famille, légendes, journal d'audit) · `#/e/<entité>` (bandeau d'identité, alertes, filtres par axe, carte de décision, sections repliables contexte / essais / références) · `#/e/<entité>/s/<id>` (fiche) · `#/e/<entité>/flow` (logigramme D3, protocoles de recherche uniquement, feuilles cliquables vers la fiche).
- Pas de questionnaire dès que les situations finales sont peu nombreuses (≤ 15 environ).
- **Fiche situation, gabarit invariable** : en-tête (entité, titre, badge de risque) · définition de la situation · **colonnes protocole côte à côte** (badge de preuve obligatoire, outcomes, chimiothérapie, bloc RT ou absence de RT explicite, `⚠ verify` si besoin) · colonne réservée en pointillés pour un protocole annoncé · notes cliniques · références citées dans la fiche · précédent / suivant · PDF.
- Couleurs de situation : `lr` vert · `sr` bleu · `hr` rouge · `sp` jaune · `inf` violet (nourrisson / sans RT) · `off` gris (hors protocole). Puces protocole : COG orange · SIOP bleu · littérature violet · en attente pointillés.
- Données : un objet par entité, un seul moteur de rendu (garantie d'homogénéité). Toute modification passe par un script de `build/` (jamais à la main dans le fichier généré).
- `lang="en"`.

### Type M — Mode opératoire du service (nouveau, v3.0)
- **Une page longue** avec sommaire, sections dans l'ordre du parcours : indications · aide à la décision · contention et acquisition · délinéation · protocoles de doses · planimétrie · contraintes OAR · IGRT et traitement · suivi · recommandations et essais · références.
- **Tableaux de contraintes filtrables** (fractionnement, situation : lésion en place / lit opératoire…), chaque contrainte avec la structure à créer dans le TPS, sa source et son badge de preuve ; badge **« Service »** pour un seuil interne non publié.
- **Calculateurs et estimateurs** (BED/EQD2, risque de radionécrose, gradient) : saisies bornées ; hors du domaine des données publiées, afficher « hors domaine », jamais une extrapolation.
- Textes prêts à copier (consignes aux physiciens) quand le service en a besoin.
- **Journal compagnon** `MO_[localisation]_[année]_journal.md` : modifications entre versions, journal d'audit des sources, points en attente.
- Bascule de thème et export PDF comme les autres types.

---

## 3. Avant de coder (ex-6-A et 6-F §2)

**Données indispensables** (sinon, ne pas créer) : critères de stratification à jour (source, année, version) · protocole de chaque bras · RT de chaque groupe (timing, volumes, doses) · chimiothérapie de chaque bras.
**Souhaitables** : OS / EFS / PFS à 5 ans par groupe (avec source) · toxicités tardives majeures · références complètes avec DOI · NCT des essais actifs.
**Optionnelles** : sous-analyses (protons / photons, biomarqueurs), essais en recrutement, date de version du protocole.

**Cas tests définis avant le code**, conservés (`// ═══ CAS TESTS ═══` en tête de script ou fichier joint) et rejoués à chaque modification :
- calculateur / score : 3–5 patients fictifs calculés à la main depuis la publication, dont un à chaque seuil de bascule ;
- questionnaire / logigramme / atlas : parcours « profil → situation ou bras attendu (source, page ou tableau) » couvrant chaque groupe de risque et les cas limites (âge seuil, résection limite, statut M ambigu) ;
- table de contraintes : recoupement valeur par valeur avec la source.

---

## 4. Contenu clinique des résultats

### 4.1 Groupes de risque (ordre fixe)
VLR très bas risque · LR bas risque · SR risque standard · HR haut risque · Spécial (discussion multidisciplinaire, bilatéral, prédisposition). Vert = LR et rouge = HR dans les deux thèmes.

### 4.2 Carte résultat — dans cet ordre
1. **Outcomes d'efficacité** (ex-6-B) : OS et EFS/PFS à 5 ans, effectif de la cohorte, référence `[n]`. Couleur : vert OS ≥ 95 % · jaune 80–94 % · rouge < 80 %. Toxicités si disponibles (endocrinienne, neurocognitive, second cancer, protons vs photons).
2. **Chimiothérapie** : schéma complet, durée, intensification.
3. **Radiothérapie — Timing · Volumes · Doses** (ex-6-B2, règle la plus critique ; un élément manquant est bloquant) :
   - **Timing** : quand, dans la séquence (ex. « J10–J14 postopératoire », « semaine 13 après 6 cycles », « concomitant à l'irinotécan, semaines 20–25 ») ;
   - **Volumes** : chaîne GTV → CTV → PTV avec marges chiffrées et exceptions de site (ex. « GTVp → CTVp +1 cm, +2 cm sup/inf en extrémité ; CTV → PTV 3–10 mm selon IGRT ») ;
   - **Doses** : chaque scénario avec indication, dose totale, fractions, dose/fraction (ex. « flanc stade III 10,8 Gy / 6 fr ; WLI 12 Gy / 8 fr si > 12 mois »). Couleur : bleu standard · jaune escaladé · rouge très escaladé ;
   - RT ganglionnaire si N+ ; RT métastatique par site si stade IV ;
   - **RT non indiquée** : le dire avec la raison (`.no-rt-box`), jamais une section vide.
4. **Note clinique** : pièges, particularités du sous-groupe, divergences entre protocoles.

### 4.3 Sources (ex-6-C)
Tout outcome, toute dose, tout critère de stratification porte un appel `[n]`. Références (auteurs, titre, revue, année, DOI et NCT cliquables) affichées sous le résultat, masquées pendant la navigation du questionnaire. Vérification : socle §7.

### 4.4 Export PDF (ex-6-D)
Bouton d'export du résultat (`window.print()`) qui masque progression et boutons ; feuille d'impression neutre (`DESIGN.md`, gabarit O).

---

## 5. Conventions de code

- `var` plutôt que `const` / `let` ; pas de littéraux de gabarit imbriqués (concaténation).
- Données cliniques séparées du rendu, en tête de script ; commentaires de section `// ═══ DONNÉES ═══`.
- CSS en sections commentées (`/* ── THÈME ── */`…) ; toutes les couleurs du DOM via variables (exception : SVG D3).
- `try/catch` autour de `localStorage` ; cohérence des clés `RES` et des retours `next()` vérifiée avant livraison.
- `lang="fr"` (sauf atlas / contenu anglophone) ; `maximum-scale=5.0` pour le zoom tactile du logigramme ; marge basse de 80 px sur mobile.
- Test navigateur : 0 erreur console, tous les résultats rendus, 390 px sans défilement horizontal.

---

## 6. Checklist avant livraison

- [ ] Socle §7 et §8 : sources vérifiées, compte rendu des valeurs cliniques, discordances affichées
- [ ] Cas tests définis avant le code et tous conformes ; second passage indépendant si outil nouveau ou refonte
- [ ] Type conforme au §2 et à son fichier de référence
- [ ] Chaque résultat : outcomes sourcés · chimiothérapie · RT Timing / Volumes / Doses ou absence de RT motivée
- [ ] Badge de preuve sur chaque colonne protocole (type E) et chaque contrainte (type M)
- [ ] Bascule de thème avec clé propre à l'outil ; impression neutre ; export PDF
- [ ] Info-bulles D3 aussi complètes que les cartes résultat (types AB, E)

---

## 7. Écarts connus (non corrigés d'office)

Mise à niveau seulement lors d'une modification du fichier concerné (socle §8, modifications chirurgicales) :
- export PDF absent : `aria-mb-fusion`, `siop-mb-fusion`, `sfce-mb-fusion` (neuro) ;
- pas de bascule de thème : `NeuroOnco_Revue` et `mb-comparator` (neuro), `pentec_pediatric_rt` (pedia) ;
- clé de thème générique `theme` : `siop-cnsgct2-revue` (neuro) ;
- documents de revue antérieurs au gabarit R (styles variés) : `BIBLIO_LCT_CBNPC_M_consolidation`, `CSI_metastases_leptomeningees_CLB`, `germinome_M2_boost_contourage`, `ORN_dentaire_postRT_pediatrique`, `TSC_malin_RT_sauvetage`.

## 8. Pistes envisagées

QR code vers l'index ; impression du logigramme (export PNG) ; calculateur LQ séparé ; statut de recrutement en direct (API ClinicalTrials.gov) ; filtre par institution (France / SFCE / COG) ; mode tablette questionnaire et logigramme côte à côte.
