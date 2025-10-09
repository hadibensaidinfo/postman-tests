# 🧪 Postman E2E Tests

Tests de bout en bout (E2E) automatisés avec **Postman** et exécutés via **Newman** — localement ou dans **GitHub Actions**.

---

## 🏗️ Structure du projet

```
├── collections/
│   └── E2E.postman_collection.json      # Collection Postman contenant les tests E2E
├── environments/
│   └── Data.postman_environment.json    # Variables d’environnement (baseURL, token, etc.)
├── reports/
│   └── report.html                      # Rapport HTML généré par Newman
├── .github/
│   └── workflows/
│       └── newman-tests.yml             # Workflow GitHub Actions pour exécution CI/CD
├── package.json                         # Dépendances et scripts npm
└── README.md                            # Documentation du projet
```

---

## ⚙️ Installation

```bash
npm install
```

---

## 🚀 Exécution locale

```bash
newman run collections/E2E.postman_collection.json   -e environments/Data.postman_environment.json   -r cli,html --reporter-html-export reports/report.html
```

📊 Le rapport HTML sera généré ici :
```
reports/report.html
```

---

## 💬 Scripts npm

Pour simplifier les exécutions, tu peux utiliser les scripts suivants définis dans `package.json` :

```json
  "scripts": {
  "test": "newman run collections/E2E.postman_collection.json -e environments/Data.postman_environment.json -r cli,html --reporter-html-export reports/report.html; exit 0",
  "test:local": "newman run collections/E2E.postman_collection.json -e environments/Data.postman_environment.json -r cli,html --reporter-html-export reports/report.html"
}
```

- **`npm test`** → Utilisé en CI/CD (continue même si des tests échouent, pour générer le rapport).
- **`npm run test:ci`** → Exécution locale (échoue si un test échoue).

---

## 💡 Bonnes pratiques

- 🧩 Utiliser les **environnements Postman** pour gérer les variables dynamiques (`baseURL`, `authToken`, etc.).
- 🔐 Ne jamais exposer de secrets ou tokens sensibles dans le repo.
- 📝 Ajouter des **descriptions** claires à chaque dossier et requête dans la collection.
- 🧱 Séparer les collections par modules (Auth, Users, Transactions…).
- 🔁 Toujours **réexporter** la collection et l’environnement après chaque modification dans Postman.
- 🧾 Utiliser `--reporter-html-export` pour générer des rapports clairs et exploitables.

---

## 🤖 Intégration continue – GitHub Actions

Fichier : `.github/workflows/newman-tests.yml`

```yaml
name: Run Postman Tests

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  newman-tests:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'

      - name: Install dependencies
        run: npm install

      - name: Run Postman tests with Newman
        run: npm test || true

      - name: Upload Newman HTML report
        uses: actions/upload-artifact@v4
        with:
          name: newman-report
          path: reports/report.html
```

🟢 Le `|| true` dans le script CI garantit que le **rapport est toujours généré**, même en cas d’échec de tests.

---

## 👨‍💻 Auteur

**Hedi Bensaid**  
QA Engineer | Test Automation | ISTQB® & SFPC™ Certified  
🧰 Outils : Postman, Newman, Playwright, Cypress, Robot Framework  
🌐 [LinkedIn](https://www.linkedin.com/in/hedi-bensaid/)
