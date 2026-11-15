# rules — règles de travail avec Claude

Règles personnelles (R. Tanguy, radiothérapie, Centre Léon Bérard) pour la recherche bibliographique, les outils cliniques HTML et les présentations.

**Version 3.0 — novembre 2026.** Refonte : 12 fichiers ramenés à 7, sans perte de règle clinique (voir « Historique » en bas).

## Aiguillage : quelle règle pour quelle demande

| La demande porte sur… | Exemples de formulation | Fichier | Section |
|---|---|---|---|
| Une **question** clinique ciblée | « faut-il… », « apport de X », « X vs Y », « place de X dans Y », « quels critères pour… » | `BIBLIO.md` | Mode **PICO** |
| Une **indication** de RT déjà retenue | « situation thérapeutique : … », « fiche indication pour… » | `BIBLIO.md` | Mode **Fiche indication RT** |
| Une **maladie rare** dans son ensemble | nom d'une entité rare + « prise en charge », « conduite à tenir », « fiche sur… » | `BIBLIO.md` | Mode **Pathologie rare** |
| Un **domaine** de connaissances | « que sait-on sur… », sujet large ou non clinique | `BIBLIO.md` | Mode **État de l'art** |
| La relecture d'un **manuscrit** | « review de cet article », « fiche de lecture » | `RULES-peer-review.md` | — |
| Un **outil clinique HTML** (questionnaire, logigramme, tableau de bord, atlas, mode opératoire) | « fais l'outil… », « ajoute au MO… » | `OUTILS-cliniques.md` | Types AB, C, D, E, M |
| Une **présentation** (diaporama HTML ou PowerPoint CLB) | « fais une présentation sur… » | `PRESENTATION-CLB.md` | — |

**Toujours applicables** à tout contenu clinique : `SOCLE-clinique.md` (cadrage, niveaux de preuve, références, vérification des sources, règles de modification).
**Apparence** de tout fichier HTML : `DESIGN.md` (trois gabarits : document de revue, outil interactif, charte CLB).

### Cas limites

- Une **technique sans comparateur** (« place de la RT adaptative ») relève du mode PICO, comparateur facultatif.
- Une **pathologie rare avec une question RT précise** (« dose de sauvetage d'un TSC malin ») relève du mode PICO ; le mode Pathologie rare est réservé à la vue d'ensemble.
- Une **pathologie fréquente** (« prise en charge du CBNPC ») ne relève pas du mode Pathologie rare : demander la question précise (PICO) ou l'indication (Fiche).

## Fichiers

| Fichier | Contenu |
|---|---|
| `README.md` | Ce fichier : aiguillage |
| `SOCLE-clinique.md` | Règles communes à tout contenu clinique |
| `BIBLIO.md` | Les 4 modes de recherche bibliographique |
| `DESIGN.md` | Gabarits visuels HTML (R : revue · O : outil · CLB : charte institutionnelle) |
| `OUTILS-cliniques.md` | Types d'outils cliniques HTML et règles de contenu |
| `PRESENTATION-CLB.md` | Architecture des diaporamas HTML CLB |
| `RULES-peer-review.md` | Relecture critique d'un manuscrit (inchangé) |

## Historique

| Ancien fichier | Devenu |
|---|---|
| `RULES-pico-search.md`, `RULES-fiche-indication-RT.md`, `RULES-pathology_brief.md`, `RULES-academique-gen-consensus.md` | `BIBLIO.md` (+ parties communes dans `SOCLE-clinique.md`) |
| `RULES-clinical-tools.md` (v2.3) et sa copie périmée `RULES-clinical-tools-v2.md` (v2.2) | `OUTILS-cliniques.md` (règles 6-E et 6-F dans `SOCLE-clinique.md`, système visuel dans `DESIGN.md`) |
| `CLB-presentation-rule.md` | `DESIGN.md` § CLB |
| `RULES-presentation-html-CLB.md` | `PRESENTATION-CLB.md` |
| `RULES-touristic-map.md` | déplacé dans le dépôt `tourisme` |
| `RULES-night-science.md`, `RULES-day-science.md` | supprimés (15/11/2026) |

Les anciennes versions restent consultables dans l'historique git.
