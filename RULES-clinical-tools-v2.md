# Règles de création — Outils cliniques interactifs

> Fichier de référence pour la génération d'outils de stratification et de logigrammes en oncologie pédiatrique.  
> Dernière mise à jour : avril 2026 (v2.1 — ajout Type D : tableau de bord de revue clinique avec toggle thème universel)

---

## 1. Architecture des artefacts — nouveau modèle fusionné

> **⚠️ Changement majeur v2.0** : le modèle à deux fichiers séparés (Type A + Type B) est **remplacé** par un fichier unique fusionné (Type AB). Les Types A, B séparés ne sont plus la norme. Le Type C (consultation référentielle) reste inchangé.

### Type AB — Outil fusionné : wizard + logigramme dans un seul fichier

**Référence d'implémentation :** `cog-wilms-fusion.html`, `us-rms-cog.html`

Structure HTML :
```
header (sticky, z-index 200)
  └─ logo · titre · sous-titre · badge · [btn-theme]
tab-nav (sticky sous header, z-index 150)
  ├─ onglet "⬡ Stratification pas-à-pas"   → #panel-wizard
  └─ onglet "⬡ Logigramme interactif"      → #panel-flow
panel-wizard (display:block par défaut)
  └─ .container > barre progression + #app + refs
panel-flow (display:none, height calculée dynamiquement)
  └─ SVG D3 + .btn-reset-zoom + .flow-legend + .tooltip
```

**Règles de layout du panel-flow :**
```javascript
// Hauteur calculée dynamiquement après rendu du header + tab-nav
function fixLayout() {
  var hh = document.querySelector('header').offsetHeight;
  var navH = document.querySelector('.tab-nav').offsetHeight;
  document.getElementById('panel-flow').style.height =
    (window.innerHeight - hh - navH) + 'px';
}
fixLayout();
window.addEventListener('resize', fixLayout);
```
```css
#panel-flow { overflow: hidden; position: relative; }
#chart { width: 100%; height: 100%; cursor: grab; }
#chart:active { cursor: grabbing; }
```

**Construction différée du logigramme :**
```javascript
function switchTab(tab) {
  // masquer tous les panels, afficher le bon
  if (tab === 'flow' && !window._flowBuilt) {
    buildFlow();          // D3 construit une seule fois
    window._flowBuilt = true;
  }
}
```
> Le SVG D3 n'est construit qu'au **premier accès** à l'onglet logigramme, jamais au chargement initial. Cela évite les dimensions nulles (SVG non visible = clientWidth 0) et accélère le chargement.

**Tab-nav CSS :**
```css
.tab-nav {
  display: flex;
  border-bottom: 1px solid var(--border);
  background: var(--surface);
  position: sticky;
  z-index: 150;
  /* top: calculé par JS après hauteur réelle du header */
}
.tab-btn {
  flex: 1;
  padding: 11px 16px;
  border: none;
  background: none;
  color: var(--text2);
  font-family: 'DM Sans', sans-serif;
  font-size: 12px;
  font-weight: 500;
  cursor: pointer;
  border-bottom: 2px solid transparent;
  transition: all .2s;
}
.tab-btn.active { color: var(--accent); border-bottom-color: var(--accent); }
body.light .tab-nav { background: var(--surface2); }
body.light .tab-btn:hover { background: rgba(0,0,0,.04); }
```

### Type C — Outil de consultation référentielle (inchangé)

* Interface à **étapes de filtrage** (région → OAR → fractionnement)
* Résultat : tableau ou cards de contraintes dosimétriques
* Supporte le **toggle thème sombre/clair** (voir §2.4)
* Adapté à la **planification dosimétrique, consultation directe TPS**

> **Les anciens Types A et B distincts** (`sfce-mb-2026.html`, `flowchart-sfce.html`, etc.) restent valides pour les outils déjà livrés. Tout **nouvel outil** suit le modèle Type AB.

### Type D — Tableau de bord de revue clinique (nouveau — v2.1)

**Référence d'implémentation :** `NeuroOnco_Revue.html`, `WilmsTumor_Revue.html`

**Usage :** Synthèse thématique multi-entités d'une pathologie — protocoles RT, essais actifs, signal clinique, références. Outil de consultation et de veille, pas de stratification individuelle.

Structure HTML :
```
header (sticky, z-index 100)
  └─ titre · sous-titre · métadonnées · disclaimer · [btn-theme]
main (.container)
  ├─ #landing (grille de nav-cards + tableau comparatif)
  └─ .panel × N (un par entité/scénario)
       ├─ .back-btn
       ├─ .panel-header (titre + sous-titre)
       ├─ .slabel + .rt-table  (protocoles RT)
       ├─ .slabel + .trials-grid  (essais actifs — trial-cards)
       ├─ .slabel + .signal-box  (signal clinique 2025–2026)
       └─ .slabel + .refs-list  (références DOI cliquables)
```

**Règles spécifiques Type D :**

* **Landing page** : grille de `nav-card` (une par entité/scénario), chacune avec numéro, nom, dose clé, nb d'essais, tag coloré. Suivie d'un tableau comparatif cross-entités.
* **Panels** : affichés/masqués via `show(id)` / `back()` — pas de wizard, pas de D3.
* **Trial cards** : `.trial-card` avec NCT ID cliquable → `clinicaltrials.gov`, badge statut (RECRUITING / ACTIVE · NR / NOT YET RECRUITING), tags modalité (`.th-rt` / `.th-sg` / `.th-ct` / `.th-ch`), sponsor + contexte.
* **Signal clinique** : bloc `.signal-box` narratif (3–5 phrases), highlights colorés via `.signal-hl` / `.signal-hl2` / `.signal-hl3`.
* **Références** : toutes avec DOI cliquable et `.ref-note` pour les données chiffrées clés.
* **Toggle thème** : obligatoire — même implémentation que §2.4 (voir ci-dessous), clé `localStorage` : `theme-[pathologie]-revue`.
* **Langue** : `lang="en"` si contenu anglophone (contexte St. Jude / COG / observership).

**CSS spécifique Type D (light mode overrides) :**
```css
body.light header {
  background: rgba(245,240,232,0.97);
  backdrop-filter: blur(10px);
}
body.light .nav-card:hover { background: #f8f4ee; }
body.light .ct tr:hover td,
body.light .rt-table tr:hover td { background: #ede8dc; }
body.light .trial-card { box-shadow: 0 1px 4px rgba(0,0,0,.06); }
body.light .signal-box { color: #3a2e1a; }
```

---

## 2. Identité visuelle — Système de design unifié

### 2.1 Typographie (invariable)

```
DM Serif Display  → titres principaux, noms de protocoles (q-title, result-title)
DM Sans           → corps de texte, labels, boutons, options
DM Mono           → badges, métadonnées, valeurs numériques, doses, outcomes, références
```

Import Google Fonts systématique :

```html
<link href="https://fonts.googleapis.com/css2?family=DM+Serif+Display:ital@0;1&family=DM+Mono:wght@400;500&family=DM+Sans:wght@300;400;500;600&display=swap" rel="stylesheet">
```

### 2.2 Palette de couleurs — toggle universel sombre / clair

