# Contributing to graphics-compiler

Thanks for your interest in improving this skill! Contributions are welcome — whether it's fixing a bug, improving documentation, or adding new graphic types.

---

## What you can contribute

- **Bug fixes** — incorrect layout logic, broken renderer output, or wrong token resolution
- **New brand profiles** — add a new named profile to `references/brand-profiles.md`
- **New graphic types** — extend the 13 supported types in `references/graphic-types.md`
- **Renderer improvements** — better SVG templates, Mermaid patterns, or Python chart styles in `references/renderer-templates.md`
- **Documentation** — clearer explanations, better examples, or additional prompts in `SKILL.md`

---

## How to contribute

1. **Fork** the repository
2. **Create a branch** for your change
   ```
   git checkout -b improve/brand-profile-dark-mode
   ```
3. **Make your changes** — keep edits focused and minimal
4. **Test your changes** — paste the updated skill into Claude and verify the output looks correct
5. **Commit** with a clear message
   ```
   git commit -m "Add dark-mode brand profile to brand-profiles.md"
   ```
6. **Open a Pull Request** — describe what you changed and why

---

## Guidelines

- Keep changes consistent with the existing 3-phase pipeline structure
- Do not add rendering logic to Phase 1 or Phase 2
- Do not add layout or style decisions to Phase 3
- Use the existing token vocabulary (`--accent`, `--neutral`, etc.) — do not introduce new tokens without discussion
- Keep labels concise — `max_words_per_label` is a core constraint of this skill

---

## Questions?

Open an [issue](../../issues) and tag it as a question.
