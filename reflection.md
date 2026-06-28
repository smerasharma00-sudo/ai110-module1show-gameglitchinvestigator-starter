# 💭 Reflection: Game Glitch Investigator

Answer each question in 3 to 5 sentences. Be specific and honest about what actually happened while you worked. This is about your process, not trying to sound perfect.

## 1. What was broken when you started?

The first time I ran the game, it looked like a working number guessing game with a 
difficulty selector, a score, and a text input. But after playing a few rounds, the 
hints were clearly wrong — guessing a number that was too high would sometimes say 
"Too Low", and the score went UP when I made a wrong guess on certain attempts.

**Bug 1:** On every even-numbered attempt (2nd, 4th, 6th guess), the game converts 
the secret number to a string before comparing. String comparison uses alphabetical 
order, not numerical — so "60" vs "9" says 60 is LOWER than 9 because "6" comes 
before "9" alphabetically. This gives completely wrong hints every other guess.

**Bug 2:** The score rewards you for guessing Too High on even attempts. The 
update_score() function adds +5 points when outcome is "Too High" and the attempt 
number is even. Wrong guesses should never reward points.

**Bug 3:** Hard mode uses the range 1–50, which is actually easier than Normal mode's 
1–100. Hard should have a larger range to make it harder, not smaller.

### Bug Reproduction Log

| Input | Expected Behavior | Actual Behavior | Console Output / Error |
|-------|-------------------|-----------------|------------------------|
| Guess 60, secret is 50, attempt #2 | "Too High" hint shown | "Too Low" hint shown | none |
| Guess 70, secret is 30, attempt #4 | Score decreases (wrong guess) | Score increases by +5 | none |
| Switch to Hard difficulty | Harder range than Normal (e.g. 1–200) | Easier range of 1–50 shown | none |



## 2. How did you use AI as a teammate?

- Which AI tools did you use on this project (for example: ChatGPT, Gemini, Copilot)?
- Give one example of an AI suggestion that was correct (including what the AI suggested and how you verified the result).
- Give one example of an AI suggestion that was incorrect or misleading (including what the AI suggested and how you verified the result).

---

## 3. Debugging and testing your fixes

- How did you decide whether a bug was really fixed?
- Describe at least one test you ran (manual or using pytest)  
  and what it showed you about your code.
- Did AI help you design or understand any tests? How?

---

## 4. What did you learn about Streamlit and state?

Streamlit reruns the entire Python script from top to bottom every single time 
the user interacts with anything — clicking a button, typing in a box, anything. 
This means normal variables reset to their default values on every rerun. 
Session state (st.session_state) is like a backpack the app carries between 
reruns — anything stored in it survives. So the secret number, score, and 
attempt count all live in session state, otherwise they would reset to zero 
every time you clicked Submit.

## 5. Looking ahead: your developer habits

One habit I want to reuse is adding FIXME comments before touching any code — 
it forces me to think about exactly where the problem is before jumping to a fix. 
Next time I work with AI on a coding task, I would verify every suggestion by 
running the actual tests instead of just reading the code and assuming it's right. 
This project changed how I think about AI-generated code — it can look completely 
correct on the surface while hiding logic bugs that only show up when you actually 
run it with specific inputs.

## 2. How did you use AI as a teammate?

I used Claude as my AI coding assistant on this project.

**Correct suggestion:** Claude correctly identified that the hint bug was caused by 
the secret being converted to a string on even-numbered attempts, making comparisons 
alphabetical instead of numerical. I verified this by guessing 60 when the secret was 
50 and confirming the hint flipped from wrong to correct after the fix.

**Incorrect/misleading suggestion:** Claude initially gave me the wrong emoji messages 
in check_guess (swapped "Go HIGHER" and "Go LOWER"), which caused the pytest tests to 
fail. I caught this by reading the error output carefully and comparing what the tests 
expected versus what the function returned.

## 3. Debugging and testing your fixes

I knew a bug was fixed when the pytest tests passed AND the live game showed correct 
behavior. For example, after fixing check_guess, I ran python3 -m pytest and saw 
5 passed, then verified in the browser that guessing too high showed "Too High".

Claude helped me write the pytest tests, including test_wrong_guess_loses_points and 
test_hard_mode_harder_than_normal. Running pytest showed me exactly which functions 
still had bugs before I ran the full game.
