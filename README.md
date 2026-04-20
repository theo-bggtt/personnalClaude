# Ma configuration Claude Code

Ce dépôt contient ma configuration personnelle pour [Claude Code](https://claude.com/claude-code), le CLI officiel d'Anthropic. Il est conçu pour être **reproduit à l'identique sur n'importe quelle autre machine** via un seul fichier d'instructions lisible par Claude lui-même.

## À quoi ça sert ?

Si vous utilisez Claude Code sur plusieurs machines (ou si vous changez de PC), vous voulez retrouver :

- les mêmes **paramètres globaux** (modèle, langue, hooks, niveau d'effort)
- les mêmes **plugins** et **marketplaces**
- les mêmes **serveurs MCP** (Playwright, GitHub, Magic/21st.dev, Canva, Google Drive)
- les mêmes **skills personnels** (bug-detector, security-scanner, accessibility-design-checker, web-performance-audit, claude-council)
- les mêmes **raccourcis clavier**
- le même **hook de bootstrap** qui lit automatiquement `CLAUDE.md`, `TODO.md`, `DECISION.md` et `PLAN.md` à chaque session

Tout cela est décrit dans un seul fichier : [`SETUP.md`](./SETUP.md).

## Comment l'utiliser

### Prérequis

Sur la nouvelle machine, installez d'abord :

1. **Node.js** (version LTS recommandée) — <https://nodejs.org>
2. **Git** — <https://git-scm.com>
3. **Claude Code** — <https://docs.claude.com/en/docs/claude-code/overview>
   ```bash
   npm install -g @anthropic-ai/claude-code
   ```
4. Un **abonnement Claude** actif (Pro ou Max) ou une clé API Anthropic configurée.

### Procédure

1. Clonez ce dépôt **ou** téléchargez simplement le fichier `SETUP.md`.
2. Ouvrez Claude Code dans n'importe quel dossier :
   ```bash
   claude
   ```
3. Donnez-lui cette instruction :

   > Lis le fichier `SETUP.md` et suis toutes les instructions qu'il contient pour reproduire la configuration. Crée chaque fichier délimité par `<<<FILE_START>>>` et `<<<FILE_END>>>` exactement au chemin indiqué, puis exécute les commandes d'installation des plugins et des serveurs MCP.

4. Claude va :
   - créer `~/.claude/settings.json`, `~/.claude/keybindings.json`, le script de bootstrap et les 5 fichiers de skills
   - installer les 4 marketplaces de plugins
   - installer les 6 plugins
   - configurer les serveurs MCP
5. Redémarrez Claude Code pour appliquer la configuration.

### Convention des fichiers embarqués

`SETUP.md` utilise des délimiteurs personnalisés (pas des triples backticks) pour éviter les conflits avec le code Markdown imbriqué :

```
<<<FILE_START: chemin/vers/fichier.ext>>>
... contenu du fichier ...
<<<FILE_END>>>
```

Claude sait interpréter ce format ; un humain peut aussi extraire les blocs manuellement.

## Ce qui n'est PAS inclus (à fournir séparément)

Par souci de sécurité, aucun secret n'est dans ce dépôt. Après exécution de `SETUP.md`, vous devrez fournir :

- **Token GitHub personnel (PAT)** — pour le serveur MCP GitHub
- **Clé API Magic / 21st.dev** — pour le serveur MCP `@21st-dev/magic`
- **OAuth Canva** et **Google Drive** — autorisations à ré-accorder lors de la première utilisation

## Ce qui ne sera pas strictement identique

- **Versions des plugins** : elles sont téléchargées en direct depuis les marketplaces GitHub, donc vous aurez la version la plus récente.
- **Mémoire de conversation** (`~/.claude/projects/`) : elle n'est pas transférée — chaque machine a son propre historique.
- **Version du CLI Claude Code** : dépend de votre installation npm locale.

Le *comportement* de Claude (modèle, langue, skills, outils, hooks) sera en revanche identique.

## Contenu du dépôt

| Fichier / dossier | Rôle |
|---|---|
| `SETUP.md` | Guide de reproduction complet et auto-contenu |
| `README.md` | Ce fichier |
| `skills/` | Skills personnels (référence, déjà embarqués dans `SETUP.md`) |
| `scripts/project-bootstrap.sh` | Hook de session (référence, déjà embarqué dans `SETUP.md`) |
| `settings.json`, `keybindings.json` | Paramètres (référence, déjà embarqués dans `SETUP.md`) |

Le fichier `SETUP.md` est **suffisant à lui seul** : les autres fichiers sont juste là pour inspection directe.

## Licence

Configuration personnelle publiée à titre d'exemple. Reprenez, adaptez, partagez.
