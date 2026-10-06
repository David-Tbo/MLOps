# Comprendre ce qui s'est passé lors de la création de mon premier projet Docker avec VS Code

## Contexte

J'ai créé un fichier `Dockerfile` dans VS Code.

L'extension Microsoft "Container Tools" a détecté ce fichier et m'a proposé de générer automatiquement les fichiers Docker nécessaires à une application Python.

En acceptant les options proposées, VS Code a :

1. remplacé mon Dockerfile simple par un Dockerfile plus complet ;
2. créé un fichier `compose.yaml` ;
3. créé un fichier `compose.debug.yaml` ;
4. préparé le projet pour l'exécution et le débogage dans des conteneurs.

---

## Architecture obtenue

```text
credit-scoring/
├── Dockerfile
├── compose.yaml
├── compose.debug.yaml
├── requirements.txt
├── model.pkl
├── app.py
└── src/
```

---

## Rôle des fichiers

| Fichier | Rôle |
|---|---|
| Dockerfile | Décrit comment construire l'image Docker |
| requirements.txt | Liste les packages Python à installer |
| app.py | Point d'entrée de l'application |
| compose.yaml | Décrit comment démarrer un ou plusieurs conteneurs |
| compose.debug.yaml | Configuration spécifique au mode Debug de VS Code |
| model.pkl | Modèle de scoring sérialisé |
| src/ | Code métier du projet |

---

## Ce qu'est une image Docker

Une image est une recette.

Elle contient :

- le système de base ;
- Python ;
- les bibliothèques ;
- le code ;
- les modèles.

Exemple :

```text
Image Docker
|
├── Linux
├── Python
├── FastAPI
├── Pandas
├── Code de scoring
└── model.pkl
```

Cette image peut être exécutée partout.

---

## Ce qu'est un conteneur

Un conteneur est une instance en cours d'exécution d'une image.

Relation :

```text
Dockerfile
    ↓
Image Docker
    ↓
Conteneur
```

Comme :

```text
Code source
    ↓
Programme compilé
    ↓
Processus en cours
```

---

## Analyse de votre Dockerfile

### Base Python

```dockerfile
FROM python:3-slim
```

Télécharge une image Python légère.

---

### Port exposé

```dockerfile
EXPOSE 8000
```

Indique que l'application écoute sur le port 8000.

Typiquement utilisé pour une API FastAPI.

---

### Variables d'environnement

```dockerfile
ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1
```

Effets :

- évite la création des fichiers `.pyc`
- améliore l'affichage des logs dans Docker

---

### Installation des dépendances

```dockerfile
COPY requirements.txt .
RUN python -m pip install -r requirements.txt
```

Installe automatiquement :

```text
pandas
numpy
scikit-learn
fastapi
uvicorn
```

---

### Copie du projet

```dockerfile
WORKDIR /app
COPY . /app
```

Copie tout le projet dans le conteneur.

---

### Sécurité

```dockerfile
RUN adduser ...
USER appuser
```

Le conteneur n'est pas exécuté en mode administrateur.

C'est une bonne pratique de sécurité.

---

### Lancement de l'application

```dockerfile
CMD ["gunicorn", "--bind", "0.0.0.0:8000", "-k", "uvicorn.workers.UvicornWorker", "app:app"]
```

Signifie :

```text
Lancer FastAPI
dans le fichier app.py
avec l'objet app
```

Le code suivant doit exister :

```python
from fastapi import FastAPI

app = FastAPI()
```

---

## Pourquoi VS Code a créé compose.yaml ?

Parce qu'un projet réel contient souvent plusieurs conteneurs.

Exemple :

```text
API de scoring
     +
Base PostgreSQL
     +
Monitoring
```

Docker Compose permet de démarrer tout l'environnement avec une seule commande.

```bash
docker compose up
```

---

## Pourquoi VS Code a créé compose.debug.yaml ?

Pour permettre :

- les points d'arrêt ;
- le debug pas à pas ;
- l'inspection des variables.

VS Code utilise ce fichier lorsqu'on clique sur :

```text
Run and Debug
```

---

## Que signifie "Override" ?

VS Code vous demandait probablement si :

- il pouvait remplacer votre Dockerfile ;
- il pouvait écraser une configuration existante ;
- il pouvait générer automatiquement la configuration Docker.

En répondant "Override All", vous avez autorisé VS Code à régénérer tous les fichiers nécessaires.

---

## Dans un futur projet de scoring

Je recommande :

### Étape 1

Créer :

```text
credit-scoring/
├── Dockerfile
├── requirements.txt
├── app.py
├── model.pkl
└── src/
```

### Étape 2

Installer l'extension :

```text
Microsoft Container Tools
```

### Étape 3

Laisser VS Code générer :

```text
compose.yaml
compose.debug.yaml
```

### Étape 4

Adapter uniquement :

```dockerfile
CMD [...]
```

et

```python
app.py
```

à votre modèle.

### Étape 5

Construire l'image.

