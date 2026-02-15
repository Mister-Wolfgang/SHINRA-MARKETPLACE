# SHODH Memory -- Guide complet

Systeme cognitif avec memoire 3-tiers, apprentissage hebbien, et knowledge graph. Serveur Rust local sur `localhost:3030`, expose via MCP (`@shodh/memory-mcp`). `user_id: "rufus"`.

## Types de memoire MAKO

| Type SHODH | Usage MAKO | Quand stocker |
|------------|------------|---------------|
| `Observation` | Etat du projet, features implementees | Apres chaque phase |
| `Decision` | Choix d'architecture, stack, patterns | Apres Reeve/Scarlet |
| `Learning` | Patterns appris, bonnes pratiques | Apres Sephiroth |
| `Error` | Bugs trouves, causes racines | Apres fix-bug |
| `Context` | Workflow en cours, progression | Debut/fin de workflow |
| `Pattern` | Patterns recurrents cross-projets | Quand confirme |

## Memoire episodique -- chainer les workflows

Chaque workflow MAKO = 1 episode. Chaque phase d'agent = 1 sequence dans l'episode.

Rufus genere l'`episode_id` au debut du workflow : `<project>-<workflow>-<counter>`.

```
remember(
  content: "Reno: 127 tests, 7 modules, 0 failures",
  memory_type: "Observation",
  tags: ["project:endless-sea", "phase:reno"],
  episode_id: "endless-sea-create-001",
  sequence_number: 5
)
```

## Au debut d'un workflow (ou "ou on en est")

Retrieve le state du projet :
```
recall(
  query: "<nom-du-projet>",
  mode: "semantic",
  n_results: 3
)
```

Ou pour un resume condense des decisions et learnings recents :
```
context_summary()
```

## Apres chaque phase d'agent terminee

```
remember(
  content: "<projet> | <agent>: <resume 1-2 lignes> | next: <prochaine etape>",
  memory_type: "Observation",
  tags: ["project:<nom>", "phase:<agent>"],
  episode_id: "<episode-id-en-cours>",
  sequence_number: <n>
)
```

## Stocker une decision architecturale

```
remember(
  content: "<decision concise>",
  memory_type: "Decision",
  tags: ["project:<nom>"]
)
```

## Stocker un pattern ou une erreur apprise

```
remember(
  content: "<pattern ou erreur>",
  memory_type: "Learning",  // ou "Error"
  tags: ["project:<nom>"]
)
```

## Decay et consolidation

- Pas d'action manuelle -- shodh-memory gere le decay automatiquement
- Memoires frequemment accedees se renforcent (Hebbian) : A(t) = A0 * e^(-lambda*t)
- Memoires inutilisees decroissent naturellement
- `consolidate()` disponible pour maintenance manuelle si necessaire

## Regles memoire

1. **Jamais de prose** -- JSON ou phrases courtes uniquement
2. **Jamais de code** -- Stocker le "quoi" et le "pourquoi", pas le "comment"
3. **Max 200 tokens par store** -- Si c'est plus long, c'est trop
4. **Les subagents ne touchent pas la memoire** -- Seul Rufus store/retrieve
5. **Un episode_id par workflow** -- sequencage automatique des phases
6. **Types stricts** -- utiliser la taxonomie SHODH ci-dessus
7. **`context_summary()`** -- pour le recall de debut de session (economique en tokens)
8. **Ne pas retrieve a chaque message** -- Seulement en debut de workflow ou sur demande explicite
