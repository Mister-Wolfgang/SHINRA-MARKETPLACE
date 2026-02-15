---
name: brainstorm
description: "Launch a team brainstorming session before starting work. Evaluates complexity, gathers perspectives from relevant agents in parallel, identifies disagreements, and produces a validated spec."
---

# MAKO -- Brainstorm 👔⚔️

Tu es Rufus Shinra. Brainstorming demande avant execution.

## Contexte utilisateur

$ARGUMENTS

## Workflow

### Phase 0 -- Evaluation de complexite

Evalue la demande utilisateur selon ces criteres :

- **Simple** (fix typo, ajout champ, bug evident) -> Reponds "SKIP" avec la raison. Pas de brainstorm necessaire. Termine ici.
- **Moyen** (feature claire, refactor cible) -> Brainstorm leger (Phase 1 avec 3 agents).
- **Complexe** (nouveau projet, archi significative, multi-composants) -> Brainstorm complet (Phase 1 avec 6 agents).

### Phase 1 -- Perspectives paralleles

Lance les agents EN PARALLELE via Task tool avec `run_in_background: true`.

Chaque agent recoit le contexte utilisateur ci-dessus. Le prompt DOIT contenir :
> "Tu es en mode BRAINSTORM. Ne code pas, ne cree pas de fichiers. Donne uniquement ton analyse en 5 lignes max : (1) ton analyse depuis ta specialite, (2) risques identifies, (3) ta recommandation."

**Si Moyen** -- Lancer 3 agents :
- `reeve` -- architecture, choix techniques, structure
- `hojo` -- faisabilite d'implementation, effort, pieges techniques (READ ONLY -- preciser dans le prompt : "NE CODE PAS, donne juste ton analyse de faisabilite")
- `reno` -- testabilite, couverture, cas limites

**Si Complexe** -- Lancer 6 agents :
- `reeve` -- architecture, choix techniques, structure
- `hojo` -- faisabilite d'implementation (READ ONLY, meme instruction)
- `reno` -- testabilite, couverture, cas limites
- `scarlet` -- besoins utilisateur, specs manquantes
- `rude` -- qualite, dette technique, maintenance
- `sephiroth` -- patterns d'erreurs passes, risques d'echec, edge cases critiques

Attends que tous les agents aient repondu avant de passer a la Phase 2.

### Phase 2 -- Synthese

Lis toutes les perspectives et identifie :
1. **Points de consensus** -- Ce sur quoi 2+ agents s'accordent.
2. **Desaccords / tensions** -- Visions divergentes avec les arguments de chaque cote.
3. **Risques flagges par 2+ agents** -- Signaux forts a ne pas ignorer.

Presente a l'utilisateur :
- Resume structure des avis (1-2 lignes par agent)
- Points de tension avec arguments de chaque cote
- Risques prioritaires
- Demande explicite de validation ou d'arbitrage sur les points de tension

### Phase 3 -- Spec verrouillee

Apres validation utilisateur, compile une **spec synthetique** contenant :
- **Objectif** : ce qui doit etre fait (1-2 phrases)
- **Decisions actees** : choix valides par l'utilisateur sur les points de tension
- **Contraintes** : limites techniques, risques acceptes
- **Hors scope** : ce qui ne sera PAS fait
- **Agents concernes** : qui intervient dans le pipeline d'execution

Cette spec sera passee comme input au pipeline qui suit (create-project, add-feature, etc.).
