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

Activation sous Windows :

```powershell
.venv\Scripts\activate
```

---

## 3. Ajouter les dépendances métier

```bash
uv add pandas numpy scikit-learn fastapi uvicorn
```

Mise à jour automatique de :

```text
pyproject.toml
uv.lock
```

---

## 4. Ajouter une dépendance ultérieurement

```bash
uv add joblib
```

ou

```bash
uv add matplotlib
```

---

## 5. Supprimer une dépendance

```bash
uv remove matplotlib
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