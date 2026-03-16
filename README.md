# 🎮 Game Glitch Investigator: The Impossible Guesser

## 🚨 The Situation
You asked an AI to build a simple "Number Guessing Game" using Streamlit. It wrote the code, ran away, and now the game is unplayable.

- You can't win.
- The hints lie to you.
- The score goes haywire.
- The difficulty settings make no sense.

---

## 🎯 Game Purpose
A number guessing game built with Python and Streamlit. The player selects a difficulty level and tries to guess a randomly generated secret number within a limited number of attempts. Hints guide the player higher or lower after each guess, and a score is tracked throughout.

---

## 🛠️ Setup
1. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```
2. Run the app:
   ```bash
   python -m streamlit run app.py
   ```

---

## 🐛 Bugs Found

| # | Bug | Location | Effect |
|---|-----|----------|--------|
| 1 | Hint messages were inverted | `check_guess` in `app.py` | "Too High" told you to go higher, "Too Low" told you to go lower |
| 2 | Hard difficulty had a smaller range than Easy | `get_range_for_difficulty` | Hard (1–50) was easier than Normal (1–100) |
| 3 | Secret number was cast to a string on every even attempt | `app.py` submit block | Hints broke every other guess due to string vs integer comparison |
| 4 | Score increased on wrong guesses | `update_score` | Every other wrong guess added 5 points instead of deducting |
| 5 | Range in UI was hardcoded to "1 and 100" | `app.py` info banner | Displayed wrong range regardless of difficulty |
| 6 | New Game button didn't fully reset state | `app.py` new_game block | Status, score, and history were not cleared on reset |

---

## ✅ Fixes Applied

1. **Inverted hints** — Swapped the return messages in `check_guess` so "Too High" says "Go LOWER" and "Too Low" says "Go HIGHER"
2. **Hard difficulty range** — Changed Hard range from `1–50` to `1–200` so difficulty levels are meaningful
3. **String secret bug** — Removed the conditional `str()` cast so the secret is always passed as an integer to `check_guess`
4. **Score bug** — Unified `Too High` and `Too Low` outcomes to both deduct 5 points, removing the incorrect `+5` branch
5. **Hardcoded range** — Replaced `"1 and 100"` in the info banner with dynamic `{low}` and `{high}` variables
6. **New game reset** — Updated the new game block to reset all 5 session state variables: `attempts`, `secret`, `score`, `status`, and `history`

---

## 📸 Demo
> _Insert a screenshot of your fixed, winning game here_

---

## 📝 Reflection
> _See `reflection.md` for a full write-up of the debugging process and AI collaboration notes_

---

## 🚀 Stretch Features
> _If you completed Challenge 4, insert a screenshot of your Enhanced Game UI here_
