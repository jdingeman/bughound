## Part 1 (Heuristic mode)

### Running cleanish.py

- BugHound determines there are no issues
- No changes
- No changes
- No changes

### Running flaky_try_except.py

- BugHound determines a bare `except` exists because it matches a regex where `except` does not have anything after it (heuristic mode)
- `bughound_agent.py` uses this regex to decide that the change should include catching a specific exception or use `except Exception as e`
- It decides whether the change is safe during the Reflect phase immediately after scoring the risk in the Test phase, where it determined that the change needs human review instead of automatically implementing it
- Suggested change doesn't feel incomplete.

### Running mixed_issues.py

- BugHound determines that there are print statements instead of logging statements, that there is a bare `except` and that there are TODO comments.
- `bughound_agent.py` uses the `except` regex to find the bare `except` and determines that it should include catching a specific exception or use `except Exception as e`. It uses a string "`print(`" to check if it exists in the code, and determines that there should be logging statements instead for non-toy code. And it does the same for the TODO statements. The heuristic model only checks for these very specific cases, and will not use context to determine whether the change it suggests is actually meaningful to the issue
- It decides whether the change is safe during the Test and Reflect phases, where it scored an overall risk of 0, which is high. It recommends human review for such a case.
- Suggested change is too simple for this example, so it doesn't provide a complete analysis in heuristic mode.

### Running print_spam.py

- BugHound determines that there are print statements where there should be logging statements.
- The agent just checks whether print is used in the code at all, and it recommends using logging statements instead, even if the intention of the code is to print the statements.
- It decides whether the change is safe during the Test and Reflect phases where it determined that it had a medium risk level with a score of 45.
- Suggested change only says use logging statements, it doesn't say where (heuristic mode).

## Part 2 (LLM mode)

### Running mixed_issues.py

- The issues detected were the same as the heuristic mode
- This code caused a fall back to the heuristics during the Analyze and Act phases, so the LLM didn't provide meaningful data for the model.

### Running print_spam.py

- The same issues were detected
- The code used fall backs because the LLM did not provide meaningful data for the model during Analyze and Act phases.
