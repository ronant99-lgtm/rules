# SOCLE — Règles communes à tout contenu clinique

> S'applique à toute recherche bibliographique (`BIBLIO.md`), à tout outil clinique (`OUTILS-cliniques.md`) et à tout document clinique produit pour l'utilisateur.
> Reprend, sans les dupliquer ailleurs, les règles autrefois répétées dans pico-search, fiche-indication, pathology_brief, academique-gen-consensus et clinical-tools (6-E, 6-F).

---

## 1. Langue

- Réponses et documents en **français** par défaut.
- Titres d'articles, intitulés de recommandations, noms de protocoles et acronymes : langue d'origine (souvent l'anglais).
- Terme technique ambigu en français : version anglaise entre parenthèses.
- Unités : Gy, fr (fractions), sem., mois, ans. Nombres décimaux avec virgule.
- Exception : `lang="en"` quand le contenu est destiné à un contexte anglophone (atlas neuro-oncologie, observership).

---

## 2. Cadrage avant de travailler (version allégée)

1. Lire la demande et repérer ce qui manque parmi : **population** (adulte / pédiatrie, primitif / rechute, stade), **intention** (curatif, adjuvant, palliatif…), **objectif d'utilisation** (§3), **niveau de détail RT** (dosimétrie complète ou grands principes).
2. Poser **en un seul message** les questions qui manquent, **4 au maximum**, de préférence sous forme de choix cliquables. Ne pas reposer une question à laquelle la demande répond déjà.
3. Reformuler la question en une phrase (PICO, indication, pathologie ou domaine selon le mode).
4. **Attendre la réponse seulement si la demande est ambiguë** sur la population, l'intention ou l'objectif. Sinon, annoncer la reformulation et lancer le travail ; l'utilisateur peut réorienter.
5. Si l'utilisateur dispose de documents (protocole, recommandation, article), les demander avant la recherche.

---

## 3. Objectif d'utilisation → format de sortie

| Objectif | Niveau de détail | Format par défaut |
|---|---|---|
| **RCP** | Synthétique : messages clés, tableau des études, 1–2 pages | HTML, gabarit R (`DESIGN.md`) |
| **Enseignement / présentation** | Panoramique : chronologie, contexte | HTML gabarit R, ou présentation (`PRESENTATION-CLB.md`) |
| **Manuscrit** | Complet : tous détails dosimétriques, Vancouver | Markdown |
| **Note personnelle / Obsidian** | Intermédiaire : signal principal + lacunes | Markdown |
| **Fiche indication RT** | 4 items + version patient | Markdown copiable (HTML gabarit R sur demande) |

Le HTML est imprimable en PDF sans retouche (feuille d'impression intégrée).

---

## 4. Niveaux de preuve — une seule échelle

### 4.1 Niveau d'une étude ou d'une source (badge)

| Code | Libellé | Usage | Couleur (gabarit R) |
|---|---|---|---|
| `rct` | Essai randomisé | bras d'un essai randomisé, méta-analyse d'essais randomisés | vert |
| `pro` | Essai prospectif / protocole | phase I/II mono-bras, registre prospectif, traitement défini par protocole | ambre |
| `cons` | Recommandation / consensus | société savante, consensus, document ESCP | bleu |
| `retro` | Série rétrospective | ≥ 10 cas, cohorte, registre rétrospectif | gris |
| `cas` | Cas / avis d'expert | < 10 cas, pratique d'expert, analogie avec une entité voisine | gris clair |
| `tech` | Dosimétrique / technique | étude de planification, faisabilité sans critère clinique | violet |
| `pend` | Résultats en attente | essai en cours, données non matures | pointillés |

Ajouter **« abstract »** au badge quand la donnée provient d'un abstract de congrès non publié en revue.
Pour une recommandation, reprendre **le grade et le niveau de preuve tels qu'énoncés par la source** (LoE, grade, catégorie NCCN) ; ne jamais en inventer un.

### 4.2 Niveau global pour la pratique (conclusion)

Standard validé (phase III positif) · Option raisonnable (phase II randomisé) · Expérimental (phase I/II ou rétrospectif) · Contre-indiqué (essai négatif de haut niveau) · Pas de standard / insuffisant pour conclure.

Toujours énoncé explicitement en fin de synthèse.

---

## 5. Sources et références

- **Ne citer que des références réellement retrouvées dans la session.** Une référence issue des connaissances internes est labellisée `[hors recherche]` et ne compte pas comme source.
- **Lien obligatoire** pour chaque référence :
  - publication → `https://doi.org/…` ou `https://pubmed.ncbi.nlm.nih.gov/[PMID]/` ;
  - essai → `https://clinicaltrials.gov/study/NCT…` ;
  - protocole complet (détails dosimétriques) → `https://cdn.clinicaltrials.gov/large-docs/[XX]/NCT…/Prot_SAP_000.pdf` ;
  - recommandation → URL officielle de la société.
- Format **Vancouver** ; liens intégrés aux tableaux et aux références en HTML, entre crochets en Markdown.
- **Signaler** : texte intégral inaccessible (synthèse faite sur le résumé), abstract de congrès, recommandation identifiée mais non consultée (dire laquelle et demander le document).
- **Rechercher les essais négatifs** avec la même rigueur que les positifs.
- **Ne jamais inventer** une référence, un chiffre ou un grade. Une donnée tirée d'une analogie avec une entité voisine est dite comme telle.
- **Référence récurrente** (retrouvée dans ≥ 2 recherches distinctes) : marquée 🔁, probablement incontournable.
- **Lecture recommandée** : `📖 Lecture recommandée : [référence] — [pourquoi, en 1 phrase]` pour l'essai pivot ou la meilleure revue.

---

## 6. Extraction des données de radiothérapie

Pour toute étude ou recommandation impliquant une RT, relever quand la source le fournit :
- **dose totale · nombre de fractions · dose par fraction · timing** (jour, semaine ou cycle de référence) ;
- **BED / EQD2** avec l'α/β précisé, si utile à la comparaison ;
- **technique** (3D, IMRT/VMAT, SRS, SBRT, protons) ;
- **volumes** GTV → CTV → PTV avec marges, si le niveau de détail le demande ;
- **contraintes OAR** notables.

Si ces données manquent dans la publication principale, le dire et proposer le protocole complet.

---

## 7. Vérification des sources avant livraison (ex-règle 6-E)

> Origine : audit d'octobre 2026 — sur 7 outils, environ un tiers des NCT et plusieurs références étaient faux.

- **Chaque DOI / référence** vérifié dans PubMed : titre, premier auteur, année, volume, pages.
- **Chaque chiffre** présent dans la référence citée (au minimum le résumé) ; sinon retiré ou marqué `⚠ verify`.
- **Chaque NCT** vérifié sur ClinicalTrials.gov : bonne pathologie, statut à la date de mise à jour.
- **Interdit** : citer une référence sans DOI vérifié ; attribuer le résultat d'un sous-groupe à un autre ; présenter une fourchette approximative comme un résultat d'essai.
- Donnée non vérifiable mais utile (protocole non public, pratique d'expert) : conservée avec `⚠ verify` et une phrase disant ce qui reste à confirmer.
- Toute correction d'une version antérieure est consignée dans le **journal d'audit** du document ou de l'outil.

---

## 8. Création et modification (ex-règle 6-F)

1. **Expliciter, ne pas trancher seul.** Énoncer les sources (version, date d'amendement) et les hypothèses. Toute **discordance entre sources** est signalée et soumise à décision ; si elle subsiste, elle est affichée (`⚠ verify` ou note).
2. **Modifications chirurgicales.** Ne modifier que ce que la demande vise. **Une demande de design ou de structure ne modifie aucune valeur clinique** (dose, fractionnement, marge, contrainte, seuil, outcome, référence). Anomalie repérée hors périmètre : mentionnée, pas corrigée.
3. **Compte rendu obligatoire** à chaque livraison : valeurs cliniques ajoutées / modifiées / retirées (`élément — avant → après — source`), ou la mention **« Aucune valeur clinique modifiée »** ; plus les discordances non résolues et les `⚠ verify`.
4. **Simplicité** : aucune fonctionnalité non demandée. Exception de sécurité : toute saisie numérique a des **bornes plausibles** ; hors bornes, le calcul est bloqué avec un message explicite.
5. **Vérification finale** : §7 appliqué ; pour un outil, cas tests rejoués (`OUTILS-cliniques.md` §3). Document nouveau ou refonte majeure : **second passage indépendant** qui recoupe les valeurs cliniques avec les sources.

---

## 9. Limites et orientation

- Données fragiles : *« Ces données reposent sur [n] cas publiés — niveau de preuve faible. Une RCP nationale/internationale est recommandée. »*
- Recours nationaux à citer quand pertinent : neuro-oncologie **RENOCLIP-LOC** ; tumeurs rares pédiatriques **SFCE / FRACTAL** ; sarcomes **NetSarc / RRePS** ; sinon mentionner l'existence d'un recours national sans inventer de nom.
- Tout document clinique porte la mention : usage personnel, aide à la préparation des RCP, ne se substitue ni au jugement clinique ni aux décisions collégiales.
