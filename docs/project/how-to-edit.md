# How to Edit This Wiki

Every page is a plain Markdown file in the `docs/` folder of the wiki repo. Push to `main` and the site rebuilds itself in ~1 minute. No local tools required.

## Quickest way (browser)

1. Click the **pencil icon** (top right of any page): it opens the file on GitHub.
2. Edit, then **Commit changes**.
3. Wait a minute, refresh the site.

## Local way (git)

```
git clone https://github.com/cap7n/towerdrop-docs.git
```

Edit files in `docs/`, then commit and push. Done.

## Adding a new page

1. Create `docs/<section>/<name>.md`
2. Add it to the `nav:` list in `mkdocs.yml`
3. Push

## Optional: live preview on your machine

Only if you want to see changes before pushing (needs Python):

```
pip install mkdocs-material
mkdocs serve
```

Then open `http://127.0.0.1:8000`. Auto-reloads as you edit.

## Formatting cheatsheet

- `# Title`, `## Section`: headings
- `**bold**`, `*italic*`, `` `code` ``
- `[link text](../game/cart.md)`: link to another page (relative path)
- Tables: `| a | b |` rows with `|---|---|` under the header
- Task lists: `- [ ]` and `- [x]` render as checkboxes
- Callout boxes:

```
!!! note "Title"
    Indented content becomes a highlighted box.
```

`note`, `tip`, `warning`, `bug`, `danger` all work as box types.

## House rules

1. **The wiki records decisions, it doesn't replace making them.** Undecided things get marked as such (a `!!! warning` box) or go in the [Backlog](backlog.md) as **(idea)**.
2. When a decision is made, add it to the [Decision Log](decisions.md) **with the why**.
3. Prefer editing an existing page over creating a new one.
4. **Be honest about status.** A page that describes something as built when it's half-wired is worse than one that says "wired, coverage uneven." Match the tag to reality.
