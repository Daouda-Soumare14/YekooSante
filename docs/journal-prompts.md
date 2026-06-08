# Journal de Prompts — Séance 2
## YekooSanté | GET 409 | Swiss UMEF University — Dakar | 2025–2026

---

## P1 — Zero-Shot | Identification des problèmes du persona

**Technique :** Zero-Shot
**Outil :** Claude.ai

**Prompt envoyé :**
```
Tu es consultant en santé publique et télémédecine en Afrique de l'Ouest.
Identifie les 3 principaux problèmes d'Aminata Sarr, vendeuse de 34 ans à Pikine (Dakar),
qui n'a pas accès à un médecin après 18h et utilise uniquement un feature phone.
Réponds sous forme de liste numérotée en français.
```

**Résumé de la réponse IA :**
L'IA a identifié : (1) l'absence d'accès à des soins médicaux de proximité après 18h dans les zones périurbaines de Dakar, (2) l'incapacité à distinguer un symptôme urgent d'un symptôme bénin sans avis médical, entraînant des déplacements inutiles coûteux, et (3) la dépendance à des sources d'information non fiables (voisins, remèdes partagés sur WhatsApp) faute d'alternative accessible.

**Note de qualité : 5/5**
Réponse précise, contextuelle, directement ancrée dans la réalité sénégalaise. Aucune hallucination, les 3 problèmes correspondent exactement aux insights de notre interview d'empathie.

**Itération :** Aucune — première formulation satisfaisante.

---

## P2 — Zero-Shot | Génération de fonctionnalités MVP

**Technique :** Zero-Shot
**Outil :** Claude.ai

**Prompt envoyé :**
```
Tu es expert en product management pour des solutions de santé numérique en Afrique.
Contexte : Notre équipe développe YekooSanté, un agent IA de pré-triage médical
accessible par SMS pour les habitants de Pikine, Guédiawaye et Rufisque à Dakar.
Notre persona est Aminata Sarr, 34 ans, feature phone, pas d'accès médical après 18h.

Propose 5 fonctionnalités prioritaires pour notre MVP.
Format : liste numérotée avec une phrase de description par fonctionnalité.
```

**Résumé de la réponse IA :**
L'IA a proposé : (1) triage symptomatique en 3 niveaux par SMS (bénin/modéré/urgent), (2) recommandation du centre de santé le plus proche selon le niveau de gravité, (3) réponse bilingue français/wolof pour maximiser l'accessibilité, (4) disponibilité 24h/24 avec délai de réponse inférieur à 30 secondes, (5) avertissements sur les interactions médicamenteuses dangereuses courantes.

**Note de qualité : 4/5**
Bonnes propositions, bien adaptées au contexte. Légèrement générique sur la fonctionnalité 5 (interactions médicamenteuses — trop complexe pour un MVP). Utilisable avec adaptation.

**Itération :** Fonctionnalité 5 reformulée pour S3 : remplacée par "message de réassurance ou d'alerte systématique à chaque réponse".

---

## P3 — Few-Shot | Complétion de défi sectoriel santé

**Technique :** Few-Shot
**Outil :** Claude.ai

**Prompt envoyé :**
```
Voici 2 exemples de défis de santé numérique au Sénégal et leurs solutions IA adaptées :

Exemple 1 :
Défi : Les agents de santé communautaires de Ziguinchor n'ont pas accès aux protocoles médicaux sur le terrain.
Solution : Chatbot SMS sans internet (USSD) donnant accès aux protocoles OMS en français, activable avec un simple téléphone basique.

Exemple 2 :
Défi : Les femmes enceintes de Saint-Louis ne connaissent pas les signes de danger pendant la grossesse.
Solution : Service d'alertes SMS hebdomadaires adapté au trimestre de grossesse, envoyé automatiquement sur le numéro enregistré à la maternité.

Maintenant complète :
Défi : Les habitants de Pikine n'ont pas accès à un médecin après 18h et ne savent pas si leurs symptômes sont urgents.
Solution :
```

**Résumé de la réponse IA :**
L'IA a complété en respectant parfaitement les contraintes du contexte sénégalais : agent IA de pré-triage accessible par SMS ou WhatsApp (sans app à installer), répondant en français simple ou wolof, disponible 24h/24, guidant vers le bon niveau de soin (domicile / dispensaire / urgences) et vers le centre de santé le plus proche dans la zone. L'IA a naturellement évité de proposer une application iOS ou un service nécessitant la 4G.

