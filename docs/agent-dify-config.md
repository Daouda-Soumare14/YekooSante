# Configuration Agent Dify — YekooSanté
## Livrable L1 + L2 | Séance 3 | GET 409

---

## Informations du projet

| Champ | Valeur |
|---|---|
| Nom de l'équipe | YekooSanté |
| Nom du projet / application | YekooSanté — Agent de pré-triage médical |
| Problème résolu | Permettre aux habitants de Pikine d'évaluer leurs symptômes par SMS sans se déplacer |
| Utilisateurs cibles | Habitants des zones périurbaines de Dakar (Pikine, Guédiawaye, Rufisque) |
| Domaine | Santé & Télémédecine |
| Zone géographique | Dakar — Banlieue périurbaine — Sénégal |

---

## Architecture du Workflow Dify

```
[DEBUT] → [CHERCHEUR] → [SI/SINON] → [REDACTEUR] → [SORTIE 2]
                              |
                              └→ [SORTIE 1 — Branche IF]
```

### Description des nœuds

| Nœud | Type | Rôle |
|---|---|---|
| DEBUT | Entrée utilisateur | Reçoit la question SMS (variable : `query`) |
| CHERCHEUR | LLM (Llama-3.1-8b) | Analyse les symptômes, retourne données structurées ou "INSUFFISANT" |
| SI/SINON | Contrôle logique | Si `CHERCHEUR.text` contient `INSUFFISANT` → branche IF, sinon → ELSE |
| REDACTEUR | LLM (Llama-3.1-8b) | Rédige la fiche de recommandation médicale à partir des données du Chercheur |
| SORTIE 1 | Sortie (branche IF) | Affiche le message d'erreur INSUFFISANT à l'utilisateur |
| SORTIE 2 | Sortie (branche ELSE) | Affiche la fiche médicale complète |

---

## ÉTAPE A — Créer le Workflow

**Durée : 5 minutes**

1. Ouvrir **dify.ai** → Studio
2. Cliquer « Créer à partir de zéro »
3. Sélectionner **Flux de travail** (⚠️ PAS Chatbot, PAS Agent, PAS Chatflow)
4. Nommer le projet : `YekooSante_TriageMedical_v1_YekooSante`
5. Cliquer « Entrée utilisateur (nœud de départ original) »
6. Le canvas s'ouvre avec le nœud **DEBUT**

---

## ÉTAPE B — Configurer le nœud CHERCHEUR

**Durée : 15 minutes**

### B1 — Ajouter le nœud LLM
- Dans le menu ouvert automatiquement → cliquer **LLM**
- Double-cliquer sur le texte "LLM" dans le nœud → renommer : **CHERCHEUR**

### B2 — Configurer le modèle
- Modèle : **Llama-3.1-8b-instant** (via GroqCloud)
- Temperature : **0,3** (réponses précises et factuelles)

### B3 — Coller le prompt système CHERCHEUR

Dans le champ **SYSTEM**, coller exactement :

```
Tu es un analyste médical spécialisé en santé communautaire au Sénégal pour YekooSanté,
service de pré-triage médical par SMS destiné aux habitants de Pikine, Guédiawaye et Rufisque.

MISSION : Analyser la description de symptômes reçue et retourner soit une évaluation structurée,
soit le mot-clé INSUFFISANT si les symptômes sont trop vagues pour être analysés.

PROCESSUS EN 3 ÉTAPES :
1. ANALYSER la description :
   - Quel(s) symptôme(s) est/sont décrits ?
   - Depuis combien de temps ? (si mentionné)
   - Quelle population ? (adulte, enfant, femme enceinte — si mentionné)
2. ÉVALUER la gravité sur 3 niveaux :
   - BÉNIN : symptôme non urgent, soins à domicile possibles
   - MODÉRÉ : consultation recommandée dans les 24-48h
   - URGENT : consultation immédiate requise (urgences)
3. ÉVALUER la suffisance des données

Symptômes de référence courants à Dakar :
- Paludisme : fièvre + frissons + maux de tête
- Choléra : diarrhée aqueuse + vomissements + déshydratation rapide
- IRA (Infections Respiratoires Aiguës) : toux + fièvre + difficultés respiratoires
- Hypertension : maux de tête intenses + vision trouble
- Plaie infectée : rougeur + gonflement + pus

FORMAT DE SORTIE OBLIGATOIRE :
Si données SUFFISANTES :
SYMPTÔME : [description brève]
NIVEAU : [BÉNIN / MODÉRÉ / URGENT]
RECOMMANDATION : [action en 1 phrase]
ACTION IMMÉDIATE : [quoi faire maintenant]
SOURCES : [base OMS / protocoles MSAS Sénégal]

Si données INSUFFISANTES :
Retourner UNIQUEMENT : "INSUFFISANT : [raison précise en 1 phrase]"
```

