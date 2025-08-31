# 🧠 LazyVim – Advanced Programming Workflows

> Focus: search & replace, refactoring moves (copy/paste power), and system clipboard (macOS + Linux).  
> Assumes LazyVim defaults (`<leader>` = `SPACE`) and LSP enabled.

---

## ⚙️ System Clipboard (macOS + Linux)
- Check: `:set clipboard?` — Prefer `clipboard=unnamedplus` so `+` (system clipboard) is default.
- One‑off yank/paste with explicit registers:
  - `"+y` / `"+p` — use **system clipboard**
  - `"*y` / `"*p` — **primary selection** (Linux/X11). On macOS often same as `+`.
- Make system clipboard default (if not already):  
  `:set clipboard+=unnamedplus`
- Linux helpers (terminal Neovim):
  - Wayland: install `wl-clipboard` (`wl-copy/wl-paste`)
  - X11: install `xclip` or `xsel`

### Paste tricks (keep indentation tidy)
- `]p` / `[p` — paste with **indentation adjusted** to current line
- After paste, reindent selection: `gv=`
- Replace selection **without clobbering** default yank: `"_dP` (black‑hole delete, then paste)

---

## 🔎 In‑File Search & Replace (power patterns)
- Find next/prev: `/foo`, `n` / `N`
- Current line: `:s/foo/bar/g`
- Whole file: `:%s/foo/bar/g`
- Confirm each: append `c` → `:%s/foo/bar/gc`
- Visual range only: select text then `:s/foo/bar/g`
- Word boundaries: `\<foo\>` or `\bfoo\b`
- Keep case of replacement per match: `:%s/foo/\Ubar\E/g` (upper), `\L` (lower), `\u`/`\l` (first char)

### Regex captures (gold for refactors)
- `:%s/\vfunc\((.*)\)/call(\1)/g` — move args
- `:%s/\v(int|float) ([a-z_]+)/\2: \1/g` — reorder tokens
- Newlines in replacement: use `\r` → `:%s/,/\r/g`

---

## 🌍 Project‑Wide Search & Replace
### Telescope (fast lookup)
- **Files**: `<leader> f f` (find files)
- **Live grep**: `<leader> f g` (search text across project)
- **Buffers/Recent**: `<leader> ,` / `<leader> f r`
> Open match → `:copen` (if using quickfix), or jump via Telescope picker.

### Spectre (GUI‑like S&R across files)
- Open Spectre: `<leader> s r`  (or `:Spectre`)
- Populate from cursor word: `viw` then open Spectre; or use Spectre’s “replace current word”
- Toggle file/glob filters, preview diffs, then **Replace All**

### Quickfix‑driven (surgical control)
```vim
:vimgrep /pattern/j **/*.{ts,tsx,js}   " j = no jump
:copen                                " inspect hits
:cdo s/pattern/replacement/gc | update
```
- Use `:cnext` / `:cprev` to navigate; `:cdo` applies to each entry.

---

## 🧭 LSP Refactors (LazyVim defaults)
- Hover docs: `K`
- Go to definition / declaration: `gd` / `gD`
- References / implementation / type def: `gr` / `gi` / `gt`
- Rename symbol (project‑aware): `<leader> c r`
- Code actions (quick‑fix, organize imports): `<leader> c a`
- Format file/selection: `<leader> c f` (or `gq` for Vim format)
- Diagnostics: next/prev `]d` / `[d`, list (Trouble/Loclist) via `<leader> x x`
> If a keymap differs in your setup, you can always run the command:  
> `:lua vim.lsp.buf.rename()`, `:lua vim.lsp.buf.code_action()`

---

## ✂️ Text Objects & Surround (surgical edits)
- Change inside: `ciw` (word), `ci"` (quotes), `ci)` / `ci]` / `ci}`
- Delete around: `da"` / `da)` / `da{`
- Select around/inside: `vi)` / `va"` etc.
- **Surround** (LazyVim includes `mini.surround`):
  - Add: `ys{motion}{char}` (e.g., `ysiw'` → wrap *inner word* with `'`)
  - Change: `cs"'` (double → single quotes)
  - Delete: `ds"`
- **Comment** (Comment.nvim): `gcc` (line), `gc{motion}` (block)

---

## 🪄 Copy/Paste Refactor Recipes
### 1) Duplicate a code block and adapt
- Select block: `V` or `va}`
- Copy: `y`
- Move to target → `]p` to paste with indentation
- Jump to next placeholder with search: `/TODO` then `n` … or use multiple cursors (below)

### 2) Column edits / multiple cursors (visual‑block)
- Start block: `<C-v>` then move
- Insert at start of each line: `I` then text, `<Esc>` to apply to all
- Append at end: `A` then text, `<Esc>`
- Prepend `// ` to many lines quickly with `<C-v>`, select, `I// <Esc>`

### 3) Paste same snippet many places (without re‑yank)
- Yank once to named register: `"ay`
- Paste repeatedly: `"ap` (doesn’t change unnamed register)

### 4) Paste over many identifiers (keep yanked name)
- Yank the replacement once: `"ayiw`
- For each old name: `ciw` then `"ap` then `<Esc>`

---

## 🧩 Treesitter‑Aware Moves (structural)
- Swap parameters (many langs via TS): place cursor on arg, then:  
  `:TSNodeSwapPrevious` / `:TSNodeSwapNext` (map to keys if desired)
- Incremental selection: `v` then `:set opfunc=nvim_treesitter#...` (or use your LazyVim TS select mappings if enabled)
- Split/Join (if `treesj` or `splitjoin` in your stack):  
  Toggle args/objects one‑line ↔ multiline to diff and edit cleanly.

---

## 🗂️ Multi‑File Refactor Patterns
### A) Change import path across repo
```vim
:vimgrep /from '\.\/old\/path'/j **/*.{ts,tsx,js}
:copen
:cdo s#\./old/path#./new/path#g | update
```
### B) Rename a symbol safely
1. LSP rename on definition: `<leader> c r`
2. Grep to catch non‑symbol usages (strings/docs): `<leader> f g` then inspect

### C) Extract function (manual but quick)
1. Visual select logic → `"ay`
2. Create new function skeleton; paste with `]p`
3. Replace in call sites: grep usages → code action if lang server supports “extract” / otherwise `:cdo` edits

---

## 🧰 Handy opts to remember
- Soft‑wrap code while preserving motions: `:set wrap linebreak breakindent`
- Keep cursor centered when jumping: `:nnoremap n nzzzv` and `:nnoremap N Nzzzv`
- Show invisible whitespace (spot refactor points): `:set list listchars=tab:→\ ,trail:·`

---

## 🩹 If keys differ in your LazyVim
LazyVim evolves; to confirm current mappings:
- `:Telescope keymaps` → search your active keymaps
- `:map gd` / `:map <leader>ca` → check specific bindings
- `:Lazy health` and `:checkhealth` for plugin status
