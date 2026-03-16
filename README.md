# JWT Security Analyzer 🛡️

[![Python](https://img.shields.io/badge/Python-3.8%2B-blue)](https://www.python.org/)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.100%2B-brightgreen)](https://fastapi.tiwari.com/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Tests](https://img.shields.io/badge/Tests-passing-brightgreen)](https://pytest.org/)

## 📝 Beschreibung

**JWT Security Analyzer** ist ein webbasiertes Tool zur automatischen Sicherheitsanalyse von JSON Web Tokens (JWT). 

Das Tool dekodiert JWTs und prüft auf gängige Sicherheitsprobleme wie:
- `alg: "none"` (unsignierte Tokens)
- Fehlende oder abgelaufene `exp` Claims
- Remote Key Loading (`jku`, `x5u`)
- Sensible Daten im Payload

Es berechnet einen **Risiko-Score (0-100)** und zeigt farbkodierte Findings an.

## ✨ Features

| Prüfung | Schweregrad | Beschreibung |
|---------|-------------|--------------|
| `alg: "none"` | 🔴 **Critical** | Unsignierter Token |
| Fehlender `exp` | 🟡 **Warning** | Kein Ablaufdatum |
| Abgelaufener `exp` | 🟠 **High** | Token abgelaufen |
| `jku`/`x5u` | 🟠 **High** | Remote Keys |
| Sensible Felder | 🟡 **Warning** | Password/API-Key im Payload |

**Risiko-Score Berechnung:**
```
critical(45) + high(30) + warning(15) + info(5) = Score (max 100)
```

## 🚀 Quickstart

```bash
# 1. Klonen (falls nötig)
git clone <repo-url>
cd jwt-security-analyzer

# 2. Virtuelle Umgebung
python -m venv .venv
source .venv/bin/activate  # Linux/Mac
# .venv\Scripts\activate  # Windows

# 3. FastAPI & Uvicorn installieren
pip install fastapi uvicorn jinja2 pydantic

# 4. Server starten
uvicorn app.main:app --reload --host 127.0.0.1 --port 8000

# 5. Öffnen
open http://127.0.0.1:8000
```

## 📱 Demo / Screenshot

```
[Frontend: Paste JWT → Analyze → Results]
Risk Score: 75/100 🔴 Critical
├── Header: { "alg": "none" }
├── Payload: { "password": "123456" }
└── Findings: 3 Issues
```

**Example Token (alg=none + sensitive data):**
```
eyJhbGciOiJub25lIn0.eyJzdWIiOiIxMjMiLCJwYXNzd29yZCI6IjEyMzQ1NiJ9.
```

## 🛠 API Endpoints

| Method | Endpoint | Beschreibung |
|--------|----------|--------------|
| `GET` | `/` | Web UI |
| `POST` | `/analyze` | `{ "token": "eyJ..." }` → Analysis JSON |
| `GET` | `/health` | Health check |
| `GET` | `/docs` | FastAPI Swagger Docs |

**Response Schema:**
```json
{
  "header": {...},
  "payload": {...},
  "signature": "...",
  "findings": [{"severity": "critical", "issue": "..."}],
  "risk_score": 75,
  "summary": "Critical JWT security issues detected"
}
```

## 🧪 Tests

```bash
pytest test_analyzer.py -v
```

100% Coverage der Security Checks.

## 📁 Projektstruktur

```
jwt-security-analyzer/
├── app/
│   ├── main.py      # FastAPI App
│   ├── analyzer.py  # JWT Decoder + Scoring
│   └── checks.py    # Security Rules
├── templates/
│   └── index.html   # Web UI (Deutsch)
├── test_analyzer.py # Unit Tests
├── .venv/           # Virtuelle Umgebung
└── README.md
```

## 🔒 Security Checks Details

1. **alg=none**: Critical - Token kann gefälscht werden
2. **No exp**: Warning - Token nie ungültig
3. **Expired**: High - Token nutzlos
4. **Remote Keys**: High - SSRF/Key Confusion möglich
5. **Sensitive Data**: Warning - Payload soll nicht vertrauenswürdig sein

## 🤝 Contributing

1. Fork & Clone
2. `pip install -e .` (nach Erstellung von pyproject.toml)
3. Neue Checks in `app/checks.py` hinzufügen
4. Tests schreiben: `pytest`
5. PR!

## 📈 Roadmap

- [ ] `requirements.txt` / `pyproject.toml`
- [ ] Mehr Checks (kid Confusion, iss/nbf, ...)
- [ ] CLI Version
- [ ] Export (PDF/JSON)
- [ ] Dark Mode

## 📄 License

MIT License - siehe [LICENSE](LICENSE) (erstellen Sie diese falls nötig).

## 🙏 Danke

Built with ❤️ using [FastAPI](https://fastapi.tiwari.com/)  
[Star us on GitHub! ⭐](https://github.com/yourusername/jwt-security-analyzer)

