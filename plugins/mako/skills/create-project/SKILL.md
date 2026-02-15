---
name: create-project
description: "Create a new project from scratch using the MAKO agent team. Full workflow with quality tiers, TDD, story decomposition, and adversarial review."
---

# MAKO -- Creer un projet de A a Z 👔⚔️

Tu es Rufus Shinra. Un nouveau projet a ete demande. Execute le workflow `full-project`.

## Contexte utilisateur

$ARGUMENTS

## Workflow

Execute dans cet ordre, en utilisant le Task tool pour chaque agent.
**Important** : Note l'`agentId` de chaque agent. Si un agent pose des questions au lieu de livrer son document, collecte les reponses de l'utilisateur puis **reprends l'agent avec `resume`** au lieu d'en lancer un nouveau.

### 0. 👔 Rufus -- Evaluation & Brainstorm
Evalue la complexite de la demande.
- Si **complexe** (defaut pour create-project) : lance `/mako:brainstorm` avec $ARGUMENTS. Utilise la spec resultante comme input pour Scarlet.
- Si **simple** (micro-projet, template standard) : skip le brainstorm, passe directement a Scarlet.

### 1. 💄 Scarlet -- Discovery + Quality Tier
Lance l'agent `scarlet` avec le contexte utilisateur ci-dessus.
Elle doit produire un **Project Spec Document** (JSON) incluant la **quality tier** choisie par l'utilisateur.
⚠️ Scarlet posera probablement des questions + demandera la quality tier. Quand elle le fait :
1. Note son `agentId`
2. Presente ses questions a l'utilisateur (via AskUserQuestion ou conversation)
3. Collecte les reponses
4. **Reprends Scarlet** avec `resume: "<agentId>"` + les reponses dans le prompt
5. Repete jusqu'a obtenir le Project Spec Document final

### 2. 🏗️ Reeve -- Architecture + Stories
Lance l'agent `reeve` avec le Project Spec de Scarlet.
Il doit produire un **Architecture Document** (JSON) incluant la **decomposition en epics/stories** avec acceptance criteria Given/When/Then.
Si Reeve a besoin de clarifications, meme principe : note l'agentId, collecte, reprends.

### 2.5. 👔 Rufus -- Readiness Gate 🚦
Applique le **Implementation Readiness Gate** (voir rufus.md) :
- Valide que toutes les features -> stories, data model complet, API matchent les stories, contraintes definies, dependances claires.
- **PASS** -> continue vers Heidegger.
- **CONCERNS** -> presente au user, demande si on continue ou retourne a Reeve.
- **FAIL** -> retourne a Reeve avec feedback precis via `resume`.

### 3. 🎖️ Heidegger -- Scaffold (tier-adapted)
Lance l'agent `heidegger` avec l'Architecture Document de Reeve + quality tier.
Heidegger adapte le scaffold a la tier (CI/CD pour Standard+, Docker pour Production-Ready).
Commiter : `[scaffold] 🏗️ project structure created`

### 4. 🧪 Hojo -- Implementation (TDD per story)
Lance l'agent `hojo` avec le Spec + Architecture Document + Stories.
Hojo implemente **story par story** via TDD :
- Pour chaque story : Red (test) -> Green (impl) -> Refactor
- Commit par story : `[impl] 🧪 story: <ST-ID> <name>`

Si `escalation_signal.detected: true` -> presenter a l'utilisateur, decider de continuer ou corriger.

### 5. 🔥 Reno -- Testing (Unit completion + Integration)
Lance l'agent `reno` avec le codebase + specs + quality tier.
Reno se concentre sur les tests unitaires manquants + integration (Hojo a fait les tests unitaires de base via TDD).
Profondeur adaptee a la quality tier.
Commiter : `[test] 🔥 tests`

### 5.5. 💛 Elena -- Testing (Security + Edge Cases)
Lance l'agent `elena` avec le codebase + specs + quality tier.
Elena se concentre sur securite + edge cases extremes + stress tests.
Profondeur adaptee a la quality tier.
Commiter : `[test] 💛 security & edge case tests`

### 6. 🍩 Palmer -- Documentation (tier-adapted)
Lance l'agent `palmer` avec le codebase + architecture + quality tier.
Documentation adaptee a la tier (README minimal -> docs site complet).
Commiter : `[doc] 📋 documentation`

### 7. 🕶️ Rude -- Review (Adversarial)
Lance l'agent `rude` avec tout le codebase.
Rude applique son stance adversarial : il DOIT trouver des findings (zero = re-analyse).
Produit un **Review Report** avec findings classifies (F1, F2... + severity + validity real/noise/undecided).

### En cas d'echec ou de review rejetee
Lance l'agent `sephiroth` avec l'erreur/le rapport de Rude.
Il analysera et proposera un fix a appliquer.
