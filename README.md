🚀 E2E API Testing Project (Postman + Newman + GitHub Actions)

Ce projet contient une suite de tests End-to-End (E2E) automatisés pour valider les API du site Demo TestFire à l’aide de Postman et Newman.
Il est conçu pour être exécuté en local ou dans un pipeline GitHub Actions avec génération automatique d’un rapport HTML.

📁 Structure du projet
├── collections/
│   └── E2E.postman_collection.json      # Collection Postman contenant les requêtes et les tests
├── environments/
│   └── Data.postman_environment.json    # Variables d’environnement (ex: baseURL)
├── reports/
│   └── report.html                      # Rapport HTML généré par Newman
├── .github/
│   └── workflows/
│       └── newman-tests.yml             # Pipeline GitHub Actions pour exécuter les tests
├── package.json                         # Dépendances et script npm
├── README.md                            # Documentation du projet

⚙️ Installation

Cloner le dépôt

git clone https://github.com/<votre-utilisateur>/<votre-repo>.git
cd <votre-repo>


Installer les dépendances

npm install

🧪 Exécution des tests en local

Lancer tous les tests avec Newman via NPM :

npm test


Ce script exécute :

newman run collections/E2E.postman_collection.json \
  -e environments/Data.postman_environment.json \
  -r cli,html --reporter-html-export reports/report.html


📊 Le rapport sera généré automatiquement dans le dossier reports/report.html.

🧰 Variables d’environnement

Le fichier Data.postman_environment.json contient les variables globales du projet.
Exemple :

{
  "key": "baseURL",
  "value": "https://demo.testfire.net"
}


🟡 Bonne pratique :

Utiliser les variables d’environnement pour les URLs, tokens et identifiants.
Ne pas stocker d’informations sensibles directement dans la collection.
🔁 Exécution dans GitHub Actions

Le pipeline est défini dans .github/workflows/newman-tests.yml.
Il :

Installe Node.js et Newman
Exécute les tests Postman
Génère le rapport HTML
Publie le rapport comme artefact téléchargeable

Exemple de fichier simplifié :

name: Newman API Tests

on: [push, pull_request]

jobs:
  run-tests:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Install dependencies
        run: npm install

      - name: Run Postman tests
        run: npm test || true

      - name: Upload Newman HTML Report
        uses: actions/upload-artifact@v4
        with:
          name: newman-report
          path: reports/report.html


🟢 Le pipeline continue même si certains tests échouent (|| true), afin que le rapport soit toujours généré.

📄 Résultats des tests
Les logs s’affichent dans la console (cli reporter)
Un rapport détaillé est généré en HTML :
reports/report.html


Pour l’ouvrir :

start reports/report.html      # sous Windows
open reports/report.html       # sous macOS/Linux

🧠 Bonnes pratiques QA

✅ Utiliser des noms explicites pour les requêtes et dossiers dans Postman.
✅ Ajouter des descriptions claires à chaque dossier et requête.
✅ Centraliser les variables dynamiques dans les environnements.
✅ Intégrer Newman dans la CI/CD pour détecter rapidement les régressions.
✅ Générer systématiquement un rapport HTML pour les audits.

👤 Auteur

Hedi Bensaid
QA Engineer | Test Automation | ISTQB & SFPC™ Certified
🧰 Technologies : Postman, Newman, Playwright, Cypress, Robot Framework
🌐 LinkedIn | GitHub
