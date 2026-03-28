# Règles de création — Outils cliniques interactifs

> Fichier de référence pour la génération d'outils de stratification et de logigrammes en oncologie pédiatrique.  
> Dernière mise à jour : mars 2026 (v1.2 — ajout §2.4 toggle thème sombre/clair)

---

## 1. Deux types d'artefacts distincts

### Type A — Outil de stratification interactif (`sfce-mb-2026.html`, `siop-mb-stratification.html`, `aria-stratum-c.html`)

* Interface à **étapes séquentielles** (wizard / stepper)
* L'utilisateur répond à des questions cliniques une par une
* Le résultat final est une recommandation de prise en charge (protocole, doses de RT, chimiothérapie)
* Défilement vertical, page scrollable (`min-height: 100vh`, pas d'overflow hidden)
* Adapté à la **consultation au lit du patient / tablette**

### Type B — Logigramme D3.js (`flowchart-sfce.html`, `flowchart-siop.html`, `flowchart-aria.html`)

* Visualisation complète de l'arbre décisionnel en **SVG interactif**
* Vue globale de tout l'algorithme en un coup d'œil
* Navigation par **zoom + drag** (D3 zoom, `height: 100vh`, `overflow: hidden`)
* Adapté à la **présentation, formation, impression**

### Type C — Outil de consultation référentielle (`timmerman-constraints.html`, `quantec-constraints.html`)

* Interface à **étapes de filtrage** (région → OAR → fractionnement)
* Résultat : tableau ou cards de contraintes dosimétriques
* Supporte le **toggle thème sombre/clair** (voir §2.4)
* Adapté à la **planification dosimétrique, consultation directe TPS**

> **Règle implicite :** chaque algorithme clinique donne lieu aux **deux artefacts** Type A + Type B en parallèle. Ne créer qu'un seul des deux n'est pas la norme.

---

## 2. Identité visuelle — Système de design unifié

### 2.1 Typographie (invariable)

```
DM Serif Display  → titres, headers, noms de protocoles
DM Sans           → corps de texte, labels, boutons
DM Mono           → badges, métadonnées, valeurs numériques, sous-titres techniques
```

Import Google Fonts systématique :

```html
<link href="https://fonts.googleapis.com/css2?family=DM+Serif+Display:ital@0;1&family=DM+Mono:wght@400;500&family=DM+Sans:wght@300;400;500;600&display=swap" rel="stylesheet">
```

### 2.2 Deux palettes selon le type d'artefact

**Mode sombre** → outils de stratification (Type A), défaut des outils de consultation (Type C)

```css
--bg:       #0d1117;
--surface:  #161b22;
--surface2: #1c2330;
--surface3: #21262d;
--border:   #2a3441;
--border2:  #30363d;
--text:     #e6edf3;
--text2:    #8b949e;
--text3:    #6e7681;
```

**Mode clair / parchemin** → logigrammes D3 (Type B), option secondaire des outils de consultation (Type C)

```css
--bg:       #f5f0e8;
--surface:  #ede8dc;
--surface2: #e6e0d2;
--surface3: #ddd7c8;
--border:   #c8bfaa;
--border2:  #d4ccba;
--text:     #1a1208;
--text2:    #5a4f3a;
--text3:    #9a8f7a;
```

### 2.3 Palette sémantique (partagée, adaptée selon le mode)

| Sémantique | Mode sombre | Mode clair |
|-|-|-|
| Bas risque (LR) | `#3fb950` | `#2d7a3a` |
| Risque standard (SR) | `#388bfd` | `#1a5fa8` |
| Haut risque (HR) | `#f85149` | `#c0392b` |
| Discussion / spécial | `#d29922` | `#a0720a` |
| Formulaire / nourrisson | `#a371f7` | `#6b3fa0` |
| Accent orange | `#f0883e` | `#c45a10` |
| Cyan | `#39c5cf` | `#1a7a8a` |

> **Règle implicite :** ces couleurs ne sont jamais inversées entre les modes. Le vert = LR, le rouge = HR, sans exception.

---

### 2.4 Toggle thème sombre / clair (Type C — outils de consultation)

> **Applicabilité :** outils de consultation référentielle (Type C) uniquement. Les outils de stratification (Type A) restent en mode sombre fixe. Les logigrammes D3 (Type B) restent en mode parchemin fixe.

#### Principe

L'outil démarre en **mode sombre** (défaut). Un bouton dans le header permet de basculer vers le **mode clair parchemin** et inversement. Le choix est persisté en `localStorage` (`theme-quantec` ou clé spécifique à l'outil).

#### Implémentation CSS — `body.light` override

Toutes les variables de couleur sont définies dans `:root` (mode sombre). Le mode clair surcharge via `body.light` :