> **Changement v2.0 :** le toggle thème sombre/clair est désormais **universel** — il s'applique à tous les types d'artefacts (AB, C et D), y compris le wizard, le logigramme D3 et les tableaux de bord de revue clinique. Il n'est plus réservé au Type C.

**`:root` = mode sombre (défaut pour tous les outils) :**

```css
:root {
  --bg:       #0d1117;
  --surface:  #161b22;
  --surface2: #1c2330;
  --surface3: #21262d;
  --border:   #2a3441;
  --border2:  #30363d;
  --text:     #e6edf3;
  --text2:    #8b949e;
  --text3:    #6e7681;
  --green:    #3fb950;
  --blue:     #388bfd;
  --red:      #f85149;
  --yellow:   #d29922;
  --purple:   #a371f7;
  --orange:   #f0883e;
  --cyan:     #39c5cf;
  --accent:   /* couleur distinctive du protocole — orange pour COG/NCCN, blue pour SIOP, purple pour SFCE */;
  --radius:   10px;
}
```

**`body.light` = mode clair parchemin :**

```css
body.light {
  --bg:       #f5f0e8;
  --surface:  #ffffff;
  --surface2: #ede8dc;
  --surface3: #e6e0d2;
  --border:   #c8bfaa;
  --border2:  #d4ccba;
  --text:     #1a1208;
  --text2:    #5a4f3a;
  --text3:    #9a8f7a;
  --green:    #2d7a3a;
  --blue:     #1a5fa8;
  --red:      #c0392b;
  --yellow:   #a0720a;
  --purple:   #6b3fa0;
  --orange:   #c45a10;
  --cyan:     #1a7a8a;
  --accent:   /* version claire de la couleur du protocole */;
}
```

> **Règle invariante :** vert = LR, rouge = HR, sans exception entre les deux modes.

**Surcharges spécifiques en mode clair :**

```css
/* Header */
body.light header {
  background: rgba(245,240,232,0.97);
  backdrop-filter: blur(10px);
}
/* Tab-nav */
body.light .tab-nav { background: var(--surface2); }
body.light .tab-btn:hover { background: rgba(0,0,0,.04); }
/* Cards (surface blanche sur fond parchemin) */
body.light .step-card,
body.light .result-card,
body.light .r-row,
body.light .rt-block,
body.light .outcomes-block {
  box-shadow: 0 1px 4px rgba(0,0,0,.06);
}
/* Tooltip D3 — reste sombre dans les deux modes (meilleure lisibilité sur SVG) */
.tt-body {
  background: rgba(13,17,23,.97);
  border: 1px solid rgba(var(--accent-rgb), .2);
  /* Invariant : le tooltip D3 ne switche PAS avec le thème */
}
/* Options wizard */
body.light .opt-btn:hover {
  background: rgba(var(--accent-rgb), .06);
  border-color: var(--accent);
}
/* Bouton reset zoom */
body.light .btn-reset-zoom { background: var(--surface); }
```

### 2.3 Palette sémantique (invariante entre les modes)

| Sémantique | Sombre | Clair |
|-|-|-|
| Bas risque (LR / VLR) | `#3fb950` | `#2d7a3a` |
| Risque standard (SR) | `#388bfd` | `#1a5fa8` |
| Haut risque (HR) | `#f85149` | `#c0392b` |
| Prédisposition / spécial | `#d29922` | `#a0720a` |
| Bilatéral / nourrisson | `#a371f7` | `#6b3fa0` |
| Accent orange (COG/NCCN) | `#f0883e` | `#c45a10` |
| Cyan (QUANTEC/Timmerman) | `#39c5cf` | `#1a7a8a` |

### 2.4 Implémentation JS du toggle thème (universel)

```javascript
// ═══ THÈME ═══
var THEME_KEY = 'theme-[nom-outil]'; // ex: 'theme-cog-wilms', 'theme-us-rms'

function initTheme() {
  try {
    if (localStorage.getItem(THEME_KEY) === 'light') applyTheme('light');
  } catch(e) {}
}

function toggleTheme() {
  applyTheme(document.body.classList.contains('light') ? 'dark' : 'light');
}

function applyTheme(mode) {
  var btn = document.getElementById('btn-theme');
  if (mode === 'light') {
    document.body.classList.add('light');
    btn.textContent = '☀';
    btn.title = 'Passer en mode sombre';
    try { localStorage.setItem(THEME_KEY, 'light'); } catch(e) {}
  } else {
    document.body.classList.remove('light');
    btn.textContent = '◑';
    btn.title = 'Passer en mode clair';
    try { localStorage.setItem(THEME_KEY, 'dark'); } catch(e) {}
  }
}

// Appel en bas de script (pas dans DOMContentLoaded — le HTML est inline) :
initTheme();
renderStep('premiere_etape');
fixLayout();
```

> **`try/catch` autour de `localStorage`** : obligatoire pour éviter les erreurs en mode privé ou iframe.

**Bouton toggle dans le header (position fixe) :**

```html
<!-- Dans le header, après le badge protocole -->
<span class="hdr-badge">COG · NCCN 2025</span>
<button class="btn-theme" id="btn-theme" onclick="toggleTheme()" title="Passer en mode clair">◑</button>
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
.btn-theme:hover { border-color: var(--accent); color: var(--accent); }
```

---

## 3. Structure HTML commune (Type AB)

### 3.1 Balises de base

```html
<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=5.0">
  <title>[PROTOCOLE] · [Pathologie] — Stratification & Logigramme</title>
  <link href="https://fonts.googleapis.com/css2?family=DM+Serif+Display:ital@0;1&family=DM+Mono:wght@400;500&family=DM+Sans:wght@300;400;500;600&display=swap" rel="stylesheet">
  <script src="https://cdnjs.cloudflare.com/ajax/libs/d3/7.8.5/d3.min.js"></script>
</head>
```

* `lang="fr"` par défaut
* `maximum-scale=5.0` (pinch-to-zoom mobile dans le logigramme)
* D3 **toujours importé** même si le logigramme est construit différé — le script doit être disponible
* Le `<title>` suit le pattern : `PROTOCOLE · Pathologie — Stratification & Logigramme`

### 3.2 Header (composants invariables)

```html
<header>
  <div class="hdr-logo">XX</div>           <!-- 2–3 lettres acronyme -->
  <div class="hdr-texts">
    <div class="hdr-title">Pathologie — Stratification & Logigramme</div>
    <div class="hdr-sub">Protocole · Version · Essais de référence</div>
  </div>
  <span class="hdr-badge">PROTOCOLE · Année</span>
  <button class="btn-theme" id="btn-theme" onclick="toggleTheme()" title="Passer en mode clair">◑</button>
</header>
```

