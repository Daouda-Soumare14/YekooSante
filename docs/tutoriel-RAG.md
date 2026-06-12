# Tutoriel RAG — Base de Connaissances Dify
## Livrable S5 | YekooSanté | GET 409 | Swiss UMEF University — Dakar | Juin 2026

---

| Champ | Valeur |
|---|---|
| Équipe | YekooSanté |
| Nom de la base | YekooSante_KB_v1 |
| Document indexé | `yekoo_kb_symptomes_juin2026.csv` — 15 symptômes · protocoles MSAS Sénégal |
| Mode d'index | Économique · Index inversé · Top K = 3 · Chunk size = 300 |
| Agent à connecter | YekooSante_TriageMedical_v1 |
| Résultat visé | L'agent répond avec les données réelles du CSV (niveaux de triage + centres Dakar) |

---

## 💡 Qu'est-ce que le RAG ?

**RAG = Retrieval-Augmented Generation** : l'agent cherche d'abord dans VOS documents avant de générer.

- **Sans RAG** → l'agent répond depuis ses données d'entraînement → risque d'hallucination sur les centres de santé de Dakar
- **Avec RAG** → l'agent lit votre CSV de symptômes → réponses ancrées dans les protocoles MSAS Sénégal réels

**Pour YekooSanté :** le CSV `yekoo_kb_symptomes_juin2026.csv` enrichit les fiches médicales générées avec les niveaux de triage et les centres de santé locaux.

---

## Étape 1 — Préparer le CSV dans Google Sheets

### 1.1 — Ouvrir Google Sheets
1. Aller sur **sheets.google.com** → nouveau classeur
2. Coller le contenu du fichier `S5/yekoo_kb_symptomes_juin2026.csv` en cellule **A1**

### 1.2 — Vérification des règles CSV pour Dify

| Règle | Notre CSV | Statut |
|---|---|---|
| En-têtes en ligne 1 | Symptome, Niveau, Duree_typique... | ✅ |
| Séparateur virgule (pas point-virgule) | Virgules uniquement | ✅ |
| Pas de cellules fusionnées | Données nettes | ✅ |
| Taille max 15 MB | ~3 KB | ✅ |
| Encodage UTF-8 | Caractères accentués OK | ✅ |

### 1.3 — Télécharger en CSV
**Fichier → Télécharger → Format CSV (.csv)**
→ Sauvegarder sous : `yekoo_kb_symptomes_juin2026.csv`

---

## Étape 2 — Créer la base dans Dify

1. Dans **Dify**, cliquer sur l'onglet **Connaissance** (barre de navigation en haut)
2. Cliquer **+ Créer des Connaissances**
3. Laisser sélectionné : **Importer à partir d'un fichier**
4. Cliquer **Parcourir** → sélectionner `yekoo_kb_symptomes_juin2026.csv`
5. Vérifier que le fichier apparaît avec son nom et sa taille → cliquer **Suivant**

---

## Étape 3 — Configurer le chunking et l'indexation

| Paramètre | Valeur YekooSanté | Pourquoi |
|---|---|---|
| Longueur du morceau | **300** | CSV médical = données courtes par ligne |
| Chevauchement | **50** | Évite de couper une ligne CSV en deux |
| Mode d'index | **Économique** | Plan gratuit Dify — fonctionne par mots-clés |
| Récupération | **Index inversé** | Recherche par correspondance exacte |
| Top K | **3** | 3 symptômes similaires retournés par requête |

→ Cliquer **Enregistrer & Traiter**
→ Attendre le message **« INTÉGRATION TERMINÉE »** + pastille verte 🟢

---

## Étape 4 — Renommer la base et vérifier

1. Cliquer **« Aller au document »** → dans le panneau gauche → **« ... »** → Paramètres
2. Renommer : **YekooSante_KB_v1**
3. Vérifier : **Documents → Statut : 🟢 Disponible**

---

## Étape 5 — Tests de récupération (obligatoire avant connexion)

Dans la base `YekooSante_KB_v1`, cliquer sur **« Test de Récupération »** (menu gauche).

