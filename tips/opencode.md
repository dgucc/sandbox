# OPENCODE

## Install opencode in WSL

### install opencode with curl 
`$ curl -fsSL https://opencode.ai/install | bash` 

### Check version
`$ opencode --version`

### Launch it once to test
- `$ opencode`
- pick a free model to test `/models`
- exit `/exit`  


---
[OpenCode : crée ton équipe de 4 agents IA qui se contrôlent entre eux - kit à copier](https://www.youtube.com/watch?v=dl78bCncIe0&t=5s)
`$ git clone --single-branch git@github.com:fabstech-lab/agents-open-code.git`    

[OpenCode : skills et MCP, le guide complet (kit à copier)](https://www.youtube.com/watch?v=3NjrA8oM9ns)  

`$ git clone --single-branch git@github.com:fabstech-lab/opencode-skills-mcp-kit.git`  


---
[https://lobehub.com/skills/ramihollings-mr.technology-opencode-folder-creator](https://lobehub.com/skills/ramihollings-mr.technology-opencode-folder-creator)  
**OpenCode Folder Creator**

Crée des structures de répertoire `.opencode` complètes pour les projets avec tous les fichiers de configuration, `agents` et `commands` nécessaires.  
Cette `skill` configure l'ensemble de la structure de projet OpenCode que les équipes peuvent utiliser immédiatement avec `opencode serve`.

**PRINCIPE UNIVERSEL IMPORTANT :**  
Cette `skill` crée uniquement des DOSSIERS (`FOLDERS`) et des STRUCTURES (`STRUCTURES`).  
Les types d'`agent` spécifiques mentionnés (`code-analyzer`, `documentation-writer`, etc.) sont UNIQUEMENT DES EXEMPLES.  
Les vrais `swarms` auront des types d'`agent`, des objectifs et des spécialisations très différents.  
Cette `skill` est universelle et fonctionne avec N'IMPORTE QUELLE configuration d'`agent`.

## Démarrage rapide

### Configuration de base du projet

Créer la structure OpenCode de base  

```bash
mkdir -p .opencode/{agent,command}

# Créer le fichier de configuration principal
cat > .opencode/opencode.json << 'EOF'
{
  "$schema": "https://opencode.ai/config.json",
  "theme": "opencode",
  "autoupdate": true
}
EOF
```

### Création de dossiers d'agent spécialisés (Swarm-Ready)

Crée des dossiers d'`agent` spécialisés avec des protocoles de communication appropriés pour le déploiement en `swarm` :

**Structure des dossiers :**  

```plaintext
folder-name/
├── AGENTS.md                    # Répertoire des agents et protocole de communication
└── .opencode/
    ├── opencode.json           # Configuration spécifique au dossier
    ├── agent/
    │   └── folder_name.md     # Agent principal avec exigences de communication
    └── command/
```

**Protocole de communication : Chaque agent DOIT se présenter :**

```plaintext
Je suis l'agent [Agent_Name] du dossier [folder_name]. 
Je vous contacte car [reason]. 
J'ai besoin que vous [request]. 
Veuillez répondre avec [format].
```

**⚠️ EXEMPLES DE TYPES D'AGENT UNIQUEMENT :**  
Ce qui suit sont des EXEMPLES pour illustrer le modèle. Les vrais `swarms` peuvent avoir N'IMPORTE QUEL type d'`agent` :  

```plaintext
    data-scientist/ - Analyse de données et machine learning
    ui-designer/ - Conception d'interface utilisateur et prototypage
    api-developer/ - Développement et intégration d'API
    game-developer/ - Logique et mécaniques de jeu
    researcher/ - Recherche et analyse
    translator/ - Traduction de langues et localisation
    music-composer/ - Composition musicale et production audio
    financial-analyst/ - Modélisation et analyse financière
```

**VÉRITÉ UNIVERSELLE :** Cette `skill` fonctionne avec N'IMPORTE QUEL nom de dossier, N'IMPORTE QUEL objectif d'`agent`, N'IMPORTE QUELLE spécialisation. Les exemples ci-dessus sont uniquement à titre d'illustration.

### Modèles de projet

```bash
# Structure de projet de développement  
mkdir -p .opencode/{agent,command}

# Agent code reviewer
cat > .opencode/agent/code-reviewer.md << 'EOF'

---
description: Reviews code for best practices and potential issues
mode: subagent
tools:
  read: true
  grep: true
permissions:
  edit: deny
  write: deny
---

Vous êtes un code reviewer. Concentrez-vous sur :
- La qualité du code et les bonnes pratiques (best practices)
- Les bugs potentiels et les edge cases
- Les implications en matière de performance
- Les considérations de sécurité

Fournissez des retours constructifs sans apporter de modifications directes.
EOF

# Configuration principale
cat > .opencode/opencode.json << 'EOF'
{
  "$schema": "https://opencode.ai/config.json",
  "theme": "opencode",
  "autoupdate": true
}
EOF
```

## Bonnes pratiques

### Organisation des fichiers
   - Utilisez le `kebab-case` pour les noms d'`agent` et de `command`
   - Gardez les descriptions concises mais informatives
   - Regroupez les `agents` et les `commands` connexes ensemble

### Gestion des permissions
   - Restreignez l'accès en écriture/modification (`write`/`edit`) pour les `agents` axés sur la sécurité
   - N'autorisez l'accès `bash` que lorsque cela est nécessaire
   - Utilisez des permissions de commande `bash` spécifiques au lieu de caractères génériques (`wildcard`)

### Gestion de la configuration
   - Incluez toujours `$schema` pour la validation
   - Utilisez des variables d'environnement pour les données sensibles
   - Conservez la configuration spécifique au projet dans `.opencode/`
   - Stockez les préférences de l'utilisateur dans `~/.config/opencode/`

### Conception des agents (Agent Design)
   - Créez des `agents` ciblés avec des objectifs spécifiques
   - Utilisez des `subagents` pour les tâches spécialisées
   - Réservez les `agents` principaux (`primary agents`) pour les tâches de coordination
   - Incluez des instructions d'utilisation claires dans les `prompts` des `agents`

Cette `skill` fournit tout ce qui est nécessaire pour créer des structures de dossiers OpenCode complètes et fonctionnelles pour tout type de projet, avec des `agents`, des `commands` et des configurations prêts à l'emploi.