```css
header {
  position: sticky; top: 0; z-index: 200;
  background: rgba(13,17,23,0.96);
  backdrop-filter: blur(12px);
  border-bottom: 1px solid var(--border);
  padding: 12px 20px;
  display: flex; align-items: center; gap: 12px; flex-wrap: nowrap;
}
.hdr-logo {
  width: 32px; height: 32px;
  border: 1.5px solid var(--accent); border-radius: 6px;
  display: flex; align-items: center; justify-content: center;
  font-family: 'DM Mono', monospace; font-size: 9px;
  color: var(--accent); font-weight: 500; flex-shrink: 0;
}
.hdr-texts { flex: 1; min-width: 0; }
.hdr-title {
  font-family: 'DM Serif Display', serif; font-size: 15px;
  white-space: nowrap; overflow: hidden; text-overflow: ellipsis;
}
.hdr-sub { font-family: 'DM Mono', monospace; font-size: 9px; color: var(--text3); margin-top: 2px; }
.hdr-badge {
  font-family: 'DM Mono', monospace; font-size: 9px;
  padding: 3px 10px; border-radius: 20px;
  background: rgba(VAR-ACCENT-RGB, .1); border: 1px solid rgba(VAR-ACCENT-RGB, .3);
  color: var(--accent); margin-left: auto; flex-shrink: 0; white-space: nowrap;
}
```

---

## 4. Wizard (panel-wizard) — Règles spécifiques

### 4.1 Layout et progression

```css
.container { max-width: 860px; margin: 0 auto; padding: 32px 20px 80px; }
.progress-wrap { margin-bottom: 28px; }
.progress-header {
  display: flex; justify-content: space-between;
  font-family: 'DM Mono', monospace; font-size: 10px;
  color: var(--text3); margin-bottom: 8px;
}
.progress-bar-bg { height: 3px; background: var(--surface3); border-radius: 2px; overflow: hidden; }
.progress-fill {
  height: 100%; background: var(--accent);
  border-radius: 2px; transition: width .4s ease;
}
```

La barre de progression indique deux informations : `Étape X / N` (left) + nom de l'étape courante (right).

### 4.2 Cards de questions

```css
.step-card {
  background: var(--surface); border: 1px solid var(--border);
  border-radius: var(--radius); padding: 24px; margin-bottom: 14px;
  position: relative; animation: fadeSlide .3s ease; overflow: hidden;
}
.step-card::before {
  content: ''; position: absolute; top: 0; left: 0; right: 0;
  height: 3px; background: var(--accent); /* accent bar couleur protocole */
}
@keyframes fadeSlide { from { opacity:0; transform:translateY(8px) } to { opacity:1; transform:translateY(0) } }
.q-title { font-family: 'DM Serif Display', serif; font-size: 19px; margin-bottom: 6px; line-height: 1.4; }
.q-sub { font-size: 12px; color: var(--text2); margin-bottom: 20px; line-height: 1.55; }
```

### 4.3 Boutons d'options

```css
.opt-btn {
  background: var(--surface2); border: 1px solid var(--border2);
  border-radius: 8px; padding: 13px 16px;
  color: var(--text); font-family: 'DM Sans', sans-serif; font-size: 13px;
  cursor: pointer; text-align: left; transition: all .2s;
  position: relative; overflow: hidden;
}
.opt-btn::before {
  content: ''; position: absolute; left: 0; top: 0; bottom: 0;
  width: 3px; background: var(--accent);
  transform: scaleY(0); transition: transform .2s;
}
.opt-btn:hover { border-color: var(--accent); background: rgba(ACCENT, .06); }
.opt-btn:hover::before { transform: scaleY(1); }
.opt-btn strong { display: block; margin-bottom: 3px; font-weight: 600; }
.opt-desc { font-size: 11px; color: var(--text3); margin-top: 3px; }
```

### 4.4 Navigation — moteur JS

```javascript
// ─── État et historique ───
var wState = {};
var navStack = [];        // ⚠️ Ne jamais nommer 'history' — conflit avec window.history
var currentStep = 'premiere_etape';

// ─── Navigation avant ───
function navigate(sid, val) {
  navStack.push({ step: currentStep, state: JSON.parse(JSON.stringify(wState)) });
  wState[sid] = val;
  var nxt = STEPS[sid].next(val);
  if (nxt === '__compute') {
    renderResult(computeResult(wState));
  } else if (nxt.startsWith('_result_')) {
    renderResult(nxt.slice(8));       // '_result_fhwt_hr' → 'fhwt_hr'
  } else {
    currentStep = nxt;
    renderStep(nxt);
  }
}

// ─── Retour arrière ───
function goBack() {
  if (!navStack.length) return;
  var prev = navStack.pop();
  wState = prev.state;
  currentStep = prev.step;
  renderStep(prev.step);
}

// ─── Recommencer ───
function restart() {
  wState = {}; navStack = []; currentStep = 'premiere_etape';
  document.getElementById('refsSection').style.display = 'none';
  renderStep('premiere_etape');
}
```

> **Règle critique** : utiliser `navStack` (jamais `history`) pour stocker l'historique de navigation. `window.history` est un objet natif du navigateur sans méthode `.push()`.

**Structure d'un step :**
```javascript
var STEPS = {
  mon_step: {
    label: 'Nom affiché dans la progression',
    num: 1, total: 6,      // pour la barre de progression
    q: 'Question affichée en DM Serif Display',
    sub: 'Sous-titre explicatif en DM Sans',
    opts: [
      { val: 'opt_a', label: 'Libellé court', desc: 'Description optionnelle (DM Sans text3)' },
      { val: 'opt_b', label: 'Autre option',  desc: '...' }
    ],
    next: function(val) {
      // Retourner soit un id de step, soit '_result_KEY', soit '__compute'
      if (val === 'opt_a') return '_result_nom_result';
      return 'step_suivant';
    }
  }
};
```

### 4.5 Résultats — structure de données

```javascript
var RES = {
  nom_result: {
    cls:      'sr',           // classe CSS de la card : vlr | lr | sr | hr | bilat | anapla | predispo
    badge:    'SR',           // texte du badge risque
    badgeCls: 'sr',           // classe CSS du badge
    title:    'FHWT — Risque Standard (SR), Stade III',
    chemo:    'HTML string — description de la chimiothérapie',
    rt: {                     // null si pas de RT
      timing:  'J10–J14 postopératoire',
      volumes: 'Description GTV→CTV→PTV avec marges',
      doses:   'HTML avec balises <strong> pour les doses clés'
    },
    rtText:   null,           // texte alternatif si rt:null (ex: 'PAS DE RT — raison')
    os:       '~95–98 %',
    efs:      '~83–90 %',
    n:        'large cohorte',
    refs:     '[1]',
    note:     'Note clinique importante'
  }
};
```

### 4.6 Card résultat — structure HTML générée

