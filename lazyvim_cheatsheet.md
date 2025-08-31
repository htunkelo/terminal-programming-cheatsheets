# 📝 LazyVim Cheatsheet (Day-to-Day)

## 🔎 Search & Replace
- `/pattern` → Search forward  
- `?pattern` → Search backward  
- `n` / `N` → Next / Prev match  
- `:%s/foo/bar/g` → Replace all `foo` → `bar`  
- `:%s/foo/bar/gc` → Replace w/ confirmation  
- `:s/foo/bar/g` → Replace in current line  

## 📋 Copy & Paste
- `yy` → Yank (copy) line  
- `p` → Paste after cursor  
- `P` → Paste before cursor  
- `yw` → Yank word  
- `y$` → Yank to end of line  
- `dd` → Cut (delete) line  
- `x` → Delete char  

## 🚀 Moving Around
- `h / l` → Move left / right  
- `w / e / b` → Next word / word end / prev word  
- `0 / ^ / $` → Line start / first nonblank / line end  
- `gg` → Go to top  
- `G` → Go to bottom  
- `:n` → Go to line n  
- `%` → Jump to matching `() {} []`  

## 📁 File Explorer (Neo-tree)
- `<leader> e` → Toggle file tree  
- Ctrl + w h Goto left window (file tree)
- Ctrl + w l Goto right windows (src)
- `a` → Add file  
- `d` → Delete file  
- `r` → Rename file  
- `c` → Copy file  
- `p` → Paste file  
- `q` → Close tree  

## 📂 Buffer / File Management
- `<leader> f f` → Find file (Telescope)  
- `<leader> f g` → Live grep  
- `<leader> f r` → Recent files  
- `<leader> ,` → Switch buffer  
- `:bd` → Close buffer  
- `<leader> b d` → Delete buffer  
- `<leader> q` → Quit window  

## 🪄 Code Editing
- `>>` / `<<` → Indent / Unindent line  
- `=` → Auto-indent  
- `.` → Repeat last command  
- `u` → Undo  
- `<C-r>` → Redo  
- `v` → Visual mode (select)  
- `V` → Visual line  
- `<C-v>` → Visual block  

## 🔧 Splits & Windows
- `<C-w> v` → Split vertical  
- `<C-w> s` → Split horizontal  
- `<C-w> q` → Close split  
- `<C-w> h/j/k/l` → Move between splits  
- `<C-w> =` → Balance splits  

## 🧭 Git (LazyVim)
- `<leader> g g` → LazyGit  
- `<leader> g c` → Git commit  
- `<leader> g p` → Git push  
- `<leader> g l` → Git log  

---
⚡ **Tip**: LazyVim uses `<leader>` as `SPACE`.  
So `<leader> e` = `SPACE + e`.  