```css
:root {
  --bg: #0d1117; --surface: #161b22; --surface2: #1c2330; --surface3: #21262d;
  --border: #2a3441; --border2: #30363d;
  --text: #e6edf3; --text2: #8b949e; --text3: #6e7681;
  --accent: #388bfd; --cyan: #39c5cf; --orange: #f0883e;
  --purple: #a371f7; --yellow: #d29922; --green: #3fb950; --red: #f85149;
  --radius: 10px;
}
body.light {
  --bg: #f5f0e8; --surface: #ffffff; --surface2: #ede8dc; --surface3: #e6e0d2;
  --border: #c8bfaa; --border2: #d4ccba;
  --text: #1a1208; --text2: #5a4f3a; --text3: #9a8f7a;
  --accent: #1a5fa8; --cyan: #1a7a8a; --orange: #c45a10;
  --purple: #6b3fa0; --yellow: #a0720a; --green: #2d7a3a; --red: #c0392b;
}
```

> **Règle :** les surfaces des cards passent à `#ffffff` en mode clair (pas `#ede8dc`) pour contraster sur le fond parchemin. Ajouter `box-shadow: 0 1px 4px rgba(0,0,0,.06)` sur les cards en mode clair pour compenser l'absence de contraste de fond.

#### Header — background adaptatif

```css
/* Mode sombre (défaut) */
header {
  background: rgba(13,17,23,0.94);
  backdrop-filter: blur(12px);
}
/* Mode clair */
body.light header {
  background: rgba(245,240,232,0.97);
  backdrop-filter: blur(10px);
}
```

#### Bouton toggle dans le header

Position : entre le badge protocole et le bord droit du header. Symbole : `☀` (clair) / `◑` (sombre).

```html
<button class="btn-theme" id="btn-theme" onclick="toggleTheme()" title="Basculer le thème">◑</button>
```

```css
.btn-theme {
  width: 30px; height: 30px;
  border-radius: 6px;
  border: 1px solid var(--border2);
  background: transparent;
  color: var(--text2);
  font-size: 14px;
  cursor: pointer;
  transition: all .18s;
  display: flex; align-items: center; justify-content: center;
  margin-left: 8px;
  flex-shrink: 0;
}
.btn-theme:hover { border-color: var(--cyan); color: var(--cyan); }
```

#### Fonction JavaScript

```javascript
// ═══ THÈME ═══
var THEME_KEY = 'theme-[NOM_OUTIL]'; // ex: 'theme-quantec', 'theme-timmerman'

function initTheme() {
  var saved = localStorage.getItem(THEME_KEY);
  if (saved === 'light') applyTheme('light');
}

function toggleTheme() {
  var isLight = document.body.classList.contains('light');
  applyTheme(isLight ? 'dark' : 'light');
}

function applyTheme(mode) {
  if (mode === 'light') {
    document.body.classList.add('light');
    document.getElementById('btn-theme').textContent = '☀';
    document.getElementById('btn-theme').title = 'Passer en mode sombre';
    localStorage.setItem(THEME_KEY, 'light');
  } else {
    document.body.classList.remove('light');
    document.getElementById('btn-theme').textContent = '◑';
    document.getElementById('btn-theme').title = 'Passer en mode clair';
    localStorage.setItem(THEME_KEY, 'dark');
  }
}

// Appeler dans DOMContentLoaded :
document.addEventListener('DOMContentLoaded', function() {
  initTheme();
  // ... reste de l'init
});
```

#### Impression (`@media print`)

En impression, forcer systématiquement le fond blanc quelle que soit la palette active :

```css
@media print {
  body, body.light {
    background: #fff !important;
    color: #000 !important;
  }
  header, body.light header {
    position: static !important;
    background: #fff !important;
    border-bottom: 1px solid #ccc !important;
  }
  .btn-theme { display: none !important; }
  :root, body.light {
    --text: #000; --text2: #444; --text3: #888;
    --surface: #fff; --surface2: #f5f5f5;
    --border: #ddd; --border2: #eee;
    --cyan: #1a7a8a;
  }
}
```

#### Checklist toggle thème (à ajouter à §9)

* [ ] `body.light { ... }` surcharge toutes les variables `:root`
* [ ] `surface` des cards = `#ffffff` en mode clair (+ `box-shadow` léger)
* [ ] Header background adaptatif (`rgba` sombre / parchemin)
* [ ] Bouton `.btn-theme` présent dans le header, après le badge protocole
* [ ] `initTheme()` appelé dans `DOMContentLoaded`
* [ ] Clé `localStorage` spécifique à l'outil (`theme-[nom]`)
* [ ] `@media print` neutralise le thème (fond blanc forcé)

---

## 3. Structure HTML commune

### 3.1 Balises de base

```html
<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>[Protocole] · [Pathologie] — [Type d'outil]</title>
  <!-- Google Fonts -->
  <!-- D3.js si logigramme -->
</head>
```

