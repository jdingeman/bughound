# 🐶 BugHound

BugHound is a small, agent-style debugging tool. It analyzes a Python code snippet, proposes a fix, and runs basic reliability checks before deciding whether the fix is safe to apply automatically.

---

## What BugHound Does

Given a short Python snippet, BugHound:

1. **Analyzes** the code for potential issues

   - Uses heuristics in offline mode
   - Uses Gemini when API access is enabled

2. **Proposes a fix**

   - Either heuristic-based or LLM-generated
   - Attempts minimal, behavior-preserving changes

3. **Assesses risk**

   - Scores the fix
   - Flags high-risk changes
   - Decides whether the fix should be auto-applied or reviewed by a human

4. **Shows its work**
   - Displays detected issues
   - Shows a diff between original and fixed code
   - Logs each agent step

---

## Setup

### 1. Create a virtual environment (recommended)

```bash
python -m venv .venv
source .venv/bin/activate   # macOS/Linux
# or
.venv\Scripts\activate      # Windows
```

### 2. Install dependencies

```bash
pip install -r requirements.txt
```

---

## Running in Offline (Heuristic) Mode

No API key required.

```bash
streamlit run bughound_app.py
```

In the sidebar, select:

- **Model mode:** Heuristic only (no API)

This mode uses simple pattern-based rules and is useful for testing the workflow without network access.

---

## Running with Gemini

### 1. Set up your API key

Copy the example file:

```bash
cp .env.example .env
```

Edit `.env` and add your Gemini API key:

```text
GEMINI_API_KEY=your_real_key_here
```

### 2. Run the app

```bash
streamlit run bughound_app.py
```

In the sidebar, select:

- **Model mode:** Gemini (requires API key)
- Choose a Gemini model and temperature

BugHound will now use Gemini for analysis and fix generation, while still applying local reliability checks.

---

## Running Tests

Tests focus on **reliability logic** and **agent behavior**, not the UI.

```bash
pytest
```

You should see tests covering:

- Risk scoring and guardrails
- Heuristic fallbacks when LLM output is invalid
- End-to-end agent workflow shape

## TF Submission: Justin Dingeman

1. The core concepts the students needed to understand
> Students should understand the difference between using a heuristic, or otherwise somewhat "hard-coded" model vs using LLM output as the analyzer and fixer for buggy code. They should understand how the LLM's output should be structured, and how it affects the correctness/useability of the application if the LLM doesn't follow instructions.

2. Where students are most likely to struggle
> Students might get confused when copying and pasting the code and running the application in offline mode. The questions in Part 1 don't really make sense for using in Heuristic mode, particularly the last question: "What happens when the agent produces a result that feels incomplete or questionable?" This is a really strange question. What exactly does "What happens..." mean? Nothing happens, particularly in Heuristic mode. The code just runs and finishes. They are likely to struggle in determining how the LLM is useful because it very well could just produce no useable output in their cases if the code is corrected, so it'd always fallback to heuristics mode.

3. Where AI was helpful vs misleading
> The AI is helpful in identifying bugs in the existing code. It is misleading in that sometimes, I'd need to go through a problem multiple times before it actually identifies the issue.

4. One way they would guide a student without giving the answer
> This project could end up turning into a pretty open book of issues. I think that if they are struggling with something, my first instinct would be to have them follow the code to see if they understand where things are going wrong. Once they identify the spot, I'll ask them if they can see why it might not be working, and if they can think of any way to get the code to work.

_Submission Note_: I was not able to fully complete the Tinker within the 2 hours. I am submitting as far as I got, with some additions to the debug code.
