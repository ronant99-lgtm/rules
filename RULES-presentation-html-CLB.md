---
title: Règles de création — Présentations HTML CLB
version: 1.0
source: IGRT_formation_v4_33.html (Institut Léon Bérard)
depends_on: CLB-presentation-rule.md (charte graphique — design system)
not_to_be_confused_with: RULES-clinical-tools-v2.md (outils cliniques interactifs — artefact distinct)
date: 2026-06-13
applies_to: Présentations HTML autonomes (slides interactives), tout sujet
---

# Règles de création — Présentations HTML CLB

> Fichier de référence pour la génération de **présentations HTML autonomes** à l'Institut Léon Bérard : supports de formation, exposés, modules pédagogiques interactifs.
>
> Implémentation de référence : `IGRT_formation_v4_33.html`.

---

## 0. Positionnement — ce que ce fichier est et n'est pas

Ce fichier décrit l'**architecture applicative** d'une présentation HTML CLB (structure de slides, navigation, modales, composants interactifs, persistance des notes). Il ne redéfinit **pas** la charte graphique.

| Fichier | Rôle | Relation |
|---------|------|----------|
| **CLB-presentation-rule.md** | Charte graphique (couleurs, typo, layout, composants statiques) | **Référencé** — à respecter intégralement. Ne pas dupliquer. |
| **RULES-presentation-html-CLB.md** *(ce fichier)* | Architecture HTML/JS d'une présentation interactive | Le présent document. |
| **RULES-clinical-tools-v2.md** | Outils cliniques (wizard, logigramme D3, tableau de bord) | **Artefact différent.** Ne s'applique pas ici. Voir §0.1. |

### 0.1 Distinction explicite avec les outils cliniques

Une présentation HTML CLB et un outil clinique (`RULES-clinical-tools-v2.md`) sont deux artefacts **incompatibles dans leurs conventions**. Ne jamais transposer les règles de l'un à l'autre :

| Dimension | Présentation HTML CLB *(ce fichier)* | Outil clinique *(clinical-tools)* |
|-----------|--------------------------------------|-----------------------------------|
| Finalité | Support linéaire à dérouler devant un public | Outil décisionnel à manipuler |
| Police | **Century Gothic** (charte CLB) | DM Serif / DM Sans / DM Mono |
| Fond | **Blanc** `#FFFFFF` (charte CLB) | Sombre `#0d1117` par défaut |
| Thème | **Pas de toggle** — charte CLB claire uniquement | Toggle sombre/clair **obligatoire** |
| Persistance | Notes **en mémoire vive** (RAM) + export/import fichier | `localStorage` |
| Navigation | `goTo(i)` entre slides | `navStack` + `STEPS.next()` |
| Dépendances | **Aucune** (vanilla JS) | D3.js obligatoire |

> Le seul point réellement commun est `lang="fr"` sur `<html>`.

---

## 1. Architecture de l'artefact

### 1.1 Fichier unique autonome

* **Un seul fichier `.html`**, tout inline : CSS dans `<style>`, JS dans `<script>` en fin de `<body>`.
* **Aucune dépendance externe** : pas de framework, pas de CDN JS. JavaScript vanilla.
* Seules ressources externes admises : les **images locales** (dossier `images/`) et liens hypertextes sortants (références, sources).
* Le fichier doit fonctionner par simple ouverture dans un navigateur (`file://`), sans serveur.

### 1.2 Structure HTML de haut niveau

```
<head> : meta, <title>, <style> (charte CLB inline)
<body>
  #sw                      ← conteneur de toutes les slides
    .slide.active #s1      ← couverture
    .slide #s2 … #sN       ← slides de contenu
  <nav id="nav">           ← barre de navigation flottante (prev · compteur · dots · next)
  #mover                   ← overlay modal (vignettes de détail)  [optionnel]
  #lightbox                ← overlay zoom image                   [optionnel]
  .clb-footer / .clb-footer-url   ← date (bas-gauche) + URL (bas-droite)
  #preload-notes           ← conteneur JSON des notes              [si module notes]
  <script>                 ← données + logique, en fin de body
```