En environnement SG :

```text
Git
 ↓
Jenkins xbl.dev.docker-build
 ↓
Harbor
 ↓
Kubernetes
```

---

## À retenir

| Élément | À retenir |
|---|---|
| Dockerfile | Construction de l'image |
| Image Docker | Package exécutable |
| Conteneur | Exécution de l'image |
| app.py | Point d'entrée |
| compose.yaml | Démarrage des services |
| compose.debug.yaml | Débogage VS Code |
| Override All | Régénération automatique des fichiers |
| Extension Container Tools | Assistant Docker de VS Code |
| Votre Dockerfile actuel | Correct pour une API FastAPI de scoring |

---

## Analyse des fichiers générés par VS Code

### compose.yaml

```yaml
services:
  conteneur:
    image: conteneur
    build:
      context: .
      dockerfile: ./Dockerfile
    ports:
      - 8000:8000
```

#### Rôle

Ce fichier décrit comment démarrer le conteneur en mode normal.

#### Décomposition

##### Service

```yaml
services:
  conteneur:
```

Déclare un service nommé :

```text
conteneur
```

Le nom est arbitraire.

Pour un projet de scoring, on pourrait préférer :

```yaml
services:
  scoring-api:
```

##### Construction de l'image

```yaml
build:
  context: .
  dockerfile: ./Dockerfile
```

Signifie :

```text
Construire l'image à partir du Dockerfile
présent dans le répertoire courant
```

##### Nom de l'image

```yaml
image: conteneur
```

L'image créée portera le nom :

```text
conteneur
```

On pourrait aussi utiliser :

```yaml
image: credit-scoring
```

##### Ports

```yaml
ports:
  - 8000:8000
```

Correspondance :

```text
Poste local        Conteneur
-----------        ---------
8000        -->    8000
```

Ainsi :

```text
http://localhost:8000
```

redirige vers l'application dans le conteneur.

---

## compose.debug.yaml

### Contenu

```yaml
services:
  conteneur:
    image: conteneur
    build:
      context: .
      dockerfile: ./Dockerfile
    command:
      ["sh", "-c", "..."]
    ports:
      - 8000:8000
      - 5678:5678
```

### Rôle

Ce fichier est utilisé uniquement lorsque VS Code lance le débogage.

Il remplace temporairement la commande définie dans le Dockerfile.

---

## Pourquoi le Dockerfile parle d'override ?

Votre Dockerfile contient :

```dockerfile
CMD [...]
```

En mode normal :

```text
Docker
  ↓
CMD du Dockerfile
  ↓
Application lancée
```

En mode Debug :

```text
Docker
  ↓
compose.debug.yaml
  ↓
Nouvelle commande
  ↓
Débogage VS Code
```

Le terme "override" signifie simplement :

```text
Remplacer temporairement
```

---

## Que fait réellement la commande de debug ?

```yaml
python /tmp/debugpy
```

Lance :

```text
debugpy
```

qui est le moteur de débogage Python utilisé par VS Code.

### Paramètre suivant

```yaml
--wait-for-client
```

Signifie :

```text
Ne démarre pas tant que VS Code n'est pas connecté
```

Cela permet de poser des points d'arrêt dès le démarrage.

### Port de debug

```yaml
--listen 0.0.0.0:5678
```

Ouvre le port :

```text
5678
```

pour permettre à VS Code de se connecter.

---

### Exécution de l'API

```yaml
-m uvicorn app:app
```

Équivaut à :

```bash
uvicorn app:app
```

et signifie :

```text
Fichier : app.py
Variable FastAPI : app
```

---

## Différence entre les deux fichiers

| Fichier | Utilisation |
|---|---|
| compose.yaml | Exécution normale |
| compose.debug.yaml | Débogage depuis VS Code |
| Dockerfile | Définition de l'image |
| app.py | Code Python exécuté |

---

## Architecture réelle mise en place par VS Code

```text
app.py
  ↓
Dockerfile
  ↓
Image Docker
  ↓
compose.yaml
  ↓
Conteneur
  ↓
http://localhost:8000
```

Pour le debug :

```text
app.py
  ↓
Dockerfile
  ↓
Image Docker
  ↓
compose.debug.yaml
  ↓
DebugPy
  ↓
VS Code
```

---

## Ce que VS Code a supposé

En voyant :

```text
Dockerfile
+
Python
```

VS Code a supposé que vous développiez :

```text
FastAPI
+
Uvicorn
+
Docker
```

c'est-à-dire exactement l'architecture la plus courante aujourd'hui pour exposer un modèle de scoring sous forme d'API.

---

## Mon avis

Rien d'anormal ne s'est produit.

VS Code a simplement transformé votre projet en un projet Docker "professionnel" prêt pour :

- développement local ;
- débogage ;
- exécution en conteneur ;
- déploiement futur sur Kubernetes.

Même si vous débutez avec Docker, les fichiers générés sont cohérents et constituent une bonne base pour un futur projet de scoring industrialisé.