* `lang="fr"` par défaut (même les outils ARIA, dont le contenu peut être en anglais, utilisent `lang="en"` exceptionnellement)
* Le `<title>` suit le pattern : `PROTOCOLE · Pathologie — Type`

### 3.2 Header (présent dans tous les artefacts)

**Composants invariables du header :**

1. **Logo-mark** : carré ~34×34px, border `1.5px solid var(--accent)`, font DM Mono, contient l'acronyme du protocole (ex. `SF`, `SI`, `AR`)
2. **Titre** : DM Serif Display, ~16px
3. **Sous-titre** : DM Mono, ~10-11px, couleur text3, contient la source et les instructions d'interaction
4. **Badge protocole** : pill arrondie, DM Mono, couleur spécifique au protocole, `margin-left: auto`
5. **Bouton reset** (logigrammes) OU **bouton toggle thème** (outils de consultation Type C) OU rien (outils de stratification)

**Header sticky** dans les outils de stratification (Type A) :

```css
position: sticky; top: 0; z-index: 100;
background: rgba(13,17,23,0.94);
backdrop-filter: blur(12px);
```

**Header fixe** dans les logigrammes (Type B) :

```css
flex-shrink: 0; z-index: 10;
background: rgba(245,240,232,0.97);
backdrop-filter: blur(10px);
```

**Header sticky avec toggle thème** dans les outils de consultation (Type C) :

```css
position: sticky; top: 0; z-index: 100;
background: rgba(13,17,23,0.94);       /* sombre par défaut */
backdrop-filter: blur(12px);
/* body.light header → background: rgba(245,240,232,0.97) */
```

---

## 4. Règles spécifiques — Outils de stratification (Type A)

### 4.1 Layout

```css
.container { max-width: 820-860px; margin: 0 auto; padding: 36px 24px 80px; }
```

* Large padding bottom (80px) pour le confort mobile
* `font-weight: 300` sur le `body` (DM Sans Light comme base)

### 4.2 Barre de progression

* Toujours présente en haut du contenu
* Compteur d'étapes : `Étape X / N`
* Barre de progression CSS animée (`transition: width 0.4s ease`)
* Style DM Mono pour le label

### 4.3 Cards de questions

* `background: var(--surface)`, `border: 1px solid var(--border)`, `border-radius: 10px`
* Accent de couleur en haut de la card (barre de 3px = `::before` ou `border-top colored`)
* Titre de question : DM Serif Display ou DM Sans 500
* Options : boutons full-width, hover avec `border-color` coloré + fond légèrement coloré
* État actif : `border-color: var(--accent)`, fond `rgba(accent, 0.08)`
* Pseudo-élément `::before` avec `transform: scaleX(0→1)` à l'hover

### 4.4 Card de résultat final

* Couleur de fond selon le groupe de risque (LR/SR/HR)
* Sections structurées avec icône ou label monospace : Protocole / Radiothérapie CSI / Boost / Chimiothérapie
* Badge de risque proéminent (LR / SR / HR) en haut à droite ou en header de card
* Bouton "Recommencer" (`⟳ Nouvelle stratification`)

### 4.5 Navigation entre étapes

* **Toujours** : sélection d'une option → affichage immédiat de l'étape suivante (pas de bouton "Suivant" séparé sauf si formulaire multi-critères)
* Animation de transition douce (`opacity`, `transform`)
* Possibilité de revenir en arrière (breadcrumb ou bouton "← Retour")

---

## 5. Règles spécifiques — Logigrammes D3.js (Type B)

### 5.1 Layout général

```css
body { height: 100vh; overflow: hidden; display: flex; flex-direction: column; }
header { flex-shrink: 0; }
.legend { flex-shrink: 0; }
#chart { flex: 1; overflow: hidden; cursor: grab; }
#chart:active { cursor: grabbing; }
```

### 5.2 Légende (entre header et chart)

* Toujours présente, ligne horizontale scrollable
* Swatches 12×12px, `border-radius: 2px` (sauf "décision" = circle `border-radius: 50%`)
* DM Mono 9px, couleur text3
* Texte d'aide à droite (`margin-left: auto`) : "Survoler pour les détails"

### 5.3 Structure de données (TREE)

```javascript
const TREE = {
  id: 'identifiant_unique',
  label: 'Texte\nsur plusieurs\nlignes',  // \n pour les sauts
  type: 'decision',                        // decision | form | result_lr | result_sr | result_hr | result_special
  edgeLabel: 'Condition\naffichée',        // label sur l'arête
  detail: { ... },                         // contenu du tooltip
  children: [ ... ]
};
```

### 5.4 Types de nœuds et styles

**Mode clair (logigrammes) :**