### 1.3 Modèle de slide

```html
<div class="slide" id="s4">
  <div class="slide-header">
    <div class="stag">Sur-titre / catégorie</div>   <!-- cyan, uppercase, tracké -->
    <h2 class="st">Titre de la slide</h2>
  </div>
  <!-- contenu -->
</div>
```

* Une seule slide porte `.active` à la fois (`display:block`), les autres `display:none`.
* La **slide 1** est la couverture (`#s1`), traitée à part (centrage vertical, fond à motif, kicker, méta, tags, auteur+date en bas-gauche).
* Padding de slide : `2rem 3rem 6rem` (le `6rem` bas réserve la place de la barre de navigation).

---

## 2. Identité visuelle

> **La charte est définie dans `CLB-presentation-rule.md`. Elle fait foi.** Cette section ne rappelle que les points d'ancrage et les extensions propres au format HTML.

### 2.1 Rappels non négociables (charte CLB)

* Police : `--font: 'Century Gothic', 'Futura', 'Gill Sans', 'Trebuchet MS', sans-serif;`
* Fond de contenu : blanc `#FFFFFF`. Texte : navy `#2C426C`.
* Palette : navy `#2C426C`, cyan `#00ACE9`, crimson `#B41860`, rose `#EB6898`, deep blue `#0068A9`, lime `#96D654` (données/positif uniquement).
* Muted : `#767676`. Lien : cyan, hover crimson.
* Logo « INSTITUT / LÉON BÉRARD » en haut-droite de la couverture ; URL `centreleonberard.fr` bas-droite ; date `JJ/MM/AAAA` bas-gauche.

### 2.2 Variables CSS — bloc `:root` de référence

