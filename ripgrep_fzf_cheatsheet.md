# ⚡ ripgrep (rg) + fzf Cheatsheet

> Fast project search with previews, jump-to-line, and live filtering. Minimal, narrow-pane friendly.

---

## 🔎 ripgrep basics
- `rg foo` — search recursively for `foo`
- `rg -n foo` — show line numbers
- `rg -H foo` — show filenames
- `rg -S foo` — smart case (uppercase in query = case-sensitive)
- `rg -i foo` — case-insensitive
- `rg -F foo` — fixed string (no regex)
- `rg -w foo` — whole word
- `rg -C2 foo` — 2 lines of context (`-A` after, `-B` before)
- `rg -g '!*.min.js' foo` — glob exclude
- `rg -t py foo` — by language type (see `rg --type-list`)
- `rg --hidden -g '!.git' foo` — include dotfiles except .git
- `rg -uu foo` — search everything (ignore .gitignore/etc.)

## 📁 Paths
- `rg foo path/` — limit to a dir
- `rg foo file1 file2` — limit to files

## 🧪 Regex quickies
- `rg '^def ' -t py` — python defs
- `rg '\bTODO\b'` — TODOs
- `rg 'foo|bar'` — alternation
- `rg '(?i)foo'` — inline case-insensitive

---

## 🔭 fzf basics
- `fzf` — fuzzy-find from STDIN
- Keys: `Tab` multi-select • `Ctrl-/` toggle preview • `Ctrl-r` history
- Query: spaces=AND, `|`=OR, `^foo` prefix, `foo$` suffix, `'foo` exact

---

## 🧩 rg → fzf → nvim (jump to file:line)
```bash
rg -nH --color=never <pattern> | fzf --delimiter :       --with-nth=1,2,3..       --preview 'bat --style=numbers --color=always --line-range :500 {1} --highlight-line {2}'       --preview-window=right,60%       --bind 'enter:become(nvim {1} +{2})'
```
No `bat`? Use: `--preview "sed -n {2},+200p {1}"`.

### Open multiple matches
```bash
rg -nH --color=never <pattern> | fzf -m --delimiter : --bind 'enter:become(nvim {+1} +{2})'
```

---

## 🔁 Live grep UI (reload on type)
```bash
fzf --phony --query ''   --bind 'change:reload:rg -nH --color=never --hidden -g !.git -- {q} || true'   --preview 'bat --style=numbers --color=always --line-range :400 {1} --highlight-line {2}'   --delimiter : --with-nth=1,2,3..   --bind 'enter:become(nvim {1} +{2})'
```

---

## 📚 Find files (fd) → fzf
```bash
fd -H -t f -E .git | fzf --preview 'bat --style=numbers --color=always --line-range :300 {}'       --bind 'enter:become(nvim {})'
```
No `fd`? `rg --files --hidden -g !.git`

---

## 🔍 Grep within a selected file
```bash
FILE="$(rg --files -g '!*.git/*' | fzf --preview 'bat --color=always --style=numbers {}')"
[ -n "$FILE" ] && rg -nH <pattern> "$FILE" | fzf --delimiter : --bind 'enter:become(nvim {1} +{2})'
```

---

## 🧹 Replace workflow (safe)
`rg` doesn’t in-place replace. Use `-l` (files only) + `sed` (or `sd`):
```bash
# preview files to change
rg -l 'from_old' | fzf -m

# replace in selected files (GNU sed)
rg -l -0 'from_old' | xargs -0 sed -i 's/from_old/to_new/g'
# or: sd 'from_old' 'to_new' $(rg -l from_old)
```

---

## 🧭 Ignore patterns
`.rgignore` (in repo root) examples:
```
node_modules/
dist/
.build/
*.min.js
*.lock
```
`rg` reads `.gitignore`, `.ignore`, `.rgignore` by default.

---

## 🧰 Handy aliases
```bash
alias rgi='rg --hidden -g !.git'
alias rgg='rg -nH --color=never'
alias ff='fd -H -t f -E .git'
alias fzfopen="fzf --bind 'enter:become(nvim {})'"
```

---

## 🛠️ Troubleshooting
- No results? Try `-uu` to bypass ignores.
- Binary noise? ripgrep skips binaries by default; add `--binary` to include.
- Color issues in preview? Use `rg --color=never` + `bat --color=always`.