```javascript
const NODE_STYLE = {
  decision:       { fill: '#dce8f5', stroke: '#1a5fa8', text: '#0a1020' },
  form:           { fill: '#ece0f5', stroke: '#6b3fa0', text: '#0a1020' },
  result_lr:      { fill: '#d5edd8', stroke: '#2d7a3a', text: '#0a1a0a' },
  result_sr:      { fill: '#d5e4f5', stroke: '#1a5fa8', text: '#0a1020' },
  result_hr:      { fill: '#f5d8d5', stroke: '#c0392b', text: '#200505' },
  result_special: { fill: '#f5ead5', stroke: '#a0720a', text: '#201000' },
};
```

**Dimensions des nœuds :**

```javascript
const NODE_W = 120;   // nœuds de décision
const NODE_H = 42;
const DX = 160;       // espacement horizontal
const DY = 200;       // espacement vertical (profondeur)
// Nœuds résultat : width élargie à 140px, border-radius 8px (vs 4px)
```

### 5.5 Rendu des nœuds

* **Décision** : rectangle avec accent de couleur (barre 3px en haut), border-radius 4px
* **Résultat** : rectangle plus large, border-radius 8px, badge LR/SR/HR en coin supérieur droit
* **Shadow** : offset (+2, +2), `fill: rgba(0,0,0,0.08)`
* **Texte** : DM Mono pour les résultats, DM Sans pour les décisions

### 5.6 Arêtes (liens)

* Path D3 `linkHorizontal()` avec `x ↔ y` (arbre horizontal gauche→droite)
* Couleur de l'arête = couleur du nœud cible (`style.stroke`, `opacity: 0.4`)
* Markers flèche par type de nœud
* `edgeLabel` : centré sur l'arête, fond parchemin semi-transparent, DM Mono 8px

### 5.7 Tooltip au survol

```javascript
// Structure de detail :
detail: {
  title: 'Titre du tooltip',      // DM Mono gris
  q: 'Question / contexte',       // DM Sans 500
  protocol: 'Nom du protocole',
  csi: 'Dose CSI',
  boost: 'Dose boost',
  chemo: 'Schéma chimiothérapie'
}
```

* Tooltip fixe, `pointer-events: none`, `z-index: 100`
* Apparaît à `mouseenter`, disparaît à `mouseleave`
* Positionnement intelligent (reste dans le viewport)
* Highlight du nœud au survol : `stroke-width` augmenté + `filter: brightness(1.3)`

### 5.8 Zoom et navigation

```javascript
const zoom = d3.zoom()
  .scaleExtent([0.15, 3])
  .on('zoom', e => g.attr('transform', e.transform));

// Zoom initial auto-fit :
const scale = Math.min(0.85, W / treeW, H / treeH) * 0.88;
// Bouton reset : ⊙ Recentrer
```

* `maximum-scale=5.0` dans le viewport meta (pour le pinch mobile)
* Séparation des nœuds frères : `1.1`, entre familles : `1.5`

---

## 6. Règles de contenu clinique

### 6.1 Langue

* Interface en **français** par défaut
* Les noms de protocoles et acronymes restent en anglais (HIT-SKK, ACNS0334, CSI, GTR, etc.)
* Unités médicales standard : Gy (grays), ans, mois

### 6.2 Groupes de risque

Toujours représentés dans cet ordre et avec ces abréviations :

* **LR** = Low Risk / Bas risque
* **SR** = Standard Risk / Risque standard
* **HR** = High Risk / Haut risque
* **Special** = Discussion multidisciplinaire / Palliatif

### 6.3 Données affichées dans les tooltips / cards résultat

Structure **obligatoire** à maintenir dans cet ordre :