Reprendre les variables de la charte, puis ajouter les variables **fonctionnelles propres à la présentation** (dérivées de l'IGRT) :

```css
:root {
  /* — Charte CLB (cf. CLB-presentation-rule.md) — */
  --clb-navy:#2C426C; --clb-cyan:#00ACE9; --clb-crimson:#B41860;
  --clb-rose:#EB6898; --clb-blue:#0068A9; --clb-lime:#96D654;
  --clb-white:#FFFFFF; --clb-muted:#767676; --clb-amber:#F59E00;
  --clb-row-alt:#F2F7FB;

  /* — Extensions présentation HTML — */
  --clb-bg-soft:#F8FAFC;          /* fonds de cartes / encadrés doux */
  --clb-border:#E1E8F0;           /* bordures légères */
  --clb-border-strong:#B8C5D6;    /* bordures marquées */

  /* Familles de catégories (à adapter au sujet) :
     l'IGRT définit des paires couleur/fond/bordure réutilisables
     pour typer visuellement des éléments (tags, cartes, boutons). */
  --nf-color:var(--clb-blue);  --nf-bg:#E8F1F9; --nf-border:#B8D4E8;
  --sb-color:var(--clb-crimson); --sb-bg:#FBE8EF; --sb-border:#ECC0D2;
  --ck-color:var(--clb-amber); --ck-bg:#FFF4E0; --ck-border:#FCE0AC;

  --font:'Century Gothic','Futura','Gill Sans','Trebuchet MS',sans-serif;
  --shadow-soft:0 1px 3px rgba(44,66,108,.06), 0 4px 12px rgba(44,66,108,.05);
  --shadow-modal:0 16px 48px rgba(44,66,108,.18), 0 4px 16px rgba(44,66,108,.10);
}
```

> Les triplets `--xx-color / --xx-bg / --xx-border` sont un **patron de typage par catégorie** : sur l'IGRT ils servent à distinguer NF / SBRT / délai critique. Pour un autre sujet, renommer et réaffecter ces familles, mais **rester dans la palette CLB** (§usage charte : ne pas introduire de couleur hors palette).

### 2.3 Échelle typographique HTML

Tailles en `rem` (base `font-size:17px` sur `html,body`, ramenée à `15px` en mobile) :

| Élément | Classe | Taille |
|---------|--------|--------|
| Titre couverture | `h1.disp` | `clamp(2.6rem, 5.2vw, 4.4rem)` |
| Titre de slide | `h2.st` | `1.85rem` |
| Sous-titre de bloc | `h3.ct` | `1.2rem` |
| Sur-titre / tag de section | `.stag` | `0.78rem`, uppercase, `letter-spacing:.18em`, cyan |
| Corps | `p` | `1rem`, `line-height:1.65` |
| Légende / muted | — | `0.82–0.92rem`, `--clb-muted` |

Titres et corps **alignés à gauche** (charte CLB : jamais de corps centré).

---

## 3. Navigation (obligatoire)

### 3.1 Barre de navigation flottante

```html
<nav id="nav">
  <button class="nb" id="prev" aria-label="Précédent">‹</button>
  <span id="sc">1 / N</span>
  <div class="dots" id="dots"></div>
  <button class="nb" id="next" aria-label="Suivant">›</button>
</nav>
```

* Position : `fixed`, `bottom:1.5rem`, centrée (`left:50%; transform:translateX(-50%)`), pilule blanche bordée, `z-index:150`.
* Composants : flèche précédent · compteur `i / N` · points (dots, un par slide) · flèche suivant.
* Les **dots sont générés en JS** (un `<button.dot>` par slide), le dot actif porte `.on`.

### 3.2 Moteur de navigation

```javascript
const slides = document.querySelectorAll('.slide');
const totalSlides = slides.length;
let currentSlide = 0;

function goTo(i) {
  if (i < 0 || i >= totalSlides) return;
  slides[currentSlide].classList.remove('active');
  dotsContainer.children[currentSlide].classList.remove('on');
  currentSlide = i;
  slides[currentSlide].classList.add('active');
  dotsContainer.children[currentSlide].classList.add('on');
  document.getElementById('sc').textContent = (i + 1) + ' / ' + totalSlides;
  document.getElementById('prev').disabled = (i === 0);
  document.getElementById('next').disabled = (i === totalSlides - 1);
  document.body.classList.toggle('on-cover', i === 0);  // masque la date sur la couverture
  window.scrollTo({ top: 0, behavior: 'instant' });
}
```

### 3.3 Contrôles clavier (obligatoires)

* `←` / `→` : slide précédente / suivante.
* `Échap` : ferme la modale ou le lightbox **en priorité** (ne pas naviguer entre slides si un overlay est ouvert).

```javascript
document.addEventListener('keydown', (e) => {
  if (document.getElementById('lightbox').classList.contains('open')) {
    if (e.key === 'Escape') closeLightbox();
    return;
  }
  if (document.getElementById('mover')?.classList.contains('open')) {
    if (e.key === 'Escape') closeModal();
    return;
  }
  if (e.key === 'ArrowLeft')  goTo(currentSlide - 1);
  if (e.key === 'ArrowRight') goTo(currentSlide + 1);
});
```

---

## 4. Système de modales / vignettes (optionnel)

Pour les présentations où une slide sert de **carte de navigation** vers des contenus détaillés (l'IGRT : une grille → 13 vignettes), utiliser le pattern modale.

### 4.1 Données

Le contenu détaillé est stocké dans un **objet JS unique**, séparé du rendu :

```javascript
const VIGNETTES = {
  "v1": "<div class=\"mhdr\">…</div><section class=\"msec\">…</section>",
  "v2": "…",
  // …
};
```

* Clés courtes (`v1`, `v2`, …, ou identifiants parlants `rt_offline`).
* Chaque vignette suit une **trame interne cohérente** (sur l'IGRT : `mhdr` + sections `msec` titrées — enjeu, placement, logique, protocole, message clé, remarques, références).

### 4.2 Overlay et ouverture

```html
<div id="mover" role="dialog" aria-modal="true" aria-hidden="true">
  <div id="mbox">
    <button id="mcls" aria-label="Fermer">✕</button>
    <div id="mcontent"></div>
  </div>
</div>
```

```javascript
function openModal(id) {
  const content = VIGNETTES[id];
  if (!content) { console.warn('Modal inconnue :', id); return; }
  document.getElementById('mcontent').innerHTML = content;
  const mover = document.getElementById('mover');
  mover.classList.add('open');
  mover.setAttribute('aria-hidden', 'false');
  document.body.style.overflow = 'hidden';   // bloque le scroll de fond
  mover.scrollTop = 0;
  attachZoomHandlers();                       // re-brancher les zooms du contenu injecté
  // si module notes : re-brancher les textareas injectés (cf. §6)
}
```

* Fermeture : bouton `✕`, clic sur le fond (`#mover`), ou `Échap`.
* Toujours **restaurer `document.body.style.overflow`** à la fermeture.
* `role="dialog"` + `aria-modal="true"` + `aria-hidden` synchronisé : accessibilité minimale obligatoire.

### 4.3 Déclencheurs

Boutons cliquables typés par catégorie, appelant `openModal('id')` :

```html
<button class="vbtn vnf" onclick="openModal('v1')">⚪ Prostate</button>
```

Prévoir aussi l'activation **clavier** des cartes cliquables (`role="button"`, `tabindex="0"`, gestion `Enter`/`Espace`).

---

## 5. Lightbox image (optionnel mais recommandé si images)

Toute image de contenu pouvant être agrandie porte la classe `zoom` (ou `intro-img`) et ouvre un lightbox plein écran.

```html
<div id="lightbox" role="dialog" aria-modal="true" aria-hidden="true">
  <button id="lb-close" aria-label="Fermer">✕</button>
  <img id="lb-img" src="" alt="">
</div>
```

```javascript
function attachZoomHandlers() {
  document.querySelectorAll('img.zoom, img.intro-img').forEach(img => {
    if (img.dataset.zoomBound) return;
    img.dataset.zoomBound = 'true';
    img.style.cursor = 'zoom-in';
    img.addEventListener('click', () => openLightbox(img.src));
  });
}
```

* `attachZoomHandlers()` doit être **rappelé après toute injection de contenu** (ouverture de modale), car les images injectées ne sont pas branchées au chargement.
* Garde anti-double-branchement via `dataset.zoomBound`.

---

## 6. Module de notes (optionnel — selon le contexte)

> **Module activable** : pertinent pour les supports de formation où l'animateur recueille remarques et questions. À omettre pour une présentation purement descriptive.

Principe directeur : **persistance en mémoire vive uniquement** (pas de `localStorage`), avec **export/import par fichier** pour conserver une trace. Ce choix garantit la confidentialité (rien n'est écrit sur la machine sans action explicite) et la portabilité (le fichier exporté est lui-même rejouable).

### 6.1 Zones de saisie

```html
<textarea class="remarks-area" data-note-id="note-v1"
          placeholder="Notez ici les remarques…
(stockées en mémoire pendant la session — exportables via « Enregistrer »)"></textarea>
```

* Chaque zone porte un `data-note-id` **unique et stable**.
* Maintenir en parallèle un dictionnaire `NOTE_LABELS` (id → libellé lisible) pour le compte-rendu.

### 6.2 Stockage en RAM

```javascript
const NOTES = {};  // id → texte

function saveNote(el)    { const id = el.dataset.noteId; if (id) NOTES[id] = el.value; }
function restoreNote(el) { const id = el.dataset.noteId; if (id && NOTES[id] !== undefined) el.value = NOTES[id]; }
function bindNote(el) {
  if (el.dataset.noteBound) return;
  el.dataset.noteBound = 'true';
  restoreNote(el);
  el.addEventListener('input', () => saveNote(el));
}
```

* Les textareas **statiques** (dans les slides) sont branchés au chargement.
* Les textareas **injectés** (dans les modales) sont branchés à l'ouverture, et **sauvegardés à la fermeture** (avant destruction du contenu).

### 6.3 Export / Import

Trois actions, regroupées dans une barre `#notes-toolbar` en fin de présentation :

| Bouton | Fonction | Sortie |
|--------|----------|--------|
| 💾 Enregistrer la version | `exportFullVersion()` | HTML complet, notes ré-injectées, **rejouable et ré-annotable** |
| 📝 Compte-rendu | `exportNotesReport()` | `.txt` listant les remarques non vides |
| 📂 Importer | `importNotes(file)` | relit un HTML exporté et restaure les notes |

* **Auto-réhydratation** : un conteneur `<script id="preload-notes" type="application/json">` (vide à l'origine) est rempli au moment de l'export. Au chargement, si ce conteneur contient des données, les notes sont restaurées automatiquement — le fichier exporté redevient un point de reprise.
* À l'export, **échapper `<`** dans le JSON injecté (`.replace(/</g,'\\u003c')`) pour neutraliser toute balise contenue dans une note.
* Génération de fichier via `Blob` + `URL.createObjectURL` + `<a download>` (cf. `downloadBlob`).
* Nom de fichier horodaté : `Sujet_AAAA-MM-JJ.html` / `Sujet_remarques_AAAA-MM-JJ.txt`.

---

## 7. Composants récurrents

### 7.1 Tags catégoriels

```html
<span class="tag tnf">NF — Normofractionné</span>
<span class="tag tsb">SBRT</span>
<span class="tag tck">⏱ Délai critique</span>
```

Pilule bordée, `0.78rem`, gras, colorée via les triplets `--xx-color/-bg/-border` (§2.2).

### 7.2 Encadré de mise en avant (`hlbox`)

```html
<div class="hlbox">
  <div class="hllbl">Étiquette</div>
  <p>Contenu mis en avant.</p>
</div>
```

Fond doux, **bordure gauche cyan 4px**.

### 7.3 Cartes (grille d'introduction, cartes cliquables)

`.inccard` (informative), variante `.featured` (bordure + fond cyan léger) ; `.adcard` cliquable (`role="button"`, `tabindex="0"`).

### 7.4 Tables de données

Conformes à la charte CLB §5 : en-tête fond navy + texte blanc, lignes alternées blanc / `#F2F7FB`, bordures deep blue.

### 7.5 Vidéos / iframes

`<video controls>` local ou `<iframe>` YouTube dans un wrapper ratio 16:9. Pas d'autoplay sonore.

---

## 8. Responsive

Point de rupture unique à `900px` :

```css
@media (max-width: 900px) {
  html, body { font-size: 15px; }
  .slide { padding: 1.5rem 1.2rem 6rem; }
  h1.disp { font-size: 2rem; }
  h2.st   { font-size: 1.4rem; }
  /* grilles multi-colonnes → 1 colonne */
  .incgrid, .adgrid, .techgrid, .ccgrid, .grid-layout, .intro-2col { grid-template-columns: 1fr; }
  .sidebar { position: static; }
}
```

---

## 9. Conventions de code

### 9.1 Organisation CSS (sections commentées)

```css
/* ── CHARTE CLB (root) ── */
/* ── BASE / RESET ── */
/* ── NAVIGATION ── */
/* ── TAGS ── */
/* ── COUVERTURE (#s1) ── */
/* ── SLIDES DE CONTENU ── */
/* ── COMPOSANTS (cartes, hlbox, tables) ── */
/* ── MODALE / VIGNETTES ── */
/* ── LIGHTBOX ── */
/* ── NOTES (toolbar) ── */
/* ── RESPONSIVE ── */
```

### 9.2 CSS

* **Toutes les couleurs** passent par des variables CSS de la charte. Jamais de hex en dur dans les règles (sauf valeur de fallback dans un `rgba()`).
* Ne pas introduire de couleur hors palette CLB.

### 9.3 JavaScript

* **Vanilla, sans dépendance.** Pas de framework, pas de transpilation requise.
* **Données séparées du rendu** : `VIGNETTES`, `NOTES`, `NOTE_LABELS` en tête de script.
* Gardes anti-double-branchement (`dataset.*Bound`) sur tout `addEventListener` susceptible d'être rappelé.
* `try/catch` autour de toute lecture de fichier importé et du parsing JSON.
* Le script est placé **en fin de `<body>`** (le DOM est disponible, pas besoin de `DOMContentLoaded`).
* Commentaires de section en majuscules encadrés : `// ═══ NAVIGATION ═══`.

### 9.4 Accessibilité

* `lang="fr"` sur `<html>` (ou `lang="en"` si contenu anglophone).
* `<meta name="viewport" content="width=device-width, initial-scale=1.0">`.
* Overlays : `role="dialog"`, `aria-modal="true"`, `aria-hidden` synchronisé, fermeture clavier (`Échap`).
* Boutons : `aria-label` sur les boutons icôniques (`‹`, `›`, `✕`).
* Éléments cliquables non-`<button>` : `role="button"` + `tabindex="0"` + gestion clavier.

---

## 10. Checklist avant livraison

### Structure

* [ ] Fichier `.html` unique, CSS + JS inline, **aucune dépendance externe**
* [ ] Ouvrable en `file://` sans serveur
* [ ] `lang="fr"` + `meta viewport` présents
* [ ] `<title>` au format `Sujet — Contexte · CLB`
* [ ] Slide 1 = couverture (kicker, titre, sous-titre, méta, tags, auteur + date)
* [ ] Une seule `.slide.active` au chargement

### Charte (cf. CLB-presentation-rule.md)

* [ ] Century Gothic partout, fond blanc, texte navy
* [ ] Palette strictement CLB, lime réservé aux données/positif
* [ ] Logo haut-droite, URL bas-droite (8 pt), date bas-gauche `JJ/MM/AAAA`
* [ ] Titres et corps alignés à gauche
* [ ] Toutes les couleurs via variables CSS

### Navigation

* [ ] Barre `#nav` flottante : prev · compteur · dots · next
* [ ] Dots générés en JS, dot actif `.on`
* [ ] `goTo(i)` borne les index et mis à jour compteur + boutons disabled
* [ ] Clavier `←`/`→` ; `Échap` ferme overlay en priorité
* [ ] Retour en haut de page à chaque changement de slide

### Modales / lightbox (si présents)

* [ ] Contenus dans un objet `VIGNETTES{}` séparé du rendu
* [ ] `openModal` gère id inconnu, bloque/restaure le scroll de fond
* [ ] `attachZoomHandlers()` rappelé après chaque injection
* [ ] Fermeture par `✕`, clic fond, `Échap` ; `aria-hidden` synchronisé
* [ ] Images zoomables avec garde `dataset.zoomBound`

### Module notes (si activé)

* [ ] `data-note-id` unique et stable sur chaque textarea + `NOTE_LABELS` à jour
* [ ] Persistance **RAM uniquement** (pas de localStorage)
* [ ] Textareas de modale sauvegardés **avant** fermeture
* [ ] Export version complète (auto-réhydratante via `#preload-notes`)
* [ ] Export compte-rendu `.txt` ; import HTML fonctionnel
* [ ] `<` échappé dans le JSON injecté à l'export
* [ ] Noms de fichiers horodatés

### Technique

* [ ] CSS organisé en sections commentées
* [ ] JS vanilla, données séparées du rendu, gardes anti-double-branchement
* [ ] `try/catch` sur lecture fichier / parsing JSON
* [ ] Responsive `@media (max-width:900px)` : grilles en 1 colonne
* [ ] Aucun `console.error` au chargement ; liens externes en `target="_blank" rel="noopener"`
