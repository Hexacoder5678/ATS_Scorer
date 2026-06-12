<img width="584" height="470" alt="Screenshot 2026-06-12 134914" src="https://github.com/user-attachments/assets/194b6e20-3fab-4410-88b0-b5c9c50ce80b" />
<img width="1126" height="585" alt="Screenshot 2026-06-12 134757" src="https://github.com/user-attachments/assets/148da212-e5b8-4984-af56-cea19d154a3c" />
<img width="1052" height="315" alt="Screenshot 2026-06-12 134726" src="https://github.com/user-attachments/assets/0832d567-9fdc-48c6-aa47-46a8ae4caf80" />
<img width="1363" height="551" alt="Screenshot 2026-06-12 134710" src="https://github.com/user-attachments/assets/095e8f0e-9713-4338-ac7a-8bfa1dc9c367" />
<img width="1357" height="516" alt="Screenshot 2026-06-12 134654" src="https://github.com/user-attachments/assets/22962c7c-efd7-46eb-84d2-5f5701510719" />
# ATS Resume Scorer

A web app that scores how well a resume matches a job description and returns actionable feedback. Built with FastAPI + Streamlit, using spaCy and Sentence Transformers for NLP and the Groq API for LLM-generated suggestions.

## What it does

1. Upload a resume (PDF / DOC / DOCX) and paste a job description.
2. The backend parses the resume, extracts skills and experience, and compares them to the JD using semantic similarity.
3. You get an ATS score, a breakdown by category (formatting, keywords, content, skill validation, ATS compatibility), and LLM-written suggestions for what to improve.
4. Past analyses are saved to your account so you can revisit them.

## Tech stack

- **Frontend:** Streamlit
- **Backend:** FastAPI (Python)
- **NLP:** spaCy (`en_core_web_md`), Sentence Transformers (`all-MiniLM-L6-v2`)
- **LLM:** Groq API (Llama 3)
- **Auth + Database:** Supabase (email/password and Google OAuth)
- **PDF report export:** WeasyPrint + Jinja2

## Project structure

```
ATS_SCORER/
├── backend/              FastAPI app, NLP services, API routes
├── frontend/             Streamlit app, views, components
├── jupyter notebooks/    Research and dataset prep (not used at runtime)
├── ml model/             Exported ML artifacts
├── requirements.txt      Combined backend + frontend dependencies
└── .env.example          Template for environment variables
```

## Setup

### 1. Clone and create a virtual environment

```bash
git clone <repo-url>
cd ATS_SCORER
python -m venv venv
source venv/bin/activate         # Windows: venv\Scripts\activate
```

### 2. Install dependencies

```bash
pip install -r requirements.txt
python -m spacy download en_core_web_md
```

WeasyPrint needs system libraries on Linux:

```bash
# Fedora
sudo dnf install -y cairo pango gdk-pixbuf2 libffi

# Debian / Ubuntu
sudo apt install -y libcairo2 libpango-1.0-0 libpangoft2-1.0-0 libffi-dev
```

### 3. Configure environment variables

Copy the template and fill in your keys:

```bash
cp .env.example .env
```

You need:

- A **Supabase** project — grab `SUPABASE_URL`, `SUPABASE_KEY` (service role), and `SUPABASE_ANON_KEY` from Project Settings → API.
- A **Groq** API key from [console.groq.com](https://console.groq.com).
- (Optional) Google OAuth set up in the Supabase dashboard if you want Google sign-in.

The Streamlit frontend also reads Supabase config from `frontend/.streamlit/secrets.toml`. Copy `secrets.toml.example` to `secrets.toml` and fill it in.

### 4. Run the backend

From the project root:

```bash
uvicorn backend.main:app --reload --host 0.0.0.0 --port 8000
```

The API is now at `http://localhost:8000`.

### 5. Run the frontend

In a new terminal (with the venv activated):

```bash
streamlit run frontend/streamlit_app.py
```

The app opens at `http://localhost:8501`.

