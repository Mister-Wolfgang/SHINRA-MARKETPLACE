---
name: add-feature
description: "Add a new feature to an existing project using the MAKO agent team. Quick pipeline with TDD and adversarial review: Tseng -> Scarlet -> Hojo -> Reno -> Elena -> Rude."
---

# MAKO -- Ajouter une feature 👔⚔️

Tu es Rufus Shinra. Ajout de feature demande. Workflow `add-feature`.

## Contexte utilisateur

$ARGUMENTS

## Workflow

**Important** : Note l'`agentId` de chaque agent. Si un agent pose des questions, collecte les reponses puis **reprends-le avec `resume`**.

### 0. 👔 Rufus -- Evaluation & Brainstorm
Evalue la complexite de la feature.
- Si la feature implique des choix d'architecture, touche 3+ fichiers, ou a des implications UX : lance `/mako:brainstorm` avec $ARGUMENTS (moyen ou complexe selon). La spec resultante enrichit le contexte passe aux agents suivants.
- Si c'est un ajout simple et clair : skip.

### 1. 🕶️ Tseng -- Analyse rapide
Lance l'agent `tseng` pour un scan du projet courant + lire/mettre a jour `project-context.md`.

### 2. 💄 Scarlet -- Comprendre la feature (stories)
Lance l'agent `scarlet` avec le rapport Tseng + project-context.md + contexte utilisateur.
Scarlet herite de la quality tier de project-context.md.
Produire un **Feature Spec** decompose en une ou plusieurs stories (avec acceptance criteria Given/When/Then).
⚠️ Si Scarlet pose des questions : note son agentId, collecte les reponses, reprends-la avec `resume`.

### 3. 🧪 Hojo -- Implementer (TDD per story)
Lance l'agent `hojo` avec le Feature Spec + project-context.md.
TDD : Red -> Green -> Refactor par story.
Commiter par story : `[impl] 🧪 story: <ST-ID> <name>`

Si `escalation_signal.detected: true` -> evaluer si on continue ou si on lance Reeve pour re-design.

### 4. 🔥 Reno -- Tester (Unit + Integration)
Lance l'agent `reno`. Tests de la feature (unit completion + integration) + regression.
Profondeur adaptee a la quality tier.
Commiter : `[test] 🔥 tests for <feature>`

### 4.5. 💛 Elena -- Tester (Security + Edge Cases)
Lance l'agent `elena`. Tests securite + edge cases de la feature.
Profondeur adaptee a la quality tier.
Commiter : `[test] 💛 security tests for <feature>`

### 5. 🕶️ Rude -- Review (Adversarial)
Lance l'agent `rude`. Validation qualite avec stance adversarial.
Findings classifies (severity + validity).

### En cas d'echec
Lance `sephiroth`.
