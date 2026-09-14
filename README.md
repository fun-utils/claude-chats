# claude-chats

Your Claude Code chats in one full-screen terminal app. Find any chat, then move it, fork it, symlink it to another project folder, or make it show up in Claude Desktop. One bash script, no Python.

## Why

Claude Code keeps every chat as one file: `~/.claude/projects/<folder name>/<chat id>.jsonl`. The folder name is the project folder the chat started in. So a chat is stuck to that folder, and Claude Desktop only lists chats it made itself. This app fixes both, and lets you search all chats at once.

## Install

```bash
git clone git@github.com:fun-utils/claude-chats.git ~/claude-chats
ln -s ~/claude-chats/claude-chats ~/.local/bin/claude-chats
claude-chats
```

Needs Linux, bash 5+, `gawk`, `jq`, GNU `find` and `sort`. Optional: `rg` (ripgrep) to search words inside chats, `xclip` or `wl-copy` to copy an id.

## The screen

- **Top bar**: the `◆ claude-chats` badge, the tabs `1 Chats`, `2 Projects`, `3 Folder tree`, and the buttons `Sort`, `Actions`, `Preview`, `Rescan`, `Help`, `Quit`.
- **List card**: a search line, clickable column titles, and the list. A thin scrollbar appears when the list is longer than the card.
  - *Chats*: title, project folder, when, size, messages, where it was made (terminal, desktop, sdk/acp = Zed and other editors). `◆` = shown in Claude Desktop, `↗` = a symlink.
  - *Projects*: one row per folder with chat count, last use and size. Enter shows only that folder's chats.
  - *Folder tree*: folders with their chats inside. Click a folder to fold it.
- **Preview card**: the selected chat's id, folder, dates, size, first message and last answer.

## Right-click

| Where you right-click | What you get |
|---|---|
| A chat | Resume, Move, Fork, Symlink, Show in / remove from Claude Desktop, Copy id, File paths, Open in Zed |
| A folder in the tree | Show only its chats, Fold / unfold, Copy the path |
| Anywhere else | Every view, search, sort, preview on/off, show all, rescan, help, quit |

Arrows, Enter and the letter shown next to each item also work in a menu. Esc closes it.

## What the actions do

| Action | What happens |
|---|---|
| Resume | Opens the chat with `claude --resume <id>` in its folder, in this terminal |
| Move | Moves the chat to another folder. Folder paths inside the chat are rewritten, the id stays, Claude Desktop is updated |
| Fork | Copies the chat under a new id, into the same or another folder |
| Symlink | Links the same chat into another folder, so `claude --resume <id>` works from both |
| Desktop | Adds or removes the chat in Claude Desktop. Quit and reopen Claude Desktop to see it |

Move and remove-from-Desktop ask first.

## Keys

| Key | What it does |
|---|---|
| `Enter` | Actions menu for the selected chat |
| `r` `m` `f` `l` `d` | Resume, Move, Fork, Symlink, Desktop |
| `c` `i` `z` | Copy id, File paths, Open in Zed (how) |
| `/` | Search titles, first messages, folders and ids (every word must match) |
| `w` | Switch to searching words inside the chats (needs `rg`) |
| `s` | Sort menu. Clicking a column title also sorts, click again to flip |
| `1` `2` `3`, `Tab` | Switch view |
| `p` | Preview on / off |
| `Esc` | Clear the search and the folder filter |
| `F5` `?` `q` | Rescan, Help, Quit |

## Command line

```bash
claude-chats list [folder]          # plain list, newest first
claude-chats move <id> <folder>     # move to another folder
claude-chats fork <id> [folder]     # copy under a new id
claude-chats link <id> <folder>     # symlink into another folder
claude-chats desktop-add <id>       # show in Claude Desktop
claude-chats desktop-rm <id>        # remove from Claude Desktop (the chat stays)
```

## Notes

- Fast: chats are read in parallel once, then cached in `~/.cache/claude-chats/index.tsv`. Only changed chats are read again. About 500 chats (1.7 GB) take 2-3 s the first time, 0.5 s after.
- Claude Desktop keeps one record per chat in `~/.config/Claude/claude-code-sessions/`. It reads them only when it starts.
- Settings by environment: `CLAUDE_CONFIG_DIR` (default `~/.claude`), `CLAUDE_CHATS_DESKTOP_DIR`, `CLAUDE_CHATS_INDEX`.
- Colors use the 256-color palette. Needs a UTF-8 terminal.
- Not included: deleting chats.
