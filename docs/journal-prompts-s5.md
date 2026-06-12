# Journal de Prompts — Séance 5
## YekooSanté | GET 409 | Swiss UMEF University — Dakar | Juin 2026

---

| Équipe | YekooSanté |
|---|---|
| Séance | S5 — Intégration RAG + Webhook |
| Outils | Dify.ai · Lovable.dev · Claude.ai |

---

## Tableau récapitulatif

| # | Technique | Outil | Objectif | Note /5 |
|---|---|---|---|---|
| P1 | Prompt structuré | Claude.ai | Préparer le CSV pour la base RAG | 5/5 |
| P2 | Zero-Shot | Dify — Test de récupération | Valider la qualité d'indexation | 5/5 |
| P3 | Few-Shot (webhook) | Lovable.dev | Intégrer l'agent IA dans le MVP | 5/5 |
| P4 | Chain-of-Thought | Claude.ai | Note d'éthique RAG (L4) | 4/5 |

---

## P1 — Prompt structuré | Préparer le CSV pour la base RAG

**Technique :** Prompt structuré (Rôle + Tâche + Format strict)
**Outil :** Claude.ai

**Prompt envoyé :**
```
Tu es expert en systèmes RAG pour Dify.ai et en santé communautaire au Sénégal.
Génère un fichier CSV optimisé pour notre base de connaissances RAG YekooSanté.

Contexte : agent de pré-triage médical pour habitants de Pikine, Guédiawaye, Rufisque.

Colonnes obligatoires (séparateur virgule, encodage UTF-8) :
Symptome, Niveau (BÉNIN/MODÉRÉ/URGENT), Duree_typique, Recommandation,
Action_immediate, Centres_conseilles, Source

Contraintes :
- 15 lignes minimum avec symptômes réalistes à Dakar (paludisme, choléra, IRA, HTA, plaies)
- Centres de santé réels : Pikine, Hôpital Youssou Mbargane Rufisque, Thiaroye, Wakhinane, Ngor, Keur Massar
- Sources : MSAS Sénégal 2026 et OMS
- Chaque cellule doit être auto-suffisante pour le chunking (chunk size 300)
- Pas de cellules fusionnées, pas de markdown dans les cellules
```

**Résumé de la réponse IA :**
L'IA a généré un CSV complet de 15 lignes couvrant les 5 pathologies prioritaires de Dakar (paludisme, choléra, IRA, HTA, plaies infectées) avec 4 cas supplémentaires (abdomen, cutané, syncope, etc.). Chaque ligne inclut le niveau de triage MSAS, l'action immédiate, et les vrais noms des centres de santé de Pikine/Rufisque/Guédiawaye. Format CSV propre, UTF-8, virgules uniquement.

**Note : 5/5**
CSV directement uploadable dans Dify sans modification. La contrainte "chunk size 300" a bien été respectée — chaque ligne est auto-suffisante et ne dépasse pas 250 caractères.

**Itération :** Aucune — résultat opérationnel dès la première version.

---

## P2 — Zero-Shot | Tests de validation de la base RAG

**Technique :** Zero-Shot (questions directes dans Dify — onglet Test de Récupération)
**Outil :** Dify.ai — Base YekooSante_KB_v1

**Questions test envoyées (3) :**

**Q1 :** `fièvre frissons maux de tête`
**Résultat attendu :** Chunk paludisme · MODÉRÉ · Centre Santé Pikine
**Résultat obtenu :** ✅ Chunk correct retourné avec score de similarité 0.89

**Q2 :** `diarrhée vomissements déshydratation`
**Résultat attendu :** Chunk choléra · URGENT · Hôpital Youssou Mbargane
**Résultat obtenu :** ✅ Chunk correct retourné avec score 0.92

**Q3 :** `centre de santé urgences nuit Rufisque`
**Résultat attendu :** Chunk Hôpital Youssou Mbargane · Urgences 24h/24
**Résultat obtenu :** ✅ Chunk correct avec adresse et horaires retournés

**Note : 5/5**
Les 3 tests ont retourné les chunks attendus. L'index inversé fonctionne correctement pour les mots-clés médicaux courants. La base est prête à être connectée au workflow.

**Itération :** Aucune — la configuration chunk 300 + overlap 50 + Top K 3 est optimale pour notre CSV.

---

## P3 — Few-Shot (via exemple Webhook) | Intégration dans Lovable

**Technique :** Few-Shot implicite — le prompt webhook fournit l'exemple de comportement attendu
**Outil :** Lovable.dev — éditeur du MVP YekooSanté

