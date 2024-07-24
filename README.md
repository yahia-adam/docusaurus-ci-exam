# Initialisation du projet et configuration de Docusaurus

## Initialisation du projet

1. Installez Node.js et npm.
2. Créez un nouveau projet Docusaurus :

```bash
npx create-docusaurus@latest my-website classic
cd my-website
npm install
npm run start
```

## Configuration de Docusaurus

 1.  Modifiez le fichier docusaurus.config.js pour personnaliser votre site.
 2.  Ajoutez des documents dans le répertoire docs.
 3.  Ajoutez des articles de blog dans le répertoire blog.
 4.  Configurez les barres de navigation dans sidebars.js.
   
Pour plus de détails, consultez la documentation officielle de Docusaurus.

### 2. Configuration de l'intégration continue (CI) avec GitHub Actions

Créez un fichier `ci-configuration.md` dans le répertoire `docs` :

# Configuration de l'intégration continue (CI) avec GitHub Actions

## Création d'un workflow CI

1. Créez le répertoire `.github/workflows` à la racine de votre projet.
2. Ajoutez un fichier `ci.yml` avec le contenu suivant :

```yaml
   name: CI/CD

    on:
        push:
        branches:
        - main

    jobs:
      build:
        runs-on: ubuntu-latest

        steps:
          - name: Checkout repository
            uses: actions/checkout@v2

          - name: Set up Node.js
            uses: actions/setup-node@v2
            with:
              node-version: '18'

          - name: Install dependencies
            run: npm install

          - name: Build Docusaurus site
            run: npm run build

          - name: Verify build files
            run: |
              test -d build || (echo "Build directory does not exist" && exit 1)
              test -f build/index.html || (echo "index.html does not exist" && exit 1)
              test -d build/docs || (echo "Docs directory does not exist" && exit 1)

          - name: Deploy to GitHub Pages
            if: ${{ github.ref == 'refs/heads/main' }}
            uses: peaceiris/actions-gh-pages@v3
            with:
              github_token: ${{ secrets.GITHUB_TOKEN }}
              publish_dir: ./build
```