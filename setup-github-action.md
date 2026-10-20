# 🚀 GitHub Profile Automation Suite (Workflows)

Ce guide contient les workflows GitHub Actions automatisés pour alimenter dynamiquement votre profil GitHub :
1. **GitHub Profile 3D Contrib** (Calendrier de contributions isométrique 3D)
2. **GitHub Contribution Snake** (Le jeu Snake qui mange vos contributions vertes en temps réel)

---

## 🛠️ Installation dans votre Repo Spécial

1. Créez votre repository public GitHub qui porte exactement le même nom que votre pseudo (ex: `https://github.com/USERNAME/USERNAME`).
2. Copiez le contenu de `README.md` dans la racine de ce repository.
3. Créez un dossier `.github/workflows/` dans ce repo.
4. Ajoutez les deux fichiers ci-dessous.

---

### 1️⃣ `.github/workflows/profile-3d.yml` (Calendrier 3D)

```yaml
name: GitHub-Profile-3D-Contrib

on:
  schedule:
    - cron: "0 18 * * *" # Exécution quotidienne à 18h00 UTC
  workflow_dispatch:

permissions:
  contents: write

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: yoshi389111/github-profile-3d-contrib@0.7.1
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          USERNAME: ${{ github.repository_owner }}
      - name: Commit & Push 3D Assets
        run: |
          git config user.name github-actions
          git config user.email github-actions@github.com
          git add -A .
          git commit -m "chore: update 3d-contrib graph [skip ci]" || exit 0
          git push
```

---

### 2️⃣ `.github/workflows/snake.yml` (Animation Snake)

```yaml
name: Generate Contribution Snake

on:
  schedule:
    - cron: "0 0 * * *" # Chaque nuit à minuit
  workflow_dispatch:

permissions:
  contents: write

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      - name: Generate Snake Animation SVG
        uses: Platane/snk@v3
        with:
          github_user_name: ${{ github.repository_owner }}
          outputs: |
            dist/github-contribution-grid-snake.svg
            dist/github-contribution-grid-snake-dark.svg?palette=github-dark

      - name: Push Snake SVG to Output Branch
        uses: crazy-max/ghaction-github-pages@v3.1.0
        with:
          target_branch: output
          build_dir: dist
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

---

## 💡 Remarque
Si votre nom d'utilisateur GitHub est différent de `fourari`, faites simplement un remplacement de `fourari` par votre pseudo dans le [README.md](file:///c:/Users/foura/Desktop/github/mon-profile/README.md).
