# FitBuddy – AI Fitness Plan Generator

FitBuddy is a FastAPI + Jinja2 + SQLite web application based on the supplied project documentation. It generates personalized 7-day workout plans, nutrition/recovery tips, and feedback-based plan updates using Google Gemini.

## Features

- User form: name, user ID, age, weight, goal, intensity
- AI-generated 7-day workout plan
- AI-generated nutrition/recovery tip
- Feedback-based plan regeneration
- SQLite + SQLAlchemy persistence
- Admin dashboard
- JSON API plus browser/Jinja2 interface
- Automatic local demo fallback when Gemini is unavailable
- FastAPI `/docs` and `/redoc`
- Input validation and error handling

## Implementation note

The supplied documentation uses the older `google-generativeai` SDK and Gemini 1.5 Pro/Flash. This implementation uses Google's current `google-genai` SDK. Model names are configurable in `.env`; the defaults preserve the documentation's Pro/Flash split with stable Gemini 2.5 model IDs.

If your API key has quota/access restrictions, set `DEMO_MODE=true` to test the complete application without calling Gemini.

## Structure

```text
FitBuddy/
├── app/
│   ├── __init__.py
│   ├── main.py
│   ├── config.py
│   ├── database.py
│   ├── models.py
│   ├── schemas.py
│   ├── gemini_client.py
│   ├── gemini_generator.py
│   ├── gemini_flash_generator.py
│   ├── updated_plan.py
│   ├── nutrition.py
│   └── routes.py
├── templates/
│   ├── base.html
│   ├── index.html
│   ├── result.html
│   ├── feedback_result.html
│   ├── admin_login.html
│   └── all_users.html
├── static/
│   ├── css/style.css
│   ├── js/app.js
│   └── images/README.txt
├── tests/
│   └── test_app.py
├── .env.example
├── .gitignore
├── requirements.txt
└── README.md
```

## Windows / VS Code

```powershell
cd FitBuddy
py -3.10 -m venv .venv
.venv\Scriptsctivate
python -m pip install --upgrade pip
pip install -r requirements.txt
copy .env.example .env
```

Put your Gemini key in `.env`:

```env
GEMINI_API_KEY=your_key_here
DEMO_MODE=false
ADMIN_KEY=fitbuddy-admin
```

For offline/demo testing:

```env
DEMO_MODE=true
```

## Run

```powershell
python -m uvicorn app.main:app --reload
```

Open:

- http://127.0.0.1:8000
- http://127.0.0.1:8000/docs
- http://127.0.0.1:8000/redoc
- http://127.0.0.1:8000/view-all-users

The admin page asks for the `ADMIN_KEY` configured in `.env`.

## Test

```powershell
python -m unittest discover -s tests -v
```

Tests use demo mode and do not require a Gemini key.

## API examples

`POST /api/generate-workout`

```json
{
  "user_id": "U001",
  "name": "Alex",
  "age": 24,
  "weight": 70,
  "goal": "muscle gain",
  "intensity": "medium"
}
```

`POST /api/submit-feedback`

```json
{
  "user_id": "U001",
  "feedback": "Add more cardio and one extra rest day."
}
```

`GET /api/nutrition-tip?goal=weight%20loss`

## Fitness safety

FitBuddy is a general wellness/planning application, not a medical diagnosis or treatment system. Users with injuries, medical conditions, pregnancy, or other special circumstances should consult a qualified professional before following a new exercise or nutrition program.
