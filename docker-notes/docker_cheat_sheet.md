# Docker - Fiche Mémo

## Concepts clés

```text
Dockerfile
    ↓
Image Docker
    ↓
Conteneur
```

- **Dockerfile** : recette de construction.
- **Image** : application packagée (Python, librairies, code, modèle).
- **Conteneur** : instance en cours d'exécution de l'image.

---

## Structure minimale d'un projet de scoring

```text
credit-scoring/
├── Dockerfile
├── requirements.txt
├── model.pkl
├── app.py
└── src/
```

---

## Dockerfile

Exemple :

```dockerfile
FROM python:3-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install -r requirements.txt

COPY . .

CMD ["python", "app.py"]
```

Rôle :
- installer Python ;
- installer les dépendances ;
- copier le code ;
- définir le point d'entrée.

---

## Docker Compose

### compose.yaml

Permet de démarrer les conteneurs :

```bash
docker compose up
```

Exemple :

```yaml
services:
  scoring-api:
    build: .
    ports:
      - 8000:8000
```

---

### compose.debug.yaml

Utilisé uniquement pour le débogage depuis VS Code.

Ajoute un port de debug et remplace temporairement la commande de démarrage.

---

## Comprendre `app:app`

Dans :

```bash
uvicorn app:app
```

le format est :

```text
module:objet
```

Donc :

```text
app.py
  ↓
app
```

Exemple :

```python
from fastapi import FastAPI

app = FastAPI()
```

---

## Comprendre Uvicorn

Dans :

```dockerfile
CMD [
  "gunicorn",
  "--bind", "0.0.0.0:8000",
  "-k", "uvicorn.workers.UvicornWorker",
  "app:app"
]
```

- **Gunicorn** : gère les processus serveur.
- **Uvicorn** : exécute une application Python ASGI.
- **ASGI** : standard moderne pour les API Python.

Cette configuration indique que VS Code a supposé que l'application est une application **ASGI**, très probablement **FastAPI**.

---

## Pourquoi VS Code a généré ces fichiers ?

L'extension **Microsoft Container Tools** a détecté :

```text
Projet Python
+
Dockerfile
```

et a automatiquement créé :

```text
Dockerfile
compose.yaml
compose.debug.yaml
```

pour préparer :
- le développement ;
- le débogage ;
- la conteneurisation ;
- un futur déploiement Kubernetes.

---

## Cas d'usage typique pour un score de crédit

```text
Modèle sklearn
      ↓
FastAPI
      ↓
Docker
      ↓
Harbor
      ↓
Kubernetes
      ↓
API de scoring
```

---

## À retenir

- Dockerfile = recette.
- Image = package exécutable.
- Conteneur = application qui tourne.
- `app:app` = fichier `app.py` + objet `app`.
- `uvicorn` = serveur ASGI.
- `gunicorn + uvicorn` = architecture standard des API FastAPI.
- `compose.yaml` = exécution.
- `compose.debug.yaml` = débogage VS Code.