```javascript
function buildResult(key) {
  var r = RES[key];

  // ── Couleur OS/EFS ──
  var osNum = parseFloat(r.os);
  var osCls = osNum >= 95 ? 'ov-good' : (osNum >= 80 ? 'ov-mid' : 'ov-bad');

  // ── Bloc RT ──
  var rtHtml = '';
  if (r.rt) {
    rtHtml = '<div class="rt-block">'
      + '<div class="rt-block-title">Radiothérapie — Timing · Volumes · Doses</div>'
      + '<div class="rt-section">'
      + '  <div class="rt-section-hdr">Timing <span class="rt-tag">' + r.rt.timing + '</span></div>'
      + '</div>'
      + '<div class="rt-section">'
      + '  <div class="rt-section-hdr">Volumes cibles</div>'
      + '  <div class="rt-note">' + r.rt.volumes + '</div>'
      + '</div>'
      + '<div class="rt-section">'
      + '  <div class="rt-section-hdr">Doses prescrites</div>'
      + '  <div class="rt-note">' + r.rt.doses + '</div>'
      + '</div>'
      + '</div>';
  } else {
    rtHtml = '<div class="no-rt-box">' + (r.rtText || 'Pas de RT indiquée') + '</div>';
  }

  return '<button class="btn-back" onclick="goBack()">← Retour</button>'
    + '<div class="result-card ' + r.cls + '">'
    + '<div class="result-header">'
    + '  <div class="result-title">' + r.title + '</div>'
    + '  <span class="risk-badge ' + r.badgeCls + '">' + r.badge + '</span>'
    + '</div>'
    + '<div class="outcomes-block">...</div>'     // OS, EFS, cohorte, réf.
    + '<div class="r-row">...</div>'              // chimiothérapie
    + rtHtml
    + (r.note ? '<div class="r-row">...</div>' : '')
    + '<div class="result-actions">'
    + '  <button class="btn-restart" onclick="restart()">⟳ Nouvelle stratification</button>'
    + '  <button class="btn-pdf" onclick="printResult()">⬇ Exporter PDF</button>'
    + '</div>'
    + '</div>';
}
```

---

## 5. Logigramme D3 (panel-flow) — Règles spécifiques

### 5.1 Construction différée

```javascript
function switchTab(tab) {
  document.querySelectorAll('.panel').forEach(function(p) { p.classList.remove('active'); });
  document.querySelectorAll('.tab-btn').forEach(function(b) { b.classList.remove('active'); });
  document.getElementById('panel-' + tab).classList.add('active');
  document.getElementById('tab-' + tab).classList.add('active');
  if (tab === 'flow' && !window._flowBuilt) {
    buildFlow();
    window._flowBuilt = true;
  }
}
```

### 5.2 Nœuds — deux formes possibles selon la densité de l'arbre

**Ellipses** (arbres larges, nombreux nœuds, positions manuelles) — référence `us-rms-cog.html` :
```javascript
// Positions x/y définies manuellement dans le tableau nodes[]
var nodes = [
  { id: 'root', x: 0,   y: 0,   type: 'd',  label: 'Question\nracine' },
  { id: 'r_lr', x: -200, y: 200, type: 'lr', label: 'BAS RISQUE\nEE4A' },
  // ...
];
// Rendu
grp.append('ellipse').attr('rx', st.r).attr('ry', st.ry)
  .attr('fill', st.fill).attr('stroke', st.stroke).attr('stroke-width', 1.8);
```

**Rectangles + hiérarchie D3** (arbres profonds, moins de nœuds) — référence `flowchart-aria.html` :
```javascript
var root = d3.hierarchy(TREE_DATA, function(d) { return d.children; });
d3.tree().nodeSize([DY, DX]).separation(...)(root);
// Rendu rectangles avec accent-bar
g.append('rect').attr('width', NODE_W).attr('height', NODE_H).attr('rx', 4)...
```

> Choisir la forme selon la complexité : **ellipses** pour les arbres > 15 nœuds avec branches croisées (positions manuelles plus flexibles) ; **rectangles hiérarchiques** pour les arbres < 15 nœuds avec structure propre.

### 5.3 Styles des nœuds — palettes fixes (ne changent PAS avec le thème)

> **Règle** : les nœuds D3 utilisent des couleurs **hardcodées** (pas de variables CSS), car le SVG ne hérite pas de `body.light`. Les couleurs sont celles du mode clair (parchemin), qui offre le meilleur contraste sur fond blanc ou parchemin.

```javascript
var NODE_STYLE = {
  d:  { fill: '#dce8f5', stroke: '#1a5fa8', tc: '#0a1020', r: 52, ry: 26 },  // décision
  lr: { fill: '#d5edd8', stroke: '#2d7a3a', tc: '#0a1a0a', r: 64, ry: 28 },  // bas risque
  sr: { fill: '#d5e4f5', stroke: '#1a5fa8', tc: '#0a1020', r: 64, ry: 28 },  // standard
  hr: { fill: '#f5d8d5', stroke: '#c0392b', tc: '#200505', r: 64, ry: 28 },  // haut risque
  sp: { fill: '#f5ead5', stroke: '#a0720a', tc: '#201000', r: 64, ry: 28 },  // prédisposition / spécial
  bi: { fill: '#ece0f5', stroke: '#6b3fa0', tc: '#100020', r: 64, ry: 28 },  // bilatéral / nourrisson
};
```

> Les nœuds de **décision** ont `r` plus petit et une couleur neutre. Les nœuds **résultats** sont plus larges et colorés selon le groupe de risque.

### 5.4 Arêtes

```javascript
// Courbe de Bézier cubique entre nœuds
g.append('path')
  .attr('d', 'M' + sx + ',' + sy + ' C' + mx + ',' + sy + ' ' + mx + ',' + ty + ' ' + tx + ',' + ty)
  .attr('fill', 'none')
  .attr('stroke', '#2a3441')           // couleur fixe, lisible sombre et clair
  .attr('stroke-width', 1.5)
  .attr('marker-end', 'url(#arr)');

// Label sur arête (multi-lignes avec \n)
var lines = e.label.split('\n');
var lt = g.append('text').attr('x', mx).attr('y', my)
  .attr('text-anchor', 'middle')
  .attr('font-size', '8.5').attr('font-family', 'DM Mono, monospace')
  .attr('fill', '#6e7681');            // couleur fixe
lines.forEach(function(ln, i) {
  lt.append('tspan').attr('x', mx).attr('dy', i === 0 ? -2 : 11).text(ln);
});
```

### 5.5 Tooltip D3 — structure avec données RT détaillées

> Le tooltip du logigramme doit contenir **autant d'informations** que la card résultat du wizard. L'utilisateur dans l'onglet logigramme ne doit pas avoir moins d'accès à la granularité clinique.

```javascript
// Structure complète d'un tooltip de nœud résultat
var TT = {
  node_id: {
    title:   'GROUPE DE RISQUE — Acronyme',    // DM Mono gris, uppercase
    q:       'Description du bras en 1–2 lignes',
    chemo:   'Schéma chimiothérapie complet',
    rt:      'Indication RT + dose(s) principales',
    timing:  'Timing RT (ex: J10–J14 postop, sem 13...)',
    // Données RT supplémentaires si l'espace le permet :
    volumes: 'GTV → CTV → PTV (résumé)',      // optionnel mais encouragé
    efs:     'EFS/OS : XX % · n = XX',        // obligatoire, coloré
    ec:      'efs-good'                        // efs-good | efs-mid | efs-bad
  }
};
```

