# 🌿 YekooSanté
### Pré-triage médical par SMS — Pikine · Guédiawaye · Rufisque · Dakar

[![Live Demo](https://img.shields.io/badge/🌿_Demo-yekoosante.lovable.app-059669)](https://yekoosante.lovable.app)
[![GitHub](https://img.shields.io/badge/GitHub-GET409--YekooSante-181717?logo=github)](https://github.com/daouda-soumare/GET409-YekooSante)

> *"Comment pourrions-nous permettre aux habitants des zones périurbaines de Dakar d'obtenir un premier diagnostic médical fiable par SMS ou WhatsApp, afin de réduire les déplacements inutiles ?"*

---

## Description du projet

YekooSanté est un agent IA de pré-triage médical accessible par SMS et WhatsApp, destiné aux habitants de Pikine, Guédiawaye et Rufisque (banlieue de Dakar, Sénégal). L'utilisateur décrit ses symptômes en français ou en wolof, et l'agent évalue la gravité (Bénin / Modéré / Urgent) en moins de 30 secondes. Il oriente ensuite vers la bonne action : rester à domicile, aller au dispensaire, ou se rendre aux urgences — sans avoir besoin d'un smartphone.

**Problème résolu :** Plus de 60% des consultations aux urgences de Dakar concernent des cas qui auraient pu être orientés sans déplacement. Aucun médecin n'est disponible après 18h dans les zones périurbaines.

---

## Membres de l'équipe

| Prénom & Nom | Rôle | Responsabilité | GitHub |
|---|---|---|---|
| Mohamed El Fadilou Kebe | Responsable Communication & Pitch | Présentation jury · Vidéo NanoBanana · Démonstration live · Supports visuels | @mohamed-kebe |
| Elhadji Falilou Fall | 🎯 Chef de Produit (PM) | Coordination, planning, pitch jury | @falilou-fall |
| Fatoumata Diallo | ✍️ Master Prompt Engineer | Conception et documentation des prompts IA | @fatoumata-diallo |
| Daouda Soumaré | 🛠️ Dev UI (No-Code) | GitHub, Dify.ai, Lovable, interface | @daouda-soumare |

---

## Fonctionnalités principales

- 🔍 **Pré-triage IA** — Analyse des symptômes en 3 niveaux (Bénin / Modéré / Urgent)
- 💬 **SMS & WhatsApp** — Aucune application à installer, fonctionne sur feature phone
- 🌍 **Bilingue** — Français et wolof supportés
- 🕐 **24h/24** — Disponible même après 18h quand les centres sont fermés
- 🏥 **Centres de santé** — Annuaire des 6 centres de Pikine, Guédiawaye, Rufisque avec disponibilité temps réel
- 🤖 **Agent Dify RAG** — Base de connaissances médicales locale indexée (protocoles MSAS Sénégal)

---

## Guide de démarrage

**MVP live :**
```
https://yekoosante.lovable.app
```

**Tester l'agent Dify :**
1. Ouvrir le workflow `YekooSante_TriageMedical_v1` sur [dify.ai](https://dify.ai)
2. Cliquer **Exécuter test**
3. Saisir : *"J'ai de la fièvre depuis 2 jours avec des frissons et des maux de tête"*
4. Vérifier la fiche médicale générée (NIVEAU / RECOMMANDATION / ACTION)

---

## Outils et technologies

| Outil | Usage |
|---|---|
| [Lovable.dev](https://lovable.dev) | Interface MVP no-code (React + Tailwind) |
| [Dify.ai](https://dify.ai) | Workflow multi-agents IA (Chercheur → SI/SINON → Rédacteur) |
| [GroqCloud](https://groq.com) | Modèle LLM : Llama-3.1-8b-instant |
| [Claude.ai](https://claude.ai) | Prompt engineering & documentation |
| [GitHub](https://github.com) | Versioning & collaboration |

---

## Architecture du workflow Dify

```
[DEBUT: query] → [CHERCHEUR: analyse symptômes] → [SI/SINON: INSUFFISANT?]
                                                        ↓ IF          ↓ ELSE
                                                  [SORTIE 1]    [RÉDACTEUR]
                                                  (Demande      [SORTIE 2]
                                                  précisions)   (Fiche médicale)
```

---

## Structure du dépôt

```
GET409-YekooSante/
├── S1/          — Fiche équipe + Carte d'empathie
├── S2/          — VPC + Journal de prompts S2
├── S3-S4/       — Agent Dify + Architecture + Journal S3 + Réflexion éthique
├── S4/          — Template Lovable + Journal itérations + README
├── S5/          — Base RAG (CSV) + Template Webhook + Journal S5
└── README.md
```

---

## Captures d'écran

| Page Accueil | Page Centres | Page Contact |
|---|---|---|
| *(capture à ajouter)* | *(capture à ajouter)* | *(capture à ajouter)* |

---

> ⚠️ **Avertissement médical :** YekooSanté est un service de pré-triage — il ne remplace pas un avis médical professionnel. En cas d'urgence vitale, appelez le **SAMU Sénégal : 15**.

---

**GET 409 — Atelier IA | M. Malick Faye Diagne | Swiss UMEF University — Campus de Dakar | 2025–2026**
