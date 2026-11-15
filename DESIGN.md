# DESIGN — Gabarits visuels HTML

Trois gabarits, un par usage. Ne pas mélanger leurs conventions.

| Gabarit | Pour | Référence | Caractère |
|---|---|---|---|
| **R — Document de revue** | Toute sortie de `BIBLIO.md` en HTML | `pneumo/RTA_CBNPC_revue.html` | Sobre, fond blanc, police système, sans JavaScript, pensé pour l'impression |
| **O — Outil interactif** | Outils de `OUTILS-cliniques.md` (types AB, C, D, E, M) | `pedia/cog-wilms-fusion.html`, `neuro/neuro-onco-atlas.html` | Polices DM, sombre par défaut avec bascule clair |
| **CLB — Charte institutionnelle** | Présentations (`PRESENTATION-CLB.md`, PowerPoint) et documents institutionnels | `masque_theme_CLB.pptx` | Century Gothic, navy / cyan / crimson, fond blanc |

Règles communes : un seul fichier HTML autonome (CSS inline) ; `meta viewport` ; lisible à 390 px sans défilement horizontal (tableaux larges dans un conteneur défilant) ; toutes les couleurs via variables CSS ; mention d'usage personnel en pied de page.

---

## R — Document de revue

### Principes
- Une seule page longue, façon document ; **pas de JavaScript**, pas de police externe, pas de bascule de thème.
- En-tête : `h1` + sous-titre (type de document · sources interrogées · date).
- **Sommaire** à ancres en tête (`nav`), masqué à l'impression.
- Sections `h2` soulignées par la couleur d'accent ; `h3` pour les sous-parties.
- **Encadré « Synthèse »** (`.key`) en tête : réponse en quelques points, verdicts en gras.
- **Encadré d'alerte** (`.warn`) pour les mises en garde (niveau de preuve faible, données non publiées).
- **Tableaux** sobres dans `.tbl` (défilement horizontal sur mobile), badges de preuve dans les cellules, légende des badges sous le premier tableau.
- **Références** numérotées en fin de document, petite taille, lien `doi` ou `NCT` sur chaque ligne.
- Pied de page : sources interrogées, limites (« textes intégraux non relus », « sans valeur de recommandation »).

### Squelette
```html
<!DOCTYPE html>
<html lang="fr"><head>
<meta charset="utf-8"><meta name="viewport" content="width=device-width, initial-scale=1">
<title>Sujet court – contexte</title>
<style>/* bloc CSS ci-dessous */</style>
</head><body><div class="wrap">
<h1>Titre</h1>
<p class="sub">Type de revue · Sources (PubMed, ClinicalTrials.gov…) · JJ/MM/AAAA</p>
<nav><a href="#synthese">Synthèse</a> <a href="#…">…</a> <a href="#refs">Références</a></nav>
<h2 id="synthese">Synthèse</h2>
<div class="key"><ul><li><b>Message clé</b> …</li></ul></div>
<h2 id="…">Section</h2>
<div class="tbl"><table><tr><th>Étude</th><th>Preuve</th>…</tr>
<tr><td>Auteur 2025, <i>Revue</i></td><td><span class="b rct">Randomisé</span></td>…</tr></table></div>
<p class="legend">Légende : <span class="b rct">Randomisé</span> <span class="b pro">Prospectif</span> …</p>
<h2 id="refs">Références</h2>
<ol class="refs"><li>Auteur A et al. Titre. <i>Revue</i> Année;vol:pages. <a href="https://doi.org/…">doi</a></li></ol>
<footer>Sources : … Synthèse descriptive, sans valeur de recommandation.</footer>
</div></body></html>
```

