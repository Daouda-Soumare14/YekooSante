# Journal de Prompts — Séance 3
## YekooSanté | GET 409 | Swiss UMEF University — Dakar | 2025–2026

---

## Tableau récapitulatif

| # | Technique | Nœud | Note /5 | Itération |
|---|---|---|---|---|
| P1 | Zero-Shot | Agent CHERCHEUR (prompt système) | 5/5 | Oui — adaptation contexte YekooSanté |
| P2 | Few-Shot | Agent RÉDACTEUR (prompt système + exemple) | 5/5 | Non — directement opérationnel |
| P3 | Chain-of-Thought | Réflexion éthique (génération L4) | 4/5 | Oui — personnalisation pour YekooSanté |

---

## P1 — Zero-Shot | Prompt système Agent CHERCHEUR

**Technique :** Zero-Shot structuré (Rôle + Contexte + Mission + Format strict)
**Nœud Dify :** CHERCHEUR — champ SYSTEM
**Outil de test :** Claude.ai (testé avant intégration Dify)

**Prompt envoyé (version finale) :**
```
Tu es un analyste médical spécialisé en santé communautaire au Sénégal pour YekooSanté,
service de pré-triage médical par SMS destiné aux habitants de Pikine, Guédiawaye et Rufisque.

MISSION : Analyser la description de symptômes reçue et retourner soit une évaluation structurée,
soit le mot-clé INSUFFISANT si les symptômes sont trop vagues pour être analysés.

PROCESSUS EN 3 ÉTAPES :
1. ANALYSER la description : quel(s) symptôme(s), depuis combien de temps, quelle population
2. ÉVALUER la gravité : BÉNIN / MODÉRÉ / URGENT
3. ÉVALUER la suffisance des données

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

**Résumé de la réponse IA (test Claude.ai) :**
Testé avec "J'ai de la fièvre depuis 2 jours avec des frissons et des maux de tête" → l'IA a retourné un output structuré complet (SYMPTÔME / NIVEAU / RECOMMANDATION / ACTION / SOURCES) sans dépasser le format. Testé avec "Je ne me sens pas bien" → l'IA a retourné exactement "INSUFFISANT : description trop vague pour évaluer le niveau de gravité." Le mot-clé INSUFFISANT en majuscules est bien respecté, ce qui permet au nœud SI/SINON Dify de détecter la condition.

**Note de qualité : 5/5**
Format de sortie parfaitement respecté. Le signal INSUFFISANT fonctionne comme protocole de communication inter-agents. La température 0,3 garantit des réponses cohérentes et non créatives.

**Itération effectuée :** Version initiale rédigée pour GreenSprint (projet exemple) → adaptée pour YekooSanté en remplaçant le contexte agricole par le contexte médical et en ajoutant les 5 symptômes de référence courants à Dakar (paludisme, choléra, IRA, HTA, plaie infectée).

---

## P2 — Few-Shot | Prompt système Agent RÉDACTEUR

**Technique :** Few-Shot (exemple de fiche médicale imposé dans le prompt)
**Nœud Dify :** RÉDACTEUR — champ SYSTEM
**Outil de test :** Claude.ai (testé avant intégration Dify)

**Prompt envoyé (version finale) :**
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

SUR CE MODELE, rédige la fiche pour les données reçues.
Ton : clair, rassurant, accessible. Maximum 200 mots.
Langue : français simple (niveau CM2).
```

**Résumé de la réponse IA (test Claude.ai) :**
Testé en fournissant un output simulé du CHERCHEUR (données sur fièvre + frissons + zone Pikine). L'IA a généré une fiche respectant exactement le format de l'exemple : mêmes séparateurs `――――`, mêmes sections, ton rassurant, longueur ≤ 200 mots, français accessible. Le Few-Shot a imposé le format visuel de façon efficace — sans l'exemple, l'IA aurait produit du texte en prose ou du markdown inutilisable sur SMS.

**Note de qualité : 5/5**
La technique Few-Shot est justifiée ici : le format SMS d'une fiche médicale est très précis et visuel. Un seul exemple suffit à imposer le pattern. La cohérence entre toutes les fiches générées est garantie, ce qui est essentiel pour la lisibilité sur feature phone.

**Itération :** Aucune — le prompt a fonctionné dès la première version car l'exemple de fiche était suffisamment détaillé.

---

## P3 — Chain-of-Thought | Génération de la réflexion éthique (L4)

**Technique :** Chain-of-Thought (CoT) — raisonnement en étapes
**Usage :** Génération du livrable L4 dans Claude.ai
**Outil :** Claude.ai

**Prompt envoyé :**
```
Analyse les enjeux éthiques de MON agent Dify YekooSanté.
Raisonne étape par étape. Sois spécifique à mon cas.

MON AGENT :
Nom : Agent YekooSanté
Ce qu'il fait : Analyser des symptômes décrits par SMS en français ou wolof,
évaluer leur gravité (BÉNIN/MODÉRÉ/URGENT), et recommander une action concrète
(rester à la maison, aller au dispensaire, aller aux urgences).
Données utilisées : descriptions textuelles de symptômes, sans données personnelles stockées.
Utilisateurs : habitants de Pikine, Guédiawaye, Rufisque — feature phone — revenu modeste.
Contexte : Sénégal · Santé · No-code · Dify · Llama-3.1-8b via GroqCloud

ÉTAPE 1 — IDENTIFIER 2 RISQUES CONCRETS :
Pour chaque risque : nom, scénario concret, qui est impacté et comment.

ÉTAPE 2 — ÉVALUER LA GRAVITÉ :
Pour chaque risque : Probabilité + Impact + Urgence

ÉTAPE 3 — PROPOSER DES GARDE-FOUS :
Pour chaque risque :
1. Une mesure technique réaliste
2. Un message de transparence à afficher dans l'interface
3. Une règle à intégrer dans le prompt système

LIVRABLE ATTENDU :
Un texte de 1/2 page (150-200 mots) structuré en 3 paragraphes,
directement utilisable comme réflexion éthique pour le livrable L4.
```

**Résumé de la réponse IA :**
L'IA a structuré son raisonnement en 3 étapes comme demandé. **Étape 1 :** Deux risques identifiés — (1) faux diagnostic de sécurité : l'agent classe un symptôme urgent comme BÉNIN (ex: appendicite précoce présentée comme "douleur abdominale légère"), entraînant un retard de soins dangereux ; (2) dépendance substitutive : les utilisateurs cessent de consulter un médecin réel en s'appuyant exclusivement sur l'agent, y compris pour des cas qui dépassent ses capacités. **Étape 2 :** Risque 1 — probabilité modérée, impact critique (urgence vitale possible) ; Risque 2 — probabilité élevée, impact modéré (dégradation progressive des soins). **Étape 3 :** Garde-fous proposés et intégrés dans la réflexion éthique L4.

**Note de qualité : 4/5**
Excellent raisonnement structuré, risques bien ancrés dans le contexte YekooSanté. Légère réduction (-1) car le risque "dépendance substitutive" était moins contextualisé géographiquement que le risque 1. Reformulation effectuée pour mieux l'ancrer dans la réalité des zones périurbaines de Dakar.

**Itération effectuée :** Le risque 2 a été reformulé dans L4 pour mentionner explicitement le contexte "absence de médecin après 18h à Pikine" et la tendance documentée au remplacement des consultations par des solutions numériques dans les zones sous-médicalisées.

---

*Journal de Prompts S3 — YekooSanté — GET 409 | Swiss UMEF University — Dakar | 2025–2026*
