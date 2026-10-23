# Thoughts

**Turn any long AI chat into a clean, reusable workflow.**

Thoughts is a colorful visual board that takes a long AI conversation and turns it into a structured, at-a-glance workspace: what was discussed, what mattered, how much to trust it, and what to do next.

> **Status:** Early development. Nothing below is built yet unless marked otherwise. See the [Roadmap](#roadmap).

---

## The Problem

Long AI conversations are hard to use after the fact:

- Important ideas get buried in walls of text.
- It's rarely clear *why* the AI said what it said.
- It's hard to tell well-supported claims from guesses.
- Re-finding an earlier chat means scrolling through endless messages.

## The Solution

Paste in a conversation and Thoughts produces a board built around three goals.

### 1. Transparency: the "Why" and the "Where"

For every AI response, the board shows:

- **Why** the AI produced that type of information.
- **Where** it likely came from (for example, general knowledge, a cited source, or a guess).
- **An estimated confidence score from 1 to 100%**, so users can see at a glance how much to rely on it.

> **Note:** Confidence is *estimated* by analyzing the text of the response (hedging, specificity, citations). It is not a measurement of the original model's internal certainty, and the UI labels it that way.

### 2. Organization: Templates, Priorities, and Key Phrases

- **Templates:** switch between layouts such as *Project Plan*, *Study Guide*, and *Decision Matrix*, and the board rearranges the conversation's content to fit.
- **Priority buckets:** sort items into **Highly Important**, **Moderate**, and **Do It Later**.
- **Key phrases:** detect repeated terms across the chat (for example, a term like "Newseries" that keeps coming up) and highlight the recurring themes.

### 3. Navigation: Moving Between Conversations

- Save each analyzed conversation.
- Switch between past conversations quickly and pick up where you left off.
- Reuse the structure you built, so each chat becomes a building block instead of a one-off exchange.

---

## Planned Tech Stack

| Layer | Choice | Why |
|---|---|---|
| Interface | [Streamlit](https://streamlit.io/) | Python-only, quick to build colorful cards, tabs, and sidebars |
| AI analysis | **To be decided** (depends on what our club has access to; see below) | One function sends the chat to a model and returns structured JSON |
| Storage | SQLite | Built into Python, no setup, enough for saved conversations |
| Key phrases | Plain Python | Counting repeated words doesn't need AI |

### How it works

1. The user pastes a conversation.
2. The app splits it into turns (`User:` / `Assistant:`).
3. One AI call analyzes the chat and returns **structured JSON**, one entry per AI response.
4. The UI displays that JSON as cards, columns, and badges. Templates, priorities, and confidence badges are just different views of the same data.

Example of the data contract between the analysis engine and the UI:

```json
{
  "responses": [
    {
      "summary": "...",
      "why_this_response": "Answered a factual question",
      "likely_basis": "General knowledge, no cited source",
      "confidence": 72,
      "confidence_reason": "States specifics but cites nothing",
      "priority": "Highly Important",
      "action_items": ["..."]
    }
  ]
}
```

### AI provider

Our club switched from Claude to Microsoft, and the exact access is still being confirmed (for example Azure OpenAI or Microsoft Foundry). The AI call is kept in one file (`analysis.py`) so the provider can be swapped without touching the rest of the app. Until access is confirmed, the UI is built against sample JSON.

---

## Planned Project Structure

```
Thoughts/
├── app.py              # Streamlit entry point
├── analysis.py         # AI call + JSON parsing
├── keyphrases.py       # Repetition / key-phrase detection
├── storage.py          # SQLite: save and load conversations
├── templates.py        # Project plan / study guide / decision matrix layouts
├── requirements.txt
├── .env.example        # Shows required settings (no real secrets)
└── docs/
    └── sample_chat.txt # Sample conversation for testing
```

The current repo also contains empty `frontend/` and `backend/` folders from the initial setup. These can be removed once the Streamlit structure is in place.

---

## Getting Started

> Setup steps will be filled in as the app is built.

```bash
# 1. Clone the repo
gh repo clone UnitedAI-Projects/Thoughts
cd Thoughts

# 2. Install dependencies (once requirements.txt exists)
pip install -r requirements.txt

# 3. Add your own settings
cp .env.example .env
# then edit .env with your key

# 4. Run the app (once app.py exists)
streamlit run app.py
```

### Keep secrets out of GitHub

API keys must **never** be committed, even in a private repo. Keys live in a local `.env` file, which is already listed in `.gitignore`. Each teammate creates their own `.env`. Share `.env.example` (with fake values) so others know what to fill in.

---

## Roadmap

Planned for roughly two weeks.

- [x] Create repo and initial structure
- [ ] **Days 1-2:** Environment setup, sample chat, confirm AI access
- [ ] **Days 3-5:** Analysis engine (chat in, validated JSON out)
- [ ] **Days 5-8:** Board UI: cards, confidence badges, priority columns, templates, key phrases
- [ ] **Days 8-10:** Navigation: save conversations to SQLite, sidebar to switch between chats
- [ ] **Days 11-14:** Polish, error handling, demo preparation

**Later / stretch goals**

- Upload exported chat files (input method still to be decided)
- Additional templates
- Hosting the app (for example Streamlit Community Cloud)

---

## Team Roles

| Area | Owner |
|---|---|
| Analysis engine and AI prompt | TBD |
| Board UI and templates | TBD |
| Storage, navigation, and key phrases | TBD |

Everyone agrees on the JSON format above first, since it's the contract between the analysis engine and the UI.

---

## Workflow for Teammates

```bash
git pull                       # get the latest changes before starting
# ...make your changes...
git add .
git commit -m "Describe what you did"
git push
```

---

## License

To be decided.
