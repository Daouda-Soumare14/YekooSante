# GET 409 — S4 — Template Étudiant
## Lovable.dev — Du prompt à l'URL live
### Swiss UMEF University — Campus de Dakar — Juin 2026

---

| Champ | Valeur |
|---|---|
| Équipe | YekooSanté |
| Projet | YekooSanté — Agent de pré-triage médical par SMS |
| Persona | Aminata Sarr · 34 ans · Vendeuse · Pikine · Feature phone |
| URL obtenue | yekoosante.lovable.app *(à noter après l'Étape 5)* |

---

## ⚠️ Règle absolue
> Toujours se connecter **GitHub AVANT** de créer le projet sur Lovable — sinon le projet sera perdu.

---

## Étape 1 — Connexion GitHub (obligatoire avant toute création)

| # | Action | Ce que vous voyez |
|---|---|---|
| 1 | Aller sur **lovable.dev** | Page d'accueil avec zone de texte centrale |
| 2 | Cliquer « Get started » | Fenêtre de connexion |
| 3 | « Continue with GitHub » | Autoriser Lovable à accéder à votre compte GitHub |
| 4 | Choisir le style | Sélectionner style « Modern » |
| 5 | « What should we build ? » | Zone de texte prête → passer à l'Étape 2 |

---

## Étape 2 — Prompt initial YekooSanté (à coller dans Lovable)

```
Crée une application web complète appelée YekooSanté.

# CONTEXTE
YekooSanté est une plateforme numérique de pré-triage médical par SMS
qui analyse les symptômes décrits en français ou en wolof et oriente les patients
vers le bon niveau de soin (domicile / dispensaire / urgences),
pour les habitants des zones périurbaines de Dakar (Pikine, Guédiawaye, Rufisque)
afin de réduire les déplacements inutiles vers des centres de santé surchargés.

# PAGES À CRÉER (3 pages)

1. ACCUEIL
   → Header : logo emoji 🌿 + nom YekooSanté
   → Hero : titre accrocheur + sous-titre + 2 boutons CTA :
     « Décrire mes symptômes » et « Trouver un centre de santé »
   → Section chiffres : 3 stats réalistes sur la santé à Dakar :
     60% des urgences évitables · 45 min de transport moyen · 1 médecin pour 14 000 habitants
   → Footer : mentions légales + contact + numéro SAMU Sénégal (15)

2. CENTRES DE SANTÉ
   → Liste de 6 centres avec : Nom, Zone/Quartier, Horaires, statut
   → Boutons de filtre : Tous | Urgences 24h/24 | Dispensaire | Maternité
   → Chaque centre en carte avec pastille Disponible (vert) ou Indisponible (rouge)

3. CONTACT
   → Formulaire : Nom complet, E-mail, Téléphone, Description des symptômes
   → Bouton d'envoi vert #059669
   → Adresse : Swiss UMEF University — Campus de Dakar, Sénégal

# DESIGN
→ Couleur principale : #059669 (vert santé)
→ Couleur secondaire : #FFFFFF (blanc)
→ Accent : #F59E0B (or/ambre)
→ Police : Inter (sans-serif moderne)
→ Style : moderne, épuré, professionnel
→ Responsive mobile first (breakpoint 768px)
→ Navigation fixe en haut avec les 3 pages

# DONNÉES — 6 centres de santé réels de Dakar
⚠ Données locales précises — critère éliminatoire au barème

1. Centre de Santé de Pikine     | Pikine Icotaf, Dakar          | Lun–Sam · 8h–18h    | Disponible
2. Hôpital Youssou Mbargane      | Rufisque, Dakar               | Urgences 24h/24     | Disponible
3. Centre de Santé de Thiaroye   | Thiaroye-sur-Mer, Guédiawaye  | Lun–Ven · 8h–17h   | Indisponible
4. Poste de Santé de Wakhinane   | Wakhinane Nimzatt, Guédiawaye | Lun–Sam · 8h–15h   | Disponible
5. Centre de Santé de Ngor       | Almadies, Dakar               | Lun–Dim · 8h–20h   | Disponible
6. Poste de Santé de Keur Massar | Keur Massar, Banlieue Dakar   | Lun–Sam · 8h–17h   | Disponible

# STACK TECHNIQUE
→ React + Tailwind CSS + Vite
```

---

## Étape 3 — Checklist MVP (à cocher dans le preview Lovable)

| ☐ | Ce que vous testez | Ce que vous devez voir |
|---|---|---|
| ☐ | 🌿 YekooSanté dans le header | Logo emoji + nom en haut à gauche |
| ☐ | Navigation 3 pages | Accueil \| Centres de santé \| Contact cliquables |
| ☐ | Hero + 2 CTA | Section verte avec « Décrire mes symptômes » + « Trouver un centre » |
| ☐ | 6 cartes + pastilles | Données réelles Dakar + Disponible (vert) / Indisponible (rouge) |
| ☐ | Filtres fonctionnels | Tous / Urgences 24h/24 / Dispensaire / Maternité filtrent les cartes |
| ☐ | Formulaire Contact | 4 champs + bouton vert + adresse Swiss UMEF Dakar |

**⚠️ Si un point échoue :**
Tapez dans le chat Lovable : *« [Description du problème] — corrige cela »*
→ 1 prompt = 1 correction uniquement.

---

## Étape 4 — 3 itérations minimum à documenter (Livrable L3)

| # | Type | Votre prompt | Résultat |
|---|---|---|---|
| P1 | Correction | `Change le statut du Centre de Santé de Thiaroye de Indisponible à Disponible` | ✅ / ❌ |
| P2 | Visuelle | `Ajoute une bannière d'annonce fine en haut de toutes les pages avec : "⚕️ YekooSanté disponible 24h/24 — SAMU Sénégal : 15" — fond vert foncé, texte blanc` | ✅ / ❌ |
| P3 | Fonctionnelle | `Anime les 3 statistiques de la page Accueil : les chiffres défilent de 0 jusqu'à leur valeur finale au chargement de la page` | ✅ / ❌ |
| P4 | Libre | *[Votre choix — correction ou amélioration supplémentaire]* | ✅ / ❌ |

---

## Étape 5 — Publication Lovable (4 clics exacts)

| Clic | Bouton | Ce que vous voyez |
|---|---|---|
| 1 | Icône **Publish** (nuage) — haut droite | Fenêtre de publication |
| 2 | **Continue** (1ère fois) | Choix visibilité : « Public — Anyone with the URL » déjà sélectionné |
| 3 | **Continue** (2ème fois) | Métadonnées SEO auto-générées — ne rien modifier |
| 4 | **Publish** — bouton bleu | URL générée : `yekoosante.lovable.app` |

**🎉 Votre L1 est prêt :**
- Notez l'URL dans la fiche d'identité ci-dessus
- Testez l'URL dans un nouvel onglet
- Soumettez dans les 48h suivant la S4 (35 pts)

---

## Barème S4

| Livrable | Critère éliminatoire | Barème | Délai |
|---|---|---|---|
| L1 — URL Lovable | App inaccessible = 0 | 35 pts | 48h après S4 |
| L2 — GitHub | Dépôt privé = 0 | 25 pts | 48h après S4 |
| L3 — Journal Prompts | Moins de 4 prompts = -10 | 25 pts | 48h après S4 |
| L4 — Captures + Note éthique | Non rendu = 0 | 15 pts | 48h après S4 |

---

*Template S4 — YekooSanté — GET 409 | Swiss UMEF University — Dakar | Juin 2026*