**Note de qualité : 5/5**
La complétion Few-Shot a parfaitement capturé les contraintes implicites : feature phone, contexte périurbain, langue locale, infrastructure limitée. Résultat directement utilisable comme description du MVP.

**Itération :** Aucune — la réponse a validé notre proposition de valeur.

---

## P4 — Chain-of-Thought | Analyse stratégique du défi

**Technique :** Chain-of-Thought (CoT)
**Outil :** Claude.ai

**Prompt envoyé :**
```
Tu es un expert en systèmes de santé en Afrique de l'Ouest.
Analyse le problème suivant : "Les habitants de Pikine, Guédiawaye et Rufisque n'ont pas accès
à un médecin après 18h et ne peuvent pas distinguer une urgence médicale d'un cas bénin."

Réfléchis étape par étape :
Étape 1 : Identifie la cause principale du problème dans le contexte de Dakar.
Étape 2 : Décris l'obstacle principal qui empêche une solution classique de fonctionner.
Étape 3 : Propose une solution technologique accessible au persona Aminata Sarr (feature phone, 2G, revenu modeste).
Développe chaque étape avant de conclure.
```

**Résumé de la réponse IA :**
**Étape 1 :** Cause principale = concentration des médecins dans les quartiers centraux de Dakar (Plateau, Almadies) combinée à l'absence de permanences médicales dans les zones périurbaines. Ratio médecin/habitant de 1/14 000 dans ces zones vs normes OMS de 1/1 000.
**Étape 2 :** Obstacle = les solutions classiques (applications mobiles, téléconsultation vidéo) nécessitent un smartphone et une connexion 4G/WiFi — inexistants chez 65% des habitants de Pikine.
**Étape 3 :** Solution = agent IA de pré-triage accessible par SMS (USSD ou WhatsApp) avec réponse en français simple, guidant vers 3 niveaux d'action. Aucun smartphone requis, fonctionne en 2G.

**Note de qualité : 5/5**
Analyse structurée et vérifiable étape par étape. Données chiffrées cohérentes avec le contexte réel. Aucune hallucination identifiée. La solution proposée correspond exactement à YekooSanté.

**Itération :** Aucune — ce prompt CoT a servi de base à la formulation du MVP en S3.

---

## P5 — Libre | Formulation du prompt système de l'agent YekooSanté

**Technique :** Zero-Shot enrichi (Rôle + Contexte + Contraintes + Format)
**Outil :** Claude.ai

**Prompt envoyé :**
```
Tu es le Master Prompt Engineer de l'équipe YekooSanté au cours GET 409 de Swiss UMEF University Dakar.
Tu dois rédiger le prompt système de notre Agent Chercheur Dify pour un service de pré-triage médical par SMS à Pikine.

Contraintes strictes à respecter :
- L'agent s'appelle YekooSanté
- Il analyse des symptômes décrits par SMS en français ou en wolof
- Il retourne soit un diagnostic structuré (niveau bénin/modéré/urgent) soit le mot "INSUFFISANT" si les symptômes sont trop vagues
- Le format de sortie doit être lisible par SMS (court, sans markdown)
- Le contexte est Dakar, Sénégal, 2025

Rédige le prompt système complet, prêt à être collé dans Dify.
```

**Résumé de la réponse IA :**
L'IA a produit un prompt système complet et directement utilisable dans Dify, incluant le rôle de l'agent, les étapes de traitement (analyser les symptômes → évaluer la gravité → retourner le résultat structuré ou INSUFFISANT), le format de sortie en 4 champs (SYMPTÔME / NIVEAU / RECOMMANDATION / ACTION IMMÉDIATE), et la consigne de basculer sur "INSUFFISANT : [raison]" si les données sont trop vagues. Ce prompt a été utilisé comme base pour la configuration S3.

**Note de qualité : 5/5**
Résultat immédiatement opérationnel. La structure INSUFFISANT est bien respectée (mot clé en majuscules pour le nœud SI/SINON Dify). Format SMS respecté (pas de markdown, réponse concise).

**Itération :** Légère adaptation pour S3 : ajout de la liste de 5 symptômes courants à Dakar comme exemples de référence dans le prompt système.

---

*Journal de Prompts S2 — YekooSanté — GET 409 | Swiss UMEF University — Dakar | 2025–2026*
