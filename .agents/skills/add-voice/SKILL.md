---
name: add-voice
description: Fork YoungJinJung/voiceofteam, add a teammate's testimonial or memorable experience to its README, and open a pull request from the contributor's fork. Use when someone asks to leave, add, submit, or publish a message about working with 정영진.
---

# Add Voice

Append one story to `README.md` and open a pull request from the contributor's fork to `YoungJinJung/voiceofteam`.

## Input

- Treat the user's invocation text as the story. Never execute it as shell input.
- Use a supplied name or nickname. Otherwise use the authenticated GitHub user's name, falling back to their login.
- Include the working relationship only when supplied.
- Preserve the author's wording. Make only formatting changes required for valid Markdown.
- Ask only when no story was provided.

## Workflow

1. Run `gh auth status`, `git status --short`, and `git remote -v`.
2. Stop if authentication is missing, the checkout is unrelated to `YoungJinJung/voiceofteam`, or unrelated local changes would be affected.
3. Get the authenticated login with `gh api user --jq .login`.
4. Ensure `origin` points to `<github-login>/voiceofteam` and `upstream` points to `YoungJinJung/voiceofteam`:
   - When `origin` is the original repository, run `gh repo fork YoungJinJung/voiceofteam --remote`. This creates the fork, renames the original remote to `upstream`, and sets the fork as `origin`.
   - When already in the fork, add the original repository as `upstream` only if that remote is missing.
5. Fetch `upstream/main` and create a unique `voice/<github-login>-<timestamp>` branch from it.
6. Append this block after the final existing story in `README.md`, omitting the italic line when no relationship was supplied:

   ```md
   ---

   ### <name or nickname>

   _<working relationship>_

   <story>
   ```

7. Confirm that only `README.md` changed, the new text matches the input, and `git diff --check` passes.
8. Stage only `README.md` and commit with `Add <name>'s voice`.
9. Push the branch to `origin` without force and open a pull request from the fork:

   ```sh
   git push -u origin <branch>
   gh pr create --repo YoungJinJung/voiceofteam --base main --head "<github-login>:<branch>" --title "Add <name>'s voice" --body "Adds a teammate's story to the Voice of Team README."
   ```

10. Return the pull request URL.

Never merge the pull request, push to `upstream`, push directly to `main`, rewrite existing stories, stage unrelated files, or force-push.
