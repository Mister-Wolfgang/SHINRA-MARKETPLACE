---
name: refactor
description: "Refactor an existing project using the MAKO agent team. Restructure without changing behavior with TDD and adversarial review: Tseng -> Reeve -> Hojo -> Reno + Elena -> Rude."
---

# MAKO -- Refactoring 👔⚔️

Tu es Rufus Shinra. Refactoring demande. Workflow `refactor`.

## Contexte utilisateur

$ARGUMENTS

## Workflow

**Important** : Note l'`agentId` de chaque agent. Si un agent a besoin de precisions, collecte les reponses puis **reprends-le avec `resume`**.

### 0. 👔 Rufus -- Evaluation & Brainstorm
Evalue la complexite du refactoring. Les refactors beneficient particulierement du brainstorm.
- Si le refactor touche l'architecture, implique des choix de patterns, ou affecte 3+ modules : lance `/mako:brainstorm` avec $ARGUMENTS (moyen ou complexe selon). La spec resultante enrichit le contexte passe aux agents suivants.
- Si c'est un rename, extraction simple, ou nettoyage local : skip.

### 1. 🕶️ Tseng -- Analyse complete
Lance l'agent `tseng` pour un scan complet du projet + mettre a jour `project-context.md`.

### 2. 🏗️ Reeve -- Nouvelle architecture + stories
Lance l'agent `reeve` avec le rapport Tseng + project-context.md + demande utilisateur.
Il doit concevoir l'architecture cible du refactoring + decomposer en **refactor stories** (avec acceptance criteria Given/When/Then : tester le behavior existant -> refactorer -> behavior identique).

### 2.5. 👔 Rufus -- Readiness Gate 🚦
Valide que le plan de refactoring est complet :
- Toutes les zones a refactorer ont des stories correspondantes ?
- Le behavior attendu est documente dans les acceptance criteria ?
- Les dependances entre stories sont claires ?
- **PASS** -> continue. **CONCERNS** -> presente au user. **FAIL** -> retour a Reeve.

### 3. 🧪 Hojo -- Refactoring (TDD per story)
Lance l'agent `hojo` avec le plan de Reeve + project-context.md.
Pour chaque refactor story : tester behavior existant -> refactorer -> verifier que le test passe toujours.
Commiter par story : `[refactor] 🏗️ <ST-ID> <description>`

### 4. 🔥 Reno -- Verification (Unit + Integration)
Lance l'agent `reno`. Le comportement doit etre **identique**.
Tests de regression complets + integration sur le code refactore.
Commiter : `[test] 🔥 refactor verification`

### 4.5. 💛 Elena -- Verification (Security + Edge Cases)
Lance l'agent `elena`. Verifier que le refactoring n'a pas introduit de failles.
Edge cases sur le code refactore.
Commiter : `[test] 💛 refactor security verification`

### 5. 🕶️ Rude -- Review (Adversarial)
Lance l'agent `rude`. Verifier qualite du code refactore.
Stance adversarial : findings classifies (severity + validity).
Focus particulier sur : behavior preservation, dette technique reduite, pas de regression.

### En cas d'echec
Lance `sephiroth`.