```javascript
// Rendu HTML du tooltip
function showTT(event, d) {
  var html = '<div class="tt-title">' + d.title + '</div>'
    + '<div class="tt-q">' + d.q + '</div>'
    + '<div class="tt-section"><div class="tt-section-lbl">Chimiothérapie</div>'
    +   '<div class="tt-val">' + d.chemo + '</div></div>'
    + '<div class="tt-section"><div class="tt-section-lbl">Radiothérapie</div>'
    +   '<div class="tt-val">' + d.rt + '</div></div>'
    + '<div class="tt-section"><div class="tt-section-lbl">Timing RT</div>'
    +   '<div class="tt-val">' + d.timing + '</div></div>'
    + (d.volumes ? '<div class="tt-section"><div class="tt-section-lbl">Volumes</div>'
    +   '<div class="tt-val">' + d.volumes + '</div></div>' : '')
    + '<div class="tt-outcome-line"><span class="' + d.ec + '">' + d.efs + '</span></div>';
  ttEl.innerHTML = html;
  ttEl.style.display = 'block';
  moveTT(event);
}
```

**CSS du tooltip (fixe — sombre dans les deux thèmes) :**
```css
.tt-body {
  position: fixed; pointer-events: none; z-index: 300;
  background: rgba(13,17,23,.97);
  border-radius: 10px; padding: 14px 16px; width: 300px;
  box-shadow: 0 6px 28px rgba(0,0,0,.35);
  border: 1px solid rgba(56,139,253,.2);
  display: none;
}
/* Couleurs internes du tooltip : fixes, pas de var(--text) */
.tt-title { font-family: 'DM Mono', monospace; font-size: 9px; color: #6e7681;
            letter-spacing: 1.5px; text-transform: uppercase; margin-bottom: 7px; }
.tt-q { font-size: 13px; font-weight: 500; color: #e6edf3; margin-bottom: 10px; line-height: 1.4; }
.tt-section { margin-bottom: 9px; padding-bottom: 9px; border-bottom: 1px solid #2a3441; }
.tt-section:last-of-type { margin-bottom: 0; padding-bottom: 0; border-bottom: none; }
.tt-section-lbl { font-family: 'DM Mono', monospace; font-size: 9px; color: #6e7681;
                  letter-spacing: 1px; text-transform: uppercase; margin-bottom: 5px; }
.tt-val { font-size: 11px; color: #8b949e; line-height: 1.5; }
.tt-outcome-line { margin-top: 8px; font-family: 'DM Mono', monospace; font-size: 11px; }
.efs-good { color: #3fb950; }
.efs-mid  { color: #d29922; }
.efs-bad  { color: #f85149; }
```

### 5.6 Légende et contrôles

```html
<!-- Position : absolute, coin bas-gauche du panel-flow -->
<div class="flow-legend">
  <div class="leg-it"><div class="leg-dot" style="background:#388bfd"></div>Décision</div>
  <div class="leg-it"><div class="leg-dot" style="background:#3fb950"></div>Bas risque / VLR</div>
  <div class="leg-it"><div class="leg-dot" style="background:#388bfd;opacity:.5"></div>Risque standard</div>
  <div class="leg-it"><div class="leg-dot" style="background:#f85149"></div>Haut risque / Anaplasie</div>
  <!-- etc. selon les types de nœuds utilisés -->
</div>

<!-- Bouton reset : absolute, coin haut-droit -->
<button class="btn-reset-zoom" id="btnReset">⊙ Recentrer</button>
```

```css
.btn-reset-zoom {
  position: absolute; top: 14px; right: 14px;
  background: var(--surface); border: 1px solid var(--border2); color: var(--text2);
  padding: 7px 14px; border-radius: 6px;
  font-family: 'DM Mono', monospace; font-size: 11px;
  cursor: pointer; z-index: 10; transition: all .2s;
}
.btn-reset-zoom:hover { border-color: var(--accent); color: var(--accent); }
.flow-legend {
  position: absolute; bottom: 14px; left: 14px;
  display: flex; flex-wrap: wrap; gap: 8px; z-index: 10; pointer-events: none;
}
.leg-it { display: flex; align-items: center; gap: 5px;
          font-family: 'DM Mono', monospace; font-size: 9px; color: var(--text2); }
.leg-dot { width: 9px; height: 9px; border-radius: 50%; }
```

### 5.7 Zoom auto-fit et bouton reset

```javascript
// ── Calcul auto-fit ──
var xs = nodes.map(function(n) { return n.x; });
var ys = nodes.map(function(n) { return n.y; });
var minX = Math.min.apply(null, xs) - 80, maxX = Math.max.apply(null, xs) + 80;
var minY = Math.min.apply(null, ys) - 50, maxY = Math.max.apply(null, ys) + 60;
var initScale = Math.min(W / (maxX - minX), H / (maxY - minY), 1) * 0.86;
var initTx = W / 2 - (minX + maxX) / 2 * initScale;
var initTy = H / 2 - (minY + maxY) / 2 * initScale;
var initT = d3.zoomIdentity.translate(initTx, initTy).scale(initScale);

var zoom = d3.zoom().scaleExtent([0.1, 3])
  .on('zoom', function(event) { g.attr('transform', event.transform); });
svg.call(zoom);
svg.call(zoom.transform, initT);

// ── Bouton reset ──
document.getElementById('btnReset').onclick = function() {
  svg.transition().duration(600).call(zoom.transform, initT);
};
```

---

## 6. Règles de contenu clinique

### 6.1 Langue

* Interface en **français**
* Noms de protocoles, acronymes, termes techniques : anglais toléré (COG, NCCN, EFS, OS, GTV, CTV, PTV, NSS, WLI, WAI, etc.)
* Unités : Gy, fr (fractions), sem. (semaines), ans, mois

### 6.2 Groupes de risque — ordre et abréviations

| Ordre | Abréviation | Libellé | Classe CSS |
|-------|------------|---------|-----------|
| 1 | VLR | Très bas risque | `vlr` |
| 2 | LR | Bas risque | `lr` |
| 3 | SR | Risque standard | `sr` |
| 4 | HR | Haut risque | `hr` |
| 5 | Spécial | Discussion multidisciplinaire / Bilatéral / Prédisposition | `sp`, `bi`, `predispo` |

### 6.3 Données affichées — structure obligatoire dans les cards résultat

Dans cet ordre :

