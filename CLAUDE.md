# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Running the game

```bash
open tictactoe.html   # macOS
```

No build step, no dependencies — it's a single self-contained HTML file.

## Git workflow

Every code change must be committed and pushed to GitHub:

```bash
git add tictactoe.html   # or CLAUDE.md if that changed too
git commit -m "<message>"
git push
```

The `.claude/` directory is local tooling state — do not commit it.

Remote: https://github.com/oscarhui-28/tic-tac-toe (branch: `main`)

## Architecture

Everything lives in `tictactoe.html` as a single file with three sections:

- **CSS** (`<style>`) — dark theme using three colors: red `#e94560` (X), teal `#a8dadc` (O), dark navy backgrounds.
- **HTML** — static 3×3 grid of `.cell` divs with `data-i` index attributes (0–8); scores and status bar above, reset button below.
- **JS** (`<script>`) — plain vanilla JS, no framework. Key pieces:
  - `board` — flat 9-element array (`null | 'X' | 'O'`), the single source of truth for game state.
  - `WINS` — hardcoded array of all 8 winning index triplets.
  - `checkWinner()` — iterates `WINS`; returns `{ winner, line }` on win, `{ winner: null }` on draw, or `null` if game continues.
  - `init()` — resets `board`, `current`, `over`, and clears DOM classes/text. Called on load and on "New Game".
  - Click handler — writes to `board`, updates cell classes (`x`/`o`/`taken`/`win`), and calls `checkWinner()` after each move.
  - `scores` object (`{ X, O, D }`) persists across games within the same page session (resets on page reload).
