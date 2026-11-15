# PRÉSENTATION CLB — Diaporamas HTML

> Supports de formation, exposés, modules pédagogiques interactifs du Centre Léon Bérard.
> **Charte graphique : `DESIGN.md` § CLB** (fait foi, non répétée ici). Référence d'implémentation : `IGRT_formation_v4_33.html`.
> Ne pas transposer les conventions des outils cliniques (gabarit O) : ici Century Gothic, fond blanc, pas de bascule de thème, pas de `localStorage`, pas de D3.

---

## 1. Architecture

- **Un seul fichier `.html`**, CSS dans `<style>`, JS vanilla en fin de `<body>` ; **aucune dépendance externe** (ni framework ni CDN). Fonctionne en `file://`.
- Ressources externes admises : images locales (`images/`) et liens sortants (`target="_blank" rel="noopener"`).
- Structure :
```
#sw                         conteneur des diapositives
  .slide.active #s1         couverture (kicker, titre, sous-titre, méta, tags, auteur + date)
  .slide #s2 … #sN          diapositives de contenu
nav#nav                     barre flottante : ‹ · compteur i / N · points · ›
#mover                      modale (vignettes)        [optionnel]
#lightbox                   zoom d'image              [optionnel]
.clb-footer / .clb-footer-url   date bas-gauche · URL bas-droite
#preload-notes              JSON des notes            [si module notes]
```
- Diapositive : `.slide-header` = sur-titre `.stag` (cyan, majuscules espacées) + titre `h2.st` ; une seule `.active` à la fois ; padding `2rem 3rem 6rem` (place de la barre).
- `<title>` : `Sujet — Contexte · CLB`.

## 2. Échelle typographique HTML

Base 17 px (15 px sous 900 px). Couverture `clamp(2.6rem,5.2vw,4.4rem)` · titre de diapositive `1.85rem` · sous-titre de bloc `1.2rem` · sur-titre `0.78rem` · corps `1rem`, interligne 1,65 · légende `0.82–0.92rem` gris. Tout aligné à gauche.

Variables propres au HTML, en plus de la charte : `--clb-bg-soft:#F8FAFC`, `--clb-border:#E1E8F0`, `--clb-border-strong:#B8C5D6`, ombres douces navy. **Familles de catégories** en triplets `--xx-color / --xx-bg / --xx-border` (ex. IGRT : normofractionné / SBRT / délai critique), toujours dans la palette CLB.

## 3. Navigation (obligatoire)

- `goTo(i)` borne l'index, met à jour compteur, points (`.on`) et boutons désactivés, masque la date sur la couverture, remonte en haut.
- Points générés en JS, un par diapositive.
- Clavier : `←` / `→` ; **`Échap` ferme d'abord la modale ou le zoom**, sans changer de diapositive.

## 4. Modales / vignettes (optionnel)

- Contenus dans un objet unique `VIGNETTES{}` séparé du rendu, trame interne cohérente.
- `openModal(id)` : id inconnu géré, défilement de fond bloqué puis **restauré** à la fermeture, zooms et notes rebranchés après injection.
- Fermeture : `✕`, clic sur le fond, `Échap` ; `role="dialog"`, `aria-modal="true"`, `aria-hidden` synchronisé.
- Cartes cliquables accessibles au clavier (`role="button"`, `tabindex="0"`, Entrée / Espace).

## 5. Zoom d'image (recommandé si images)

Images `.zoom` ouvrant un plein écran ; `attachZoomHandlers()` rappelé après chaque injection, garde `dataset.zoomBound` contre le double branchement.

## 6. Module de notes (optionnel, supports de formation)

- Zones `textarea` avec `data-note-id` unique et stable ; dictionnaire `NOTE_LABELS` (id → libellé).
- **Mémoire vive uniquement** (objet `NOTES`, jamais `localStorage`) ; notes des modales sauvegardées **avant** fermeture.
- Barre d'outils en fin de présentation : **Enregistrer la version** (HTML complet, notes réinjectées dans `#preload-notes`, rechargées automatiquement à l'ouverture) · **Compte rendu** (`.txt` des remarques non vides) · **Importer** (relit un HTML exporté).
- À l'export, échapper `<` dans le JSON (`<`) ; fichiers horodatés `Sujet_AAAA-MM-JJ.html` / `Sujet_remarques_AAAA-MM-JJ.txt` ; `try/catch` sur la lecture et le parsing.

## 7. Composants

Tags catégoriels (pastilles bordées, triplets de couleur) · encadré `hlbox` (fond doux, bordure gauche cyan 4 px) · cartes `.inccard` / `.featured` / `.adcard` cliquable · tableaux selon la charte · vidéo locale ou iframe 16:9, sans lecture automatique sonore.

## 8. Responsive et code

- Point de rupture unique à 900 px : grilles en une colonne, marges réduites.
- CSS en sections commentées ; couleurs uniquement via variables de la charte.
- JS : données séparées du rendu, gardes anti-double-branchement, commentaires `// ═══ NAVIGATION ═══`.
- `lang="fr"` ; `aria-label` sur les boutons-icônes (`‹`, `›`, `✕`).

## 9. Checklist avant livraison

- [ ] Fichier unique sans dépendance, ouvrable en `file://`, une seule diapositive active au chargement
- [ ] Charte CLB respectée (police, palette, logo, URL, date, alignement à gauche)
- [ ] Navigation : barre, points, clavier, `Échap` prioritaire sur les overlays
- [ ] Modales / zoom / notes (si présents) conformes aux §4–6
- [ ] Aucune erreur console ; responsive à 900 px
