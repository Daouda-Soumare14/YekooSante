# Schéma d'Architecture — Agent YekooSanté
## Livrable L2 | Workflow Dify Annoté

---

## Architecture Multi-agents

```
╔══════════════════════════════════════════════════════════════════════════════╗
║              AGENT YEKOO SANTÉ — WORKFLOW DIFY                             ║
║         Pré-triage médical par SMS — Pikine, Dakar, Sénégal                ║
╚══════════════════════════════════════════════════════════════════════════════╝

  ENTRÉE UTILISATEUR (SMS / WhatsApp)
  ┌─────────────────────────────────────┐
  │  "J'ai de la fièvre depuis 2 jours  │
  │   avec des frissons à Pikine"        │
  └──────────────────┬──────────────────┘
                     │
                     ▼
  ╔═════════════════════════════════════╗
  ║         NŒUD DEBUT                 ║
  ║  Variable : query (Short Text)      ║
  ║  Label : "Décrivez vos symptômes"   ║
  ╚══════════════════╦══════════════════╝
                     ║ query →
                     ▼
  ╔═════════════════════════════════════╗
  ║         NŒUD CHERCHEUR             ║
  ║  Modèle : Llama-3.1-8b-instant     ║
  ║  Temp. : 0.3 (précision)           ║
  ║  Rôle : Analyste médical           ║
  ║  Sénégal / Protocoles OMS          ║
  ║                                     ║
  ║  → Analyse symptômes (3 étapes)    ║
  ║  → Retourne données structurées    ║
  ║    OU "INSUFFISANT : [raison]"     ║
  ╚══════════════════╦══════════════════╝
                     ║ text →
                     ▼
  ╔═════════════════════════════════════╗
  ║         NŒUD SI/SINON              ║
  ║  Condition :                        ║
  ║  Chercheur.text CONTIENT            ║
  ║  "INSUFFISANT"                      ║
  ╚══════════╦══════════════╦═══════════╝
             ║ IF (vrai)    ║ ELSE (faux)
             ▼              ▼
  ╔══════════════╗  ╔═══════════════════════════╗
  ║  SORTIE 1   ║  ║     NŒUD RÉDACTEUR        ║
  ║  (branche   ║  ║  Modèle : Llama-3.1-8b    ║
  ║  IF)        ║  ║  Temp. : 0.7 (fluidité)   ║
  ║             ║  ║  Rôle : Rédacteur santé    ║
  ║  Variable : ║  ║  Reçoit : Chercheur.text  ║
  ║  message_   ║  ║  Few-Shot : exemple fiche  ║
  ║  erreur     ║  ╚═══════════╦═══════════════╝
  ║  ← Cherch. ║               ║ text →
  ║    .text   ║               ▼
  ╚══════════════╝  ╔═══════════════════════════╗
         │          ║       SORTIE 2            ║
         │          ║  Variable : fiche_        ║
         │          ║  medicale                 ║
         │          ║  ← Redacteur.text         ║
         │          ╚═══════════════════════════╝
         │
  ┌──────▼──────────────────────────────────────┐
  │        SORTIE VERS L'UTILISATEUR            │
  │                                              │
  │  CAS 1 (INSUFFISANT) :                      │
  │  "Pouvez-vous décrire vos symptômes         │
  │   plus précisément ?"                       │
  │                                              │
  │  CAS 2 (DONNÉES OK) :                       │
  │  ━━━━━━━━━━━━━━━━━━━━                       │
  │  YEKOO SANTÉ                                │
  │  SYMPTÔME : Fièvre + frissons               │
  │  NIVEAU : MODÉRÉ                            │
  │  RECOMMANDATION : Consultez dans 24h        │
  │  ACTION : Paracétamol + hydratation         │
  │  OÙ : Centre de santé Pikine               │
  │  ━━━━━━━━━━━━━━━━━━━━                       │
  └─────────────────────────────────────────────┘
```

---

## Description des nœuds (tableau annoté)

| Nœud | Type Dify | Modèle | Temp. | Entrée | Sortie |
|---|---|---|---|---|---|
| DEBUT | Entrée utilisateur | — | — | Saisie libre (SMS) | Variable `query` |
| CHERCHEUR | LLM | Llama-3.1-8b-instant | 0,3 | `query` depuis DEBUT | `text` (données structurées ou INSUFFISANT) |
| SI/SINON | Contrôle logique | — | — | `Chercheur.text` | Branche IF ou ELSE |
| REDACTEUR | LLM | Llama-3.1-8b-instant | 0,7 | `Chercheur.text` | `text` (fiche médicale formatée) |
| SORTIE 1 | Sortie (branche IF) | — | — | `Chercheur.text` | Message d'erreur à l'utilisateur |
| SORTIE 2 | Sortie (branche ELSE) | — | — | `Redacteur.text` | Fiche médicale complète |

---

## Variables critiques

| Variable | Nœud source | Nœud cible | Valeur type |
|---|---|---|---|
| `query` | DEBUT | CHERCHEUR (message USER) | "J'ai de la fièvre..." |
| `Chercheur.text` | CHERCHEUR | SI/SINON + REDACTEUR | "SYMPTÔME : Fièvre..." ou "INSUFFISANT : ..." |
| `Redacteur.text` | REDACTEUR | SORTIE 2 | Fiche médicale formatée |

---

## Protocole de communication entre agents

Le mot-clé **INSUFFISANT** (en majuscules) est la convention de signalisation entre l'Agent Chercheur et le nœud SI/SINON. C'est le premier contact des étudiants avec la notion de **protocole de communication inter-agents** : un signal textuel choisi par convention pour déclencher une logique de branchement.

Alternatives équivalentes (si personnalisation souhaitée) : `RETRY`, `DONNÉES_MANQUANTES`, `INCOMPLET` — mais la casse doit correspondre exactement à la condition SI/SINON configurée.

---

## Techniques de Prompt Engineering utilisées

| Nœud | Technique | Justification |
|---|---|---|
| CHERCHEUR | Zero-Shot structuré (Rôle + Contexte + Tâche + Format) | Analyse factuelle → température basse, format de sortie strict |
| REDACTEUR | Few-Shot | Le format d'une fiche médicale SMS est précis — l'exemple garantit la cohérence visuelle |

---

*Livrable L2 — Schéma d'architecture annoté — YekooSanté — GET 409 | Swiss UMEF University — Dakar | 2025–2026*