### CSS de référence
```css
:root{--ink:#1d2430;--muted:#5b6575;--line:#d9dee6;--bg:#fff;--soft:#f4f6f9;--accent:#1f4e79;
--rct:#1b7a3d;--pro:#b26b00;--cons:#2f6fb0;--retro:#8a8f98;--cas:#b3b8c0;--tech:#5a4fa0;}
*{box-sizing:border-box}
body{margin:0;font:15px/1.55 -apple-system,"Segoe UI",Roboto,Helvetica,Arial,sans-serif;color:var(--ink);background:var(--bg)}
.wrap{max-width:980px;margin:0 auto;padding:24px 16px 64px}
h1{font-size:1.6rem;margin:0 0 4px;color:var(--accent)}
.sub{color:var(--muted);margin:0 0 20px;font-size:.92rem}
nav{background:var(--soft);border:1px solid var(--line);border-radius:6px;padding:10px 14px;margin-bottom:24px;font-size:.9rem}
nav a{color:var(--accent);text-decoration:none;margin-right:14px;white-space:nowrap;line-height:1.9}
h2{font-size:1.2rem;color:var(--accent);border-bottom:2px solid var(--accent);padding-bottom:4px;margin:34px 0 12px}
h3{font-size:1.02rem;margin:20px 0 8px}
.key{background:#eef4fa;border-left:4px solid var(--accent);padding:10px 16px;border-radius:4px}
.key li{margin:4px 0}
.warn{background:#fff6e8;border-left:4px solid var(--pro);padding:8px 14px;border-radius:4px;margin:12px 0;font-size:.93rem}
table{width:100%;border-collapse:collapse;margin:10px 0 6px;font-size:.86rem}
th,td{border:1px solid var(--line);padding:6px 8px;vertical-align:top;text-align:left}
th{background:var(--soft)}
.tbl{overflow-x:auto}
.b{display:inline-block;font-size:.72rem;font-weight:600;color:#fff;border-radius:3px;padding:1px 6px;white-space:nowrap}
.rct{background:var(--rct)}.pro{background:var(--pro)}.cons{background:var(--cons)}
.retro{background:var(--retro)}.cas{background:var(--cas);color:var(--ink)}.tech{background:var(--tech)}
.pend{background:transparent;color:var(--muted);border:1px dashed var(--muted)}
.legend{font-size:.8rem;color:var(--muted);margin:4px 0 0}
.refs{font-size:.82rem}
.refs li{margin:3px 0}
a{color:var(--accent)}
footer{margin-top:40px;font-size:.78rem;color:var(--muted);border-top:1px solid var(--line);padding-top:8px}
@media print{
 nav{display:none}
 body{font-size:10.5pt}
 .wrap{max-width:none;padding:0}
 h2{page-break-after:avoid}
 table,.key,.warn{page-break-inside:avoid}
 a{color:inherit;text-decoration:none}
 .b{-webkit-print-color-adjust:exact;print-color-adjust:exact}
}
```

Badges : codes et libellés de l'échelle unique (`SOCLE-clinique.md` §4.1). Ajouter des classes propres au sujet seulement si nécessaire, sans toucher aux couleurs des badges de preuve.

---

## O — Outil interactif

### Typographie
`DM Serif Display` (titres, noms de protocoles) · `DM Sans` (texte, boutons) · `DM Mono` (badges, métadonnées, doses, valeurs).
```html
<link href="https://fonts.googleapis.com/css2?family=DM+Serif+Display:ital@0;1&family=DM+Mono:wght@400;500&family=DM+Sans:wght@300;400;500;600&display=swap" rel="stylesheet">
```

