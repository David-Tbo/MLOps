# uv - Mémo Projet Machine Learning

## 1. Initialiser le projet

```bash
mkdir diabetes-risk-modeling
cd diabetes-risk-modeling

uv init
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

```bash
source .venv/bin/activate      # Linux/Mac
# ou
.venv\Scripts\activate         # Windows
```

Remarque : avec uv, l'activation n'est pas toujours nécessaire. Il est souvent possible d'utiliser directement :

```bash
uv run <commande>
```

Exemples :

```bash
uv run python main.py
uv run jupyter lab
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
```

Dépendances de développement :

```bash
uv add --dev pytest
uv add --dev ruff
uv add --dev mypy
uv add --dev ipykernel
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

Mise à jour automatique de :

```text
pyproject.toml
uv.lock
```

---

## 5. Utilisation avec Jupyter

Installer le kernel :

```bash
uv add --dev ipykernel
```

Créer un kernel Jupyter associé à l'environnement :

```bash
python -m ipykernel install \
    --user \
    --name diabetes-risk-modeling
```

Dans VS Code ou Jupyter, sélectionner ensuite le kernel :

```text
diabetes-risk-modeling
```

Les notebooks utiliseront exactement les packages gérés par uv.

### Importer son code Python depuis les notebooks

Avec une structure en package :

```python
from diabetes_risk_modeling.discretization import QuantileDiscretizer
```

plutôt que :

```python
import sys
sys.path.append("../src")
```

Si le package est correctement déclaré dans `pyproject.toml` :

```bash
uv sync
```

suffit généralement.

Alternative :

```bash
uv pip install -e .
```

---

## 6. Verrouillage des versions

L'un des principaux intérêts de uv :

```bash
uv lock
```

génère :

```text
uv.lock
```

Ce fichier garantit la reproductibilité exacte des dépendances.

Les autres développeurs recréent le même environnement avec :

```bash
uv sync
```

---

## 7. Installer les dépendances du projet

Après un clone Git :

```bash
uv sync
```

Cette commande :

- crée l'environnement virtuel si nécessaire ;
- installe les dépendances ;
- applique les versions définies dans `uv.lock`.

Fichiers utilisés :

```text
pyproject.toml
uv.lock
```

---

## 8. Exécuter le projet

Exécution d'un script :

```bash
uv run python main.py
```

Exécution d'un notebook :

```bash
uv run jupyter lab
```

Exemple FastAPI (si présent dans le projet) :

```bash
uv run uvicorn app:app --reload
```

---

## 9. Mettre à jour les dépendances

Mettre à jour le lockfile :

```bash
uv lock --upgrade
```

Puis synchroniser l'environnement :

```bash
uv sync
```

---

## 10. Explorer les dépendances

Afficher l'arbre des dépendances :

```bash
uv tree
```

Très utile pour comprendre les dépendances transitives.

---

## 11. Exporter vers requirements.txt

Uniquement pour des besoins de compatibilité :

- Docker
- CI/CD
- Jenkins
- plateformes ne supportant pas uv

```bash
uv export --format requirements-txt -o requirements.txt
```

---

## 12. Structure cible du projet

```text
diabetes-risk-modeling/
├── .venv/
├── pyproject.toml
├── uv.lock
├── README.md
├── docs/
│   ├── architecture.md
│   ├── methodology.md
│   └── data_dictionary.md
├── data/
├── notebooks/
│   ├── 01_eda.ipynb
│   ├── 02_feature_engineering.ipynb
│   └── 03_modeling.ipynb
├── tests/
├── main.py
└── src/
    └── diabetes_risk_modeling/
        ├── __init__.py
        ├── preprocessing.py
        ├── discretization.py
        ├── modeling.py
        └── evaluation.py
```

---

## Règle MLOps

Source de vérité :

```text
pyproject.toml
    → dépendances déclarées

uv.lock
    → versions figées et reproductibles
```

Compatibilité :

```text
requirements.txt
```

Ne jamais maintenir manuellement :

```text
requirements.txt
```

Le générer automatiquement à partir de :

```text
pyproject.toml
+
uv.lock
```

Principe :

```text
Modifier les dépendances avec :

uv add
uv remove
uv lock

Jamais directement dans requirements.txt
```
