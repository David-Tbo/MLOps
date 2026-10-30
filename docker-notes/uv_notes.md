# uv - Mémo Projet Scoring

## 1. Initialiser le projet

```bash
mkdir credit-scoring
cd credit-scoring

uv init .
```

Fichiers créés :

```text
pyproject.toml
main.py
.gitignore
```

---

## 2. Créer l'environnement virtuel

```bash
uv venv
```

Activation :
```shell
source .venv/bin/activate      # Linux/Mac
# ou
.venv\Scripts\activate         # Windows
```

---

## 3. Ajouter les dépendances métier

```bash
uv add pandas
uv add numpy
uv add scikit-learn
uv add jupyterlab
uv add matplotlib
uv add seaborn
uv add scorecardpy
uv add matplotlib
uv add scorecardpy
```

Pour les dépendances de développement :
```shell
uv add --dev pytest
uv add --dev ruff
uv add --dev mypy
```

Mise à jour automatique de :

```text
pyproject.toml
uv.lock
```

---

## 4. Supprimer une dépendance

```bash
uv remove matplotlib
```


---

## 5. Utilisation avec Jupyter


Installer le kernel :
```shell
uv add ipykernel
```

Puis :
```shell
python -m ipykernel install \
    --user \
    --name credit-scoring
```

Ensuite dans VS Code ou Jupyter tu sélectionnes le kernel :
```text
credit-scoring
```

Les notebooks utiliseront exactement les packages gérés par uv.

Importer ton code Python depuis les notebooks

Si tu travailles en mode package :
```python
from credit_scoring.discretization import QuantileDiscretizer
```

au lieu de :
```python
import sys
sys.path.append("../src")
```

Pour cela :
```shell
uv pip install -e .
```

ou plus simplement :
```shell
uv sync
```

si le package est déclaré dans pyproject.toml.

Verrouillage des versions

L'un des gros intérêts d'uv :
```shell
uv lock
```

génère :
```text
uv.lock
```

qui remplace efficacement un requirements.txt figé.

Les autres développeurs reconstruisent exactement le même environnement :
```shell
uv sync
```


---

## 6. Installer les dépendances du projet

Après un clone Git :

```bash
uv sync
```

Recrée automatiquement l'environnement à partir du :

```text
pyproject.toml
uv.lock
```

---

## 7. Exécuter le projet

```bash
uv run python app.py
```

ou

```bash
uv run uvicorn app:app --reload
```

---

## 8. Mettre à jour les dépendances

```bash
uv lock --upgrade
```

Puis :

```bash
uv sync
```

---

## 9. Exporter vers requirements.txt

Seulement si nécessaire (compatibilité CI/CD, Docker, Jenkins, etc.)

```bash
uv export -o requirements.txt
```

---

## 10. Structure cible

```text
credit-scoring/
├── .venv/
├── pyproject.toml
├── uv.lock
├── Dockerfile
├── compose.yaml
├── compose.debug.yaml
├── app.py
└── src/
```

---

## Règle MLOps

```text
Source de vérité :
    pyproject.toml
    uv.lock
```

Compatibilité :
    `requirements.txt` (généré si besoin)

Ne jamais maintenir manuellement :

```text
requirements.txt
```

Le générer à partir de :

```text
pyproject.toml
+
uv.lock
```