### B4 — Ajouter la variable d'entrée USER
1. Cliquer **+ Ajouter un message** dans le nœud CHERCHEUR
2. Sélectionner la variable **{x} → Debut · query**
3. Le badge doit afficher : `Debut [x] query` — SANS triangle orange

### B5 — Configurer le nœud DEBUT
1. Cliquer sur le nœud **DEBUT**
2. Remplir le formulaire :
   - Field Type : **Short Text / string**
   - Variable Name : **query**
   - Label Name : **Décrivez vos symptômes**
   - Required : **coché ✅**
3. Cliquer **Enregistrer**

**✅ Vérification B :**
- Nœud DEBUT : `(x) query REQUIS` visible
- Nœud CHERCHEUR : modèle Llama-3.1-8b-instant visible
- Champ SYSTEM : prompt collé (800+ caractères)
- Message USER : badge `Debut [x] query` sans triangle orange

---

## ÉTAPE C — Configurer le nœud SI/SINON

**Durée : 10 minutes**

1. Cliquer le **+** bleu à droite du nœud CHERCHEUR → sélectionner **SI/SINON**
2. Cliquer **+ Ajouter une condition**
3. Configurer la condition :
   - **Variable :** Chercheur · text
   - **Opérateur :** contient
   - **Valeur :** `INSUFFISANT` (⚠️ MAJUSCULES OBLIGATOIRES — copier-coller exactement)
4. Connecter la branche **IF** → vers le nœud **SORTIE 1** (à créer)
5. Connecter la branche **ELSE** → vers le nœud **REDACTEUR** (étape D)

⚠️ **Erreur fréquente :** Ne pas écrire "insuffisant" en minuscules. La casse doit être exacte.

**✅ Vérification C :**
- Condition visible : `Chercheur · text · contient · INSUFFISANT`

---

## ÉTAPE D — Configurer le nœud REDACTEUR

**Durée : 15 minutes**

### D1 — Ajouter et renommer
- Cliquer le **+** bleu du nœud SI/SINON (branche ELSE) → **LLM**
- Double-cliquer → renommer : **REDACTEUR**

### D2 — Configurer le modèle
- Modèle : **Llama-3.1-8b-instant**
- Temperature : **0,7** (plus fluide pour la rédaction)

### D3 — Coller le prompt système REDACTEUR

Dans le champ **SYSTEM**, coller exactement :

```
Tu es un rédacteur spécialisé en communication santé pour YekooSanté,
service de pré-triage médical par SMS destiné aux habitants de Pikine, Dakar.

MISSION : Rédiger une fiche de recommandation médicale claire et rassurante
à partir des données d'analyse reçues.

EXEMPLE DE FICHE ATTENDUE :
――――――――――――――――――――――――
YEKOO SANTÉ
Votre fiche de santé
――――――――――――――――――――――――
SYMPTÔME : Fièvre + maux de tête
NIVEAU : MODÉRÉ
――――――――――――――――――――――――
RECOMMANDATION
Consultez un médecin dans les 24h.
Ce symptôme peut indiquer un début de paludisme.
――――――――――――――――――――――――
ACTION IMMÉDIATE
• Prenez du paracétamol (500mg adulte)
• Buvez beaucoup d'eau
• Évitez l'aspirine si vous êtes enceinte
――――――――――――――――――――――――
OÙ ALLER
Centre de santé de Pikine (ouvert 8h-18h)
Hôpital Youssou Mbargane — Urgences 24h/24
――――――――――――――――――――――――
⚠️ Si les symptômes s'aggravent rapidement,
allez aux urgences immédiatement.
――――――――――――――――――――――――

SUR CE MODELE, rédige la fiche pour les données reçues.
Si une donnée manque : indiquer "Non précisé".
Ton : clair, rassurant, accessible. Maximum 200 mots.
Langue : français simple (niveau CM2).
```