**Prompt envoyé :**
*(Version complète dans `S5/template-webhook.md` — extrait clé ci-dessous)*
```
Dans mon MVP YekooSanté, ajoute une fonctionnalité de consultation de l'agent IA
sur la page Contact.
[...interface + connexion webhook Dify + traitement réponse + style #059669...]
Ajoute aussi en bas de la zone résultat un avertissement fixe :
"⚠️ YekooSanté ne remplace pas un médecin. Urgences : 15"
```

**Résumé de la réponse Lovable :**
Lovable a généré un composant React complet intégrant : champ textarea avec placeholder "Décrivez vos symptômes...", bouton vert "Demander à YekooSanté 🌿", spinner de chargement, zone de résultat fond gris (#F3F4F6), gestion des erreurs (401 → "Service indisponible", timeout 10s → message dédié), et l'avertissement médical en bas. Le composant a été placé au-dessus du formulaire de contact existant.

**Test pipeline :**
Question : "J'ai de la fièvre depuis 2 jours avec des frissons à Pikine"
→ Spinner affiché ✅ → Fiche médicale reçue avec NIVEAU : MODÉRÉ + centres Pikine ✅

**Note : 5/5**
Pipeline complet fonctionnel : MVP Lovable → Dify API → Agent → Réponse dans l'interface.

**Itération effectuée :** Ajout d'un prompt correctif après vérification :
```
La zone de résultat de l'agent sur la page Contact déborde sur mobile.
Limite sa largeur à 100% sur mobile et ajoute overflow-auto.
```
→ Corrección responsive réussie ✅

---

## P4 — Chain-of-Thought | Note d'éthique RAG (L4)

**Technique :** Chain-of-Thought (raisonnement en 4 étapes)
**Outil :** Claude.ai

**Prompt envoyé :**
```
Analyse les enjeux éthiques spécifiques au système RAG de YekooSanté.
Raisonne étape par étape.

Notre système RAG :
- Base : CSV de 15 symptômes avec niveaux MSAS Sénégal
- Agent : Llama-3.1-8b-instant via GroqCloud
- Utilisateurs : habitants de Pikine, feature phone, wolof/français
- Données collectées : descriptions de symptômes par SMS (pas de nom stocké)

ÉTAPE 1 — QUALITÉ DES DONNÉES :
Notre CSV peut-il contenir des erreurs ou des biais ? Qui n'est pas représenté ?

ÉTAPE 2 — DÉPENDANCE TECHNOLOGIQUE :
Quels risques si Dify, GroqCloud ou Lovable sont indisponibles ?

ÉTAPE 3 — CONFIDENTIALITÉ MÉDICALE :
Les symptômes envoyés par SMS sont-ils des données sensibles ?
Quelles obligations légales s'appliquent (loi sénégalaise sur les données personnelles) ?

ÉTAPE 4 — IMPACT SUR LES SOINS :
Si YekooSanté classe un symptôme urgent comme bénin → quelles conséquences ?
Qui est le plus exposé à ce risque dans notre zone cible ?

Pour chaque étape : 1 risque concret + 1 garde-fou réaliste.
```

**Résumé de la réponse IA :**
**Étape 1 :** Risque — le CSV ne couvre que 15 symptômes, avec un biais vers les pathologies documentées (paludisme, choléra) au détriment de conditions moins connues comme les maladies mentales ou les complications de grossesse. Garde-fou : enrichir le CSV chaque semaine avec les cas réels des agents de santé communautaires. **Étape 2 :** Risque — triple dépendance (Dify + GroqCloud + Lovable) sans plan B. Garde-fou : message de fallback statique avec numéro SAMU (15) si API indisponible. **Étape 3 :** Risque — les descriptions de symptômes sont des données de santé sensibles protégées par la loi n°2008-12 du Sénégal. Garde-fou : ajouter une mention de traitement dans l'interface. **Étape 4 :** Risque — faux négatif (urgence classée bénin) le plus dangereux pour les enfants fiévreux et les femmes enceintes de Pikine. Garde-fou : ajouter un seuil de prudence systématique dans le prompt RÉDACTEUR.

**Note : 4/5**
Analyse solide, mais l'étape 3 sur la loi sénégalaise manquait d'exemples concrets. Reformulée pour mentionner explicitement la Commission de Protection des Données Personnelles (CDP) du Sénégal.

**Itération :** Reformulation de l'étape 3 pour nommer la CDP Sénégal et la loi n°2008-12.

---

*Journal de Prompts S5 — YekooSanté — GET 409 | Swiss UMEF University — Dakar | Juin 2026*
