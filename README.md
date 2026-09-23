# learn

Mon système d'apprentissage avec [pi](https://github.com/earendil-works/pi), basé sur [amosblomqvist/learn](https://github.com/amosblomqvist/learn) (détails du système : [.pi/README.md](.pi/README.md)).

- `.pi/` : config pi (skills, extensions, agents)
- racine : mes notes et leçons (`sujet/lecon-01.md`), lues dans Obsidian
- sync entre PC : git uniquement

## Installation (une fois par PC, dans WSL)

```bash
# Outils
sudo apt update && sudo apt install -y git tmux curl unzip \
  libnspr4 libnss3 libatk1.0-0 libatk-bridge2.0-0 libcups2 libdrm2 libxkbcommon0 \
  libxcomposite1 libxdamage1 libxfixes3 libxrandr2 libgbm1 libpango-1.0-0 libcairo2 libasound2t64
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/master/install.sh | bash
source ~/.bashrc
nvm install 24

# pi
npm install -g --ignore-scripts @earendil-works/pi-coding-agent

# Git (réutilise la connexion GitHub de Git pour Windows)
git config --global user.name "Fabien Sartre"
git config --global user.email "fabien.sartre@digitalvalue.fr"
git config --global credential.helper "/mnt/c/Program\ Files/Git/mingw64/bin/git-credential-manager.exe"

# Repo (sur un nouveau PC, hors OneDrive)
git clone https://github.com/Fabien-Sartre/learn ~/learn
cd ~/learn/.pi/extensions/visual-tools
npm install --omit=dev
npm rebuild puppeteer   # télécharge Chromium pour le rendu Mermaid

# Test du rendu : doit afficher OK
echo 'graph LR; A-->B' > /tmp/t.mmd && npx mmdc -i /tmp/t.mmd -o /tmp/t.svg && echo OK
```

Subagents ([pi-interactive-subagents](https://github.com/amosblomqvist/pi-interactive-subagents), tmux uniquement ; installé dans la config globale de pi, donc à refaire sur chaque PC) :

```bash
pi install git:github.com/amosblomqvist/pi-interactive-subagents@main
```

Sans lui, pas de researcher ni de visuels générés, mais l'enseignement fonctionne. Ne pas utiliser l'extension subagent d'exemple fournie avec pi : les agents de `.pi/agents/` attendent les outils de celle-ci (`web_search`, `web_fetch`, `safe_bash`).

Le researcher utilise `openrouter/z-ai/glm-5.3` : il faut une clé OpenRouter (`/login`), ou changer `model:` dans `.pi/agents/researcher.md`.

Obsidian : « Ouvrir un dossier comme coffre » → le dossier du repo.

### Dépannage

- `no zip archiver is available` → `sudo apt install -y unzip`
- `browser folder exists but the executable is missing` → `rm -rf ~/.cache/puppeteer` puis `npm rebuild puppeteer`
- `error while loading shared libraries: xxx.so` → bibliothèque manquante, lister avec `ldd ~/.cache/puppeteer/chrome-headless-shell/*/chrome-headless-shell-linux64/chrome-headless-shell | grep "not found"`
- Ubuntu < 24.04 : remplacer `libasound2t64` par `libasound2`
- Ne pas lancer `
```

Crée la session tmux `pi` (ou s'y rattache) et lance pi dedans. `Ctrl+b` puis `d` : se détacher sans arrêter pi. Quitter pi ferme la session.

Dans pi :
- `/login` (une seule fois) → Anthropic → clé API, stockée dans `auth.json` hors du repo
- `/md-log sujet/lecon-01.md` → la leçon s'écrit dans ce fichier, à lire dans Obsidian
- `/md-unlog` → arrête l'écriture

En fin de session :

```bash
git add -A && git commit -m "leçon ..." && git push
```

Toujours pousser avant de changer de PC, sinon conflit au prochain `pull`. L'historique de conversation de pi reste sur le PC : pour reprendre ailleurs, donner à pi le md de la leçon précédente.
