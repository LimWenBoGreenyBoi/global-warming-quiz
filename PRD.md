# Product Requirements Document (PRD)

## 1. Summary
- Project: Global Warming Quiz + Q&A Board
- Purpose: Educational quiz with a community board, designed to feel fresh via randomized questions from a local JSON API.
- Current State: Functional quiz UI, site-wide styling, background image overlay, question map, JSON API, and Streamlit Q&A Board.

## 2. Program Description
- Frontend Pages:
  - `index.html` — Main menu; links to quiz and Q&A Board.
  - `global-warming-quiz.html` — Quiz UI with info/how-to/quiz/result screens, and a live-updating question map.
  - `quiz.css` — Site-wide styles, background image overlay via `body::before` at 30% opacity, 'Inter' for body, 'Playfair Display' for headings.
- Backend Services:
  - `app.py` (Flask) — Serves static pages and provides `GET /api/quiz/global-warming` randomized question API.
  - `streamlit_app.py` — Local Q&A Board storing threads/replies in `board.json` (auto-created). Accessible at `http://localhost:8501/`.
- Data:
  - `data/global_warming.json` — Canonical question + explanation pool.
- Local URLs:
  - App + API: `http://localhost:8000/`
  - Quiz: `http://localhost:8000/global-warming-quiz.html`
  - API: `http://localhost:8000/api/quiz/global-warming?count=10`
  - Q&A Board: `http://localhost:8501/`

## 3. Goals
- Deliver an engaging, educational experience about climate change.
- Keep runs fresh and unrepetitive via randomized question selection and shuffled options.
- Provide a simple local “Board” for questions and discussion.

## 4. Users & Use Cases
- Learners exploring climate topics; quick self-assessment.
- Teachers/demo presenters running a lightweight local quiz.
- Community members posting questions and replies on the Board.

## 5. Feature Overview
- Quiz:
  - Info/How-To/Quiz/Result screens.
  - Multiple-choice questions with correct answer validation.
  - Result view with score, time, and per-question explanations.
  - Question Map showing current and completed items.
  - Randomized question set and options via local API; fallback to embedded set.
- Board:
  - Post questions (threads) and add replies.
  - Stored locally in `board.json` for shared visibility on the host machine.
- Styling:
  - Background image overlay (30% opacity) using `background.jpg` if present.
  - Typography: 'Inter' for body, 'Playfair Display' for headings, font smoothing enabled.

## 6. API Specification
- Endpoint: `GET /api/quiz/global-warming`
  - Params:
    - `count` — number of questions to return (1–16; capped by pool size).
    - `seed` — string seed for deterministic shuffles (default: daily `YYYYMMDD`).
  - Response:
    ```json
    {
      "seed": "20251214",
      "count": 10,
      "questions": [
        {
          "q": "…",
          "options": ["…", "…", "…", "…"],
          "answer": 2,
          "explanation": "…"
        }
      ]
    }
    ```
  - Behavior:
    - Shuffles question order and per-question options while keeping the correct index accurate.
    - Deterministic per `seed` for reproducible sessions.

## 7. UX Flows
- Start Quiz:
  - Click “Start” → `beginQuiz()` → fetch API (10 questions, daily seed) → render question and map.
- Answer Navigation:
  - Choose option → Next or Submit enabled appropriately.
  - Map highlights current; completed items marked.
- Submit:
  - Shows score, time, and per-question breakdown with explanations.
- Board:
  - Open at `http://localhost:8501/` → create thread → add replies → stored to `board.json`.

## 8. Content Guidelines
- Keep questions factual, concise, and grounded in reputable sources (UN, NASA, IPCC, NRDC).
- Ensure options are plausible; avoid trick questions.
- Pair each item with a one-sentence explanation.

## 9. Success Metrics
- Engagement: number of questions completed per session.
- Completion rate: percent of users reaching results.
- Quiz freshness: low repetition across sessions (unique sets via API).

## 10. Risks & Constraints
- Local-only storage; content resets when files change or move.
- No authentication; Board is open to anyone who can access the local server.
- The development Flask server is not suitable for production deployment.

## 11. Roadmap
- Near Term:
  - Add categories/difficulty filters to API (e.g., `?topic=ocean&difficulty=easy`).
  - Clickable question map to jump to items.
  - Session seeds per user for tailored runs.
- Mid Term:
  - Expand question pool; tag metadata (topic, difficulty, source).
  - Board enhancements: search, moderation, and pinned threads.
  - Optional CSV/JSON export of quiz results.

## 12. Open Questions (for you to fill in)  
- Target audience specifics (age group, prior knowledge).
- Desired session length (question count default).
- Any accessibility requirements (keyboard-only, ARIA labels, color contrast).
- Preferred topics and difficulty ramp.
- Hosting plan beyond local dev.

## 13. Setup & Run
- Install deps: `pip install -r requirements.txt`
- Start API + site: `python app.py` → `http://localhost:8000/`
- Start Board: `streamlit run streamlit_app.py --server.port 8501`
- Background image: place `background.jpg` in project root.
- Hard refresh if assets look stale (Ctrl+Shift+R).