1. **Outcomes d'efficacité** (OS + EFS/PFS + cohorte + référence)
2. **Chimiothérapie** (schéma complet, durée, intensification si applicable)
3. **Radiothérapie** (bloc structuré — voir §6-B2)
4. **Note clinique** (points d'attention, pièges, particularités du sous-groupe)

---

## 6-A. RÈGLE — Collecte des données avant création ⚠️

> **Cette règle s'applique avant toute génération de code.**

### Données indispensables (bloquer la création si absentes)

* [ ] Critères de stratification complets et à jour (source/année du protocole)
* [ ] Noms des protocoles de traitement pour chaque bras
* [ ] Doses de radiothérapie pour chaque groupe (timing + volumes + doses)
* [ ] Schémas de chimiothérapie pour chaque bras

### Données fortement souhaitables

* [ ] OS et/ou EFS/PFS à 5 ans pour chaque groupe de risque (avec source)
* [ ] Taux de toxicités tardives majeures si RT impliquée
* [ ] Références bibliographiques complètes (auteurs, journal, année, DOI)
* [ ] NCT numbers des essais actifs cités

### Données optionnelles

* [ ] Sous-analyses notables (protons vs photons, réponse histologique, biomarqueurs)
* [ ] Essais cliniques actifs (recrutement en cours)
* [ ] Date de version du protocole

---

## 6-B. RÈGLE — Données d'outcomes dans les nœuds résultat ⚠️

### Outcomes d'efficacité — obligatoires dans chaque nœud résultat

```
OS 5 ans : XX %       (ou EFS, PFS selon la métrique principale)
EFS 5 ans : XX %
n = XX patients       (cohorte de référence)
Référence : [n]
```

**Card résultat (wizard) :**
```html
<div class="outcomes-block">
  <div class="r-label">Données d'efficacité <sup>[1]</sup></div>
  <div class="outcome-row">
    <span class="outcome-label">OS 5 ans</span>
    <span class="outcome-val ov-good">~95–98 %</span>
  </div>
  <div class="outcome-row">
    <span class="outcome-label">EFS 4–5 ans</span>
    <span class="outcome-val ov-mid">~83–90 %</span>
  </div>
  <div class="outcome-row">
    <span class="outcome-label">Cohorte</span>
    <span class="outcome-val">large cohorte</span>
  </div>
</div>
```

Couleurs : `.ov-good` (vert, OS ≥ 95 %) · `.ov-mid` (jaune, OS 80–94 %) · `.ov-bad` (rouge, OS < 80 %).

**Tooltip D3 (logigramme) :** champ `efs` obligatoire, champ `ec` pour la couleur.

### Outcomes de toxicité — à inclure si disponibles

```
Toxicité endocrinienne : déficit GH XX %, hypothyroïdie XX %
Toxicité neurocognitive : QI -X points
Second cancer (SMN) : XX % à 10 ans
Protons vs photons : GH XX % vs XX %
```

---

## 6-B2. RÈGLE — Détails de radiothérapie dans les nœuds résultat ⚠️

> **Cette règle est la plus critique.** Chaque nœud résultat avec RT indiquée doit impérativement fournir les **trois éléments** suivants. L'absence de l'un d'eux est un défaut bloquant.

### Les trois éléments obligatoires

#### 1. Timing

Préciser quand la RT intervient dans la séquence :

```
J10–J14 postopératoire (idéalement J10)
Semaine 13 (après 6 cycles de chimiothérapie d'induction)
Concurrent avec irinotecan (radiosensibilisant) — semaines 20–25
RT préopératoire : après C4, semaine 13. Chirurgie 4–6 sem après fin RT.
WLI : décision après réévaluation pulmonaire à semaine 6
```

#### 2. Volumes cibles

Décrire la chaîne complète GTV → CTV → PTV avec marges numériques :

```
GTVp (préop) → CTVp : +1 cm circumférentiel (respecter barrières anatomiques)
Exception extrémité : +2 cm sup/inf, +1 cm circumférentiel
GTVp_post → CTVp_post : +0,5 cm (boost uniquement)
Ganglions N1 : GTVn → CTVn +3 cm sup/inf (direction drainage lymphatique)
  Résidu bulky : GTVn_post → CTVn_post +0,5 cm
CTV → PTV : +3 à 10 mm (selon technique et IGRT locale)

Volumes spécifiques nephroblastome :
Flanc : GTV = tumeur préop ; CTV = GTV + 1 cm ; PTV = CTV + 5–10 mm
WAI : CTV = cavité péritonéale entière (diaphragme → plancher pelvien) + PTV 5–10 mm
WLI : CTV = plèvre entière sur CT simulation ; PTV = CTV + 5–10 mm
```

#### 3. Doses et fractionnement

Distinguer **chaque scénario dosimétrique** avec indication et fractionnement complet :

```
Standard local stade III (flanc) : 10,8 Gy / 6 fr / 1,8 Gy/fr
WAI (rupture / implants périt.) : 10,5 Gy / 7 fr / 1,5 Gy/fr
WLI > 12 mois : 12 Gy / 8 fr / 1,5 Gy/fr ; < 12 mois : 10,5 Gy
Flanc DAWT stade III : 19,8 Gy (dose élevée — réduction rechutes locales)
WLI + méta : boost tumoral à 41,4 Gy si persistance
Méta ganglionnaires réséquées : 10,8 Gy ; non réséquées : boost à 19,8 Gy
SBRT méta osseuses : selon protocole local

RMS standard (flanc) — SLFR : 41,4 Gy / 23 fr / 1,8 Gy/fr
RMS escaladé — HLFR RT1B : 50,4 Gy (Ph1 : 41,4 Gy + Ph2 : +9 Gy / 5 fr)
RMS très escaladé — RT1C : 59,4 Gy (Ph1 : 41,4 Gy + Ph2 : +18 Gy / 10 fr)
```

**Code couleur des doses (CSS) :**
```css
.rt-dose          { color: var(--blue);   font-family: 'DM Mono', monospace; font-size: 13px; font-weight: 500; }
.rt-dose.esc      { color: var(--yellow); }   /* dose escaladée */
.rt-dose.highesc  { color: var(--red);    }   /* dose très escaladée */
```

### Cas où la RT n'est PAS indiquée

**Ne pas laisser la section RT vide.** Indiquer explicitement :

```html
<div class="no-rt-box">
  PAS DE RT — Stade I histologie favorable, pas de rupture ni biopsie préalable
</div>
```

```css
.no-rt-box {
  background: var(--surface2);
  border: 1px solid var(--border);
  border-left: 3px solid var(--text3);
  border-radius: 8px;
  padding: 12px 16px;
  font-size: 13px; color: var(--text3);
}
```

### Mise en forme complète du bloc RT dans la card résultat

```html
<div class="rt-block">
  <div class="rt-block-title">Radiothérapie — Timing · Volumes · Doses</div>

  <!-- Section 1 : Indication et timing -->
  <div class="rt-section">
    <div class="rt-section-hdr">
      Timing
      <span class="rt-tag">J10–J14 postopératoire</span>
      <span class="rt-tag yellow">Coordonner avec chimio — éviter dactinomycine/doxorubicine à pleine dose pendant WAI</span>
    </div>
  </div>

  <!-- Section 2 : Volumes (grille 2 colonnes) -->
  <div class="rt-section">
    <div class="rt-section-hdr">Volumes cibles</div>
    <div class="rt-2col">
      <div class="rt-box">
        <div class="rt-box-label">GTV</div>
        <div class="rt-box-val">Volume tumoral préopératoire sur imagerie initiale</div>
      </div>
      <div class="rt-box">
        <div class="rt-box-label">CTV</div>
        <div class="rt-box-val">GTV + 1 cm (barrières anatomiques respectées)</div>
      </div>
      <div class="rt-box">
        <div class="rt-box-label">PTV</div>
        <div class="rt-box-val">CTV + 5–10 mm</div>
      </div>
      <div class="rt-box">
        <div class="rt-box-label">Technique</div>
        <div class="rt-box-val">3D-CRT / IMRT / Protons selon centre</div>
      </div>
    </div>
  </div>

  <!-- Section 3 : Doses par scénario -->
  <div class="rt-section">
    <div class="rt-section-hdr">Doses prescrites</div>
    <div class="rt-box">
      <div class="rt-box-label">Flanc — Stade III FHWT</div>
      <div class="rt-box-val">
        <span class="rt-dose">10,8 Gy / 6 fr / 1,8 Gy/fr</span>
        <div class="rt-sub">Résidu macroscopique : boost +10,8 Gy conformal</div>
      </div>
    </div>
    <div class="rt-box">
      <div class="rt-box-label">WAI — Rupture / Implants</div>
      <div class="rt-box-val">
        <span class="rt-dose esc">10,5 Gy / 7 fr / 1,5 Gy/fr</span>
        <div class="rt-sub">Patients ≤ 12 mois : dose limitée à 10,5 Gy</div>
      </div>
    </div>
    <div class="rt-box">
      <div class="rt-box-label">WLI — Méta pulmonaires</div>
      <div class="rt-box-val">
        <span class="rt-dose">> 12 mois : 12 Gy / 1,5 Gy/fr</span>
        <div class="rt-sub">< 12 mois : 10,5 Gy · AP/PA ou IMRT ou protons</div>
      </div>
    </div>
  </div>

  <!-- Section 4 : RT ganglionnaire si N+ -->
  <div class="rt-section">
    <div class="rt-section-hdr">RT ganglionnaire <span class="rt-tag yellow">N+</span></div>
    <div class="rt-note">LN réséquées : 10,8 Gy · LN non réséquées : 10,8 Gy + boost à 19,8 Gy</div>
  </div>
</div>
```

```css
.rt-block {
  background: var(--surface2); border: 1px solid var(--border);
  border-left: 3px solid var(--blue); border-radius: 8px;
  padding: 16px; margin-bottom: 10px;
}
.rt-block-title {
  font-family: 'DM Mono', monospace; font-size: 10px;
  letter-spacing: 1.5px; color: var(--blue);
  text-transform: uppercase; margin-bottom: 12px;
}
.rt-section { margin-bottom: 11px; padding-bottom: 11px; border-bottom: 1px solid var(--border); }
.rt-section:last-child { margin-bottom: 0; padding-bottom: 0; border-bottom: none; }
.rt-section-hdr {
  font-size: 12px; font-weight: 500; color: var(--text);
  margin-bottom: 7px; display: flex; align-items: center; gap: 7px; flex-wrap: wrap;
}
.rt-tag { font-family: 'DM Mono', monospace; font-size: 9px; padding: 2px 7px;
          border-radius: 4px; background: rgba(56,139,253,.12); color: var(--blue); flex-shrink: 0; }
.rt-tag.no-rt  { background: rgba(110,118,129,.12); color: var(--text3); }
.rt-tag.yellow { background: rgba(210,153,34,.12);  color: var(--yellow); }
.rt-tag.alert  { background: rgba(248,81,73,.12);   color: var(--red); }
.rt-2col { display: grid; grid-template-columns: 1fr 1fr; gap: 8px; }
@media (max-width: 560px) { .rt-2col { grid-template-columns: 1fr; } }
.rt-box { background: var(--surface3); border: 1px solid var(--border); border-radius: 6px; padding: 9px 11px; margin-bottom: 6px; }
.rt-box-label { font-family: 'DM Mono', monospace; font-size: 9px; color: var(--text3);
                letter-spacing: 1px; text-transform: uppercase; margin-bottom: 4px; }
.rt-box-val { font-size: 12px; color: var(--text); line-height: 1.5; }
.rt-sub { font-size: 11px; color: var(--text2); margin-top: 3px; line-height: 1.4; }
.rt-note { font-size: 11px; color: var(--text2); margin-top: 5px; line-height: 1.5; }
```

---

## 6-C. RÈGLE — Citations et sources ⚠️

### Principe

Tout outcome chiffré, toute dose, tout critère de stratification doit être sourcé. Numéro `[n]` en superscript.

### Section références (panel-wizard uniquement, affichée après le résultat)

```html
<div class="refs-section" id="refsSection" style="display:none">
  <div class="refs-title">Références</div>
  <div class="ref-item">
    <span class="ref-num">1</span>
    <div class="ref-body">
      <div class="ref-authors">Auteur A, Auteur B, et al.</div>
      <div class="ref-title-text">Titre complet de l'article</div>
      <div class="ref-journal">Journal, Année ; Volume(Numéro) : Pages</div>
      <div class="ref-doi">
        <a href="https://doi.org/XX.XXXX/XXXXX" target="_blank">doi:XX.XXXX/XXXXX</a>
        · <a href="https://clinicaltrials.gov/study/NCTXXXXXXXX" target="_blank">NCT XXXXXXXX</a>
      </div>
    </div>
  </div>
</div>
```

La section références est masquée (`display:none`) pendant la navigation dans le wizard et révélée uniquement à l'affichage du résultat :
```javascript
document.getElementById('refsSection').style.display = 'block'; // dans renderResult()
document.getElementById('refsSection').style.display = 'none';  // dans renderStep() et goBack()
```

---

## 6-D. RÈGLE — Export PDF du résultat ⚠️

```javascript
function printResult() {
  document.getElementById('progressWrap').style.display = 'none';
  document.querySelectorAll('.btn-back').forEach(function(b) { b.style.display = 'none'; });
  window.print();
  document.getElementById('progressWrap').style.display = 'block';
  document.querySelectorAll('.btn-back').forEach(function(b) { b.style.display = ''; });
}
```

**CSS print (`@media print`) — neutre quelle que soit le thème actif :**
```css
@media print {
  body, body.light { background: #fff !important; color: #000 !important; }
  header, body.light header {
    position: static !important; background: #fff !important;
    border-bottom: 1px solid #ccc !important;
  }
  .tab-nav, .btn-pdf, .btn-restart, .progress-wrap,
  .btn-back, #panel-flow, .btn-theme { display: none !important; }
  .panel { display: block !important; }
  .result-card { border: 1.5px solid #333 !important; background: #fff !important; break-inside: avoid; }
  .rt-block { border-left: 3px solid #1a5fa8 !important; background: #f5f5f5 !important; }
  .rt-dose { color: #1a5fa8 !important; }
  .rt-dose.esc { color: #a0720a !important; }
  .rt-dose.highesc { color: #c0392b !important; }
  .print-footer { display: block; font-size: 8pt; color: #999;
                  margin-top: 22px; border-top: 1px solid #ddd; padding-top: 8px; }
  :root, body.light {
    --text: #000; --text2: #444; --text3: #888;
    --surface: #fff; --surface2: #f5f5f5; --surface3: #eeeeee;
    --border: #ddd; --border2: #eee; --blue: #1a5fa8;
  }
}
```

---

## 7. Conventions de code

### 7.1 Organisation CSS (sections commentées)

```css
/* ── RESET ── */
/* ── THÈME (root + body.light) ── */
/* ── BASE ── */
/* ── HEADER ── */
/* ── TAB NAV ── */
/* ── PANELS ── */
/* ── WIZARD LAYOUT ── */
/* ── CARDS QUESTIONS ── */
/* ── RESULT CARD ── */
/* ── RT BLOCK ── */
/* ── OUTCOMES ── */
/* ── REFS ── */
/* ── D3 FLOWCHART ── */
/* ── TOOLTIP D3 ── */
/* ── PRINT ── */
```

### 7.2 Variables CSS

* **Toutes les couleurs UI** passent par des custom properties CSS
* **Exception** : couleurs des nœuds D3 et du tooltip D3 = hardcodées (SVG hors scope CSS)
* Jamais de couleur en dur dans les règles CSS du DOM (sauf valeur de fallback dans rgba)

### 7.3 JavaScript — règles critiques

* **`navStack`** pour l'historique de navigation (jamais `history` → conflit `window.history`)
* **`var`** plutôt que `const`/`let` pour compatibilité maximale (pas de transpilation)
* Données cliniques séparées du code de rendu (objets `RES`, `STEPS`, `TT` en tête de script)
* **Commentaires de section en majuscules encadrés** : `// ═══ DONNÉES ═══`
* Éviter les **template literals imbriqués** — préférer la concaténation de strings
* `try/catch` autour de tous les accès `localStorage`
* Valider la cohérence des clés `RES` / retours `next()` avant livraison

### 7.4 Accessibilité et mobile

* `meta viewport` avec `maximum-scale=5.0` (pinch-to-zoom logigramme)
* Padding bottom 80px dans `.container` (confort mobile, évite le chevauchement avec barres nav)
* Touch events supportés nativement par D3 zoom
* `lang="fr"` sur `<html>`

---

## 8. Patterns UI récurrents

### Logo-mark header

```html
<div class="hdr-logo">XX</div>
```
2–3 lettres, `border: 1.5px solid var(--accent)`, `font-family: 'DM Mono'`, `font-size: 9px`.

### Badge protocole header

```html
<span class="hdr-badge">COG · NCCN 2025</span>
```
Pill arrondie, DM Mono 9px, couleur = couleur de l'accent du protocole, `margin-left: auto`.

### Bouton toggle thème

```html
<button class="btn-theme" id="btn-theme" onclick="toggleTheme()" title="Passer en mode clair">◑</button>
```
Symbole `◑` = mode sombre actif. Symbole `☀` = mode clair actif.

### Bouton reset zoom (logigramme)

```html
<button class="btn-reset-zoom" id="btnReset">⊙ Recentrer</button>
```
Toujours `⊙ Recentrer`. Position : `absolute; top: 14px; right: 14px`.

### Bouton retour wizard

```html
<button class="btn-back" onclick="goBack()">← Retour</button>
```
Affiché uniquement si `navStack.length > 0`.

### Section label (section-label)

```css
.section-label {
  font-family: 'DM Mono', monospace; font-size: 10px;
  letter-spacing: 2px; color: var(--text3);
  text-transform: uppercase; margin-bottom: 10px;
}
```

---

## 9. Checklist avant livraison

### Architecture

* [ ] Fichier unique `[protocole]-[pathologie]-fusion.html` (Type AB)
* [ ] Deux onglets : "⬡ Stratification pas-à-pas" + "⬡ Logigramme interactif"
* [ ] `fixLayout()` appelé au chargement et sur `resize`
* [ ] Logigramme D3 construit différé (`window._flowBuilt`)
* [ ] `navStack` utilisé (jamais `var history`)

### Thème

* [ ] `:root` contient toutes les variables en mode sombre
* [ ] `body.light { ... }` surcharge toutes les variables
* [ ] `--surface` des cards = `#ffffff` en mode clair + `box-shadow: 0 1px 4px rgba(0,0,0,.06)`
* [ ] Header adaptatif sombre / parchemin
* [ ] Bouton `.btn-theme` présent dans le header, après le badge
* [ ] `initTheme()` appelé en bas de script (hors DOMContentLoaded)
* [ ] Clé `localStorage` spécifique : `theme-[nom-outil]`
* [ ] Tooltip D3 = couleurs fixes (non affecté par body.light)
* [ ] Nœuds D3 = couleurs hardcodées (mode clair — lisibles sur les deux fonds)

### Données et contenu

* [ ] Outcomes OS + EFS dans chaque nœud résultat (wizard + tooltip D3)
* [ ] Chaque donnée chiffrée sourcée avec `[n]`
* [ ] Section références présente, masquée pendant navigation, visible au résultat
* [ ] DOI cliquables pour toutes les références

### Radiothérapie

* [ ] Bloc RT structuré : **Timing · Volumes · Doses** pour chaque résultat avec RT
* [ ] Timing précis (jour, semaine, cycle de référence)
* [ ] Chaîne GTV → CTV → PTV avec marges numériques
* [ ] Chaque scénario dosimétrique avec dose totale + fractionnement + indication
* [ ] Code couleur doses : `.rt-dose` (standard) / `.rt-dose.esc` (escaladé) / `.rt-dose.highesc` (très escaladé)
* [ ] RT ganglionnaire documentée si N+
* [ ] RT métastatique documentée par site si stade IV
* [ ] Nœuds sans RT : raison explicite dans `.no-rt-box`
* [ ] Tooltip D3 : champs `rt`, `timing`, `volumes` présents (même résumé court)

### Technique

* [ ] 3 polices DM importées (Serif Display + Sans + Mono)
* [ ] Tous les nœuds D3 ont un `id` unique
* [ ] Zoom auto-fit calculé depuis dimensions réelles du panel
* [ ] Bouton "⊙ Recentrer" fonctionnel
* [ ] Légende couvre tous les types de nœuds utilisés
* [ ] Labels d'arêtes présents sur toutes les branches significatives
* [ ] `lang="fr"` sur `<html>`
* [ ] `@media print` masque tab-nav, panel-flow, btn-theme, btn-back, progress-wrap
* [ ] Footer de non-substitution présent (visible uniquement à l'impression)

### Type D — Tableau de bord de revue clinique

* [ ] Fichier `[pathologie]-revue.html`
* [ ] Landing page : grille `nav-card` + tableau comparatif cross-entités
* [ ] Un `.panel` par entité/scénario (show/back navigation)
* [ ] Chaque panel : `.rt-table` + `.trials-grid` + `.signal-box` + `.refs-list`
* [ ] Toutes les trial-cards avec NCT ID cliquable et badge statut
* [ ] Tous les DOI cliquables dans `.refs-list` + `.ref-note` avec données chiffrées
* [ ] Toggle thème présent dans le header — clé `localStorage` : `theme-[pathologie]-revue`
* [ ] `body.light` surcharge toutes les variables + overrides Type D spécifiques
* [ ] `initTheme()` appelé après les fonctions JS (hors DOMContentLoaded)
* [ ] `lang="fr"` ou `lang="en"` selon langue du contenu
* [ ] Disclaimer footer dans le header

---

## 10. Extensions futures envisagées

* **QR code** : lien vers l'index de tous les outils (`index.html` sur GitHub Pages)
* **Impression logigramme** : bouton dédié dans le panel-flow, fond blanc forcé, export PNG via `canvas`
* **Calculateur LQ** : outil séparé pour l'équivalence de dose
* **Essais cliniques actifs** : section dédiée dans chaque outil avec statut de recrutement en temps réel (ClinicalTrials.gov API)
* **Filtrage institution** : afficher uniquement les protocoles accessibles en France / SFCE / COG
* **Mode tablette optimisé** : disposition horizontale wizard / logigramme côte à côte sur iPad landscape
