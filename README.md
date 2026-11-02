# Resume-ATS-Score-Analyzer# AI ATS Resume Checker

Upload a resume (PDF, DOCX or TXT), optionally paste a job description, and get an
**ATS score out of 100**, a score breakdown, keyword coverage and specific improvements,
powered by **Google Gemini Flash** with a **Streamlit** interface.

## Features

- Reads PDF, DOCX and TXT resumes (including DOCX headers and tables)
- Five weighted category scores combined into one overall score
- Prioritised improvements (High / Medium / Low) and example bullet rewrites
- Keyword match against a job description (or the role the resume targets)
- Quick automated checks: contact info, sections, length, bullets
- Downloadable Markdown report

## How the score works

Gemini rates five categories from 0 to 100. The app then calculates the overall score itself,
so the weighting is always the same:

| Category | Weight |
|---|---|
| Keywords & relevance | 30% |
| Experience & impact | 25% |
| Formatting & ATS-readability | 20% |
| Skills | 15% |
| Clarity & language | 10% |

This is an AI estimate, not the output of any employer's real ATS. Treat it as guidance.

## Run locally

1. Install Python 3.10 or newer.
2. Get a free Gemini API key at https://aistudio.google.com/apikey
3. In the project folder:

```bash
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate
pip install -r requirements.txt
streamlit run app.py
```

4. Open http://localhost:8501, paste your API key in the sidebar, upload a resume and click **Analyze resume**.

To avoid pasting the key every time, create `.streamlit/secrets.toml` (never commit this file):

```toml
GEMINI_API_KEY = "your-key-here"
```

## Models

The sidebar offers `gemini-3.8-flash` (default) and `gemini-3.6-flash`. If one is not available for
your key, the app tries the other. Google renames and retires models over time; if both stop
working, update the `MODELS` list at the top of `app.py` using the current names at
https://ai.google.dev/gemini-api/docs/models

## Privacy

Resume text is sent to Google's Gemini API for analysis. The app does not store files or API keys.

## Project structure

```
app.py             # the whole app
requirements.txt   # dependencies
README.md
.gitignore         # keeps secrets and venv out of Git
```

## Deploy on Streamlit Community Cloud

1. Push this project to a **GitHub** repository (see below).
2. Go to https://share.streamlit.io and sign in with GitHub.
3. Click **Create app** (or **New app**) and choose your repository, the `main` branch and `app.py` as the main file.
4. Open **Advanced settings → Secrets** and add:
   ```toml
   GEMINI_API_KEY = "your-key-here"
   ```
5. Click **Deploy**. After a minute or two you get a public `*.streamlit.app` link.

> Note: if you add your key as a secret, everyone who opens your link uses *your* free quota.
> To make visitors bring their own key, skip step 4; the sidebar will then ask for a key.

## Push to GitHub

```bash
git init
git add .
git commit -m "Initial commit: AI ATS resume checker"
git branch -M main
git remote add origin https://github.com/YOUR-USERNAME/ats-resume-checker.git
git push -u origin main
```

Create the empty repository first on https://github.com/new (do not tick "Add a README").
If GitHub asks for a password, use a Personal Access Token (Settings → Developer settings) or sign in with GitHub CLI (`gh auth login`).

## Troubleshooting

- **"Very little text could be extracted"**: the PDF is probably a scan or image. Export a text-based PDF or use DOCX.
- **API key rejected**: copy the key again from AI Studio, with no spaces.
- **Rate limit / quota reached**: wait a minute; the free tier has limits.
- **Garbled order of text from a two-column PDF**: this is also what an ATS may see; a single-column layout scores better.
