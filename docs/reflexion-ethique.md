# L4 — Réflexion Éthique
## YekooSanté | GET 409 | Swiss UMEF University — Dakar | 2025–2026

---

| Champ | Détail |
|---|---|
| Équipe | YekooSanté |
| Projet | Agent de pré-triage médical par SMS |
| Livrable | L4 — Réflexion éthique |
| Persona | Aminata Sarr · 34 ans · Vendeuse · Pikine · Feature phone |
| Outil IA | Dify.ai — Llama-3.1-8b-instant via GroqCloud |

---

## Contexte de la réflexion

YekooSanté est un agent IA de pré-triage médical destiné à fournir aux habitants de Pikine, Guédiawaye et Rufisque une évaluation de leurs symptômes par SMS, disponible 24h/24. Le système repose sur un workflow Dify (Chercheur → SI/SINON → Rédacteur) utilisant Llama-3.1-8b-instant via GroqCloud. Deux risques éthiques majeurs ont été identifiés lors de l'analyse Chain-of-Thought (P3, Journal S3).

---

## Risque 1 — Faux diagnostic de sécurité

**Description :** Le modèle Llama-3.1-8b-instant évalue la gravité des symptômes à partir de ses paramètres d'entraînement, sans accès à l'état réel du patient, sans examen physique, et sans historique médical. Un symptôme urgent présenté de façon incomplète (ex : "douleur au ventre depuis hier" pour une appendicite débutante) peut être classé BÉNIN, entraînant un retard de soins potentiellement fatal pour Aminata ou ses enfants.

**Garde-fou technique :** Intégrer un avertissement systématique dans chaque fiche générée : *"⚠️ YekooSanté ne remplace pas un médecin. En cas de doute, consultez le centre de santé le plus proche."* Ajouter dans le prompt système du RÉDACTEUR une règle : si le niveau est BÉNIN, toujours inclure un critère d'aggravation ("Si la fièvre dépasse 39,5°C ou dure plus de 3 jours : allez consulter").

**Garde-fou organisationnel :** Établir un partenariat avec les agents de santé communautaires de Pikine pour valider mensuellement un échantillon de fiches générées contre les cas réels observés en consultation.

---

## Risque 2 — Dépendance substitutive

**Description :** Dans les zones périurbaines de Dakar où aucun médecin n'est disponible après 18h, YekooSanté risque de devenir la seule source de conseil médical pour des familles comme celle d'Aminata — y compris pour des cas qui dépassent les capacités d'un agent IA. Une panne de GroqCloud, une révocation de clé API ou une réponse incorrecte peut laisser une famille sans recours à une heure critique. La dépendance à un système unique sans alternative est un risque d'exclusion médicale aggravée.

**Garde-fou technique :** Configurer un modèle de repli (fallback) dans Dify : si GroqCloud est indisponible, basculer automatiquement vers un message statique de redirection vers les urgences (Hôpital Youssou Mbargane — numéro d'urgence : 33 827 XX XX). Mettre en cache le dernier message de contact d'urgence avec horodatage visible.

**Garde-fou organisationnel :** Afficher dans chaque fiche le numéro du SAMU Sénégal (15) et du centre de santé de Pikine. YekooSanté doit être présenté comme un premier filtre, jamais comme un substitut médical.

---

## Recommandation finale

→ **Phase recommandée : Pilote contrôlé (S4–S5)**

YekooSanté ne doit pas être déployé à grande échelle avant d'avoir testé les deux garde-fous identifiés. La recommandation est de conduire un pilote contrôlé sur 20 utilisateurs volontaires dans la zone de Pikine pendant 4 semaines, avec collecte systématique des cas où la recommandation IA divergeait d'une consultation réelle, test du mécanisme de fallback, et validation du message d'avertissement auprès des utilisateurs cibles. Un go/no-go de déploiement sera décidé au terme du pilote.

---

*YekooSanté — GET 409 — Swiss UMEF University — Campus de Dakar — Juin 2026*