1. **Protocole** (nom du trial / recommandation)
2. **CSI** (dose d'irradiation cranio-spinale)
3. **Boost** (surimpression tumorale)
4. **Chimio** (schéma de chimiothérapie)
5. **Outcomes d'efficacité** — OS et/ou EFS/PFS à 5 ans (voir §6.5)
6. **Outcomes de toxicité** — effets tardifs notables si disponibles (voir §6.5)
7. **Sources** — références numérotées avec lien (voir §6.6)

### 6.4 Critères de stratification pédiatrique

Variables systématiquement prises en compte dans les algorithmes médulloblastome :

* Âge (< 3 ans / < 5 ans / ≥ 5 ans selon protocole)
* Sous-groupe moléculaire (WNT / SHH / Groupe 3 / Groupe 4)
* Stade métastatique (M0 / M+)
* Résidu tumoral (GTR / < GTR / résidu > 1.5 cm²)
* Histologie (CMB / LCA / DN / MBEN)
* Amplification MYC/MYCN
* Mutations spécifiques (TP53 pour SHH, SUFU pour < 5 ans)

---

## 6-A. RÈGLE — Collecte des données avant création ⚠️

> **Cette règle s'applique avant toute génération de code.**

Avant de créer un algorithme ou un logigramme, vérifier systématiquement la disponibilité des données suivantes. Si l'une d'elles manque, **demander explicitement à Ronan** de les fournir plutôt que de générer un outil incomplet ou approximatif.

### Données indispensables (bloquer la création si absentes)

* [ ] Critères de stratification complets et à jour (source/année du protocole)
* [ ] Noms des protocoles de traitement pour chaque bras
* [ ] Doses de radiothérapie (CSI + boost) pour chaque groupe
* [ ] Schémas de chimiothérapie pour chaque bras

### Données fortement souhaitables (demander si non fournies)

* [ ] OS et/ou EFS/PFS à 5 ans pour chaque groupe de risque (avec source)
* [ ] Taux de toxicités tardives majeures (si RT impliquée : déficit endocrinien, neurocognitif, second cancer)
* [ ] Références bibliographiques des études princeps (auteurs, journal, année, DOI)
* [ ] Liens ClinicalTrials.gov (NCT numbers) pour les essais actifs cités

### Données optionnelles (enrichissent l'outil si disponibles)

* [ ] Sous-analyses notables (ex. : impact protons vs photons, réponse histologique)
* [ ] Essais cliniques actifs pertinents (recrutement en cours)
* [ ] Date de la version du protocole utilisée

### Formulation type de la demande de données

Quand des données manquent, utiliser cette formulation :

> "Pour créer l'outil [NOM] de façon complète, il me manque les données suivantes : [liste]. Peux-tu me les fournir, ou indiquer si elles ne sont pas disponibles pour ce protocole ?"

---

## 6-B. RÈGLE — Données d'outcomes dans les nœuds résultat ⚠️

> **Objectif de référence : niveau ARIA Stratum C** (le plus abouti en termes de données d'outcome intégrées).

### Outcomes d'efficacité — à inclure systématiquement

Format dans le tooltip/card (DM Mono, couleur accent) :

```
OS 5 ans : XX %       (ou EFS, PFS selon la métrique principale de l'étude)
EFS 5 ans : XX %
n = XX patients       (taille de la cohorte de référence)
```

Exemples de données intégrées dans ARIA :

* `PFS 5 ans DN/MBEN M0 : 93 %` (HIT-SKK)
* `PFS 5 ans ~25 % (SJYC07)` (SHH M+)
* `PFS 5 ans ~8 %` (Non-WNT/non-SHH nourrisson M+)

### Outcomes de toxicité — à inclure si disponibles

Particulièrement pertinents pour les décisions de RT :

```
Toxicité endocrinienne : déficit GH XX %, hypothyroïdie XX %
Toxicité neurocognitive : QI -X points (si données disponibles)
Protons vs photons : GH XX % vs XX %
Second cancer : SMN XX % à 10 ans
Nécroses : XX % (si SRS / SBRT impliqués)
```

### Mise en forme dans les tooltips D3 (logigrammes)

Ajouter un champ `outcome` dans l'objet `detail` :

```javascript
detail: {
  title: '...',
  protocol: '...',
  csi: '...',
  boost: '...',
  chemo: '...',
  outcome: 'OS 5 ans : 80–85 % (avec chimio) · EFS 5 ans : 75 %',
  toxicity: 'Protons : GH 33 % vs 57 % (photons) · SMN équivalent à 10 ans',
  ref: '[1] Thomas 2021'   // numéro de référence court
}
```

### Mise en forme dans les cards résultat (outils de stratification)

Section dédiée avec fond légèrement coloré :

```html
<div class="outcome-row">
  <span class="outcome-label">OS 5 ans</span>
  <span class="outcome-val outcome-good">~80–85 %</span>
</div>
<div class="outcome-row toxicity">
  <span class="outcome-label">Déficit GH (protons)</span>
  <span class="outcome-val">33 % vs 57 % photons</span>
</div>
```

---

## 6-B2. RÈGLE — Détails de radiothérapie dans les nœuds résultat ⚠️

> **Cette règle s'applique à tout nœud résultat pour lequel une radiothérapie est indiquée (totalement ou partiellement).**

Dès qu'une RT est mentionnée dans un groupe de risque, les trois éléments suivants sont **obligatoires** dans la card résultat (outil de stratification) et dans le tooltip (logigramme D3). L'absence de l'un d'eux est un défaut bloquant.

### Les trois éléments obligatoires

#### 1. Timing
Préciser à quel moment la RT intervient dans la séquence de traitement :

```
Décision locale après C3 (localisé) ou C6 (métastatique)
RT préopératoire : après C4 — semaine 13. Chirurgie 4–6 sem après fin RT.
RT postopératoire : dès le 2ème cycle de chimio postopératoire.
Randomisation RT : fenêtre d'éligibilité (ex. après C3, avant début C5)
```

#### 2. Volumes cibles
Décrire la chaîne GTV → CTV → PTV avec les marges du protocole :

```
GTVp_pre → CTVp_pre : +1 cm (circumférentiel)
  Exception extrémité : +2 cm sup/inf, +1 cm circumférentiel
GTVp_post → CTVp_post : +0,5 cm (Phase 2 / boost uniquement)
Ganglions N1 : GTVn → CTVn +3 cm sup/inf (direction drainage lymphatique)
  Résidu bulky : GTVn_post → CTVn_post +0,5 cm
CTV → PTV : +3 à 10 mm (selon technique et IGRT locale)
```

#### 3. Doses et fractionnement
Distinguer explicitement chaque scénario dosimétrique :

```
Standard (SLFR, R0/R1 résécable ou RC) : 41.4 Gy / 23 fr / 4,5 sem
HLFR résécable escaladé (RT1B)         : 50.4 Gy / 28 fr — Ph1 : 41.4 Gy + Ph2 : +9 Gy / 5 fr
HLFR non résécable + RI escaladé (RT1C): 59.4 Gy / 33 fr — Ph1 : 41.4 Gy + Ph2 : +18 Gy / 10 fr
Ganglions standard                     : 41.4 Gy / 23 fr sur CTVn
Boost ganglionnaire bulky résiduel     : +9 Gy / 5 fr sur CTVn_post
```

### Cas où la RT n'est PAS indiquée

Indiquer explicitement la raison de l'absence de RT, ne pas laisser le champ vide :

```
PAS DE RT — IRS I R0 complet, Fusion–  (sous-groupe A)
RT non systématique — décision après C3 si doute résidu  (sous-groupes avec exception)
```

### Mise en forme — Outil de stratification (Type A)

Bloc RT dédié avec `border-left: 3px solid var(--sr)`, séparé du bloc chimiothérapie :

```html
<div class="rt-block">
  <div class="rt-block-title">Radiothérapie — Timing · Volumes · Doses</div>
  <div class="rt-section"><!-- Indication + randomisations disponibles --></div>
  <div class="rt-section"><!-- Timing --></div>
  <div class="rt-section"><!-- Volumes (grille 2 colonnes) --></div>
  <div class="rt-section"><!-- Doses (grille 2 colonnes, code couleur par niveau) --></div>
</div>
```

Code couleur des doses :
- `.rt-dose` (bleu `--sr`) → dose standard 41.4 Gy
- `.rt-dose.esc` (orange `--vhr`) → dose escaladée 50.4 Gy
- `.rt-dose.highesc` (rouge `--hr`) → dose très escaladée 59.4 Gy

### Mise en forme — Logigramme D3 (Type B)

```javascript
detail: {
  title: '...',
  timing: 'Décision après C3. RT préop → C4 sem.13.',
  rt_ind: 'RT indiquée — site primitif. RT1A/RT1B/RT1C selon résécabilité.',
  doses: '41.4 Gy / 23 fr (SLFR). HLFR RT1B : 50.4 Gy.',
  volumes: 'GTVp_pre → CTVp_pre +1 cm. CTV → PTV +3–10 mm.',
  chemo: '...',
  outcome: '...'
}
```

---

## 6-C. RÈGLE — Citations et sources ⚠️

> **Objectif de référence : NeuroOnco_Revue.html** (système de références numérotées avec DOI cliquables).

### Principe général

Tout outcome chiffré, toute recommandation de dose, tout critère de stratification **doit être sourcé**. Le numéro de référence `[n]` apparaît en superscript à côté de la donnée.

### Section références — obligatoire dans chaque outil

```html
<div class="ref-item">
  <span class="ref-num">1</span>
  <span class="ref-authors">Auteur A, Auteur B, et al.</span>
  <div class="ref-title">Titre complet de l'article</div>
  <div class="ref-journal">Journal, Année ; Volume(Numéro) : Pages</div>
  <div class="ref-doi">
    <a href="https://doi.org/XX.XXXX/XXXXX" target="_blank">https://doi.org/XX.XXXX/XXXXX</a>
  </div>
  <div class="ref-note">Note contextuelle optionnelle</div>
</div>
```

### Liens à inclure systématiquement quand disponibles

1. **DOI de l'article** → `https://doi.org/...`
2. **ClinicalTrials.gov** → `https://clinicaltrials.gov/study/NCTXXXXXXXX`
3. **PubMed** → `https://pubmed.ncbi.nlm.nih.gov/XXXXXXXX/`
4. **Recommandations officielles** (NCCN, SIOP, COG) → liens directs si publics

### Style CSS des références

```css
.refs-section { margin-top: 48px; border-top: 1px solid var(--border); padding-top: 24px; }
.refs-title { font-family: 'DM Mono', monospace; font-size: 10px; letter-spacing: 2px;
              text-transform: uppercase; color: var(--text3); margin-bottom: 16px; }
.ref-item { display: flex; gap: 12px; margin-bottom: 16px; font-size: 12px; }
.ref-num { font-family: 'DM Mono', monospace; font-size: 10px; color: var(--text3);
           min-width: 20px; padding-top: 2px; }
.ref-authors { font-weight: 500; }
.ref-title { color: var(--text); margin-top: 2px; }
.ref-journal { color: var(--text2); font-size: 11px; margin-top: 1px; }
.ref-doi a { color: var(--accent); font-size: 11px; font-family: 'DM Mono', monospace;
             text-decoration: none; }
.ref-doi a:hover { text-decoration: underline; }
.ref-note { color: var(--text3); font-size: 11px; font-style: italic; margin-top: 2px; }
```

---

## 6-D. RÈGLE — Export PDF du résultat ⚠️

> **Objectif :** permettre à l'utilisateur de garder une trace du résultat de stratification, utile pour le dossier patient ou la réunion de concertation (RCP).

### Implémentation standard

Bouton présent dans la **card de résultat final uniquement** :

```html
<button class="btn-pdf" onclick="printResult()">
  ⬇ Exporter en PDF
</button>
```

### Fonction JavaScript

```javascript
function printResult() {
  document.querySelectorAll('.step-card:not(.result), .progress-wrap').forEach(el => {
    el.dataset.hiddenForPrint = 'true';
    el.style.display = 'none';
  });
  window.print();
  document.querySelectorAll('[data-hidden-for-print]').forEach(el => {
    el.style.display = '';
    delete el.dataset.hiddenForPrint;
  });
}
```

### CSS d'impression (`@media print`)

```css
@media print {
  body { background: #fff !important; color: #000 !important; font-size: 11pt; }
  header { position: static !important; background: #fff !important;
           border-bottom: 1px solid #ccc !important; }
  .btn-pdf, .btn-reset, .btn-theme, .progress-wrap { display: none !important; }
  .result-card { border: 1.5px solid #333 !important; background: #fff !important;
                 box-shadow: none !important; break-inside: avoid; }
  .refs-section { break-before: auto; }
  a[href]::after { content: " (" attr(href) ")"; font-size: 8pt; color: #666; }
  .ref-doi a::after { content: none; }
}
```

### Footer de non-substitution (obligatoire à l'impression)

```html
<div class="print-footer">
  Outil d'aide à la décision — Ne se substitue pas au jugement clinique ni aux recommandations en vigueur.
  Généré le [DATE] · [NOM_DU_PROTOCOLE] · ronant99-lgtm.github.io
</div>
```

```css
.print-footer { display: none; }
@media print { .print-footer { display: block; font-size: 8pt; color: #999;
                                margin-top: 24px; border-top: 1px solid #ddd;
                                padding-top: 8px; } }
```

---

## 7. Conventions de code

### 7.1 Organisation du CSS

Sections commentées systématiquement :

```css
/* ── RESET ── */
/* ── HEADER ── */
/* ── THÈME ── */
/* ── LAYOUT ── */
/* ── LEGEND ── */
/* ── SVG CONTAINER ── */
/* ── TOOLTIP ── */
/* ── NODES ── */
/* ── PRINT ── */
```

### 7.2 Variables CSS

* Toutes les couleurs passent par des custom properties (`:root`)
* Mode sombre défini dans `:root`, mode clair dans `body.light` (Type C uniquement)
* Jamais de couleur codée en dur dans les règles CSS (sauf rgba avec valeur hex en fallback)

### 7.3 JavaScript

* Données de l'arbre séparées du code de rendu (constante `TREE` ou `NODES` en tête de script)
* Styles des nœuds dans un objet `NODE_STYLE` séparé
* Commentaires de section en majuscules : `// ═══ DONNÉES ═══`
* **Éviter les template literals imbriqués** dans les fonctions de rendu HTML — préférer la concaténation de strings ou des variables intermédiaires pour prévenir les erreurs d'échappement
* **Valider le JS** avec `node --check` avant livraison

### 7.4 Accessibilité mobile

* `meta viewport` avec `maximum-scale=5.0` pour les logigrammes (pinch-to-zoom)
* Padding bottom généreux (80px) dans les outils de stratification
* Touch events supportés via D3 zoom natif

---

## 8. Patterns UI récurrents

### Badge de protocole (header)

```html
<span class="hdr-badge">PROTOCOLE · Institution</span>
```

```css
.hdr-badge {
  font-family: 'DM Mono', monospace;
  font-size: 9px;
  padding: 2px 8px;
  border-radius: 20px;
  background: rgba(COLOR, 0.1);
  border: 1px solid rgba(COLOR, 0.35);
  color: var(--COLOR);
  margin-left: auto;
}
```

Couleur du badge = couleur distinctive du protocole (purple pour SFCE, blue pour SIOP, orange pour ARIA, cyan pour QUANTEC/Timmerman).

### Bouton toggle thème (Type C — header)

```html
<button class="btn-theme" id="btn-theme" onclick="toggleTheme()" title="Passer en mode clair">◑</button>
```

Toujours le symbole `◑` (mode sombre actif → cliquer pour aller en clair) ou `☀` (mode clair actif → cliquer pour aller en sombre).

### Section label (outils de stratification)

```css
.section-label {
  font-family: 'DM Mono', monospace;
  font-size: 10px;
  letter-spacing: 2px;
  color: var(--text3);
  text-transform: uppercase;
  margin-bottom: 12px;
}
```

### Bouton reset (logigrammes)

```html
<button class="btn-reset-zoom" id="btnReset">⊙ Recentrer</button>
```

Toujours le symbole `⊙` suivi de "Recentrer".

### Card résultat avec border colorée

```css
.result-card {
  border: 1px solid var(--RISK-COLOR);
  border-left: 3px solid var(--RISK-COLOR);
  background: rgba(RISK-COLOR, 0.05);
  border-radius: var(--radius);
}
```

---

## 9. Checklist avant livraison

### Données et contenu

* [ ] Les données d'outcome (OS/EFS 5 ans) sont présentes dans **chaque** nœud résultat
* [ ] Les données de toxicité tardive sont incluses si RT présente
* [ ] Chaque donnée chiffrée a un numéro de référence `[n]`
* [ ] La section références est présente et complète (auteurs, journal, année, DOI cliquable)
* [ ] Les NCT numbers des essais cités sont liés vers clinicaltrials.gov
* [ ] Les données manquantes ont été demandées à Ronan avant la génération

### Radiothérapie (voir règle §6-B2)

* [ ] Chaque nœud résultat avec RT indique le **timing** (cycle de référence, semaine)
* [ ] Les **volumes** GTV→CTV→PTV avec marges numériques sont présents
* [ ] Toutes les **doses** sont détaillées par scénario (standard / escaladé / très escaladé)
* [ ] Si N1 : RT ganglionnaire avec ses propres volumes (CTVn) et doses documentée
* [ ] Si métastatique : RT2 ou RT sur métastases documentée avec doses par site
* [ ] Les nœuds sans RT indiquent explicitement la raison de l'absence

### Technique

* [ ] Les trois polices DM sont importées (Serif Display + Sans + Mono)
* [ ] Tous les nœuds du TREE ont un `id` unique
* [ ] Les tooltips contiennent les 5 champs standard (protocole, CSI, boost, chimio, outcome)
* [ ] Le zoom initial est calculé automatiquement pour afficher tout l'arbre
* [ ] Le bouton "⊙ Recentrer" fonctionne
* [ ] La légende couvre tous les types de nœuds utilisés
* [ ] Les labels d'arêtes (`edgeLabel`) sont présents sur chaque branche significative
* [ ] L'outil est testé sur mobile (viewport correct)
* [ ] `lang="fr"` sur la balise `<html>`
* [ ] Le titre HTML suit le pattern `PROTOCOLE · Pathologie — Type`
* [ ] Le JS a été validé syntaxiquement (`node --check`) avant livraison

### Toggle thème (Type C uniquement — voir §2.4)

* [ ] `body.light { ... }` surcharge toutes les variables `:root`
* [ ] `--surface` des cards = `#ffffff` en mode clair, avec `box-shadow` léger
* [ ] Header background adaptatif (sombre / parchemin)
* [ ] Bouton `.btn-theme` présent dans le header, symbole `◑` / `☀`
* [ ] `initTheme()` appelé dans `DOMContentLoaded`
* [ ] Clé `localStorage` spécifique à l'outil (`theme-[nom]`)
* [ ] `@media print` neutralise le thème et masque `.btn-theme`

### Export PDF

* [ ] Le bouton "⬇ Exporter en PDF" est présent dans la card résultat
* [ ] Le `@media print` masque les éléments de navigation et `.btn-theme`
* [ ] Le footer de non-substitution est présent (visible uniquement à l'impression)
* [ ] Les références pertinentes s'impriment avec le résultat

---

## 10. Extensions futures envisagées

* **QR code** : lien vers l'index de tous les outils (`index.html` sur GitHub Pages)
* **Mode impression logigrammes** : version noir et blanc des arbres D3
* **Calculateur LQ** : outil séparé pour l'équivalence de dose (déjà existant dans le repo)
* **Essais cliniques actifs** : section dédiée dans chaque outil, avec statut de recrutement
* **Filtrage par institution** : afficher uniquement les protocoles accessibles en France / SFCE
* **Toggle thème sur Type A** : si demande utilisateur — nécessite refactorisation des palettes
