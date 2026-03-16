# Reflection: Game Glitch Investigator

---

## 1. What was broken when you started?

When I first ran the game, it was immediately clear something was wrong. The hints were completely misleading — when I guessed too high, the game told me to go even higher, making it nearly impossible to converge on the right answer. The score also behaved strangely, sometimes going up after a wrong guess instead of down. On top of that, the difficulty settings were broken: Hard mode (1–50) was actually easier than Normal mode (1–100), which made the difficulty selector meaningless. Every other guess also seemed to give inconsistent hints, which turned out to be caused by the secret number being silently converted to a string on even-numbered attempts.

---

## 2. How did you use AI as a teammate?

I used Claude (by Anthropic) as my primary AI tool throughout this project. Claude helped me read through the code systematically and identify all six bugs, which I then verified manually by running the game and checking the Developer Debug Info panel.

**Correct suggestion:** Claude correctly identified that the hint messages in `check_guess` were swapped — "Too High" was returning "Go HIGHER!" and "Too Low" was returning "Go LOWER!", which is the opposite of what they should say. I verified this by opening the debug panel to see the secret number, then deliberately guessing above it and confirming the hint told me to go higher instead of lower.

**Incorrect/misleading suggestion:** Early in the process, Claude suggested the attempts counter starting at 1 instead of 0 was itself a bug that needed fixing throughout the codebase. On closer inspection, the counter starting at 1 was intentional — the real issue was just that the "New Game" button was resetting it to 0 inconsistently. Blindly following that suggestion would have broken the attempts logic further, so I narrowed the fix to just the new game reset block.

---

## 3. Debugging and testing your fixes

I decided a bug was truly fixed only when I could reproduce the original broken behavior, apply the fix, and then confirm the correct behavior in the live Streamlit app. For example, after fixing the inverted hints, I opened the debug panel, noted the secret number, guessed above it, and confirmed the game now correctly said "Go LOWER." For automated testing, I wrote a pytest case for `check_guess` that passed a guess of 60 against a secret of 50 and asserted the outcome was "Too High" with the message "Go LOWER." Before the fix this test would have failed because the message was wrong. Claude helped me structure the test by suggesting the input/output pairs to cover, though I wrote the actual assertions myself to make sure I understood what was being tested.

---

## 4. What did you learn about Streamlit and state?

Streamlit works differently from most frameworks because every time a user interacts with the app — clicking a button, typing in a field — the entire Python script reruns from top to bottom. This means any regular variable you define gets reset on every interaction. Session state (`st.session_state`) solves this by acting like a persistent memory that survives across reruns. Think of it like a notepad that Streamlit keeps on the table between rounds — without it, the app forgets everything the moment you click anything. This is why the "New Game" bug was so impactful: failing to reset `st.session_state.status` meant the game remembered a previous win or loss even after the player asked for a fresh start.

---

## 5. Looking ahead: your developer habits

One habit I want to carry forward is marking bug locations with `# FIXME` comments before touching any code. It forced me to fully understand the problem first rather than jumping straight into editing, which led to more targeted and confident fixes. In the future I would also ask the AI to explain its reasoning step by step before accepting a suggestion — on this project I caught the misleading attempts-counter suggestion because I read the surrounding code carefully, but a habit of asking "why does this fix work?" would make that verification more systematic. This project changed how I think about AI-generated code: I now treat it as a capable but overconfident first draft that always needs a human to read it critically, test it, and take ownership of the result.