### Couleurs : sombre par défaut (`:root`), clair parchemin (`body.light`)
```css
:root{--bg:#0d1117;--surface:#161b22;--surface2:#1c2330;--surface3:#21262d;--border:#2a3441;--border2:#30363d;
--text:#e6edf3;--text2:#8b949e;--text3:#6e7681;--green:#3fb950;--blue:#388bfd;--red:#f85149;--yellow:#d29922;
--purple:#a371f7;--orange:#f0883e;--cyan:#39c5cf;--accent:/* couleur du protocole */;--radius:10px;}
body.light{--bg:#f5f0e8;--surface:#fff;--surface2:#ede8dc;--surface3:#e6e0d2;--border:#c8bfaa;--border2:#d4ccba;
--text:#1a1208;--text2:#5a4f3a;--text3:#9a8f7a;--green:#2d7a3a;--blue:#1a5fa8;--red:#c0392b;--yellow:#a0720a;
--purple:#6b3fa0;--orange:#c45a10;--cyan:#1a7a8a;}
```
- **Accent par référentiel** : orange COG / NCCN · bleu SIOP · violet SFCE.
- **Sémantique invariante** : vert = bas risque (LR/VLR), bleu = standard (SR), rouge = haut risque (HR), jaune = prédisposition / situation spéciale, violet = bilatéral / nourrisson, cyan = QUANTEC / Timmerman.
- En clair : cartes blanches `box-shadow:0 1px 4px rgba(0,0,0,.06)` ; en-tête `rgba(245,240,232,.97)` + `backdrop-filter`.
- **Logigramme D3** : couleurs des nœuds codées en dur (le SVG n'hérite pas de `body.light`) ; info-bulle D3 toujours sombre.

### Bascule de thème
- Bouton `◑` (sombre actif) / `☀` (clair actif) dans l'en-tête, après le badge du protocole.
- Clé `localStorage` **propre à l'outil** : `theme-[nom-outil]` (jamais `theme` seul) ; tout accès dans un `try/catch`.
- `initTheme()` appelé en bas de script (pas dans `DOMContentLoaded`).
- Implémentation de référence : `pedia/cog-wilms-fusion.html`.

### Impression
`@media print` neutre quel que soit le thème : fond blanc, texte noir, en-tête statique ; masquer onglets, logigramme, boutons de thème / retour / progression ; cartes résultat avec bordure et `break-inside:avoid` ; pied de page de non-substitution visible uniquement à l'impression.

### En-tête type
Logo de 2–3 lettres (bordure accent, DM Mono 9 px) · titre (DM Serif) + sous-titre (DM Mono) · badge du protocole en pastille (`PROTOCOLE · Année`) · bouton de thème. En-tête collant.

---

## CLB — Charte institutionnelle

### Format
16:9 (33,87 × 19,05 cm) ; fond blanc pour le contenu ; fond navy `#2C426C` pour les séparateurs de section et l'écran de fin.

### Couleurs
| Rôle | Hex |
|---|---|
| Navy (principal, ~60 %) | `#2C426C` |
| Cyan (support, ~25 %) | `#00ACE9` |
| Crimson (ponctuation, ~15 %) | `#B41860` |
| Rose | `#EB6898` |
| Deep blue | `#0068A9` |
| Lime (données / positif uniquement) | `#96D654` |
| Texte secondaire | `#767676` |
| Lien | `#F59E00` (visité `#B2B2B2`) |

Aucune couleur hors palette.

### Typographie
**Century Gothic** exclusivement (`'Century Gothic','Futura','Gill Sans',sans-serif`). Titre de présentation 40–44 pt gras ; titre de diapositive 28–36 pt gras ; corps 16–18 pt (niveau 2 : 14 pt) ; légende 10–12 pt `#767676`. Tout aligné à gauche ; pas de majuscules sauf le logotype « LÉON BÉRARD ».

### Mise en page
Marges ≥ 0,7 cm ; logo en haut à droite ; URL `centreleonberard.fr` en bas à droite (8 pt) ; date `JJ/MM/AAAA` en bas à gauche ; deux colonnes 50/50 (texte à gauche, image à droite).

### Composants
- Listes : 2 niveaux maximum, ~6 puces par diapositive.
- Tableaux : en-tête navy texte blanc gras ; lignes alternées blanc / `#F2F7FB` ; bordures `#0068A9`.
- Chiffre clé : 48–60 pt gras cyan ou navy, libellé 14 pt.
- Graphiques : ordre navy → cyan → crimson → deep blue → lime → gris ; pas de 3D ni d'ombre ; grille `#D9D9D9`.

### Variables CSS
```css
:root{--clb-navy:#2C426C;--clb-cyan:#00ACE9;--clb-crimson:#B41860;--clb-rose:#EB6898;--clb-blue:#0068A9;
--clb-lime:#96D654;--clb-white:#FFFFFF;--clb-muted:#767676;--clb-amber:#F59E00;--clb-row-alt:#F2F7FB;
--font:'Century Gothic','Futura','Gill Sans','Trebuchet MS',sans-serif;}
```