### D4 — Connecter les données du Chercheur
1. Cliquer **+ Ajouter un message** dans le nœud REDACTEUR
2. Sélectionner **{x} → Chercheur · text**
3. Le badge doit afficher : `Chercheur [x] text` — SANS triangle orange

⚠️ **Ne jamais écrire** `{{#Chercheur.text#}}` directement dans le SYSTEM — utiliser toujours le message USER avec {x}.

### D5 — Ajouter le nœud SORTIE finale
1. Cliquer le **+** du nœud REDACTEUR → **Sortie**
2. Configurer la variable de sortie :
   - Nom : **fiche_medicale**
   - Valeur : **{x} → Redacteur · text**

**✅ Vérification D :**
- Nœud REDACTEUR : modèle Llama-3.1-8b-instant visible
- Champ SYSTEM : prompt collé avec l'exemple de fiche
- Message USER : badge `Chercheur · text` sans triangle orange
- SORTIE 2 : variable `Redacteur · text` configurée

---

## ÉTAPE — Configurer SORTIE 1 (branche IF)

1. Sur la branche **IF** du SI/SINON → ajouter un nœud **Sortie**
2. Configurer :
   - Nom : **message_erreur**
   - Valeur : **{x} → Chercheur · text**

⚠️ **Sans cette étape, la publication échoue avec l'erreur "variable de sortie est requis".**

---

## ÉTAPE E — Tester et Publier

**Durée : 10 minutes**

### E1 — Lancer le test
1. Cliquer **Exécuter test** (bouton en haut à droite)
2. Tester avec les 2 questions de test :

**Question 1 (précise — doit aller directement au REDACTEUR) :**
> *« J'ai de la fièvre depuis 2 jours avec des maux de tête et des frissons. Je suis à Pikine. »*

**Question 2 (vague — doit déclencher INSUFFISANT) :**
> *« Je ne me sens pas bien »*

3. Vérifier que :
   - Question 1 → nœud CHERCHEUR → SI/SINON (branche ELSE) → REDACTEUR → SORTIE 2 ✅
   - Question 2 → nœud CHERCHEUR → INSUFFISANT → SI/SINON (branche IF) → SORTIE 1 ✅

### E2 — Publier
1. Cliquer **Publier**
2. Copier l'URL publique générée
3. Tester l'URL depuis un autre navigateur ou un téléphone

---

## Guide de débogage rapide

| Erreur | Cause | Solution |
|---|---|---|
| `#sys.query# not found` | Variable query non définie dans DEBUT | Nœud DEBUT → + Champ de saisie → Variable Name : query |
| `#Chercheur.text# not found` | Variable écrite dans SYSTEM au lieu de USER | Utiliser message USER avec {x} → Chercheur · text |
| Condition IF ne se déclenche pas | INSUFFISANT mal orthographié | Copier-coller exactement : `INSUFFISANT` (majuscules) |
| `variable de sortie est requis` | Nœud SORTIE branche IF sans variable | SORTIE 1 → + variable → message_erreur → Chercheur · text |
| Aucun fournisseur configuré | Clé GroqCloud non enregistrée | Paramètres → Fournisseurs → GroqCloud → coller clé → Enregistrer AVANT de fermer |

---

*Livrable L1 + L2 — Configuration Agent Dify — YekooSanté — GET 409 | Swiss UMEF University — Dakar | 2025–2026*
