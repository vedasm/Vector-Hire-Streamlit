# Vector Hire

A two-stage AI candidate ranking system built for the RedRob challenge, designed to rank candidates for a **Senior AI Engineer** role and deployed as a Streamlit app.

## 🔗 Live App

[Launch the app](https://vector-hire-app.streamlit.app) <!-- replace with your actual Streamlit/HF Spaces URL -->

## Target Role

This ranker is built specifically around the **Senior AI Engineer — Founding Team** job description (see `data/job_description.md`), covering candidate-job matching, search, retrieval, ranking, and recommendation systems expertise (5–9 YOE, embeddings/vector search, LLM fine-tuning, production ML systems).

## How It Works

**Stage 1 — Rule-based scoring**
Scores each candidate against the Senior AI Engineer profile using structured signals: relevant skills (retrieval, ranking, embeddings, vector search, LLM/fine-tuning), title/company fit, years of experience, location, and behavioral signals (response rate, notice period, GitHub activity, etc.). Honeypot/fraudulent profiles are filtered out first.

**Stage 2 — Semantic reranking**
The top 1,000 rule-scored candidates are re-ranked using `all-MiniLM-L6-v2` sentence embeddings, computing cosine similarity between each candidate profile and the Senior AI Engineer job description. Final score = `0.65 × rule_score + 0.35 × semantic_score`.

## Usage

1. Open the app.
2. Upload a `.jsonl` or `.json` candidates file.
3. View the ranked results table (top 100 candidates with reasoning).
4. Download `submission.csv`.

## Tech Stack

- **Frontend/Hosting:** Streamlit
- **Embeddings:** `sentence-transformers` (`all-MiniLM-L6-v2`)
- **Scoring:** NumPy, custom rule-based engine
- **Data I/O:** JSON/JSONL parsing, CSV export

## Project Structure

```
.
├── app.py                      # Streamlit UI
├── main.py                     # Core ranking logic (rule scoring + reranking) + CLI entry point
├── data/
│   └── job_description.md      # Senior AI Engineer JD used for semantic matching
├── models/
│   └── all-MiniLM-L6-v2/       # Local model cache (optional, falls back to HF Hub)
└── requirements.txt
```

## Running Locally

```bash
pip install -r requirements.txt
streamlit run app.py
```

## CLI Mode

To run the pipeline directly on a local `data/candidates.jsonl` file without the UI:

```bash
python main.py
```

This generates `submission.csv` in the project root.

## Output Format

| Column | Description |
|---|---|
| `candidate_id` | Unique candidate identifier |
| `rank` | Final rank (1–100) |
| `score` | Combined rule + semantic score, weighted toward Senior AI Engineer fit |
| `reasoning` | Human-readable explanation of the score |
