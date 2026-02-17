![SHINRA](logo.jpg)

# Claude Code Shinra Marketplace

> *"Le pouvoir n'est rien sans contrôle."* -- Rufus Shinra

Marketplace de plugins pour **Claude Code**, opérée par la **Shinra Electric Power Company**. Architecture multi-repo optimisée pour l'agilité et le déploiement granulaire via **Git Submodules**.

---

## 🚀 Plugins Disponibles

| Plugin | Description | Version | Repository |
| --- | --- | --- | --- |
| **MAKO** | **Modular Agent Kit for Orchestration** — 13 agents spécialisés (Shinra Personalities) pour le cycle de vie projet (SDLC). | [`mako-claude-agent-kit`](https://github.com/Mister-Wolfgang/mako-ai-agents) |
| **JENOVA** | *Projet de monitoring et mutation de code en temps réel* | `Incoming` | -- |

---

## 🛠 Installation

### Via Claude Code CLI

```bash
# Ajouter la marketplace SHINRA
/plugin marketplace add git@github.com:Mister-Wolfgang/claude-code-shinra-marketplace.git

# Installer un plugin spécifique
/plugin install MAKO@shinra-marketplace

```

### Clone Manuel (Développeurs)

Le repository utilise des **submodules**. L'option `--recurse-submodules` est requise :

```bash
git clone --recurse-submodules git@github.com:Mister-Wolfgang/claude-code-shinra-marketplace.git

```

---

## 🏗 Architecture Technique

Chaque projet est un repository Git indépendant, permettant :

* **Versioning Atomique** : Chaque plugin suit son propre cycle SemVer.
* **CI/CD Isolée** : Les tests et builds sont déclenchés par repository.
* **Installation Sélective** : Déploiement uniquement des composants nécessaires.

### Arborescence

```text
claude-code-shinra-marketplace/
├── .claude-plugin/
│   └── marketplace.json      # Registre central des plugins
├── .gitmodules               # Définition des pointeurs submodules
├── PROJETS/
│   ├── MAKO/                 # Git submodule -> mako-claude-agent-kit
│   └── JENOVA/               # (À venir)
├── logo.jpg
└── README.md

```

---

## 🔧 Maintenance & Contribution

### Ajouter un Projet (Submodule)

```bash
# Ajouter le lien distant
git submodule add git@github.com:Mister-Wolfgang/<NOM-REPO>.git PROJETS/<NOM-PROJET>

# Mettre à jour le registre
# Éditer .claude-plugin/marketplace.json

# Commit & Push
git add .
git commit -m "feat: add <NOM-PROJET> to marketplace"
git push origin main

```

### Mettre à jour les Plugins

```bash
git submodule update --remote --merge

```

---

## 📜 Changelog

### v6.1.0 — Architecture Multi-repo

* Migration du monolithe vers une structure multi-repo.
* Extraction de **MAKO** vers son propre repository dédié.
* Renommage `plugins/` en `PROJETS/` pour clarification de scope.
* Optimisation SEO des noms de repositories.

---