| # | Question test | Résultat attendu |
|---|---|---|
| 1 | `fièvre frissons maux de tête` | Chunk : Fièvre + frissons · MODÉRÉ · Paludisme possible ✅ |
| 2 | `diarrhée vomissements déshydratation` | Chunk : Choléra · URGENT · Hôpital Youssou Mbargane ✅ |
| 3 | `centre de santé urgences Rufisque` | Chunk : Hôpital Youssou Mbargane · Urgences 24h/24 ✅ |

⚠️ **Si un test échoue :** utilisez exactement les mots du CSV dans votre question (correspondance exacte en mode Économique).

---

## Étape 6 — Connecter la base au workflow Dify

1. Aller dans **Studio** → ouvrir `YekooSante_TriageMedical_v1`
2. Cliquer sur le **+** entre le nœud **DÉBUT** et le nœud **CHERCHEUR**
3. Sélectionner **« Récupération de connaissances »**
4. Dans le panneau du nœud :
   - **TEXTE DE LA REQUÊTE** : sélectionner `Début · query`
   - **CONNAISSANCES** : cliquer **+** → sélectionner `YekooSante_KB_v1`
5. Cliquer sur le nœud **CHERCHEUR** → section **CONTEXTE** → sélectionner **« Récupération de connaissances · result »**
6. Dans le prompt **SYSTEM** du CHERCHEUR, ajouter à la fin :

```
DONNÉES DE LA BASE DE CONNAISSANCES MÉDICALES (MSAS SÉNÉGAL) :
{{#context#}}

Utilise ces données en priorité pour évaluer les symptômes décrits.
Si un symptôme correspond à une entrée de la base → reprends le NIVEAU et la RECOMMANDATION indiqués.
```

7. Cliquer **Publier → Publier une mise à jour**

---

## Checklist complète S5 — RAG

| ☐ | Action | Paramètre / Résultat |
|---|---|---|
| ☐ | CSV uploadé dans Connaissance | `yekoo_kb_symptomes_juin2026.csv` visible |
| ☐ | Chunk size | 300 · Overlap 50 |
| ☐ | Mode index | Économique · Index inversé |
| ☐ | Statut base | 🟢 Disponible |
| ☐ | Base renommée | `YekooSante_KB_v1` |
| ☐ | Test Q1 (fièvre frissons) | Chunks avec MODÉRÉ + Pikine retournés |
| ☐ | Test Q2 (diarrhée vomissements) | Chunks avec URGENT retournés |
| ☐ | Test Q3 (centre urgences) | Chunks Hôpital Youssou Mbargane retournés |
| ☐ | Nœud RAG ajouté au workflow | DÉBUT → RÉCUPÉRATION → CHERCHEUR visible |
| ☐ | Requête connectée | TEXTE DE LA REQUÊTE → `Début · query` |
| ☐ | Base connectée | CONNAISSANCES → `YekooSante_KB_v1` affiché |
| ☐ | Contexte injecté | CHERCHEUR · CONTEXTE → `Récupération · result` |
| ☐ | `{{#context#}}` dans le prompt | Ligne ajoutée au bas du SYSTEM |
| ☐ | Workflow republié | Publier une mise à jour → build sans erreur |
| ☐ | Test final end-to-end | Fiche générée avec données CSV intégrées |

---

## Guide de débogage RAG

| ❌ Problème | ✅ Solution |
|---|---|
| Base ne s'indexe pas | Fichier > 15MB ou mauvais encodage → re-télécharger en UTF-8 |
| Test de récupération vide | Utiliser les mots exacts du CSV dans la question test |
| Agent ignore la base | Vérifier que `YekooSante_KB_v1` est dans CONNAISSANCES du nœud RAG |
| `{{#context#}}` non reconnu | Vérifier que le contexte est connecté dans CHERCHEUR · CONTEXTE |
| Réponses hors-sujet | Chunk size trop grand → réduire à 200 |

---

*Tutoriel RAG S5 — YekooSanté — GET 409 | Swiss UMEF University — Dakar | Juin 2026*
