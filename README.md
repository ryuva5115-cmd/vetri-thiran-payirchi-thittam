# PocketSmart AI — Smart Budget & Recommendation Assistant

A complete FastAPI + Jinja2 + SQLite + Gemini multimodal project based on the supplied PocketSmart AI documentation.

## What is included

- User registration, login, logout and JWT authentication
- Home Interior Budget Planner
- Party Budget Planner
- Jewelry Budget Planner with optional outfit image
- Gemini-powered recommendations
- Local fallback recommendations when Gemini is unavailable
- Recommendation history
- Dashboard
- Responsive HTML/CSS/JavaScript UI
- Mock/simulated product/service catalog links for Amazon, Flipkart, IKEA, Swiggy, Zomato and OYO
- API endpoints matching the documentation
- SQLite persistence
- Automated API tests

## Important model note

The source document specifies Gemini 1.5 Flash Pro. That model is no longer the recommended current Gemini API model. This implementation therefore makes the model configurable with `GEMINI_MODEL` and defaults to `gemini-3.8-flash`, while keeping the architecture described in the document. Change the environment variable if your account supports another model.

## Project structure

```text
PocketSmartAI/
├── app/
│   ├── __init__.py
│   ├── main.py
│   ├── config.py
│   ├── database.py
│   ├── models.py
│   ├── schemas.py
│   ├── auth.py
│   ├── dependencies.py
│   ├── routes/
│   │   ├── __init__.py
│   │   ├── auth.py
│   │   ├── pages.py
│   │   └── recommendations.py
│   ├── services/
│   │   ├── __init__.py
│   │   ├── gemini_utils.py
│   │   └── fallback.py
│   ├── templates/
│   │   ├── base.html
│   │   ├── index.html
│   │   ├── login.html
│   │   ├── register.html
│   │   ├── dashboard.html
│   │   ├── planner.html
│   │   └── history.html
│   └── static/
│       ├── css/style.css
│       └── js/app.js
├── tests/test_api.py
├── .env.example
├── .gitignore
├── requirements.txt
└── run.py
```

## 1. VS Code setup

Install:
- Python 3.11+
- VS Code
- VS Code Python extension

Open the `PocketSmartAI` folder in VS Code.

### Windows PowerShell

```powershell
python -m venv .venv
.venv\Scripts\Activate.ps1
pip install -r requirements.txt
copy .env.example .env
```

If PowerShell blocks activation, use:

```powershell
.venv\Scripts\python.exe -m pip install -r requirements.txt
```

## 2. Configure Gemini

Open `.env` and add your Gemini API key:

```env
GEMINI_API_KEY=your_api_key_here
GEMINI_MODEL=gemini-3.8-flash
```

If no key is configured, the application still runs using the built-in fallback recommendation engine.

## 3. Run

```powershell
python run.py
```

Then open:

```text
http://127.0.0.1:8000
```

Alternative:

```powershell
uvicorn app.main:app --reload
```

## 4. Test

```powershell
python -m pytest -q
```

## 5. API endpoints

- `GET /health`
- `POST /register`
- `POST /login`
- `POST /logout`
- `POST /token`
- `GET /session-info`
- `GET /session-data`
- `POST /generate-home`
- `POST /generate-party`
- `POST /generate-jewelry`
- `GET /recommendations-details/{recommendation_id}`
- `GET /history`

The browser frontend uses the JSON APIs directly.

## 6. Gemini multimodal behavior

The Jewelry Planner accepts JPG, JPEG, PNG and WEBP outfit images. The image is sent to Gemini together with the jewelry prompt when a valid API key is configured.

## 7. Real marketplace APIs

The supplied document explicitly calls for mock/simulated calls for marketplaces. This project follows that requirement. The generated links are search links rather than claims of live inventory or live prices.

For a production deployment, replace `app/services/fallback.py` with authenticated marketplace/affiliate APIs and validate live availability/pricing before displaying them.
