# TDS GA1 Q6 — Configuration Precedence API

A small FastAPI application that merges, in order:

1. hard-coded defaults
2. `config.development.yaml`
3. `.env`
4. OS environment variables beginning with `APP_`
5. repeated `?set=key=value` query parameters

It supports the `NUM_WORKERS` alias in the `.env` layer, type coercion for `port`, `workers`, and `debug`, masks `api_key`, and enables CORS for browser-based grading.

## Run locally (PowerShell)

```powershell
py -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
$env:APP_WORKERS = "12"
$env:APP_DEBUG = "true"
$env:APP_LOG_LEVEL = "warning"
uvicorn main:app --reload --host 127.0.0.1 --port 8000
```

Then open:

- `http://127.0.0.1:8000/effective-config`
- `http://127.0.0.1:8000/effective-config?set=port=9000&set=debug=false`

## Deploy on Render

- Build command: `pip install -r requirements.txt`
- Start command: `uvicorn main:app --host 0.0.0.0 --port $PORT`
- Add these OS environment variables in the Render service dashboard:
  - `APP_WORKERS` = `12`
  - `APP_DEBUG` = `true`
  - `APP_LOG_LEVEL` = `warning`

The included `.env` has the exercise's assigned sample values. Keep `.env` out of GitHub: `.gitignore` excludes it. After creating your Render service, open **Environment → Secret Files → Add Secret File**, set filename to `.env`, and paste the contents of your local `.env` file there. Render makes non-Docker secret files available in the service root at runtime. These are exercise-provided sample values, not production credentials. Never commit real secrets to a public repository.
