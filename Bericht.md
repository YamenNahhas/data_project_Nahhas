# GitHub-Konnektor testen

Dieses Dokument fasst den Test des GitHub-Zugriffs auf `YamenNahhas/data_project_Nahhas` zusammen.

## Abgerufene Repository-Daten

- Repository: [YamenNahhas/data_project_Nahhas](https://github.com/YamenNahhas/data_project_Nahhas)
- Standard-Branch: `main`
- Berechtigungsstufe des aktuellen Zugriffs: `ADMIN`
- Dateien im Stammverzeichnis beim Abruf: `.gitignore`, `README.md`, `my_project.ipynb`, `pyproject.toml`, `uv.lock`

## Fähigkeiten im Überblick

Je nach Berechtigungen kann der GitHub-Zugriff unter anderem:

- Repository-Metadaten und Dateien lesen
- Issues und Pull Requests untersuchen
- Branches und Commits abrufen
- Repository-Dateien erstellen oder aktualisieren

Schreibzugriffe benötigen passende Repository-Rechte. Zugangstoken sollten sicher als Umgebungsvariablen hinterlegt und niemals in Dateien oder Notebooks festgeschrieben werden.

## Beispiel: Repository-Daten über die REST API abrufen

```python
import os
import requests

owner, repo = "YamenNahhas", "data_project_Nahhas"
headers = {"Accept": "application/vnd.github+json"}
if os.getenv("GITHUB_TOKEN"):
    headers["Authorization"] = f"Bearer {os.environ['GITHUB_TOKEN']}"

base = f"https://api.github.com/repos/{owner}/{repo}"
metadata = requests.get(base, headers=headers, timeout=20)
metadata.raise_for_status()
files = requests.get(f"{base}/contents", headers=headers, timeout=20)
files.raise_for_status()

print("Repository:", metadata.json()["full_name"])
print("Standard-Branch:", metadata.json()["default_branch"])
print("Dateien:", [item["name"] for item in files.json()])
```

Öffentliche Repository-Daten sind ohne Token lesbar. Für private Repositorys ist ein geeignetes Token mit den erforderlichen Rechten nötig.
