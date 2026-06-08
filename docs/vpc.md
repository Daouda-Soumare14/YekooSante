# Value Proposition Canvas — YekooSanté

## Profil Client (Aminata Sarr — Pikine)

### Jobs to be done
*Ce qu'Aminata essaie d'accomplir dans sa vie*

- **Job principal :** Évaluer rapidement la gravité d'un symptôme (le sien ou de ses enfants) pour décider si elle doit se déplacer
- **Job secondaire :** Trouver le bon centre de santé selon la gravité de l'état
- **Job social :** Être une bonne mère qui prend soin de sa famille sans dépenser inutilement
- **Job émotionnel :** Avoir la paix d'esprit la nuit quand quelqu'un est malade à la maison

### Pains (Frustrations)

| # | Frustration | Gravité |
|---|---|---|
| P1 | Pas de médecin accessible après 18h dans les zones périurbaines | 🔴 |
| P2 | Incapacité à distinguer urgence réelle vs symptôme bénin | 🔴 |
| P3 | Déplacement coûteux (3 500 FCFA + 45 min) pour cas non urgents | 🟠 |
| P4 | Information médicale en ligne inutilisable (anglais, technique, lente en 2G) | 🟠 |
| P5 | Médicaments achetés sans conseil = risque d'erreur | 🟡 |

### Gains (Résultats désirés)

| # | Gain espéré |
|---|---|
| G1 | Réponse médicale fiable en < 5 min, en français ou wolof |
| G2 | Orientation claire : rester chez soi / aller au dispensaire / aller aux urgences |
| G3 | Économiser 3 000–5 000 FCFA et une demi-journée de travail |
| G4 | Accessibilité par SMS/WhatsApp sans smartphone |
| G5 | Tranquillité d'esprit la nuit pour les enfants malades |

---

## Proposition de Valeur (YekooSanté)

### Produits & Services
*Notre MVP — ce que nous construisons concrètement*

**YekooSanté** est un agent IA de pré-triage médical accessible par SMS et WhatsApp. L'utilisateur décrit ses symptômes en langage naturel (français ou wolof), et l'agent lui fournit :
1. Une évaluation de gravité (bénin / modéré / urgent)
2. Une recommandation d'action concrète (repos + remède maison / aller au dispensaire X / aller aux urgences maintenant)
3. Un message de réassurance ou d'alerte selon le cas

**Canal :** SMS ou WhatsApp — pas d'application à installer, fonctionne sur feature phone.
**Disponibilité :** 24h/24, 7j/7.
**Technologie :** Agent Dify multi-nœuds (Chercheur médical → SI/SINON → Rédacteur de recommandation), modèle Llama-3.1-8b via GroqCloud.

### Pain Relievers ↔ Pains
*Comment notre MVP réduit chaque frustration identifiée*

| Pain | Pain Reliever | FIT ✅ |
|---|---|---|
| P1 — Pas de médecin après 18h | Disponibilité 24h/24 par SMS, sans déplacement | ✅ |
| P2 — Incapable de distinguer urgence vs bénin | Triage automatisé sur 3 niveaux avec recommandation claire | ✅ |
| P3 — Déplacement coûteux pour cas bénins | 70% des cas redirigés vers soins à domicile = économie directe | ✅ |
| P4 — Info médicale inaccessible en ligne | Réponse en français simple / wolof, optimisée pour 2G | ✅ |
| P5 — Médicaments sans conseil | Conseils de base sur les remèdes OMS et avertissements interactions | ✅ |

### Gain Creators ↔ Gains
*Bénéfices concrets que notre MVP crée*

| Gain | Gain Creator | FIT ✅ |
|---|---|---|
| G1 — Réponse fiable en < 5 min | Workflow IA optimisé, réponse en moins de 30 secondes | ✅ |
| G2 — Orientation vers le bon niveau de soin | 3 niveaux de réponse + liste des centres de santé les plus proches | ✅ |
| G3 — Économiser 3 000–5 000 FCFA | Chaque consultation évitée = économie directe + journée de travail préservée | ✅ |
| G4 — Accessible par SMS | Interface SMS/WhatsApp, aucun smartphone requis | ✅ |
| G5 — Tranquillité d'esprit | Réponse immédiate la nuit avec message de réassurance ou d'alerte clair | ✅ |

---

## Vérification du FIT

**FIT ✅ — Tous les Pains ont un Pain Reliever. Tous les Gains ont un Gain Creator.**

Aucun Pain Reliever superflu : chaque fonctionnalité répond à un besoin réel observé en interview.

---

## HMW Définitif (confirmé S2)

> **« Comment pourrions-nous permettre aux habitants des zones périurbaines de Dakar d'obtenir un premier diagnostic médical fiable par SMS ou WhatsApp, afin de réduire les déplacements inutiles vers des centres de santé déjà surchargés ? »**

---

*VPC complété en S2 — YekooSanté — GET 409 | Swiss UMEF University — Dakar | 2025–2026*
