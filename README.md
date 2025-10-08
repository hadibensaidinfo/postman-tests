# 🧪 Postman E2E Tests

### Exécution locale

```bash
newman run collections/E2E.postman_collection.json \
  -e environments/Data.postman_environment.json \
  -r cli,html --reporter-html-export reports/report.html
