# Journal des Itérations — Séance 4
## Livrable L3 | YekooSanté | GET 409 | Swiss UMEF University — Dakar | Juin 2026

---

| Équipe | YekooSanté |
|---|---|
| Projet | YekooSanté — Pré-triage médical par SMS |
| Outil | Lovable.dev |

---

## ⚡ Règles d'or des itérations Lovable
- **1 prompt = 1 modification** — ne jamais empiler plusieurs demandes dans un même message
- Si ça échoue 2 fois → simplifiez le prompt, ne le relancez pas tel quel
- Si ça part en boucle → cliquez **Stop** puis **Revert** pour revenir à la version précédente
- Commencez par les corrections simples (données, couleurs) avant les améliorations visuelles

---

## P1 — CORRECTION | Corriger une donnée existante

**⚡ Consommation estimée : faible (~5K tokens)**

**Objectif :** Mettre à jour le statut du Centre de Santé de Thiaroye

**Prompt exact envoyé :**
```
Change le statut du Centre de Santé de Thiaroye-sur-Mer de "Indisponible"
à "Disponible" et mets ses horaires à "Lun–Sam · 8h–18h" au lieu de "Lun–Ven · 8h–17h".
```

**Résultat :** ✅
Lovable a modifié uniquement la carte concernée. La pastille est passée de rouge (Indisponible) à verte (Disponible). Les horaires ont été mis à jour. Le build a compilé sans erreur. Version sauvegardée automatiquement dans l'historique.

**Vérification :** Page Centres de santé → carte Thiaroye → pastille verte + horaires corrects ✅

---

## P2 — AMÉLIORATION VISUELLE | Ajouter un élément à l'interface

**⚡ Consommation estimée : moyenne (~20K tokens)**

**Objectif :** Ajouter une bannière d'alerte médicale en haut de toutes les pages

**Prompt exact envoyé :**
```
Ajoute une bannière d'annonce fine en haut de toutes les pages
avec le texte : "⚕️ YekooSanté disponible 24h/24 — En cas d'urgence vitale : SAMU Sénégal 15"
Fond vert foncé #065F46, texte blanc, hauteur fine (40px max).
```

**Résultat :** ✅
Bannière ajoutée sur les 3 pages (Accueil, Centres de santé, Contact). Hauteur fine respectée. Texte blanc sur fond vert foncé. La bannière s'affiche au-dessus de la navigation fixe. Sur mobile, le texte se réorganise en 2 lignes lisiblement.

**Vérification :** Naviguer sur les 3 pages → bannière visible partout ✅

---

## P3 — AMÉLIORATION FONCTIONNELLE | Ajouter un comportement dynamique

**⚡ Consommation estimée : élevée (~30K tokens)**

**Objectif :** Animer les statistiques de la page Accueil

**Prompt exact envoyé :**
```
Anime les 3 statistiques de la page Accueil :
les chiffres défilent de 0 jusqu'à leur valeur finale au chargement de la page.
- "60%" part de 0% et monte jusqu'à 60%
- "45 min" part de 0 et monte jusqu'à 45
- "1/14 000" apparaît progressivement en fondu (fade-in)
Durée de l'animation : 2 secondes. Déclenchement au scroll vers la section.
```

**Résultat :** ✅
Les deux premiers chiffres s'animent en comptant de 0 à leur valeur cible (2 secondes). Le troisième s'affiche en fondu. L'animation se déclenche quand l'utilisateur fait défiler jusqu'à la section. Effet professionnel et fluide.

**Vérification :** Page Accueil → défiler jusqu'aux stats → observer l'animation ✅

---

## P4 — LIBRE | Amélioration supplémentaire

**⚡ Consommation estimée : moyenne (~15K tokens)**

**Objectif :** Améliorer l'accessibilité mobile du menu de navigation

**Prompt exact envoyé :**
```
Sur mobile (écran < 768px), le menu de navigation affiche tous les liens en ligne
ce qui dépasse de l'écran. Transforme-le en menu hamburger (☰) pour les écrans
sous 768px. Le menu s'ouvre en overlay quand on clique sur l'icône ☰
et se ferme avec un ✕. Ne modifie PAS la version desktop du menu.
```

**Résultat :** ✅
Menu hamburger (☰) ajouté sur mobile. Clic sur ☰ ouvre un menu vertical en overlay avec les 3 liens (Accueil, Centres de santé, Contact). Clic sur un lien ferme le menu et navigue vers la page. Le menu desktop (horizontal) n'a pas été modifié. Responsive parfait sur iPhone et Android.

**Vérification :** DevTools (F12) → mode mobile → tester le menu hamburger ✅

---

## Tableau récapitulatif

| # | Type | Objectif | Tokens consommés | Résultat |
|---|---|---|---|---|
| P1 | Correction | Statut + horaires Thiaroye | ~5K | ✅ |
| P2 | Visuelle | Bannière SAMU toutes pages | ~20K | ✅ |
| P3 | Fonctionnelle | Animation statistiques accueil | ~30K | ✅ |
| P4 | Libre | Menu hamburger mobile | ~15K | ✅ |

**Total tokens consommés : ~70K** — Dans les limites du plan gratuit Lovable.

---

## Protocole d'urgence

| Problème | Solution |
|---|---|
| Build en erreur après itération | Copier le message → taper : `Corrige cette erreur : [coller le message]` |
| Lovable tourne en boucle | Cliquer **Stop** → **Revert** → simplifier le prompt |
| Modification non visible | Cliquer le bouton reload (↺) en haut du preview |
| Tokens épuisés | Revenir le lendemain — recharge quotidienne |
| Itération a cassé une page | Historique (icône horloge) → sélectionner dernière version fonctionnelle → Restore |

---

*Journal Itérations L3 — YekooSanté — GET 409 | Swiss UMEF University — Dakar | Juin 2026*
