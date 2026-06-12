# Template Webhook — YekooSanté ↔ Dify
## Livrable S5 | GET 409 | Swiss UMEF University — Dakar | Juin 2026

---

| Champ | Valeur |
|---|---|
| Équipe | YekooSanté |
| MVP Lovable | yekoosante.lovable.app |
| Agent Dify | YekooSante_TriageMedical_v1 — Llama-3.1-8b-instant via GroqCloud |
| URL API Dify | `https://api.dify.ai/v1/workflows/run` |
| Résultat visé | L'agent YekooSanté répond aux symptômes directement dans le MVP Lovable |

---

## Étape 1 — Récupérer votre clé API Dify

1. Ouvrir votre workflow `YekooSante_TriageMedical_v1` dans **Dify Studio**
2. Cliquer **Publier** (haut droite) → puis **flèche** → **Accéder à la référence API**
3. Cliquer **« Clé API »** en haut à droite → **+ Créer une nouvelle clé secrète**
4. **Copier immédiatement** la clé (format : `app-xxxxxxxxxxxx`)
5. La coller dans un bloc-notes — **elle ne s'affiche qu'une seule fois**

| Ce que vous notez | Valeur |
|---|---|
| URL API (identique pour tous) | `https://api.dify.ai/v1/workflows/run` |
| Votre clé API secrète | `app-` *(coller ici — ne pas partager)* |

---

## Étape 2 — Prompt webhook à envoyer dans Lovable

**Aller sur [lovable.dev](https://lovable.dev) → votre projet YekooSanté → chat Lovable**

Remplacer `[COLLER_VOTRE_CLÉ_API_ICI]` par votre vraie clé `app-xxxx`, puis coller ce prompt :

```
Dans mon MVP YekooSanté, ajoute une fonctionnalité de
consultation de l'agent IA sur la page Contact.

INTERFACE À AJOUTER :
1. Un champ de texte avec placeholder :
   "Décrivez vos symptômes en français ou en wolof..."
2. Un bouton vert "Demander à YekooSanté 🌿"
3. Une zone de résultat sous le formulaire (fond gris clair #F3F4F6)
4. Un spinner de chargement pendant la requête
5. Un message d'erreur rouge si la requête échoue

CONNEXION WEBHOOK DIFY :
URL : https://api.dify.ai/v1/workflows/run
Méthode : POST
Headers :
  Authorization: Bearer [COLLER_VOTRE_CLÉ_API_ICI]
  Content-Type: application/json
Body JSON :
  { "inputs": {"query": valeurDuChampTexte},
    "response_mode": "blocking",
    "user": "user-yekoosante-" + Date.now() }

TRAITEMENT DE LA RÉPONSE :
- Succès : afficher response.data.outputs dans la zone résultat
- Erreur réseau : "Service temporairement indisponible"
- Timeout (>10s) : "La réponse prend trop de temps — réessayez"

STYLE : cohérent avec le MVP vert #059669. Responsive mobile.
Ajoute aussi en bas de la zone résultat un avertissement fixe :
"⚠️ YekooSanté ne remplace pas un médecin. Urgences : 15"
```

---

## Étape 3 — Vérifier l'interface générée

1. Dans le **preview Lovable**, cliquer sur **« Contact »** dans la navigation
2. Vérifier que l'encadré **« Consulter l'agent YekooSanté »** apparaît
3. Vérifier que le champ texte et le bouton vert sont présents

---

## Étape 4 — Tester le pipeline complet

Saisir cette question test dans le champ :

```
J'ai de la fièvre depuis 2 jours avec des frissons et des maux de tête, je suis à Pikine
```

Cliquer sur **« Demander à YekooSanté 🌿 »**

**Résultat attendu :**
```
――――――――――――――――――――――――
YEKOO SANTÉ
Votre fiche de santé
――――――――――――――――――――――――
SYMPTÔME : Fièvre + frissons + maux de tête
NIVEAU : MODÉRÉ
――――――――――――――――――――――――
RECOMMANDATION
Consultez un médecin dans les 24h.
Ce symptôme peut indiquer un début de paludisme.
――――――――――――――――――――――――
ACTION IMMÉDIATE
• Prenez du paracétamol (500mg adulte)
• Buvez beaucoup d'eau
• Utilisez une moustiquaire
――――――――――――――――――――――――
OÙ ALLER
Centre de santé de Pikine (8h-18h)
Hôpital Youssou Mbargane — Urgences 24h/24
――――――――――――――――――――――――
```

---

## Checklist validation pipeline

| ☐ | Vérification | Résultat attendu |
|---|---|---|
| ☐ | Composant agent visible | Section « Consulter l'agent YekooSanté » sur page Contact |
| ☐ | Champ + bouton | Champ texte actif + bouton vert cliquable |
| ☐ | Spinner affiché | Animation de chargement visible (quelques secondes) |
| ☐ | Fiche médicale reçue | Contenu structuré avec NIVEAU + RECOMMANDATION + OÙ ALLER |
| ☐ | Avertissement médical | "⚠️ YekooSanté ne remplace pas un médecin. Urgences : 15" visible |
| ☐ | Pipeline complet | Lovable → Dify API → Agent → Réponse ✅ |

---

## Guide de débogage Webhook

| ❌ Problème | ✅ Solution |
|---|---|
| Erreur **401 Unauthorized** | Clé API incorrecte → Dify → Settings → API Keys → régénérer |
| Composant absent sur la page | Vérifier que le prompt mentionne bien « page Contact » |
| Réponse **vide ou undefined** | Essayer `response.data.answer` au lieu de `response.data.outputs` |
| **Timeout** — pas de réponse | Le workflow Dify n'est peut-être pas publié → vérifier qu'il est actif |
| Erreur **CORS** dans la console | La clé doit être un Bearer token standard — vérifier le format `app-xxxx` |

---

## ⚠️ Note sécurité

La clé API est visible dans le code frontend — acceptable pour un **prototype de cours**.

En production réelle, il faudrait la passer via une **server function** (Lovable Cloud).

> **Ne partagez jamais votre clé API sur un dépôt public GitHub.**

---

*Template Webhook S5 — YekooSanté — GET 409 | Swiss UMEF University — Dakar | Juin 2026*
